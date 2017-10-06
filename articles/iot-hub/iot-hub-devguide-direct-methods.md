---
title: "metody přímé aaaUnderstand Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použít přímé metody tooinvoke kód na zařízení z aplikace služby."
services: iot-hub
documentationcenter: .net
author: nberdy
manager: timlt
editor: 
ms.assetid: 9f0535f1-02e6-467a-9fc4-c0950702102d
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d15d44a0c3e1d1cda1669c1ed011c2f932e3d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="60e64-103">Rady pro pochopení a volat přímé metody ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="60e64-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="60e64-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="60e64-104">Overview</span></span>
<span data-ttu-id="60e64-105">Centrum IoT vám dává možnost tooinvoke přímé metody na zařízení z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="60e64-105">IoT Hub gives you ability tooinvoke direct methods on devices from hello cloud.</span></span> <span data-ttu-id="60e64-106">Přímé metody představují požadavku a odpovědi interakci s zařízení podobné tooan HTTP volání v, že jejich úspěch nebo neúspěch okamžitě (po časový limit definované uživatelem).</span><span class="sxs-lookup"><span data-stu-id="60e64-106">Direct methods represent a request-reply interaction with a device similar tooan HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="60e64-107">To je užitečné pro scénáře, kde se liší v závislosti na tom, jestli zařízení hello byl schopný toorespond, například odeslání zařízení s SMS funkce wake-up tooa, pokud je zařízení offline (Probíhá dražší než volání metody SMS) během hello okamžitý zásah.</span><span class="sxs-lookup"><span data-stu-id="60e64-107">This is useful for scenarios where hello course of immediate action is different depending on whether hello device was able toorespond, such as sending an SMS wake-up tooa device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="60e64-108">Každá metoda zařízení cílí jedno zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-108">Each device method targets a single device.</span></span> <span data-ttu-id="60e64-109">[Úlohy] [ lnk-devguide-jobs] poskytují způsob, jak tooinvoke přímé metody na několika zařízeních a naplánovat volání metody pro odpojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-109">[Jobs][lnk-devguide-jobs] provide a way tooinvoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="60e64-110">Každý, kdo má **služba připojit** oprávnění na IoT Hub může vyvolat metodu na zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-toouse"></a><span data-ttu-id="60e64-111">Když toouse</span><span class="sxs-lookup"><span data-stu-id="60e64-111">When toouse</span></span>
<span data-ttu-id="60e64-112">Přímé metody postupujte podle vzoru požadavků a odpovědí a jsou určené pro komunikaci, které vyžadují okamžitou potvrzení jejich výsledku, obvykle interaktivní kontrolu nad hello zařízení, například tooturn na ventilátoru.</span><span class="sxs-lookup"><span data-stu-id="60e64-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of hello device, for example tooturn on a fan.</span></span>

