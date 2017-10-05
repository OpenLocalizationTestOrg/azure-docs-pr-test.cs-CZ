---
title: "Příručka vývojáře pro Azure IoT Hub | Microsoft Docs"
description: "Příručka pro vývojáře Azure IoT Hub obsahuje diskusí koncových bodů, zabezpečení, registru identit, správu zařízení, přímé metody, dvojčata zařízení, nahrávání souborů, úlohy, dotazovací jazyk IoT Hub a zasílání zpráv."
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
ms.openlocfilehash: adb9a12899e9040cd83d522c734448989636fe29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-hub-developer-guide"></a><span data-ttu-id="027f1-103">Příručka vývojáře pro službu Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="027f1-103">Azure IoT Hub developer guide</span></span>

<span data-ttu-id="027f1-104">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="027f1-104">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span>

<span data-ttu-id="027f1-105">Azure IoT Hub nabízí:</span><span class="sxs-lookup"><span data-stu-id="027f1-105">Azure IoT Hub provides you with:</span></span>

* <span data-ttu-id="027f1-106">Zabezpečená komunikace pomocí přihlašovacích údajů na zařízení zabezpečení a řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="027f1-106">Secure communications by using per-device security credentials and access control.</span></span>
* <span data-ttu-id="027f1-107">Více možností velkého rozsahu komunikace zařízení cloud a cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-107">Multiple device-to-cloud and cloud-to-device hyper-scale communication options.</span></span>
* <span data-ttu-id="027f1-108">Dotazovatelné úložiště informace o stavu licence vázané na zařízení a metadata.</span><span class="sxs-lookup"><span data-stu-id="027f1-108">Queryable storage of per-device state information and meta-data.</span></span>
* <span data-ttu-id="027f1-109">Připojení snadno zařízení s knihovny zařízení pro Nejoblíbenější jazyky a platformy.</span><span class="sxs-lookup"><span data-stu-id="027f1-109">Easy device connectivity with device libraries for the most popular languages and platforms.</span></span>

<span data-ttu-id="027f1-110">Tato příručka vývojáře IoT Hub obsahuje v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="027f1-110">This IoT Hub developer guide includes the following articles:</span></span>

* <span data-ttu-id="027f1-111">[Komunikace zařízení cloud pokyny] [ lnk-d2c-guidance] pomůže vám zvolit zpráv typu zařízení cloud, dvojče zařízení hlášené vlastností a nahrávání souborů.</span><span class="sxs-lookup"><span data-stu-id="027f1-111">[Device-to-cloud communication guidance][lnk-d2c-guidance] helps you choose between device-to-cloud messages, device twin's reported properties, and file upload.</span></span>
* <span data-ttu-id="027f1-112">[Cloud zařízení komunikace pokyny] [ lnk-c2d-guidance] pomůže vám zvolit přímé metody dvojče zařízení požadované vlastnosti a zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-112">[Cloud-to-device communication guidance][lnk-c2d-guidance] helps you choose between direct methods, device twin's desired properties, and cloud-to-device messages.</span></span>
* <span data-ttu-id="027f1-113">[Zařízení cloud a cloud zařízení zasílání zpráv službou IoT Hub] [ devguide-messaging] popisuje funkce zasílání zpráv (zařízení cloud a z cloudu do zařízení), které IoT Hub zpřístupní.</span><span class="sxs-lookup"><span data-stu-id="027f1-113">[Device-to-cloud and cloud-to-device messaging with IoT Hub][devguide-messaging] describes the messaging features (device-to-cloud and cloud-to-device) that IoT Hub exposes.</span></span>
  * <span data-ttu-id="027f1-114">[Odesílání zpráv typu zařízení cloud do služby IoT Hub][devguide-messages-d2c].</span><span class="sxs-lookup"><span data-stu-id="027f1-114">[Send device-to-cloud messages to IoT Hub][devguide-messages-d2c].</span></span>
  * <span data-ttu-id="027f1-115">[Číst zprávy typu zařízení cloud z předdefinovaných koncového bodu][devguide-builtin].</span><span class="sxs-lookup"><span data-stu-id="027f1-115">[Read device-to-cloud messages from the built-in endpoint][devguide-builtin].</span></span>
  * <span data-ttu-id="027f1-116">[Použít vlastní koncové body a pravidla směrování pro zprávy typu zařízení cloud][devguide-custom].</span><span class="sxs-lookup"><span data-stu-id="027f1-116">[Use custom endpoints and routing rules for device-to-cloud messages][devguide-custom].</span></span>
  * <span data-ttu-id="027f1-117">[Odesílání zpráv typu cloud zařízení ze služby IoT Hub][devguide-messages-c2d].</span><span class="sxs-lookup"><span data-stu-id="027f1-117">[Send cloud-to-device messages from IoT Hub][devguide-messages-c2d].</span></span>
  * <span data-ttu-id="027f1-118">[Vytvoření a čtení zpráv služby IoT Hub][devguide-format].</span><span class="sxs-lookup"><span data-stu-id="027f1-118">[Create and read IoT Hub messages][devguide-format].</span></span>
