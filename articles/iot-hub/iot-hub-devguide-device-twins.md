---
title: "aaaUnderstand dvojčata zařízení Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použití zařízení dvojčata toosynchronize stavu a konfiguraci dat mezi IoT Hub a zařízení"
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7dade18665108ed352ff3d18e864dc34f451bbf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="206d9-103">Rady pro pochopení a použít dvojčata zařízení IoT hub</span><span class="sxs-lookup"><span data-stu-id="206d9-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="206d9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="206d9-104">Overview</span></span>
<span data-ttu-id="206d9-105">*Dvojčata zařízení* jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="206d9-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="206d9-106">IoT Hub trvá dvojče zařízení pro každé zařízení připojení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="206d9-106">IoT Hub persists a device twin for each device that you connect tooIoT Hub.</span></span> <span data-ttu-id="206d9-107">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="206d9-107">This article describes:</span></span>

* <span data-ttu-id="206d9-108">Hello struktura dvojče zařízení hello: *značky*, *požadované* a *hlášené vlastnosti*, a</span><span class="sxs-lookup"><span data-stu-id="206d9-108">hello structure of hello device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="206d9-109">Hello operace, které aplikace pro zařízení a back-EndY můžete provádět na dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-109">hello operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="206d9-110">V současné době jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-110">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="206d9-111">Odkazovat toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="206d9-111">Refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>
> 
> 

### <a name="when-toouse"></a><span data-ttu-id="206d9-112">Když toouse</span><span class="sxs-lookup"><span data-stu-id="206d9-112">When toouse</span></span>
<span data-ttu-id="206d9-113">Použijte dvojčata zařízení na:</span><span class="sxs-lookup"><span data-stu-id="206d9-113">Use device twins to:</span></span>

* <span data-ttu-id="206d9-114">Metadata specifická pro zařízení ukládat v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-114">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="206d9-115">Například hello počítače prodejních umístění nasazení.</span><span class="sxs-lookup"><span data-stu-id="206d9-115">For example, hello deployment location of a vending machine.</span></span>
* <span data-ttu-id="206d9-116">Sestavy aktuální informace o stavu, jako jsou k dispozici funkce a podmínky z vaší aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="206d9-117">Například zařízení je připojených tooyour IoT hub přes mobilní nebo Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="206d9-117">For example, a device is connected tooyour IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="206d9-118">Synchronizujte hello stavu pracovních postupů dlouho běžící mezi aplikace zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="206d9-118">Synchronize hello state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="206d9-119">Například při řešení hello back end určuje hello nové tooinstall verze firmwaru a sestavy aplikace hello zařízení hello různé fáze procesu aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-119">For example, when hello solution back end specifies hello new firmware version tooinstall, and hello device app reports hello various stages of hello update process.</span></span>
* <span data-ttu-id="206d9-120">Dotaz na vaše zařízení metadata, konfigurace nebo stavu.</span><span class="sxs-lookup"><span data-stu-id="206d9-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="206d9-121">Odkazovat příliš[pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] pokyny k používání hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="206d9-121">Refer too[Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="206d9-122">Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pokyny k použití požadované vlastnosti, přímé metody nebo zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-122">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="206d9-123">Dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="206d9-123">Device twins</span></span>
<span data-ttu-id="206d9-124">Dvojčata zařízení ukládat informace týkající se zařízení který:</span><span class="sxs-lookup"><span data-stu-id="206d9-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="206d9-125">Zařízení a zpět končí pomocí toosynchronize zařízení podmínky a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="206d9-125">Device and back ends can use toosynchronize device conditions and configuration.</span></span>
* <span data-ttu-id="206d9-126">back-end Hello řešení můžete použít tooquery a cíle dlouhotrvající operace.</span><span class="sxs-lookup"><span data-stu-id="206d9-126">hello solution back end can use tooquery and target long-running operations.</span></span>