<span data-ttu-id="60e64-113">Odkazovat příliš[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] v případě pochybností mezi použitím požadované vlastnosti přímé metody nebo zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-113">Refer too[Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="60e64-114">Životní cyklus – metoda</span><span class="sxs-lookup"><span data-stu-id="60e64-114">Method lifecycle</span></span>
<span data-ttu-id="60e64-115">Přímé metody jsou definovány pro hello zařízení a může vyžadovat nula nebo více vstupů v hello metoda datové části toocorrectly vytvoření instance.</span><span class="sxs-lookup"><span data-stu-id="60e64-115">Direct methods are implemented on hello device and may require zero or more inputs in hello method payload toocorrectly instantiate.</span></span> <span data-ttu-id="60e64-116">Vyvolání přímá metoda prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="60e64-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="60e64-117">Zařízení obdrží přímé metod v tématu MQTT konkrétní zařízení (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="60e64-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="60e64-118">Podporujeme může přímé metody dalších síťových protokolech straně zařízení v budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="60e64-118">We may support direct methods on additional device-side networking protocols in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="60e64-119">Při vyvolání přímá metoda na zařízení, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch hello následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="60e64-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in hello following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="60e64-120">Přímé metody jsou synchronní a buď úspěšné nebo neúspěšné po hello časový limit (výchozí: 30 sekund, nastavit až too3600 sekund).</span><span class="sxs-lookup"><span data-stu-id="60e64-120">Direct methods are synchronous and either succeed or fail after hello timeout period (default: 30 seconds, settable up too3600 seconds).</span></span> <span data-ttu-id="60e64-121">Přímé metody jsou užitečné v interaktivní scénářích, kde chcete zařízení tooact jenom v případě hello zařízení je online a přijímající příkazy, například zapnutí light z telefonního čísla.</span><span class="sxs-lookup"><span data-stu-id="60e64-121">Direct methods are useful in interactive scenarios where you want a device tooact if and only if hello device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="60e64-122">V těchto scénářích chcete toosee okamžité úspěšné nebo neúspěšné, takže hello cloudové služby může fungovat pro výsledek hello co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="60e64-122">In these scenarios, you want toosee an immediate success or failure so hello cloud service can act on hello result as soon as possible.</span></span> <span data-ttu-id="60e64-123">Hello zařízení může vrátit některé tělo zprávy v důsledku hello metoda, ale není nutné u hello metoda toodo tak.</span><span class="sxs-lookup"><span data-stu-id="60e64-123">hello device may return some message body as a result of hello method, but it isn't required for hello method toodo so.</span></span> <span data-ttu-id="60e64-124">Neexistuje žádná záruka, řazení v nebo všechny souběžnosti sémantiky pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="60e64-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="60e64-125">Přímá metoda jsou ze strany cloudu hello jen HTTP a MQTT jen ze strany zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="60e64-125">Direct method are HTTP-only from hello cloud side, and MQTT-only from hello device side.</span></span>

<span data-ttu-id="60e64-126">datová část Hello metoda požadavky a odpovědi je dokument JSON až too8KB.</span><span class="sxs-lookup"><span data-stu-id="60e64-126">hello payload for method requests and responses is a JSON document up too8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="60e64-127">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="60e64-127">Reference topics:</span></span>
<span data-ttu-id="60e64-128">Hello následující odkazy na témata poskytují další informace o používání přímé metody.</span><span class="sxs-lookup"><span data-stu-id="60e64-128">hello following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="60e64-129">Volání metody přímé z back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="60e64-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="60e64-130">Volání metody</span><span class="sxs-lookup"><span data-stu-id="60e64-130">Method invocation</span></span>
<span data-ttu-id="60e64-131">Přímá metoda volání na zařízení jsou volání protokolu HTTP, které tvoří:</span><span class="sxs-lookup"><span data-stu-id="60e64-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="60e64-132">Hello *URI* konkrétní toohello zařízení (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="60e64-132">hello *URI* specific toohello device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="60e64-133">Hello POST *– metoda*</span><span class="sxs-lookup"><span data-stu-id="60e64-133">hello POST *method*</span></span>
* <span data-ttu-id="60e64-134">*Hlavičky* který obsahovat hello autorizace, žádat o ID, typu obsahu a kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="60e64-134">*Headers* which contain hello authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="60e64-135">Transparentní JSON *textu* v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="60e64-135">A transparent JSON *body* in hello following format:</span></span>

```
{
    "methodName": "reboot",
    "responseTimeoutInSeconds": 200,
    "payload": {
        "input1": "someInput",
        "input2": "anotherInput"
    }
}
```

