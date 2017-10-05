---
title: "Pochopení dvojčata zařízení Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - dvojčata zařízení použít k synchronizaci dat stavu a konfiguraci mezi IoT Hub a zařízení"
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
ms.openlocfilehash: b316aa419d558547f90a914a22fb29935076de21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a><span data-ttu-id="e0bf1-103">Rady pro pochopení a použít dvojčata zařízení IoT hub</span><span class="sxs-lookup"><span data-stu-id="e0bf1-103">Understand and use device twins in IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="e0bf1-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e0bf1-104">Overview</span></span>
<span data-ttu-id="e0bf1-105">*Dvojčata zařízení* jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="e0bf1-105">*Device twins* are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="e0bf1-106">IoT Hub udržuje takové dvojče pro každé zařízení, které k IoT Hub připojíte.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-106">IoT Hub persists a device twin for each device that you connect to IoT Hub.</span></span> <span data-ttu-id="e0bf1-107">Tento článek popisuje:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-107">This article describes:</span></span>

* <span data-ttu-id="e0bf1-108">Struktura dvojče zařízení: *značky*, *požadované* a *hlášené vlastnosti*, a</span><span class="sxs-lookup"><span data-stu-id="e0bf1-108">The structure of the device twin: *tags*, *desired* and *reported properties*, and</span></span>
* <span data-ttu-id="e0bf1-109">Operace, které aplikace pro zařízení a back-EndY můžete provádět na dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-109">The operations that device apps and back ends can perform on device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="e0bf1-110">V současné době jsou přístupné pouze ze zařízení, která se připojují ke službě IoT Hub pomocí protokolu MQTT dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-110">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="e0bf1-111">Odkazovat [MQTT podporu] [ lnk-devguide-mqtt] článek pokyny o tom, jak převést stávající aplikace zařízení používat MQTT.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-111">Refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>
> 
> 

### <a name="when-to-use"></a><span data-ttu-id="e0bf1-112">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="e0bf1-112">When to use</span></span>
<span data-ttu-id="e0bf1-113">Použijte dvojčata zařízení na:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-113">Use device twins to:</span></span>

* <span data-ttu-id="e0bf1-114">Metadata specifická pro zařízení ukládat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-114">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="e0bf1-115">Například nasazení umístění prodejních počítače.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-115">For example, the deployment location of a vending machine.</span></span>
* <span data-ttu-id="e0bf1-116">Sestavy aktuální informace o stavu, jako jsou k dispozici funkce a podmínky z vaší aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-116">Report current state information such as available capabilities and conditions from your device app.</span></span> <span data-ttu-id="e0bf1-117">Například je zařízení připojené ke službě IoT hub přes mobilní nebo Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-117">For example, a device is connected to your IoT hub over cellular or WiFi.</span></span>
* <span data-ttu-id="e0bf1-118">Synchronizujte stav pracovních dlouho běžící mezi aplikace zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-118">Synchronize the state of long-running workflows between device app and back-end app.</span></span> <span data-ttu-id="e0bf1-119">Při řešení back end Určuje novou verzi firmwaru pro instalaci a aplikace zařízení sestavy různé fáze procesu aktualizace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-119">For example, when the solution back end specifies the new firmware version to install, and the device app reports the various stages of the update process.</span></span>
* <span data-ttu-id="e0bf1-120">Dotaz na vaše zařízení metadata, konfigurace nebo stavu.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-120">Query your device metadata, configuration, or state.</span></span>

