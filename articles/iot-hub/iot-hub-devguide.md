---
title: "Průvodce aaaDeveloper pro Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře pro službu Azure IoT Hub Hello zahrnuje diskusí koncových bodů, zabezpečení, hello registru identit, správu zařízení, přímé metody, dvojčata zařízení, nahrávání souborů, úlohy, hello dotazovacího jazyka pro IoT Hub a zasílání zpráv."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: d534ff9d-2de5-4995-bb2d-84a02693cb2e
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 2d3f18399e4cef6f9c4850a5caeb170a2d091a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-developer-guide"></a><span data-ttu-id="93719-103">Příručka vývojáře pro službu Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="93719-103">Azure IoT Hub developer guide</span></span>

<span data-ttu-id="93719-104">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="93719-104">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span>

<span data-ttu-id="93719-105">Azure IoT Hub nabízí:</span><span class="sxs-lookup"><span data-stu-id="93719-105">Azure IoT Hub provides you with:</span></span>

* <span data-ttu-id="93719-106">Zabezpečená komunikace pomocí přihlašovacích údajů na zařízení zabezpečení a řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="93719-106">Secure communications by using per-device security credentials and access control.</span></span>
* <span data-ttu-id="93719-107">Více možností velkého rozsahu komunikace zařízení cloud a cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-107">Multiple device-to-cloud and cloud-to-device hyper-scale communication options.</span></span>
* <span data-ttu-id="93719-108">Dotazovatelné úložiště informace o stavu licence vázané na zařízení a metadata.</span><span class="sxs-lookup"><span data-stu-id="93719-108">Queryable storage of per-device state information and meta-data.</span></span>
* <span data-ttu-id="93719-109">Připojení snadno zařízení s knihovny zařízení pro hello Nejoblíbenější jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="93719-109">Easy device connectivity with device libraries for hello most popular languages and platforms.</span></span>

<span data-ttu-id="93719-110">Tato příručka vývojáře IoT Hub obsahuje hello následující články:</span><span class="sxs-lookup"><span data-stu-id="93719-110">This IoT Hub developer guide includes hello following articles:</span></span>