<span data-ttu-id="206d9-127">životní cyklus Hello dvojče zařízení je propojený odpovídající toohello [identitu zařízení][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="206d9-127">hello lifecycle of a device twin is linked toohello corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="206d9-128">Dvojčata zařízení jsou implicitně vytvořen a odstraněn, když je vytvořené nebo odstraněné v IoT Hub novou identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="206d9-129">Dvojče zařízení je dokument JSON, který zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="206d9-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="206d9-130">**Značky**.</span><span class="sxs-lookup"><span data-stu-id="206d9-130">**Tags**.</span></span> <span data-ttu-id="206d9-131">Oddíl hello dokument JSON, který hello back-end řešení může číst z a zapisovat.</span><span class="sxs-lookup"><span data-stu-id="206d9-131">A section of hello JSON document that hello solution back end can read from and write to.</span></span> <span data-ttu-id="206d9-132">Značky nejsou viditelné toodevice aplikace.</span><span class="sxs-lookup"><span data-stu-id="206d9-132">Tags are not visible toodevice apps.</span></span>
* <span data-ttu-id="206d9-133">**Požadovaného vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="206d9-133">**Desired properties**.</span></span> <span data-ttu-id="206d9-134">Použít konfiguraci zařízení hlášené vlastnosti toosynchronize nebo podmínky.</span><span class="sxs-lookup"><span data-stu-id="206d9-134">Used along with reported properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="206d9-135">Požadované vlastnosti lze nastavit pouze zpět hello řešení end a mohli číst hello zařízení aplikací.</span><span class="sxs-lookup"><span data-stu-id="206d9-135">Desired properties can only be set by hello solution back end and can be read by hello device app.</span></span> <span data-ttu-id="206d9-136">aplikace Hello zařízení můžete také upozorněni v reálném čase změn v hello požadovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="206d9-136">hello device app can also be notified in real time of changes in hello desired properties.</span></span>
* <span data-ttu-id="206d9-137">**Hlášené vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="206d9-137">**Reported properties**.</span></span> <span data-ttu-id="206d9-138">Použít konfiguraci zařízení toosynchronize požadované vlastnosti nebo podmínky.</span><span class="sxs-lookup"><span data-stu-id="206d9-138">Used along with desired properties toosynchronize device configuration or conditions.</span></span> <span data-ttu-id="206d9-139">Hlášené vlastnosti lze nastavit pouze hello zařízení aplikace a můžete číst a je dotazován pomocí back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="206d9-139">Reported properties can only be set by hello device app and can be read and queried by hello solution back end.</span></span>

<span data-ttu-id="206d9-140">Kromě toho obsahuje hello kořen dokumentu JSON twin zařízení hello hello jen pro čtení vlastnosti z odpovídající identitu zařízení hello uložené v hello [registru identit][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="206d9-140">Additionally, hello root of hello device twin JSON document contains hello read-only properties from hello corresponding device identity stored in hello [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="206d9-141">Hello následující příklad ukazuje dvojče zařízení dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="206d9-141">hello following example shows a device twin JSON document:</span></span>

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

<span data-ttu-id="206d9-142">V hello kořenový objekt, jsou hello vlastnosti systému a kontejner objektů pro `tags` a obě `reported` a `desired` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-142">In hello root object, are hello system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="206d9-143">Hello `properties` kontejner obsahuje některé prvky jen pro čtení (`$metadata`, `$etag`, a `$version`) popsané v hello [metadat zařízení twin] [ lnk-twin-metadata] a [ Optimistickou metodu souběžného] [ lnk-concurrency] oddíly.</span><span class="sxs-lookup"><span data-stu-id="206d9-143">hello `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in hello [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="206d9-144">Příklad hlášené vlastnost</span><span class="sxs-lookup"><span data-stu-id="206d9-144">Reported property example</span></span>
<span data-ttu-id="206d9-145">V předchozím příkladu hello dvojče zařízení hello obsahuje `batteryLevel` vlastnosti, který je hlášen aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-145">In hello previous example, hello device twin contains a `batteryLevel` property that is reported by hello device app.</span></span> <span data-ttu-id="206d9-146">Tato vlastnost je možné tooquery a provozovat na zařízení podle hello poslední hlášené stav baterie.</span><span class="sxs-lookup"><span data-stu-id="206d9-146">This property makes it possible tooquery and operate on devices based on hello last reported battery level.</span></span> <span data-ttu-id="206d9-147">Další příklady zahrnují hello zařízení aplikace zařízení možnosti vytváření sestav nebo možnosti připojení.</span><span class="sxs-lookup"><span data-stu-id="206d9-147">Other examples include hello device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="206d9-148">Hlášené vlastnosti zjednodušit scénáře, kde je back-end hello řešení zajímá hello poslední známá hodnota vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-148">Reported properties simplify scenarios where hello solution back end is interested in hello last known value of a property.</span></span> <span data-ttu-id="206d9-149">Použití [zpráv typu zařízení cloud] [ lnk-d2c] Pokud back-end hello řešení musí tooprocess telemetrie zařízení v podobě hello pořadí označen časovým razítkem události, jako je například časové řady.</span><span class="sxs-lookup"><span data-stu-id="206d9-149">Use [device-to-cloud messages][lnk-d2c] if hello solution back end needs tooprocess device telemetry in hello form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="206d9-150">Příklad požadovanou vlastnost</span><span class="sxs-lookup"><span data-stu-id="206d9-150">Desired property example</span></span>
<span data-ttu-id="206d9-151">V předchozím příkladu hello hello `telemetryConfig` potřeby dvojče zařízení a hlášené vlastnosti jsou používány back-end hello řešení a hello zařízení aplikaci toosynchronize hello telemetrická data konfigurace pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-151">In hello previous example, hello `telemetryConfig` device twin desired and reported properties are used by hello solution back end and hello device app toosynchronize hello telemetry configuration for this device.</span></span> <span data-ttu-id="206d9-152">Například:</span><span class="sxs-lookup"><span data-stu-id="206d9-152">For example:</span></span>

1. <span data-ttu-id="206d9-153">back-end Hello řešení nastaví vlastnost hello požadovaného s hodnotou hello požadovaných konfigurací.</span><span class="sxs-lookup"><span data-stu-id="206d9-153">hello solution back end sets hello desired property with hello desired configuration value.</span></span> <span data-ttu-id="206d9-154">Zde je část hello hello dokumentu s hello požadovaných vlastností nastavenou:</span><span class="sxs-lookup"><span data-stu-id="206d9-154">Here is hello portion of hello document with hello desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="206d9-155">Hello zařízení aplikaci je upozornění na změnu hello okamžitě, pokud je připojený, nebo v hello nejprve znovu připojit.</span><span class="sxs-lookup"><span data-stu-id="206d9-155">hello device app is notified of hello change immediately if connected, or at hello first reconnect.</span></span> <span data-ttu-id="206d9-156">Hello aplikaci zařízení hlásí hello aktualizovat konfiguraci (nebo chybový stav pomocí hello `status` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="206d9-156">hello device app then reports hello updated configuration (or an error condition using hello `status` property).</span></span> <span data-ttu-id="206d9-157">Tady je hello část hello hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="206d9-157">Here is hello portion of hello reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="206d9-158">back-end Hello řešení můžete hello výsledky operace konfigurace hello sledovat prostřednictvím zařízení, pomocí [dotazování] [ lnk-query] dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-158">hello solution back end can track hello results of hello configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="206d9-159">Hello předchozí fragmenty kódu jsou příklady, optimalizované pro čitelnost tooencode jedním ze způsobů konfigurace zařízení a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="206d9-159">hello preceding snippets are examples, optimized for readability, of one way tooencode a device configuration and its status.</span></span> <span data-ttu-id="206d9-160">IoT Hub nepředstavuje určité schéma pro dvojče zařízení hello potřeby a hlásí vlastnosti ve dvojčata zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-160">IoT Hub does not impose a specific schema for hello device twin desired and reported properties in hello device twins.</span></span>
> 
> 

<span data-ttu-id="206d9-161">Můžete použít dvojčata toosynchronize dlouhotrvající operace jako jsou aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="206d9-161">You can use twins toosynchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="206d9-162">Další informace o způsobu toouse vlastnosti toosynchronize a sledovat dlouhotrvající operace v zařízeních, najdete v části [použití požadovaného vlastnosti tooconfigure zařízení][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="206d9-162">For more information on how toouse properties toosynchronize and track a long running operation across devices, see [Use desired properties tooconfigure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="206d9-163">Operace back-end</span><span class="sxs-lookup"><span data-stu-id="206d9-163">Back-end operations</span></span>
<span data-ttu-id="206d9-164">back-end Hello řešení funguje na dvojče zařízení hello pomocí hello následující atomické operací, k dispozici prostřednictvím protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="206d9-164">hello solution back end operates on hello device twin using hello following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="206d9-165">**Načíst dvojče zařízení tak, že id**. Tato operace vrátí hello zařízení twin dokumentu, včetně značky a potřeby nahlásí a vlastnosti systému.</span><span class="sxs-lookup"><span data-stu-id="206d9-165">**Retrieve device twin by id**. This operation returns hello device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="206d9-166">**Částečné aktualizace dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="206d9-166">**Partially update device twin**.</span></span> <span data-ttu-id="206d9-167">Tato operace umožňuje hello řešení back-end toopartially aktualizace hello značky nebo požadované vlastnosti v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-167">This operation enables hello solution back end toopartially update hello tags or desired properties in a device twin.</span></span> <span data-ttu-id="206d9-168">částečné aktualizace Hello je vyjádřen jako dokument JSON, který přidá nebo aktualizuje libovolné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-168">hello partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="206d9-169">Příliš nastaveny vlastnosti`null` se odeberou.</span><span class="sxs-lookup"><span data-stu-id="206d9-169">Properties set too`null` are removed.</span></span> <span data-ttu-id="206d9-170">Hello následující příklad vytvoří novou požadovanou vlastnost s hodnotou `{"newProperty": "newValue"}`, přepíše existující hodnotu hello `existingProperty` s `"otherNewValue"`a také odebere `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="206d9-170">hello following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites hello existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="206d9-171">Tooexisting potřeby vlastnosti a značky jsou provedeny žádné další změny:</span><span class="sxs-lookup"><span data-stu-id="206d9-171">No other changes are made tooexisting desired properties or tags:</span></span>
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. <span data-ttu-id="206d9-172">**Nahraďte požadované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="206d9-172">**Replace desired properties**.</span></span> <span data-ttu-id="206d9-173">Tato operace povoluje hello řešení back-end toocompletely přepsat všechny existující požadované vlastnosti a nahraďte nový dokument JSON pro `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="206d9-173">This operation enables hello solution back end toocompletely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="206d9-174">**Nahraďte značky**.</span><span class="sxs-lookup"><span data-stu-id="206d9-174">**Replace tags**.</span></span> <span data-ttu-id="206d9-175">Tato operace povoluje hello řešení back-end toocompletely přepsat všechny existující značky a nahraďte nový dokument JSON pro `tags`.</span><span class="sxs-lookup"><span data-stu-id="206d9-175">This operation enables hello solution back end toocompletely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="206d9-176">**Přijímat oznámení twin**.</span><span class="sxs-lookup"><span data-stu-id="206d9-176">**Receive twin notifications**.</span></span> <span data-ttu-id="206d9-177">Tato operace povoluje toobe back-end řešení hello oznámení o změně hello twin.</span><span class="sxs-lookup"><span data-stu-id="206d9-177">This operation allows hello solution back end toobe notified when hello twin is modified.</span></span> <span data-ttu-id="206d9-178">toodo tedy řešení IoT potřebuje toocreate trasu a tooset hello zdroj dat rovná příliš*twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="206d9-178">toodo so, your IoT solution needs toocreate a route and tooset hello Data Source equal too*twinChangeEvents*.</span></span> <span data-ttu-id="206d9-179">Ve výchozím nastavení žádné twin oznámení se odesílají, tedy předem neexistuje žádný takový trasy.</span><span class="sxs-lookup"><span data-stu-id="206d9-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="206d9-180">Pokud je příliš vysoká rychlost hello změny, nebo z jiných důvodů, jako je například interní chyby, hello IoT Hub může odeslat pouze jedno oznámení, která obsahuje všechny změny.</span><span class="sxs-lookup"><span data-stu-id="206d9-180">If hello rate of change is too high, or for other reasons, such as internal failures, hello IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="206d9-181">Proto, pokud aplikace potřebuje spolehlivé auditování a protokolování všechny zprostředkující stavy, pak je stále doporučujeme použít D2C zprávy.</span><span class="sxs-lookup"><span data-stu-id="206d9-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="206d9-182">zpráva oznámení twin Hello zahrnuje vlastnosti a text.</span><span class="sxs-lookup"><span data-stu-id="206d9-182">hello twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="206d9-183">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="206d9-183">Properties</span></span>

    | <span data-ttu-id="206d9-184">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="206d9-184">Name</span></span> | <span data-ttu-id="206d9-185">Hodnota</span><span class="sxs-lookup"><span data-stu-id="206d9-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="206d9-186">$content – typ</span><span class="sxs-lookup"><span data-stu-id="206d9-186">$content-type</span></span> | <span data-ttu-id="206d9-187">application/json</span><span class="sxs-lookup"><span data-stu-id="206d9-187">application/json</span></span> |
    <span data-ttu-id="206d9-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="206d9-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="206d9-189">Čas odeslání oznámení hello</span><span class="sxs-lookup"><span data-stu-id="206d9-189">Time when hello notification was sent</span></span> |
    <span data-ttu-id="206d9-190">$iothub – zpráva – zdroj</span><span class="sxs-lookup"><span data-stu-id="206d9-190">$iothub-message-source</span></span> | <span data-ttu-id="206d9-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="206d9-191">twinChangeEvents</span></span> |
    <span data-ttu-id="206d9-192">$content – kódování</span><span class="sxs-lookup"><span data-stu-id="206d9-192">$content-encoding</span></span> | <span data-ttu-id="206d9-193">znakové sady UTF-8</span><span class="sxs-lookup"><span data-stu-id="206d9-193">utf-8</span></span> |
    <span data-ttu-id="206d9-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="206d9-194">deviceId</span></span> | <span data-ttu-id="206d9-195">ID zařízení hello</span><span class="sxs-lookup"><span data-stu-id="206d9-195">Id of hello device</span></span> |
    <span data-ttu-id="206d9-196">hubName</span><span class="sxs-lookup"><span data-stu-id="206d9-196">hubName</span></span> | <span data-ttu-id="206d9-197">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="206d9-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="206d9-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="206d9-198">operationTimestamp</span></span> | <span data-ttu-id="206d9-199">[ISO8601] časové razítko operace</span><span class="sxs-lookup"><span data-stu-id="206d9-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="206d9-200">schéma zprávy iothub</span><span class="sxs-lookup"><span data-stu-id="206d9-200">iothub-message-schema</span></span> | <span data-ttu-id="206d9-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="206d9-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="206d9-202">opType</span><span class="sxs-lookup"><span data-stu-id="206d9-202">opType</span></span> | <span data-ttu-id="206d9-203">"replaceTwin" nebo "updateTwin"</span><span class="sxs-lookup"><span data-stu-id="206d9-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="206d9-204">Vlastnosti zprávu systému mají předponu hello `'$'` symbol.</span><span class="sxs-lookup"><span data-stu-id="206d9-204">Message system properties are prefixed with hello `'$'` symbol.</span></span>

    - <span data-ttu-id="206d9-205">Tělo</span><span class="sxs-lookup"><span data-stu-id="206d9-205">Body</span></span>
        
    <span data-ttu-id="206d9-206">Tato část obsahuje všechny změny twin hello ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="206d9-206">This section includes all hello twin changes in a JSON format.</span></span> <span data-ttu-id="206d9-207">Používá stejný formát hello jako opravu, s hello rozdíl to může obsahovat všechny části twin: značky, properties.reported, properties.desired a že obsahuje prvky hello "$metadata".</span><span class="sxs-lookup"><span data-stu-id="206d9-207">It uses hello same format as a patch, with hello difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains hello “$metadata” elements.</span></span> <span data-ttu-id="206d9-208">Například:</span><span class="sxs-lookup"><span data-stu-id="206d9-208">For example,</span></span>
    ```
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ``` 

<span data-ttu-id="206d9-209">Všechny hello předchozí operace podporu [optimistickou metodu souběžného] [ lnk-concurrency] a vyžadovat hello **ServiceConnect** oprávnění, jak jsou definovány v hello [zabezpečení ] [ lnk-security] článku.</span><span class="sxs-lookup"><span data-stu-id="206d9-209">All hello preceding operations support [Optimistic concurrency][lnk-concurrency] and require hello **ServiceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="206d9-210">Kromě toho toothese operace, řešení hello back end může:</span><span class="sxs-lookup"><span data-stu-id="206d9-210">In addition toothese operations, hello solution back end can:</span></span>

* <span data-ttu-id="206d9-211">Dotaz hello dvojčata zařízení pomocí hello SQL jako [IoT Hub dotazovací jazyk][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="206d9-211">Query hello device twins using hello SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="206d9-212">Provádění operací na velkých sad dvojčata zařízení pomocí [úlohy][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="206d9-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="206d9-213">Operace zařízení</span><span class="sxs-lookup"><span data-stu-id="206d9-213">Device operations</span></span>
<span data-ttu-id="206d9-214">aplikace Hello zařízení funguje na dvojče zařízení hello pomocí hello následující atomické operací:</span><span class="sxs-lookup"><span data-stu-id="206d9-214">hello device app operates on hello device twin using hello following atomic operations:</span></span>

1. <span data-ttu-id="206d9-215">**Načtení dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="206d9-215">**Retrieve device twin**.</span></span> <span data-ttu-id="206d9-216">Tato operace vrátí hello zařízení twin dokument (včetně značky a potřeby, hlášené a vlastnosti systému) pro hello aktuálně připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-216">This operation returns hello device twin document (including tags and desired, reported and system properties) for hello currently connected device.</span></span>
2. <span data-ttu-id="206d9-217">**Částečné aktualizace hlášené vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="206d9-217">**Partially update reported properties**.</span></span> <span data-ttu-id="206d9-218">Tato operace povoluje hello částečné aktualizace hello hlášené vlastnosti hello aktuálně připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-218">This operation enables hello partial update of hello reported properties of hello currently connected device.</span></span> <span data-ttu-id="206d9-219">Tato operace hello používá stejné JSON aktualizovat formátu této hello řešení back end používá pro částečné aktualizace požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-219">This operation uses hello same JSON update format that hello solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="206d9-220">**Sledovat požadované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="206d9-220">**Observe desired properties**.</span></span> <span data-ttu-id="206d9-221">Hello aktuálně připojené zařízení můžete zvolit toobe oznámeno dějí aktualizace toohello požadovaných vlastností.</span><span class="sxs-lookup"><span data-stu-id="206d9-221">hello currently connected device can choose toobe notified of updates toohello desired properties when they happen.</span></span> <span data-ttu-id="206d9-222">Hello zařízení obdrží hello stejného formuláře aktualizace (náhrada částečně nebo zcela) provedený back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="206d9-222">hello device receives hello same form of update (partial or full replacement) executed by hello solution back end.</span></span>

<span data-ttu-id="206d9-223">Všechny hello předchozí operace vyžadují hello **DeviceConnect** oprávnění, jak jsou definovány v hello [zabezpečení] [ lnk-security] článku.</span><span class="sxs-lookup"><span data-stu-id="206d9-223">All hello preceding operations require hello **DeviceConnect** permission, as defined in hello [Security][lnk-security] article.</span></span>

<span data-ttu-id="206d9-224">Hello [sady SDK pro zařízení Azure IoT] [ lnk-sdks] bylo snadné toouse hello předcházející operace z mnoha jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="206d9-224">hello [Azure IoT device SDKs][lnk-sdks] make it easy toouse hello preceding operations from many languages and platforms.</span></span> <span data-ttu-id="206d9-225">Další informace o hello podrobnosti primitiv IoT Hub pro požadované vlastnosti synchronizace naleznete v [toku opětovné připojení zařízení][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="206d9-225">More information on hello details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="206d9-226">V současné době jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-226">Currently, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="206d9-227">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="206d9-227">Reference topics:</span></span>
<span data-ttu-id="206d9-228">Hello následující odkazy na témata poskytují další informace o řízení přístupu tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="206d9-228">hello following reference topics provide you with more information about controlling access tooyour IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="206d9-229">Formát značky a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="206d9-229">Tags and properties format</span></span>
<span data-ttu-id="206d9-230">Značky, požadovanou a oznámená vlastnosti jsou objekty JSON s hello následující omezení:</span><span class="sxs-lookup"><span data-stu-id="206d9-230">Tags, desired, and reported properties are JSON objects with hello following restrictions:</span></span>

* <span data-ttu-id="206d9-231">Všechny klíče v objekty JSON jsou malá a velká písmena 64 bajtů řetězců v kódu UNICODE UTF-8.</span><span class="sxs-lookup"><span data-stu-id="206d9-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="206d9-232">Povolené znaky vyloučit řídicí znaky UNICODE (segmenty C0 a C1), a `'.'`, `' '`, a `'$'`.</span><span class="sxs-lookup"><span data-stu-id="206d9-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="206d9-233">Všechny hodnoty v objektů JSON může mít následující typy JSON hello: logická hodnota, číslo, řetězec, objekt.</span><span class="sxs-lookup"><span data-stu-id="206d9-233">All values in JSON objects can be of hello following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="206d9-234">Pole nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="206d9-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="206d9-235">Všechny objekty JSON ve značkách, požadovanou a oznámená vlastnosti může mít maximální hloubka začlenění na 5.</span><span class="sxs-lookup"><span data-stu-id="206d9-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="206d9-236">Například hello následující objektu je platná:</span><span class="sxs-lookup"><span data-stu-id="206d9-236">For instance, hello following object is valid:</span></span>

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* <span data-ttu-id="206d9-237">Všechny hodnoty řetězce může být o maximální délce 512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="206d9-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="206d9-238">Velikost twin zařízení</span><span class="sxs-lookup"><span data-stu-id="206d9-238">Device twin size</span></span>
<span data-ttu-id="206d9-239">IoT Hub vynucuje omezení velikosti 8KB na hodnotách hello `tags`, `properties/desired`, a `properties/reported`, s výjimkou elementy jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="206d9-239">IoT Hub enforces an 8KB size limitation on hello values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="206d9-240">Hello velikost se počítá podle počítání řídit všechny znaky kódování UNICODE s výjimkou znaků (segmenty C0 a C1) a místo `' '` při zdá mimo řetězcová konstanta.</span><span class="sxs-lookup"><span data-stu-id="206d9-240">hello size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="206d9-241">IoT Hub s chybou odmítne všechny operace, které by zvětšete velikost hello těchto dokumentů přesahuje omezení hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-241">IoT Hub rejects with an error all operations that would increase hello size of those documents above hello limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="206d9-242">Metadata twin zařízení</span><span class="sxs-lookup"><span data-stu-id="206d9-242">Device twin metadata</span></span>
<span data-ttu-id="206d9-243">IoT Hub uchovává hello časové razítko hello poslední aktualizace pro každý objekt JSON v dvojče zařízení potřeby a který ohlásil vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-243">IoT Hub maintains hello timestamp of hello last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="206d9-244">časová razítka Hello se ve standardu UTC a v hello kódování [ISO8601] formátu `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="206d9-244">hello timestamps are in UTC and encoded in hello [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="206d9-245">Například:</span><span class="sxs-lookup"><span data-stu-id="206d9-245">For example:</span></span>

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

<span data-ttu-id="206d9-246">Tyto informace jsou uchovávány v každé úrovni (ne jenom hello nechá hello struktuře JSON) toopreserve aktualizace, které odebrat objekt klíče.</span><span class="sxs-lookup"><span data-stu-id="206d9-246">This information is kept at every level (not just hello leaves of hello JSON structure) toopreserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="206d9-247">Optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="206d9-247">Optimistic concurrency</span></span>
<span data-ttu-id="206d9-248">Značky, potřeby a jsou uvedeny vlastnosti všech optimistickou metodu souběžného podpory.</span><span class="sxs-lookup"><span data-stu-id="206d9-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="206d9-249">Značky mají značku ETag dle [RFC7232], představující reprezentace JSON hello značky.</span><span class="sxs-lookup"><span data-stu-id="206d9-249">Tags have an ETag, as per [RFC7232], that represents hello tag's JSON representation.</span></span> <span data-ttu-id="206d9-250">Značky etag binárním rozsáhlým můžete použít v operacích podmíněného aktualizace z konzistence tooensure back-end řešení hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-250">You can use ETags in conditional update operations from hello solution back end tooensure consistency.</span></span>

<span data-ttu-id="206d9-251">Dvojče zařízení potřeby a který ohlásil vlastnosti nemají značky etag binárním rozsáhlým, ale mají `$version` hodnotu, která je zaručeno toobe přírůstkové.</span><span class="sxs-lookup"><span data-stu-id="206d9-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed toobe incremental.</span></span> <span data-ttu-id="206d9-252">Stejně tooan značka ETag, hello verzi můžete používat hello aktualizace strany tooenforce konzistence aktualizací.</span><span class="sxs-lookup"><span data-stu-id="206d9-252">Similarly tooan ETag, hello version can be used by hello updating party tooenforce consistency of updates.</span></span> <span data-ttu-id="206d9-253">Například zařízení aplikaci pro hlášené vlastnost nebo hello řešení back-end pro požadovanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="206d9-253">For example, a device app for a reported property or hello solution back end for a desired property.</span></span>

<span data-ttu-id="206d9-254">Verze jsou užitečné také při observing agenta (například aplikace zařízení hello sledování hello požadovaných vlastností) musí sjednotit RAS mezi hello výsledek operace načtení a oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="206d9-254">Versions are also useful when an observing agent (such as hello device app observing hello desired properties) must reconcile races between hello result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="206d9-255">Hello části [toku opětovné připojení zařízení] [ lnk-reconnection] poskytuje další informace.</span><span class="sxs-lookup"><span data-stu-id="206d9-255">hello section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="206d9-256">Postup opětovné připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="206d9-256">Device reconnection flow</span></span>
<span data-ttu-id="206d9-257">IoT Hub nezachovává oznámení o aktualizacích požadované vlastnosti pro odpojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="206d9-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="206d9-258">Z toho vyplývá, že zařízení, která se připojuje musí načíst hello úplné požadované vlastnosti dokumentu v přidání toosubscribing pro oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="206d9-258">It follows that a device that is connecting must retrieve hello full desired properties document, in addition toosubscribing for update notifications.</span></span> <span data-ttu-id="206d9-259">Zadána možnost hello RAS mezi oznámení o aktualizaci a úplné načtení vhodné zajistit hello následující postup:</span><span class="sxs-lookup"><span data-stu-id="206d9-259">Given hello possibility of races between update notifications and full retrieval, hello following flow must be ensured:</span></span>

1. <span data-ttu-id="206d9-260">Aplikace zařízení připojí tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="206d9-260">Device app connects tooan IoT hub.</span></span>
2. <span data-ttu-id="206d9-261">Aplikace zařízení pro požadované vlastnosti Odběratel oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="206d9-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="206d9-262">Aplikace zařízení načte hello celého dokumentu pro požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="206d9-262">Device app retrieves hello full document for desired properties.</span></span>

<span data-ttu-id="206d9-263">aplikace Hello zařízení můžete ignorovat všechna oznámení s `$version` menší nebo roven hello verzi celého dokumentu načtené hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-263">hello device app can ignore all notifications with `$version` less or equal than hello version of hello full retrieved document.</span></span> <span data-ttu-id="206d9-264">Tento přístup je možné, protože IoT Hub zaručuje, že verze vždy zvýšit.</span><span class="sxs-lookup"><span data-stu-id="206d9-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="206d9-265">Tato logika je už implementované v hello [sady SDK pro zařízení Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="206d9-265">This logic is already implemented in hello [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="206d9-266">Tento popis je užitečný jenom v případě, že aplikace hello zařízení nelze použít sady SDK pro zařízení Azure IoT a musí programu hello MQTT rozhraní přímo.</span><span class="sxs-lookup"><span data-stu-id="206d9-266">This description is useful only if hello device app cannot use any of Azure IoT device SDKs and must program hello MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="206d9-267">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="206d9-267">Additional reference material</span></span>
<span data-ttu-id="206d9-268">Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:</span><span class="sxs-lookup"><span data-stu-id="206d9-268">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="206d9-269">Hello [koncové body centra IoT] [ lnk-endpoints] článek popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="206d9-269">hello [IoT Hub endpoints][lnk-endpoints] article describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="206d9-270">Hello [omezování a kvóty] [ lnk-quotas] článek popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-270">hello [Throttling and quotas][lnk-quotas] article describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="206d9-271">Hello [sady SDK zařízení a služby Azure IoT] [ lnk-sdks] článku seznamy hello různých sadách SDK jazyka, které můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="206d9-271">hello [Azure IoT device and service SDKs][lnk-sdks] article lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="206d9-272">Hello [IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] článek popisuje hello můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení IoT Hub dotazovací jazyk .</span><span class="sxs-lookup"><span data-stu-id="206d9-272">hello [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="206d9-273">Hello [IoT Hub MQTT podporu] [ lnk-devguide-mqtt] článek obsahuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="206d9-273">hello [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="206d9-274">Další kroky</span><span class="sxs-lookup"><span data-stu-id="206d9-274">Next steps</span></span>
<span data-ttu-id="206d9-275">Nyní jste se naučili o dvojčata zařízení, může být zájem o hello následující témata Průvodce vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="206d9-275">Now you have learned about device twins, you may be interested in hello following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="206d9-276">[Volání metody přímé na zařízení][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="206d9-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="206d9-277">[Plánování úloh na několika zařízeních][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="206d9-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="206d9-278">Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzy IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="206d9-278">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorials:</span></span>

* <span data-ttu-id="206d9-279">[Jak toouse hello dvojče zařízení][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="206d9-279">[How toouse hello device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="206d9-280">[Jak dvojče zařízení toouse vlastnosti][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="206d9-280">[How toouse device twin properties][lnk-twin-properties]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