* <span data-ttu-id="027f1-119">[Odeslání souborů ze zařízení] [ devguide-upload] popisuje, jak mohou odesílat soubory ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-119">[Upload files from a device][devguide-upload] describes how you can upload files from a device.</span></span> <span data-ttu-id="027f1-120">Článek obsahuje také informace o oblastech, jako je oznámení, že proces odesílání může odesílat.</span><span class="sxs-lookup"><span data-stu-id="027f1-120">The article also includes information about topics such as the notifications the upload process can send.</span></span>
* <span data-ttu-id="027f1-121">[Správa identit zařízení IoT hub] [ devguide-identities] popisuje, jaké informace IoT hub úložiště registru identit a jak můžete používat a upravit ho.</span><span class="sxs-lookup"><span data-stu-id="027f1-121">[Manage device identities in IoT Hub][devguide-identities] describes what information each IoT hub's identity registry stores, and how you can access and modify it.</span></span>
* <span data-ttu-id="027f1-122">[Řízení přístupu ke službě IoT Hub] [ devguide-security] popisuje model zabezpečení, používá k udělení přístupu k funkci IoT Hub pro zařízení a cloudových součásti.</span><span class="sxs-lookup"><span data-stu-id="027f1-122">[Control access to IoT Hub][devguide-security] describes the security model used to grant access to IoT Hub functionality for both devices and cloud components.</span></span> <span data-ttu-id="027f1-123">Článek obsahuje informace o používání tokeny a certifikáty X.509 a podrobnosti, které můžete udělit oprávnění.</span><span class="sxs-lookup"><span data-stu-id="027f1-123">The article includes information about using tokens and X.509 certificates, and details of the permissions you can grant.</span></span>
* <span data-ttu-id="027f1-124">[Pomocí dvojčata zařízení synchronizovat stavu a konfigurace] [ devguide-device-twins] popisuje *dvojče zařízení* koncept a funkce zpřístupňuje například synchronizace zařízení s jeho dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-124">[Use device twins to synchronize state and configurations][devguide-device-twins] describes the *device twin* concept and the functionality it exposes such as synchronizing a device with its device twin.</span></span> <span data-ttu-id="027f1-125">Článek obsahuje informace o data uložená v dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-125">The article includes information about the data stored in a device twin.</span></span>
* <span data-ttu-id="027f1-126">[Volání metody přímé na zařízení] [ devguide-directmethods] popisuje životní cyklus přímá metoda, informace o tom, jak volat metody na zařízení z back-end aplikace a zpracovat přímá metoda ve vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-126">[Invoke a direct method on a device][devguide-directmethods] describes the lifecycle of a direct method, information about how to invoke methods on a device from your back-end app and handle the direct method on your device.</span></span>
* <span data-ttu-id="027f1-127">[Plánování úloh na několika zařízeních] [ devguide-jobs] popisuje, jak lze naplánovat úlohy na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="027f1-127">[Schedule jobs on multiple devices][devguide-jobs] describes how you can schedule jobs on multiple devices.</span></span> <span data-ttu-id="027f1-128">Článek popisuje, jak k odesílání úloh, které provádějí úlohy, jako provádění přímé metodu, aktualizace zařízení pomocí dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-128">The article describes how to submit jobs that perform tasks as executing a direct method, updating a device using a device twin.</span></span> <span data-ttu-id="027f1-129">Také popisuje, jak zjistit stav úlohy.</span><span class="sxs-lookup"><span data-stu-id="027f1-129">It also describes how to query the status of a job.</span></span>
* <span data-ttu-id="027f1-130">[Reference – volba komunikační protokol] [ devguide-protocol] popisuje komunikační protokoly, že podporuje pro komunikaci zařízení IoT Hub a seznam portů, kterým by se mělo otevřít.</span><span class="sxs-lookup"><span data-stu-id="027f1-130">[Reference - choose a communication protocol][devguide-protocol] describes the communication protocols that IoT Hub supports for device communication and lists the ports that should be open.</span></span>
* <span data-ttu-id="027f1-131">[Odkaz – koncové body centra IoT] [ devguide-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro operace runtime a správy.</span><span class="sxs-lookup"><span data-stu-id="027f1-131">[Reference - IoT Hub endpoints][devguide-endpoints] describes the various endpoints that each IoT hub exposes for runtime and management operations.</span></span> <span data-ttu-id="027f1-132">Článek také popisuje, jak můžete vytvořit další koncové body ve službě IoT hub a jak používat brána pole pro umožnění připojení zařízení k vaší koncové body centra IoT v nestandardním scénáře.</span><span class="sxs-lookup"><span data-stu-id="027f1-132">The article also describes how you can create additional endpoints in your IoT hub, and how to use a field gateway to enable devices connectivity to your IoT Hub endpoints in non-standard scenarios.</span></span>
* <span data-ttu-id="027f1-133">[Referenční dokumentace - IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv] [ devguide-query] popisuje této služby IoT Hub dotazovací jazyk, který vám umožňuje načíst informace z vašeho centra o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="027f1-133">[Reference - IoT Hub query language for device twins, jobs, and message routing][devguide-query] describes that IoT Hub query language that enables you to retrieve information from your hub about your device twins and jobs.</span></span>
* <span data-ttu-id="027f1-134">[Referenční dokumentace - kvót a omezování] [ devguide-quotas] shrnuje kvóty nastavit ve službě IoT Hub a omezení chování můžete očekávat zobrazíte, když překročí kvótu.</span><span class="sxs-lookup"><span data-stu-id="027f1-134">[Reference - quotas and throttling][devguide-quotas] summarizes the quotas set in the IoT Hub service and the throttling behavior you can expect to see when you exceed a quota.</span></span>
* <span data-ttu-id="027f1-135">[Reference – ceny] [ devguide-pricing] obsahuje obecné informace o různých SKU a cenách služby IoT Hub a podrobnosti o tom, jak jsou různé funkce IoT Hub měřené jako zprávy službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="027f1-135">[Reference - pricing][devguide-pricing] provides general information on different SKUs and pricing for IoT Hub and details on how the various IoT Hub functionalities are metered as messages by IoT Hub.</span></span>
* <span data-ttu-id="027f1-136">[Referenční dokumentace - zařízení a služby sady SDK] [ devguide-sdks] uvádí SDK služby Azure IoT můžete vyvíjet aplikace zařízení a služby, které spolupracují se službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="027f1-136">[Reference - Device and service SDKs][devguide-sdks] lists the Azure IoT SDKs you can use to develop both device and service apps that interact with your IoT hub.</span></span> <span data-ttu-id="027f1-137">Článek obsahuje odkazy na online dokumentaci k rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="027f1-137">The article includes links to online API documentation.</span></span>
* <span data-ttu-id="027f1-138">[Odkaz – Podpora IoT Hub MQTT] [ devguide-mqtt] poskytuje podrobné informace o tom, jak IoT Hub podporuje protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="027f1-138">[Reference - IoT Hub MQTT support][devguide-mqtt] provides detailed information about how IoT Hub supports the MQTT protocol.</span></span> <span data-ttu-id="027f1-139">Článek popisuje podporu pro protokol MQTT integrované do SDK služby Azure IoT a poskytuje informace o používání protokolu MQTT přímo.</span><span class="sxs-lookup"><span data-stu-id="027f1-139">The article describes the support for the MQTT protocol built-in to the Azure IoT SDKs and provides information about using the MQTT protocol directly.</span></span>
* <span data-ttu-id="027f1-140">[Glosář] [ devguide-glossary] seznam běžných termínů spojených se IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="027f1-140">[Glossary][devguide-glossary] a list of common IoT Hub-related terms.</span></span>

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