* <span data-ttu-id="93719-111">[Komunikace zařízení cloud pokyny] [ lnk-d2c-guidance] pomůže vám zvolit zpráv typu zařízení cloud, dvojče zařízení hlášené vlastností a nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="93719-111">[Device-to-cloud communication guidance][lnk-d2c-guidance] helps you choose between device-to-cloud messages, device twin's reported properties, and file upload.</span></span>
* <span data-ttu-id="93719-112">[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pomůže vám zvolit přímé metody dvojče zařízení požadované vlastnosti a zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-112">[Cloud-to-device communication guidance][lnk-c2d-guidance] helps you choose between direct methods, device twin's desired properties, and cloud-to-device messages.</span></span>
* <span data-ttu-id="93719-113">[Zařízení cloud a cloud zařízení zasílání zpráv službou IoT Hub] [ devguide-messaging] popisuje hello zasílání zpráv funkce (zařízení cloud a z cloudu do zařízení), které IoT Hub zpřístupní.</span><span class="sxs-lookup"><span data-stu-id="93719-113">[Device-to-cloud and cloud-to-device messaging with IoT Hub][devguide-messaging] describes hello messaging features (device-to-cloud and cloud-to-device) that IoT Hub exposes.</span></span>
  * <span data-ttu-id="93719-114">[Odesílání zpráv typu zařízení cloud tooIoT rozbočovače][devguide-messages-d2c].</span><span class="sxs-lookup"><span data-stu-id="93719-114">[Send device-to-cloud messages tooIoT Hub][devguide-messages-d2c].</span></span>
  * <span data-ttu-id="93719-115">[Číst zprávy typu zařízení cloud ze vestavěným koncovým bodem hello][devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="93719-115">[Read device-to-cloud messages from hello built-in endpoint][devguide-builtin].</span></span>
  * <span data-ttu-id="93719-116">[Použít vlastní koncové body a pravidla směrování pro zprávy typu zařízení cloud][devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="93719-116">[Use custom endpoints and routing rules for device-to-cloud messages][devguide-custom].</span></span>
  * <span data-ttu-id="93719-117">[Odesílání zpráv typu cloud zařízení ze služby IoT Hub][devguide-messages-c2d].</span><span class="sxs-lookup"><span data-stu-id="93719-117">[Send cloud-to-device messages from IoT Hub][devguide-messages-c2d].</span></span>
  * <span data-ttu-id="93719-118">[Vytvoření a čtení zpráv služby IoT Hub][devguide-format].</span><span class="sxs-lookup"><span data-stu-id="93719-118">[Create and read IoT Hub messages][devguide-format].</span></span>
* <span data-ttu-id="93719-119">[Odeslání souborů ze zařízení] [ devguide-upload] popisuje, jak mohou odesílat soubory ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-119">[Upload files from a device][devguide-upload] describes how you can upload files from a device.</span></span> <span data-ttu-id="93719-120">Hello článku také obsahuje informace o oblastech, jako je hello, že proces odesílání hello oznámení můžete odesílat.</span><span class="sxs-lookup"><span data-stu-id="93719-120">hello article also includes information about topics such as hello notifications hello upload process can send.</span></span>
* <span data-ttu-id="93719-121">[Správa identit zařízení IoT hub] [ devguide-identities] popisuje, jaké informace IoT hub úložiště registru identit a jak můžete používat a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="93719-121">[Manage device identities in IoT Hub][devguide-identities] describes what information each IoT hub's identity registry stores, and how you can access and modify it.</span></span>
* <span data-ttu-id="93719-122">[Řízení přístupu tooIoT rozbočovače] [ devguide-security] popisuje hello zabezpečení modelu použitému toogrant tooIoT rozbočovače funkce přístupu pro zařízení a součásti cloudu.</span><span class="sxs-lookup"><span data-stu-id="93719-122">[Control access tooIoT Hub][devguide-security] describes hello security model used toogrant access tooIoT Hub functionality for both devices and cloud components.</span></span> <span data-ttu-id="93719-123">Hello článek obsahuje informace o používání tokeny a certifikáty X.509 a podrobnosti o hello oprávnění, které můžete udělit.</span><span class="sxs-lookup"><span data-stu-id="93719-123">hello article includes information about using tokens and X.509 certificates, and details of hello permissions you can grant.</span></span>
* <span data-ttu-id="93719-124">[Používání stavu toosynchronize dvojčata zařízení a konfigurací] [ devguide-device-twins] popisuje hello *dvojče zařízení* koncept a hello funkce zpřístupňuje například synchronizace zařízení s jeho zařízení Twin.</span><span class="sxs-lookup"><span data-stu-id="93719-124">[Use device twins toosynchronize state and configurations][devguide-device-twins] describes hello *device twin* concept and hello functionality it exposes such as synchronizing a device with its device twin.</span></span> <span data-ttu-id="93719-125">Hello článek obsahuje informace o hello data uložená v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-125">hello article includes information about hello data stored in a device twin.</span></span>
* <span data-ttu-id="93719-126">[Volání metody přímé na zařízení] [ devguide-directmethods] popisuje životního cyklu hello přímé metody, informace o jak v zařízení z vaší aplikace a popisovač hello back-end metody tooinvoke přímá metoda ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-126">[Invoke a direct method on a device][devguide-directmethods] describes hello lifecycle of a direct method, information about how tooinvoke methods on a device from your back-end app and handle hello direct method on your device.</span></span>
* <span data-ttu-id="93719-127">[Plánování úloh na několika zařízeních] [ devguide-jobs] popisuje, jak lze naplánovat úlohy na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="93719-127">[Schedule jobs on multiple devices][devguide-jobs] describes how you can schedule jobs on multiple devices.</span></span> <span data-ttu-id="93719-128">Hello článek popisuje, jak toosubmit úlohy provádět úkoly, jako je spuštění přímé metody, aktualizace zařízení pomocí dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-128">hello article describes how toosubmit jobs that perform tasks as executing a direct method, updating a device using a device twin.</span></span> <span data-ttu-id="93719-129">Také popisuje, jak tooquery hello stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="93719-129">It also describes how tooquery hello status of a job.</span></span>
* <span data-ttu-id="93719-130">[Reference – volba komunikační protokol] [ devguide-protocol] popisuje hello komunikační protokoly, podporuje komunikaci zařízení IoT Hub a seznamy hello porty, které by se mělo otevřít.</span><span class="sxs-lookup"><span data-stu-id="93719-130">[Reference - choose a communication protocol][devguide-protocol] describes hello communication protocols that IoT Hub supports for device communication and lists hello ports that should be open.</span></span>
* <span data-ttu-id="93719-131">[Odkaz – koncové body centra IoT] [ devguide-endpoints] popisuje hello různých koncových bodů, které každý IoT hub zpřístupní pro operace runtime a správy.</span><span class="sxs-lookup"><span data-stu-id="93719-131">[Reference - IoT Hub endpoints][devguide-endpoints] describes hello various endpoints that each IoT hub exposes for runtime and management operations.</span></span> <span data-ttu-id="93719-132">článek Hello také popisuje, jak můžete vytvořit další koncové body ve službě IoT hub a jak toouse pole brány tooenable zařízení připojení tooyour IoT Hub koncových bodů v nestandardním scénáře.</span><span class="sxs-lookup"><span data-stu-id="93719-132">hello article also describes how you can create additional endpoints in your IoT hub, and how toouse a field gateway tooenable devices connectivity tooyour IoT Hub endpoints in non-standard scenarios.</span></span>
* <span data-ttu-id="93719-133">[Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ devguide-query] popisuje této služby IoT Hub dotazovací jazyk, který vám umožní tooretrieve informace z vašeho centra o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="93719-133">[Reference - IoT Hub query language for device twins, jobs, and message routing][devguide-query] describes that IoT Hub query language that enables you tooretrieve information from your hub about your device twins and jobs.</span></span>
* <span data-ttu-id="93719-134">[Referenční dokumentace - kvót a omezování] [ devguide-quotas] shrnuje hello kvóty nastavené v hello služby IoT Hub a hello omezení chování toosee můžete očekávat, když překročí kvótu.</span><span class="sxs-lookup"><span data-stu-id="93719-134">[Reference - quotas and throttling][devguide-quotas] summarizes hello quotas set in hello IoT Hub service and hello throttling behavior you can expect toosee when you exceed a quota.</span></span>
* <span data-ttu-id="93719-135">[Reference – ceny] [ devguide-pricing] obsahuje obecné informace o různých SKU a cenách služby IoT Hub a podrobnosti o tom, jak hello různé funkce IoT Hub se měří jako zprávy službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="93719-135">[Reference - pricing][devguide-pricing] provides general information on different SKUs and pricing for IoT Hub and details on how hello various IoT Hub functionalities are metered as messages by IoT Hub.</span></span>
* <span data-ttu-id="93719-136">[Referenční dokumentace - zařízení a služby sady SDK] [ devguide-sdks] seznamy hello SDK služby Azure IoT můžete použít toodevelop zařízení a služby aplikace, které spolupracují se službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="93719-136">[Reference - Device and service SDKs][devguide-sdks] lists hello Azure IoT SDKs you can use toodevelop both device and service apps that interact with your IoT hub.</span></span> <span data-ttu-id="93719-137">Hello článek obsahuje odkazy tooonline rozhraní API dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="93719-137">hello article includes links tooonline API documentation.</span></span>
* <span data-ttu-id="93719-138">[Odkaz – Podpora IoT Hub MQTT] [ devguide-mqtt] poskytuje podrobné informace o tom, jak IoT Hub podporuje protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="93719-138">[Reference - IoT Hub MQTT support][devguide-mqtt] provides detailed information about how IoT Hub supports hello MQTT protocol.</span></span> <span data-ttu-id="93719-139">Hello článek popisuje podporu hello hello MQTT protokol předdefinované toohello SDK služby Azure IoT a obsahuje informace o použití protokolu MQTT hello přímo.</span><span class="sxs-lookup"><span data-stu-id="93719-139">hello article describes hello support for hello MQTT protocol built-in toohello Azure IoT SDKs and provides information about using hello MQTT protocol directly.</span></span>
* <span data-ttu-id="93719-140">[Glosář] [ devguide-glossary] seznam běžných termínů spojených se IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="93719-140">[Glossary][devguide-glossary] a list of common IoT Hub-related terms.</span></span>

[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md
[devguide-pricing]: iot-hub-devguide-pricing.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[devguide-messages-d2c]: iot-hub-devguide-messages-d2c.md
[devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[devguide-custom]: iot-hub-devguide-messages-read-custom.md
[devguide-messages-c2d]: iot-hub-devguide-messages-c2d.md
[devguide-format]: iot-hub-devguide-messages-construct.md
[devguide-protocol]: iot-hub-devguide-protocols.md
