---
title: "Pochopení přímé metody Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - použijte přímý metody k vyvolání kódu na zařízení z aplikace služby."
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
ms.openlocfilehash: 77e788a32097edbcb1cd4faaa45f35812eabd94a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="understand-and-invoke-direct-methods-from-iot-hub"></a><span data-ttu-id="419c9-103">Rady pro pochopení a volat přímé metody ze služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="419c9-103">Understand and invoke direct methods from IoT Hub</span></span>
## <a name="overview"></a><span data-ttu-id="419c9-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="419c9-104">Overview</span></span>
<span data-ttu-id="419c9-105">Centrum IoT vám dává možnost vyvolat přímé metody na zařízení z cloudu.</span><span class="sxs-lookup"><span data-stu-id="419c9-105">IoT Hub gives you ability to invoke direct methods on devices from the cloud.</span></span> <span data-ttu-id="419c9-106">Přímé metody představuje požadavek odpověď interakci s zařízení podobné volání protokolu HTTP v, které budou úspěch nebo neúspěch okamžitě (po časový limit definované uživatelem).</span><span class="sxs-lookup"><span data-stu-id="419c9-106">Direct methods represent a request-reply interaction with a device similar to an HTTP call in that they succeed or fail immediately (after a user-specified timeout).</span></span> <span data-ttu-id="419c9-107">To je užitečné pro scénáře, kde se liší v závislosti na tom, jestli se zařízení schopné reagovat, například odeslání služby SMS funkce wake-up do zařízení, pokud je zařízení offline (Probíhá dražší než volání metody SMS) během okamžitý zásah.</span><span class="sxs-lookup"><span data-stu-id="419c9-107">This is useful for scenarios where the course of immediate action is different depending on whether the device was able to respond, such as sending an SMS wake-up to a device if a device is offline (SMS being more expensive than a method call).</span></span>

<span data-ttu-id="419c9-108">Každá metoda zařízení cílí jedno zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-108">Each device method targets a single device.</span></span> <span data-ttu-id="419c9-109">[Úlohy] [ lnk-devguide-jobs] poskytnout způsob, jak volat přímé metody na několika zařízeních a naplánovat volání metody pro odpojené zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-109">[Jobs][lnk-devguide-jobs] provide a way to invoke direct methods on multiple devices, and schedule method invocation for disconnected devices.</span></span>

<span data-ttu-id="419c9-110">Každý, kdo má **služba připojit** oprávnění na IoT Hub může vyvolat metodu na zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-110">Anyone with **service connect** permissions on IoT Hub may invoke a method on a device.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="419c9-111">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="419c9-111">When to use</span></span>
<span data-ttu-id="419c9-112">Přímé metody postupujte podle vzoru požadavků a odpovědí a jsou určené pro komunikaci, které vyžadují okamžitou potvrzení jejich výsledku, obvykle interaktivní kontrolu zařízení, např. Chcete-li zapnout ventilátoru.</span><span class="sxs-lookup"><span data-stu-id="419c9-112">Direct methods follow a request-response pattern and are meant for communications that require immediate confirmation of their result, usually interactive control of the device, for example to turn on a fan.</span></span>

<span data-ttu-id="419c9-113">Odkazovat na [Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] v případě pochybností mezi použitím požadované vlastnosti přímé metody nebo zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-113">Refer to [Cloud-to-device communication guidance][lnk-c2d-guidance] if in doubt between using desired properties, direct methods, or cloud-to-device messages.</span></span>

## <a name="method-lifecycle"></a><span data-ttu-id="419c9-114">Životní cyklus – metoda</span><span class="sxs-lookup"><span data-stu-id="419c9-114">Method lifecycle</span></span>
<span data-ttu-id="419c9-115">Přímé metody jsou implementované v zařízení a může vyžadovat nula nebo více vstupů v datové části Metoda správně vytvořit instanci.</span><span class="sxs-lookup"><span data-stu-id="419c9-115">Direct methods are implemented on the device and may require zero or more inputs in the method payload to correctly instantiate.</span></span> <span data-ttu-id="419c9-116">Vyvolání přímá metoda prostřednictvím identifikátoru URI služby přístupem (`{iot hub}/twins/{device id}/methods/`).</span><span class="sxs-lookup"><span data-stu-id="419c9-116">You invoke a direct method through a service-facing URI (`{iot hub}/twins/{device id}/methods/`).</span></span> <span data-ttu-id="419c9-117">Zařízení obdrží přímé metod v tématu MQTT konkrétní zařízení (`$iothub/methods/POST/{method name}/`).</span><span class="sxs-lookup"><span data-stu-id="419c9-117">A device receives direct methods through a device-specific MQTT topic (`$iothub/methods/POST/{method name}/`).</span></span> <span data-ttu-id="419c9-118">Přímé metody na dalších síťových protokolech straně zařízení jsme může podporovat v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="419c9-118">We may support direct methods on additional device-side networking protocols in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="419c9-119">Při vyvolání přímá metoda na zařízení, názvů a hodnot vlastností mohou obsahovat pouze US-ASCII tisknutelná alfanumerické znaky, s výjimkou těch následující sady: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span><span class="sxs-lookup"><span data-stu-id="419c9-119">When you invoke a direct method on a device, property names and values can only contain US-ASCII printable alphanumeric, except any in the following set: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.</span></span>
> 
> 