<span data-ttu-id="60e64-136">Časový limit je počet sekund.</span><span class="sxs-lookup"><span data-stu-id="60e64-136">Timeout is in seconds.</span></span> <span data-ttu-id="60e64-137">Pokud není nastavený časový limit, nastaví se jako výchozí too30 sekund.</span><span class="sxs-lookup"><span data-stu-id="60e64-137">If timeout is not set, it defaults too30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="60e64-138">Odpověď</span><span class="sxs-lookup"><span data-stu-id="60e64-138">Response</span></span>
<span data-ttu-id="60e64-139">Hello back-end aplikace obdrží odpověď, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="60e64-139">hello back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="60e64-140">*Stavový kód HTTP*, který se používá pro chyby pocházejících z hello IoT Hub, včetně chybu 404 pro zařízení není aktuálně připojený</span><span class="sxs-lookup"><span data-stu-id="60e64-140">*HTTP status code*, which is used for errors coming from hello IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="60e64-141">*Hlavičky* který obsahovat hello značka ETag, žádat o ID, typu obsahu a kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="60e64-141">*Headers* which contain hello ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="60e64-142">JSON *textu* v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="60e64-142">A JSON *body* in hello following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="60e64-143">Obě `status` a `body` jsou poskytovány hello zařízení a použít toorespond s hello zařízení vlastní stavový kód a popis.</span><span class="sxs-lookup"><span data-stu-id="60e64-143">Both `status` and `body` are provided by hello device and used toorespond with hello device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="60e64-144">Popisovač přímá metoda na zařízení</span><span class="sxs-lookup"><span data-stu-id="60e64-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="60e64-145">Volání metody</span><span class="sxs-lookup"><span data-stu-id="60e64-145">Method invocation</span></span>
<span data-ttu-id="60e64-146">Zařízení obdrží přímá metoda požadavky na hello MQTT tématu:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="60e64-146">Devices receive direct method requests on hello MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="60e64-147">tělo zprávy Hello které hello zařízení obdrží je ve formátu hello:</span><span class="sxs-lookup"><span data-stu-id="60e64-147">hello body which hello device receives is in hello following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="60e64-148">Požadavky metod jsou QoS 0.</span><span class="sxs-lookup"><span data-stu-id="60e64-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="60e64-149">Odpověď</span><span class="sxs-lookup"><span data-stu-id="60e64-149">Response</span></span>
<span data-ttu-id="60e64-150">Hello zařízení odesílá odpovědí příliš`$iothub/methods/res/{status}/?$rid={request id}`, kde:</span><span class="sxs-lookup"><span data-stu-id="60e64-150">hello device sends responses too`$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="60e64-151">Hello `status` vlastnost je hello stav spuštění metody zadané zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-151">hello `status` property is hello device-supplied status of method execution.</span></span>
* <span data-ttu-id="60e64-152">Hello `$rid` vlastnost je ID žádosti hello z volání metody hello přijatých ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="60e64-152">hello `$rid` property is hello request ID from hello method invocation received from IoT Hub.</span></span>

<span data-ttu-id="60e64-153">textu Hello je nastaven podle hello zařízení a může být jakékoli stavu.</span><span class="sxs-lookup"><span data-stu-id="60e64-153">hello body is set by hello device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="60e64-154">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="60e64-154">Additional reference material</span></span>
<span data-ttu-id="60e64-155">Další témata referenční příručka vývojáře pro službu IoT Hub hello patří:</span><span class="sxs-lookup"><span data-stu-id="60e64-155">Other reference topics in hello IoT Hub developer guide include:</span></span>

* <span data-ttu-id="60e64-156">[Koncové body centra IoT] [ lnk-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="60e64-156">[IoT Hub endpoints][lnk-endpoints] describes hello various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="60e64-157">[Omezování a kvóty] [ lnk-quotas] popisuje hello kvóty, které se vztahují toohello služby IoT Hub a hello omezení tooexpect chování při použití služby hello.</span><span class="sxs-lookup"><span data-stu-id="60e64-157">[Throttling and quotas][lnk-quotas] describes hello quotas that apply toohello IoT Hub service and hello throttling behavior tooexpect when you use hello service.</span></span>
* <span data-ttu-id="60e64-158">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] seznamy hello různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="60e64-158">[Azure IoT device and service SDKs][lnk-sdks] lists hello various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="60e64-159">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje hello dotazovací jazyk IoT Hub můžete tooretrieve informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="60e64-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes hello IoT Hub query language you can use tooretrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="60e64-160">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="60e64-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for hello MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60e64-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="60e64-161">Next steps</span></span>
<span data-ttu-id="60e64-162">Nyní jste se naučili, jak toouse přímé metody, vás může zajímat hello následující tématu Průvodce vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="60e64-162">Now you have learned how toouse direct methods, you may be interested in hello following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="60e64-163">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="60e64-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="60e64-164">Pokud chcete tootry některé z hello konceptů popsaných v tomto článku, může být zájem o hello následující kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="60e64-164">If you would like tootry out some of hello concepts described in this article, you may be interested in hello following IoT Hub tutorial:</span></span>

* <span data-ttu-id="60e64-165">[Používat přímé metody][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="60e64-165">[Use direct methods][lnk-methods-tutorial]</span></span>

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-messages]: iot-hub-devguide-messaging.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