<span data-ttu-id="e0bf1-121">Odkazovat na [pokyny komunikace zařízení cloud] [ lnk-d2c-guidance] pokyny k používání hlášen vlastnostech, zpráv typu zařízení cloud nebo nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-121">Refer to [Device-to-cloud communication guidance][lnk-d2c-guidance] for guidance on using reported properties, device-to-cloud messages, or file upload.</span></span>
<span data-ttu-id="e0bf1-122">Odkazovat na [Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pokyny k použití požadované vlastnosti, přímé metody nebo zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-122">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] for guidance on using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="device-twins"></a><span data-ttu-id="e0bf1-123">Dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-123">Device twins</span></span>
<span data-ttu-id="e0bf1-124">Dvojčata zařízení ukládat informace týkající se zařízení který:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-124">Device twins store device-related information that:</span></span>

* <span data-ttu-id="e0bf1-125">Zařízení a zpět končí můžete používat k synchronizaci zařízení podmínky a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-125">Device and back ends can use to synchronize device conditions and configuration.</span></span>
* <span data-ttu-id="e0bf1-126">Back-end řešení můžete použít k dotazu a cíl dlouho běžící operace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-126">The solution back end can use to query and target long-running operations.</span></span>

<span data-ttu-id="e0bf1-127">Životní cyklus dvojče zařízení je propojený s odpovídající [identitu zařízení][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-127">The lifecycle of a device twin is linked to the corresponding [device identity][lnk-identity].</span></span> <span data-ttu-id="e0bf1-128">Dvojčata zařízení jsou implicitně vytvořen a odstraněn, když je vytvořené nebo odstraněné v IoT Hub novou identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-128">Device twins are implicitly created and deleted when a new device identity is created or deleted in IoT Hub.</span></span>

<span data-ttu-id="e0bf1-129">Dvojče zařízení je dokument JSON, který zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-129">A device twin is a JSON document that includes:</span></span>

* <span data-ttu-id="e0bf1-130">**Značky**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-130">**Tags**.</span></span> <span data-ttu-id="e0bf1-131">Části dokumentu JSON, který může číst z a zapisovat do back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-131">A section of the JSON document that the solution back end can read from and write to.</span></span> <span data-ttu-id="e0bf1-132">Značky nejsou viditelné pro aplikace pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-132">Tags are not visible to device apps.</span></span>
* <span data-ttu-id="e0bf1-133">**Požadovaného vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-133">**Desired properties**.</span></span> <span data-ttu-id="e0bf1-134">Používat společně s hlášené vlastnosti k synchronizaci konfigurace zařízení nebo podmínky.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-134">Used along with reported properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="e0bf1-135">Požadované vlastnosti lze nastavit pouze zpět řešení end a mohou být přečteny aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-135">Desired properties can only be set by the solution back end and can be read by the device app.</span></span> <span data-ttu-id="e0bf1-136">Aplikace zařízení můžete také upozorněni v reálném čase ve vlastnostech požadované změny.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-136">The device app can also be notified in real time of changes in the desired properties.</span></span>
* <span data-ttu-id="e0bf1-137">**Hlášené vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-137">**Reported properties**.</span></span> <span data-ttu-id="e0bf1-138">Používat společně s požadované vlastnosti pro synchronizaci konfigurace zařízení nebo podmínky.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-138">Used along with desired properties to synchronize device configuration or conditions.</span></span> <span data-ttu-id="e0bf1-139">Hlášené vlastnosti lze nastavit pouze na zařízení aplikaci a můžete číst a je dotazován pomocí back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-139">Reported properties can only be set by the device app and can be read and queried by the solution back end.</span></span>

<span data-ttu-id="e0bf1-140">Kromě toho kořen dokumentu JSON twin zařízení obsahuje vlastnosti jen pro čtení z odpovídající identitu zařízení, které jsou uložené v [registru identit][lnk-identity].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-140">Additionally, the root of the device twin JSON document contains the read-only properties from the corresponding device identity stored in the [identity registry][lnk-identity].</span></span>

![][img-twin]

<span data-ttu-id="e0bf1-141">Následující příklad ukazuje dvojče zařízení dokumentu JSON:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-141">The following example shows a device twin JSON document:</span></span>

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

<span data-ttu-id="e0bf1-142">V kořenový objekt, jsou vlastnosti systému a kontejner objektů pro `tags` a obě `reported` a `desired` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-142">In the root object, are the system properties, and container objects for `tags` and both `reported` and `desired` properties.</span></span> <span data-ttu-id="e0bf1-143">`properties` Kontejner obsahuje některé prvky jen pro čtení (`$metadata`, `$etag`, a `$version`) popsané v [metadat zařízení twin] [ lnk-twin-metadata] a [ Optimistickou metodu souběžného] [ lnk-concurrency] oddíly.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-143">The `properties` container contains some read-only elements (`$metadata`, `$etag`, and `$version`) described in the [Device twin metadata][lnk-twin-metadata] and [Optimistic concurrency][lnk-concurrency] sections.</span></span>

### <a name="reported-property-example"></a><span data-ttu-id="e0bf1-144">Příklad hlášené vlastnost</span><span class="sxs-lookup"><span data-stu-id="e0bf1-144">Reported property example</span></span>
<span data-ttu-id="e0bf1-145">V předchozím příkladu obsahuje dvojče zařízení `batteryLevel` vlastnosti, který je hlášen aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-145">In the previous example, the device twin contains a `batteryLevel` property that is reported by the device app.</span></span> <span data-ttu-id="e0bf1-146">Tato vlastnost umožňuje dotazování a provozovat na zařízení podle poslední hlášené stav baterie.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-146">This property makes it possible to query and operate on devices based on the last reported battery level.</span></span> <span data-ttu-id="e0bf1-147">Další příklady zahrnují možnosti vytváření sestav zařízení zařízení aplikace nebo možnosti připojení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-147">Other examples include the device app reporting device capabilities or connectivity options.</span></span>

> [!NOTE]
> <span data-ttu-id="e0bf1-148">Hlášené vlastnosti zjednodušit scénáře, kde je back-end řešení zájem o poslední známé hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-148">Reported properties simplify scenarios where the solution back end is interested in the last known value of a property.</span></span> <span data-ttu-id="e0bf1-149">Použití [zpráv typu zařízení cloud] [ lnk-d2c] Pokud back-end řešení potřebuje ke zpracování telemetrie zařízení ve formě pořadí označen časovým razítkem události, jako je například časové řady.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-149">Use [device-to-cloud messages][lnk-d2c] if the solution back end needs to process device telemetry in the form of sequences of timestamped events, such as time series.</span></span>

### <a name="desired-property-example"></a><span data-ttu-id="e0bf1-150">Příklad požadovanou vlastnost</span><span class="sxs-lookup"><span data-stu-id="e0bf1-150">Desired property example</span></span>
<span data-ttu-id="e0bf1-151">V předchozím příkladu `telemetryConfig` potřeby dvojče zařízení a hlášené vlastnosti tak, že back-end řešení a aplikace zařízení slouží k synchronizaci telemetrická data konfigurace pro toto zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-151">In the previous example, the `telemetryConfig` device twin desired and reported properties are used by the solution back end and the device app to synchronize the telemetry configuration for this device.</span></span> <span data-ttu-id="e0bf1-152">Například:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-152">For example:</span></span>

1. <span data-ttu-id="e0bf1-153">Back-end řešení Nastaví požadovanou vlastnost s hodnotou požadované konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-153">The solution back end sets the desired property with the desired configuration value.</span></span> <span data-ttu-id="e0bf1-154">Zde je část dokumentu sadou požadovanou vlastnost:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-154">Here is the portion of the document with the desired property set:</span></span>
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. <span data-ttu-id="e0bf1-155">Aplikace zařízení obdrží oznámení změny okamžitě, pokud připojení nebo při prvním volání metody reconnect.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-155">The device app is notified of the change immediately if connected, or at the first reconnect.</span></span> <span data-ttu-id="e0bf1-156">Aplikace zařízení hlásí aktualizovanou konfiguraci (nebo podmínku chyby pomocí `status` vlastnost).</span><span class="sxs-lookup"><span data-stu-id="e0bf1-156">The device app then reports the updated configuration (or an error condition using the `status` property).</span></span> <span data-ttu-id="e0bf1-157">Zde je část hlášené vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-157">Here is the portion of the reported properties:</span></span>
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. <span data-ttu-id="e0bf1-158">Back-end řešení můžete výsledky operace konfigurace sledovat prostřednictvím zařízení, pomocí [dotazování] [ lnk-query] dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-158">The solution back end can track the results of the configuration operation across many devices, by [querying][lnk-query] device twins.</span></span>

> [!NOTE]
> <span data-ttu-id="e0bf1-159">Předchozí fragmenty kódu jsou příklady, optimalizované pro čitelnost jedním ze způsobů ke kódování konfigurace zařízení a jeho stav.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-159">The preceding snippets are examples, optimized for readability, of one way to encode a device configuration and its status.</span></span> <span data-ttu-id="e0bf1-160">IoT Hub nepředstavuje určité schéma pro potřeby dvojče zařízení a který ohlásil vlastnosti v dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-160">IoT Hub does not impose a specific schema for the device twin desired and reported properties in the device twins.</span></span>
> 
> 

<span data-ttu-id="e0bf1-161">Dvojčata slouží k synchronizaci dlouhotrvající operace, jako je aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-161">You can use twins to synchronize long-running operations such as firmware updates.</span></span> <span data-ttu-id="e0bf1-162">Další informace o tom, jak pomocí vlastnosti synchronizovat a sledovat operace probíhající dlouhou dobu na zařízeních najdete v tématu [použití požadovaného vlastnosti pro konfiguraci zařízení][lnk-twin-properties].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-162">For more information on how to use properties to synchronize and track a long running operation across devices, see [Use desired properties to configure devices][lnk-twin-properties].</span></span>

## <a name="back-end-operations"></a><span data-ttu-id="e0bf1-163">Operace back-end</span><span class="sxs-lookup"><span data-stu-id="e0bf1-163">Back-end operations</span></span>
<span data-ttu-id="e0bf1-164">Back-end řešení funguje na dvojče zařízení pomocí následující atomické operací, k dispozici prostřednictvím protokolu HTTP:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-164">The solution back end operates on the device twin using the following atomic operations, exposed through HTTP:</span></span>

1. <span data-ttu-id="e0bf1-165">**Načíst dvojče zařízení tak, že id**. Tato operace vrátí dokumentu twin zařízení, včetně značky a potřeby nahlásí a vlastnosti systému.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-165">**Retrieve device twin by id**. This operation returns the device twin document, including tags and desired, reported, and system properties.</span></span>
2. <span data-ttu-id="e0bf1-166">**Částečné aktualizace dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-166">**Partially update device twin**.</span></span> <span data-ttu-id="e0bf1-167">Tato operace povolí back-end řešení částečně aktualizace značky nebo požadované vlastnosti v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-167">This operation enables the solution back end to partially update the tags or desired properties in a device twin.</span></span> <span data-ttu-id="e0bf1-168">Částečné aktualizace je vyjádřen jako dokument JSON, který přidá nebo aktualizuje libovolné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-168">The partial update is expressed as a JSON document that adds or updates any property.</span></span> <span data-ttu-id="e0bf1-169">Vlastnosti nastavit na `null` se odeberou.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-169">Properties set to `null` are removed.</span></span> <span data-ttu-id="e0bf1-170">Následující příklad vytvoří novou požadovanou vlastnost s hodnotou `{"newProperty": "newValue"}`, přepíše existující hodnotu `existingProperty` s `"otherNewValue"`a také odebere `otherOldProperty`.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-170">The following example creates a new desired property with value `{"newProperty": "newValue"}`, overwrites the existing value of `existingProperty` with `"otherNewValue"`, and removes `otherOldProperty`.</span></span> <span data-ttu-id="e0bf1-171">Existující požadované vlastnosti a značky jsou provedeny žádné další změny:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-171">No other changes are made to existing desired properties or tags:</span></span>
   
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
3. <span data-ttu-id="e0bf1-172">**Nahraďte požadované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-172">**Replace desired properties**.</span></span> <span data-ttu-id="e0bf1-173">Tato operace povolí back-end řešení úplně přepsat všechny existující požadované vlastnosti a nahraďte nový dokument JSON pro `properties/desired`.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-173">This operation enables the solution back end to completely overwrite all existing desired properties and substitute a new JSON document for `properties/desired`.</span></span>
4. <span data-ttu-id="e0bf1-174">**Nahraďte značky**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-174">**Replace tags**.</span></span> <span data-ttu-id="e0bf1-175">Tato operace povolí back-end řešení úplně přepsat všechny existující značky a nahraďte nový dokument JSON pro `tags`.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-175">This operation enables the solution back end to completely overwrite all existing tags and substitute a new JSON document for `tags`.</span></span>
5. <span data-ttu-id="e0bf1-176">**Přijímat oznámení twin**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-176">**Receive twin notifications**.</span></span> <span data-ttu-id="e0bf1-177">Tato operace povoluje back-end řešení pro oznámení o změně twin.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-177">This operation allows the solution back end to be notified when the twin is modified.</span></span> <span data-ttu-id="e0bf1-178">Uděláte to tak, musí vaše řešení IoT vytvořit trasu a nastavte zdroj dat na hodnotu *twinChangeEvents*.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-178">To do so, your IoT solution needs to create a route and to set the Data Source equal to *twinChangeEvents*.</span></span> <span data-ttu-id="e0bf1-179">Ve výchozím nastavení žádné twin oznámení se odesílají, tedy předem neexistuje žádný takový trasy.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-179">By default, no twin notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="e0bf1-180">Pokud je příliš vysoká rychlost změny, nebo z jiných důvodů, jako je například interní chyby služby IoT Hub může odeslat pouze jedno oznámení, která obsahuje všechny změny.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-180">If the rate of change is too high, or for other reasons, such as internal failures, the IoT Hub might send only one notification that contains all changes.</span></span> <span data-ttu-id="e0bf1-181">Proto, pokud aplikace potřebuje spolehlivé auditování a protokolování všechny zprostředkující stavy, pak je stále doporučujeme použít D2C zprávy.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-181">So, if your application needs reliable auditing and logging of all intermediate states, then it is still recommended that you use D2C messages.</span></span> <span data-ttu-id="e0bf1-182">Oznámení twin zahrnuje vlastnosti a text.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-182">The twin notification message includes properties, and body.</span></span>

    - <span data-ttu-id="e0bf1-183">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e0bf1-183">Properties</span></span>

    | <span data-ttu-id="e0bf1-184">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="e0bf1-184">Name</span></span> | <span data-ttu-id="e0bf1-185">Hodnota</span><span class="sxs-lookup"><span data-stu-id="e0bf1-185">Value</span></span> |
    | --- | --- |
    <span data-ttu-id="e0bf1-186">$content – typ</span><span class="sxs-lookup"><span data-stu-id="e0bf1-186">$content-type</span></span> | <span data-ttu-id="e0bf1-187">application/json</span><span class="sxs-lookup"><span data-stu-id="e0bf1-187">application/json</span></span> |
    <span data-ttu-id="e0bf1-188">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="e0bf1-188">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="e0bf1-189">Čas odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-189">Time when the notification was sent</span></span> |
    <span data-ttu-id="e0bf1-190">$iothub – zpráva – zdroj</span><span class="sxs-lookup"><span data-stu-id="e0bf1-190">$iothub-message-source</span></span> | <span data-ttu-id="e0bf1-191">twinChangeEvents</span><span class="sxs-lookup"><span data-stu-id="e0bf1-191">twinChangeEvents</span></span> |
    <span data-ttu-id="e0bf1-192">$content – kódování</span><span class="sxs-lookup"><span data-stu-id="e0bf1-192">$content-encoding</span></span> | <span data-ttu-id="e0bf1-193">znakové sady UTF-8</span><span class="sxs-lookup"><span data-stu-id="e0bf1-193">utf-8</span></span> |
    <span data-ttu-id="e0bf1-194">deviceId</span><span class="sxs-lookup"><span data-stu-id="e0bf1-194">deviceId</span></span> | <span data-ttu-id="e0bf1-195">ID zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-195">Id of the device</span></span> |
    <span data-ttu-id="e0bf1-196">hubName</span><span class="sxs-lookup"><span data-stu-id="e0bf1-196">hubName</span></span> | <span data-ttu-id="e0bf1-197">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="e0bf1-197">Name of IoT Hub</span></span> |
    <span data-ttu-id="e0bf1-198">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="e0bf1-198">operationTimestamp</span></span> | <span data-ttu-id="e0bf1-199">[ISO8601] časové razítko operace</span><span class="sxs-lookup"><span data-stu-id="e0bf1-199">[ISO8601] timestamp of operation</span></span> |
    <span data-ttu-id="e0bf1-200">schéma zprávy iothub</span><span class="sxs-lookup"><span data-stu-id="e0bf1-200">iothub-message-schema</span></span> | <span data-ttu-id="e0bf1-201">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="e0bf1-201">deviceLifecycleNotification</span></span> |
    <span data-ttu-id="e0bf1-202">opType</span><span class="sxs-lookup"><span data-stu-id="e0bf1-202">opType</span></span> | <span data-ttu-id="e0bf1-203">"replaceTwin" nebo "updateTwin"</span><span class="sxs-lookup"><span data-stu-id="e0bf1-203">"replaceTwin" or "updateTwin"</span></span> |

    <span data-ttu-id="e0bf1-204">Vlastnosti zprávu systému mají předponu `'$'` symbol.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-204">Message system properties are prefixed with the `'$'` symbol.</span></span>

    - <span data-ttu-id="e0bf1-205">Tělo</span><span class="sxs-lookup"><span data-stu-id="e0bf1-205">Body</span></span>
        
    <span data-ttu-id="e0bf1-206">Tato část obsahuje všechny změny twin ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-206">This section includes all the twin changes in a JSON format.</span></span> <span data-ttu-id="e0bf1-207">Používá stejný formát jako opravu, s tím rozdílem, které může obsahovat všechny části twin: značky, properties.reported, properties.desired a že obsahuje elementy "$metadata".</span><span class="sxs-lookup"><span data-stu-id="e0bf1-207">It uses the same format as a patch, with the difference that it can contain all twin sections: tags, properties.reported, properties.desired, and that it contains the “$metadata” elements.</span></span> <span data-ttu-id="e0bf1-208">Například:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-208">For example,</span></span>
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

<span data-ttu-id="e0bf1-209">Podporují všechny předchozí operace [optimistickou metodu souběžného] [ lnk-concurrency] a vyžadovat **ServiceConnect** oprávnění, jak jsou definovány v [zabezpečení] [ lnk-security] článku.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-209">All the preceding operations support [Optimistic concurrency][lnk-concurrency] and require the **ServiceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="e0bf1-210">Kromě těchto operací back-end řešení může:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-210">In addition to these operations, the solution back end can:</span></span>

* <span data-ttu-id="e0bf1-211">Dotaz dvojčata zařízení pomocí SQL like [IoT Hub dotazovací jazyk][lnk-query].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-211">Query the device twins using the SQL-like [IoT Hub query language][lnk-query].</span></span>
* <span data-ttu-id="e0bf1-212">Provádění operací na velkých sad dvojčata zařízení pomocí [úlohy][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-212">Perform operations on large sets of device twins using [jobs][lnk-jobs].</span></span>

## <a name="device-operations"></a><span data-ttu-id="e0bf1-213">Operace zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-213">Device operations</span></span>
<span data-ttu-id="e0bf1-214">Aplikace zařízení funguje na dvojče zařízení pomocí následující atomické operací:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-214">The device app operates on the device twin using the following atomic operations:</span></span>

1. <span data-ttu-id="e0bf1-215">**Načtení dvojče zařízení**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-215">**Retrieve device twin**.</span></span> <span data-ttu-id="e0bf1-216">Tato operace vrátí dokument twin zařízení (včetně značky a potřeby, hlášené a vlastnosti systému) pro aktuálně připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-216">This operation returns the device twin document (including tags and desired, reported and system properties) for the currently connected device.</span></span>
2. <span data-ttu-id="e0bf1-217">**Částečné aktualizace hlášené vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-217">**Partially update reported properties**.</span></span> <span data-ttu-id="e0bf1-218">Tato operace povolí částečné aktualizace hlášené vlastnosti aktuálně připojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-218">This operation enables the partial update of the reported properties of the currently connected device.</span></span> <span data-ttu-id="e0bf1-219">Tato operace používá stejný formát JSON aktualizace, řešení back end používá pro částečné aktualizace požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-219">This operation uses the same JSON update format that the solution back end uses for a partial update of desired properties.</span></span>
3. <span data-ttu-id="e0bf1-220">**Sledovat požadované vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-220">**Observe desired properties**.</span></span> <span data-ttu-id="e0bf1-221">Aktuálně připojené zařízení můžete zvolit reálném oznamovat aktualizace na požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-221">The currently connected device can choose to be notified of updates to the desired properties when they happen.</span></span> <span data-ttu-id="e0bf1-222">Zařízení obdrží stejného formuláře aktualizace (náhrada částečně nebo zcela) provedený back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-222">The device receives the same form of update (partial or full replacement) executed by the solution back end.</span></span>

<span data-ttu-id="e0bf1-223">Předchozí operace vyžadují **DeviceConnect** oprávnění, jak jsou definovány v [zabezpečení] [ lnk-security] článku.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-223">All the preceding operations require the **DeviceConnect** permission, as defined in the [Security][lnk-security] article.</span></span>

<span data-ttu-id="e0bf1-224">[Sady SDK pro zařízení Azure IoT] [ lnk-sdks] můžete snadno použít předchozí operace z mnoha jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-224">The [Azure IoT device SDKs][lnk-sdks] make it easy to use the preceding operations from many languages and platforms.</span></span> <span data-ttu-id="e0bf1-225">Další informace o podrobnostech primitiv IoT Hub pro požadované vlastnosti synchronizace naleznete v [toku opětovné připojení zařízení][lnk-reconnection].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-225">More information on the details of IoT Hub primitives for desired properties synchronization can be found in [Device reconnection flow][lnk-reconnection].</span></span>

> [!NOTE]
> <span data-ttu-id="e0bf1-226">V současné době jsou přístupné pouze ze zařízení, která se připojují ke službě IoT Hub pomocí protokolu MQTT dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-226">Currently, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span>
> 
> 

## <a name="reference-topics"></a><span data-ttu-id="e0bf1-227">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-227">Reference topics:</span></span>
<span data-ttu-id="e0bf1-228">Následující referenční témata poskytují další informace o řízení přístupu ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-228">The following reference topics provide you with more information about controlling access to your IoT hub.</span></span>

## <a name="tags-and-properties-format"></a><span data-ttu-id="e0bf1-229">Formát značky a vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e0bf1-229">Tags and properties format</span></span>
<span data-ttu-id="e0bf1-230">Značky, požadovanou a oznámená vlastnosti jsou objekty JSON s následujícími omezeními:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-230">Tags, desired, and reported properties are JSON objects with the following restrictions:</span></span>

* <span data-ttu-id="e0bf1-231">Všechny klíče v objekty JSON jsou malá a velká písmena 64 bajtů řetězců v kódu UNICODE UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-231">All keys in JSON objects are case-sensitive 64 bytes UTF-8 UNICODE strings.</span></span> <span data-ttu-id="e0bf1-232">Povolené znaky vyloučit řídicí znaky UNICODE (segmenty C0 a C1), a `'.'`, `' '`, a `'$'`.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-232">Allowed characters exclude UNICODE control characters (segments C0 and C1), and `'.'`, `' '`, and `'$'`.</span></span>
* <span data-ttu-id="e0bf1-233">Všechny hodnoty v objektů JSON může mít následující typy JSON: logická hodnota, číslo, řetězec, objekt.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-233">All values in JSON objects can be of the following JSON types: boolean, number, string, object.</span></span> <span data-ttu-id="e0bf1-234">Pole nejsou povoleny.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-234">Arrays are not allowed.</span></span>
* <span data-ttu-id="e0bf1-235">Všechny objekty JSON ve značkách, požadovanou a oznámená vlastnosti může mít maximální hloubka začlenění na 5.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-235">All JSON objects in tags, desired, and reported properties can have a maximum depth of 5.</span></span> <span data-ttu-id="e0bf1-236">Například následující objekt je neplatný:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-236">For instance, the following object is valid:</span></span>

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

* <span data-ttu-id="e0bf1-237">Všechny hodnoty řetězce může být o maximální délce 512 bajtů.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-237">All string values can be at most 512 bytes in length.</span></span>

## <a name="device-twin-size"></a><span data-ttu-id="e0bf1-238">Velikost twin zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-238">Device twin size</span></span>
<span data-ttu-id="e0bf1-239">IoT Hub vynucuje omezení velikosti 8KB na hodnotách `tags`, `properties/desired`, a `properties/reported`, s výjimkou elementy jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-239">IoT Hub enforces an 8KB size limitation on the values of `tags`, `properties/desired`, and `properties/reported`, excluding read-only elements.</span></span>
<span data-ttu-id="e0bf1-240">Velikost se počítá podle počítání řídit všechny znaky kódování UNICODE s výjimkou znaků (segmenty C0 a C1) a místo `' '` při zdá mimo řetězcová konstanta.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-240">The size is computed by counting all characters excluding UNICODE control characters (segments C0 and C1) and space `' '` when it appears outside of a string constant.</span></span>
<span data-ttu-id="e0bf1-241">IoT Hub s chybou odmítne všechny operace, které by zvětšete velikost tyto dokumenty nad limit.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-241">IoT Hub rejects with an error all operations that would increase the size of those documents above the limit.</span></span>

## <a name="device-twin-metadata"></a><span data-ttu-id="e0bf1-242">Metadata twin zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-242">Device twin metadata</span></span>
<span data-ttu-id="e0bf1-243">IoT Hub uchovává časové razítko poslední aktualizace pro každý objekt JSON v dvojče zařízení potřeby a který ohlásil vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-243">IoT Hub maintains the timestamp of the last update for each JSON object in device twin desired and reported properties.</span></span> <span data-ttu-id="e0bf1-244">Časová razítka v UTC a v kódování [ISO8601] formátu `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-244">The timestamps are in UTC and encoded in the [ISO8601] format `YYYY-MM-DDTHH:MM:SS.mmmZ`.</span></span>
<span data-ttu-id="e0bf1-245">Například:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-245">For example:</span></span>

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

<span data-ttu-id="e0bf1-246">Tyto informace jsou uchovávány v každé úrovni (ne jenom listy struktuře JSON) Chcete-li zachovat aktualizace, které se odebrat klíče objektu.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-246">This information is kept at every level (not just the leaves of the JSON structure) to preserve updates that remove object keys.</span></span>

## <a name="optimistic-concurrency"></a><span data-ttu-id="e0bf1-247">Optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="e0bf1-247">Optimistic concurrency</span></span>
<span data-ttu-id="e0bf1-248">Značky, potřeby a jsou uvedeny vlastnosti všech optimistickou metodu souběžného podpory.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-248">Tags, desired, and reported properties all support optimistic concurrency.</span></span>
<span data-ttu-id="e0bf1-249">Značky mají značku ETag dle [RFC7232], reprezentace JSON na značku, která představuje.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-249">Tags have an ETag, as per [RFC7232], that represents the tag's JSON representation.</span></span> <span data-ttu-id="e0bf1-250">Značky etag binárním rozsáhlým v operacích podmíněného aktualizace z back-end řešení slouží k zajištění konzistence.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-250">You can use ETags in conditional update operations from the solution back end to ensure consistency.</span></span>

<span data-ttu-id="e0bf1-251">Dvojče zařízení potřeby a který ohlásil vlastnosti nemají značky etag binárním rozsáhlým, ale mají `$version` hodnotu, která představuje záruku přírůstkové.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-251">Device twin desired and reported properties do not have ETags, but have a `$version` value that is guaranteed to be incremental.</span></span> <span data-ttu-id="e0bf1-252">Podobně jako na značku ETag verze lze aktualizace stranou Pokud chcete zajistit konzistenci aktualizací.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-252">Similarly to an ETag, the version can be used by the updating party to enforce consistency of updates.</span></span> <span data-ttu-id="e0bf1-253">Například zařízení aplikaci pro hlášené vlastnost nebo back-end řešení pro požadovanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-253">For example, a device app for a reported property or the solution back end for a desired property.</span></span>

<span data-ttu-id="e0bf1-254">Verze jsou užitečné také při observing agenta (například aplikace zařízení sledování požadované vlastnosti) musí sjednotit RAS mezi výsledek operace načtení a oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-254">Versions are also useful when an observing agent (such as the device app observing the desired properties) must reconcile races between the result of a retrieve operation and an update notification.</span></span> <span data-ttu-id="e0bf1-255">V části [toku opětovné připojení zařízení] [ lnk-reconnection] poskytuje další informace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-255">The section [Device reconnection flow][lnk-reconnection] provides more information.</span></span>

## <a name="device-reconnection-flow"></a><span data-ttu-id="e0bf1-256">Postup opětovné připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="e0bf1-256">Device reconnection flow</span></span>
<span data-ttu-id="e0bf1-257">IoT Hub nezachovává oznámení o aktualizacích požadované vlastnosti pro odpojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-257">IoT Hub does not preserve desired properties update notifications for disconnected devices.</span></span> <span data-ttu-id="e0bf1-258">Z toho vyplývá, že zařízení, která se připojuje musí získat úplné požadované vlastnosti dokumentu, kromě odběr pro oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-258">It follows that a device that is connecting must retrieve the full desired properties document, in addition to subscribing for update notifications.</span></span> <span data-ttu-id="e0bf1-259">Zadána možnost RAS mezi oznámení o aktualizaci a úplné načtení vhodné zajistit následující postup:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-259">Given the possibility of races between update notifications and full retrieval, the following flow must be ensured:</span></span>

1. <span data-ttu-id="e0bf1-260">Aplikace zařízení připojí ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-260">Device app connects to an IoT hub.</span></span>
2. <span data-ttu-id="e0bf1-261">Aplikace zařízení pro požadované vlastnosti Odběratel oznámení o aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-261">Device app subscribes for desired properties update notifications.</span></span>
3. <span data-ttu-id="e0bf1-262">Aplikace zařízení načte celého dokumentu pro požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-262">Device app retrieves the full document for desired properties.</span></span>

<span data-ttu-id="e0bf1-263">Aplikace zařízení můžete ignorovat všechna oznámení s `$version` menší nebo roven verzi úplné načtené dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-263">The device app can ignore all notifications with `$version` less or equal than the version of the full retrieved document.</span></span> <span data-ttu-id="e0bf1-264">Tento přístup je možné, protože IoT Hub zaručuje, že verze vždy zvýšit.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-264">This approach is possible because IoT Hub guarantees that versions always increment.</span></span>

> [!NOTE]
> <span data-ttu-id="e0bf1-265">Tato logika je už implementované v [sady SDK pro zařízení Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="e0bf1-265">This logic is already implemented in the [Azure IoT device SDKs][lnk-sdks].</span></span> <span data-ttu-id="e0bf1-266">Tento popis je užitečný jenom v případě, že nemůžete použít žádnou z sady SDK pro zařízení Azure IoT a musí programu rozhraní MQTT přímo aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-266">This description is useful only if the device app cannot use any of Azure IoT device SDKs and must program the MQTT interface directly.</span></span>
> 
> 

## <a name="additional-reference-material"></a><span data-ttu-id="e0bf1-267">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="e0bf1-267">Additional reference material</span></span>
<span data-ttu-id="e0bf1-268">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-268">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="e0bf1-269">[Koncové body centra IoT] [ lnk-endpoints] článek popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-269">The [IoT Hub endpoints][lnk-endpoints] article describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="e0bf1-270">[Omezování a kvóty] [ lnk-quotas] článek popisuje kvóty, které platí pro službu IoT Hub a omezení chování se očekává při použití služby.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-270">The [Throttling and quotas][lnk-quotas] article describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="e0bf1-271">[Sady SDK zařízení a služby Azure IoT] [ lnk-sdks] článku jsou uvedené různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-271">The [Azure IoT device and service SDKs][lnk-sdks] article lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="e0bf1-272">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] článek popisuje dotazovací jazyk Centrum IoT, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-272">The [IoT Hub query language for device twins, jobs, and message routing][lnk-query] article describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="e0bf1-273">[IoT Hub MQTT podporu] [ lnk-devguide-mqtt] článek obsahuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="e0bf1-273">The [IoT Hub MQTT support][lnk-devguide-mqtt] article provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0bf1-274">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0bf1-274">Next steps</span></span>
<span data-ttu-id="e0bf1-275">Nyní jste se naučili o dvojčata zařízení, může zajímat v následujících tématech Příručka vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-275">Now you have learned about device twins, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="e0bf1-276">[Volání metody přímé na zařízení][lnk-methods]</span><span class="sxs-lookup"><span data-stu-id="e0bf1-276">[Invoke a direct method on a device][lnk-methods]</span></span>
* <span data-ttu-id="e0bf1-277">[Plánování úloh na několika zařízeních][lnk-jobs]</span><span class="sxs-lookup"><span data-stu-id="e0bf1-277">[Schedule jobs on multiple devices][lnk-jobs]</span></span>

<span data-ttu-id="e0bf1-278">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujících kurzech IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="e0bf1-278">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorials:</span></span>

* <span data-ttu-id="e0bf1-279">[Jak používat dvojče zařízení][lnk-twin-tutorial]</span><span class="sxs-lookup"><span data-stu-id="e0bf1-279">[How to use the device twin][lnk-twin-tutorial]</span></span>
* <span data-ttu-id="e0bf1-280">[Použití zařízení dvojici vlastností][lnk-twin-properties]</span><span class="sxs-lookup"><span data-stu-id="e0bf1-280">[How to use device twin properties][lnk-twin-properties]</span></span>

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