<span data-ttu-id="419c9-120">Přímé metody jsou synchronní a buď úspěšné nebo neúspěšné po uplynutí časového limitu (výchozí: 30 sekund, nastavit až na 3 600 sekund).</span><span class="sxs-lookup"><span data-stu-id="419c9-120">Direct methods are synchronous and either succeed or fail after the timeout period (default: 30 seconds, settable up to 3600 seconds).</span></span> <span data-ttu-id="419c9-121">Přímé metody jsou užitečné v interaktivní scénářích, kde chcete zařízení tak, aby fungoval pouze v případě zařízení je online a přijímající příkazy, například zapnutí light z telefonního čísla.</span><span class="sxs-lookup"><span data-stu-id="419c9-121">Direct methods are useful in interactive scenarios where you want a device to act if and only if the device is online and receiving commands, such as turning on a light from a phone.</span></span> <span data-ttu-id="419c9-122">V těchto scénářích chcete zobrazit okamžité úspěšné nebo neúspěšné, takže cloudové služby může fungovat na výsledek co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="419c9-122">In these scenarios, you want to see an immediate success or failure so the cloud service can act on the result as soon as possible.</span></span> <span data-ttu-id="419c9-123">Zařízení může vrátit některé tělo zprávy v důsledku metodu, ale není nutné u metodu Uděláte to tak.</span><span class="sxs-lookup"><span data-stu-id="419c9-123">The device may return some message body as a result of the method, but it isn't required for the method to do so.</span></span> <span data-ttu-id="419c9-124">Neexistuje žádná záruka, řazení v nebo všechny souběžnosti sémantiky pro volání metody.</span><span class="sxs-lookup"><span data-stu-id="419c9-124">There is no guarantee on ordering or any concurrency semantics on method calls.</span></span>

<span data-ttu-id="419c9-125">Přímá metoda jsou ze strany cloudu jen HTTP a MQTT jen ze strany zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-125">Direct method are HTTP-only from the cloud side, and MQTT-only from the device side.</span></span>

<span data-ttu-id="419c9-126">Datová část pro metoda požadavky a odpovědi je až 8KB dokumentu JSON.</span><span class="sxs-lookup"><span data-stu-id="419c9-126">The payload for method requests and responses is a JSON document up to 8KB.</span></span>

## <a name="reference-topics"></a><span data-ttu-id="419c9-127">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="419c9-127">Reference topics:</span></span>
<span data-ttu-id="419c9-128">Následující referenční témata poskytují další informace o používání přímé metody.</span><span class="sxs-lookup"><span data-stu-id="419c9-128">The following reference topics provide you with more information about using direct methods.</span></span>

## <a name="invoke-a-direct-method-from-a-back-end-app"></a><span data-ttu-id="419c9-129">Volání metody přímé z back-end aplikace</span><span class="sxs-lookup"><span data-stu-id="419c9-129">Invoke a direct method from a back-end app</span></span>
### <a name="method-invocation"></a><span data-ttu-id="419c9-130">Volání metody</span><span class="sxs-lookup"><span data-stu-id="419c9-130">Method invocation</span></span>
<span data-ttu-id="419c9-131">Přímá metoda volání na zařízení jsou volání protokolu HTTP, které tvoří:</span><span class="sxs-lookup"><span data-stu-id="419c9-131">Direct method invocations on a device are HTTP calls which comprise:</span></span>

* <span data-ttu-id="419c9-132">*URI* specifické pro zařízení (`{iot hub}/twins/{device id}/methods/`)</span><span class="sxs-lookup"><span data-stu-id="419c9-132">The *URI* specific to the device (`{iot hub}/twins/{device id}/methods/`)</span></span>
* <span data-ttu-id="419c9-133">V příspěvku *– metoda*</span><span class="sxs-lookup"><span data-stu-id="419c9-133">The POST *method*</span></span>
* <span data-ttu-id="419c9-134">*Hlavičky* který obsahovat autorizaci, žádat o ID, typu obsahu a kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="419c9-134">*Headers* which contain the authorization, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="419c9-135">Transparentní JSON *textu* v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="419c9-135">A transparent JSON *body* in the following format:</span></span>

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

<span data-ttu-id="419c9-136">Časový limit je počet sekund.</span><span class="sxs-lookup"><span data-stu-id="419c9-136">Timeout is in seconds.</span></span> <span data-ttu-id="419c9-137">Pokud není nastavený časový limit, bude výchozí 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="419c9-137">If timeout is not set, it defaults to 30 seconds.</span></span>

### <a name="response"></a><span data-ttu-id="419c9-138">Odpověď</span><span class="sxs-lookup"><span data-stu-id="419c9-138">Response</span></span>
<span data-ttu-id="419c9-139">Back-end aplikace obdrží odpověď, která zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="419c9-139">The back-end app receives a response which comprises:</span></span>

* <span data-ttu-id="419c9-140">*Stavový kód HTTP*, který se používá pro chyby přicházející ze služby IoT Hub, včetně chybu 404 pro zařízení není aktuálně připojený</span><span class="sxs-lookup"><span data-stu-id="419c9-140">*HTTP status code*, which is used for errors coming from the IoT Hub, including a 404 error for devices not currently connected</span></span>
* <span data-ttu-id="419c9-141">*Hlavičky* který obsahovat značku ETag, žádat o ID, typu obsahu a kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="419c9-141">*Headers* which contain the ETag, request ID, content type, and content encoding</span></span>
* <span data-ttu-id="419c9-142">JSON *textu* v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="419c9-142">A JSON *body* in the following format:</span></span>

```
{
    "status" : 201,
    "payload" : {...}
}
```

   <span data-ttu-id="419c9-143">Obě `status` a `body` je poskytovanému zařízením a používá odešle odpověď se stavovým kódem zařízení vlastní nebo popis.</span><span class="sxs-lookup"><span data-stu-id="419c9-143">Both `status` and `body` are provided by the device and used to respond with the device's own status code and/or description.</span></span>

## <a name="handle-a-direct-method-on-a-device"></a><span data-ttu-id="419c9-144">Popisovač přímá metoda na zařízení</span><span class="sxs-lookup"><span data-stu-id="419c9-144">Handle a direct method on a device</span></span>
### <a name="method-invocation"></a><span data-ttu-id="419c9-145">Volání metody</span><span class="sxs-lookup"><span data-stu-id="419c9-145">Method invocation</span></span>
<span data-ttu-id="419c9-146">Zařízení obdrží přímá metoda žádosti k tématu MQTT:`$iothub/methods/POST/{method name}/?$rid={request id}`</span><span class="sxs-lookup"><span data-stu-id="419c9-146">Devices receive direct method requests on the MQTT topic: `$iothub/methods/POST/{method name}/?$rid={request id}`</span></span>

<span data-ttu-id="419c9-147">Text, který obdrží zařízení je v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="419c9-147">The body which the device receives is in the following format:</span></span>

```
{
    "input1": "someInput",
    "input2": "anotherInput"
}
```

<span data-ttu-id="419c9-148">Požadavky metod jsou QoS 0.</span><span class="sxs-lookup"><span data-stu-id="419c9-148">Method requests are QoS 0.</span></span>

### <a name="response"></a><span data-ttu-id="419c9-149">Odpověď</span><span class="sxs-lookup"><span data-stu-id="419c9-149">Response</span></span>
<span data-ttu-id="419c9-150">Zařízení odesílá odpovědí `$iothub/methods/res/{status}/?$rid={request id}`, kde:</span><span class="sxs-lookup"><span data-stu-id="419c9-150">The device sends responses to `$iothub/methods/res/{status}/?$rid={request id}`, where:</span></span>

* <span data-ttu-id="419c9-151">`status` Vlastnost je stav zařízení zadaná metoda spouštění.</span><span class="sxs-lookup"><span data-stu-id="419c9-151">The `status` property is the device-supplied status of method execution.</span></span>
* <span data-ttu-id="419c9-152">`$rid` Vlastnost je ID žádosti z volání metody přijatých ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="419c9-152">The `$rid` property is the request ID from the method invocation received from IoT Hub.</span></span>

<span data-ttu-id="419c9-153">Text se nastavuje pomocí zařízení a může být jakékoli stavu.</span><span class="sxs-lookup"><span data-stu-id="419c9-153">The body is set by the device and can be any status.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="419c9-154">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="419c9-154">Additional reference material</span></span>
<span data-ttu-id="419c9-155">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="419c9-155">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="419c9-156">[Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="419c9-156">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="419c9-157">[Omezování a kvóty] [ lnk-quotas] popisuje kvóty, které platí pro službu IoT Hub a omezení chování se očekává při použití služby.</span><span class="sxs-lookup"><span data-stu-id="419c9-157">[Throttling and quotas][lnk-quotas] describes the quotas that apply to the IoT Hub service and the throttling behavior to expect when you use the service.</span></span>
* <span data-ttu-id="419c9-158">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="419c9-158">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="419c9-159">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ lnk-query] popisuje dotazovací jazyk Centrum IoT, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="419c9-159">[IoT Hub query language for device twins, jobs, and message routing][lnk-query] describes the IoT Hub query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="419c9-160">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="419c9-160">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="419c9-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="419c9-161">Next steps</span></span>
<span data-ttu-id="419c9-162">Nyní jste se naučili použití přímé metod, může zajímat v následujícím tématu Příručka vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="419c9-162">Now you have learned how to use direct methods, you may be interested in the following IoT Hub developer guide topic:</span></span>

* <span data-ttu-id="419c9-163">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="419c9-163">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="419c9-164">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="419c9-164">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="419c9-165">[Používat přímé metody][lnk-methods-tutorial]</span><span class="sxs-lookup"><span data-stu-id="419c9-165">[Use direct methods][lnk-methods-tutorial]</span></span>

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
