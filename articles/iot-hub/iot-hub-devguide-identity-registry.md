---
title: "Pochopení registru identit Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – Popis registru identit služby IoT Hub a způsobu jeho použití ke správě svých zařízení. Obsahuje informace o import a export identit zařízení hromadně."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6e9c7b71fa6fc78f97c0144c735fc44778181d8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a><span data-ttu-id="3a633-104">Pochopení registru identit ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-104">Understand the identity registry in your IoT hub</span></span>

<span data-ttu-id="3a633-105">Každé centrum IoT má registru identit, která uchovává informace o zařízení lze připojit ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-105">Every IoT hub has an identity registry that stores information about the devices permitted to connect to the IoT hub.</span></span> <span data-ttu-id="3a633-106">Než se zařízení může připojit ke službě IoT hub, musí existovat položka pro toto zařízení v registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-106">Before a device can connect to an IoT hub, there must be an entry for that device in the IoT hub's identity registry.</span></span> <span data-ttu-id="3a633-107">Zařízení musí ověřit také pomocí založené na přihlašovací údaje uložené v registru identit služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-107">A device must also authenticate with the IoT hub based on credentials stored in the identity registry.</span></span>

<span data-ttu-id="3a633-108">ID zařízení, které jsou uložené v registru identit rozlišuje velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="3a633-108">The device ID stored in the identity registry is case-sensitive.</span></span>

<span data-ttu-id="3a633-109">Na vysoké úrovni je registru identit podporující REST kolekce prostředků identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-109">At a high level, the identity registry is a REST-capable collection of device identity resources.</span></span> <span data-ttu-id="3a633-110">Když přidáte položku v registru identit, IoT Hub vytvoří sadu prostředků na zařízení, jako jsou fronty, který obsahuje neukládají zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-110">When you add an entry in the identity registry, IoT Hub creates a set of per-device resources such as the queue that contains in-flight cloud-to-device messages.</span></span>

### <a name="when-to-use"></a><span data-ttu-id="3a633-111">Kdy je použít</span><span class="sxs-lookup"><span data-stu-id="3a633-111">When to use</span></span>

<span data-ttu-id="3a633-112">Registr identit použijte, když potřebujete:</span><span class="sxs-lookup"><span data-stu-id="3a633-112">Use the identity registry when you need to:</span></span>

* <span data-ttu-id="3a633-113">Zřizování zařízení, které se připojují ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-113">Provision devices that connect to your IoT hub.</span></span>
* <span data-ttu-id="3a633-114">Řízení přístupu podle zařízení do koncových bodů rozbočovače na straně zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-114">Control per-device access to your hub's device-facing endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="3a633-115">Registr identity neobsahuje žádné metadata specifická pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3a633-115">The identity registry does not contain any application-specific metadata.</span></span>

## <a name="identity-registry-operations"></a><span data-ttu-id="3a633-116">Operace registru identit</span><span class="sxs-lookup"><span data-stu-id="3a633-116">Identity registry operations</span></span>

<span data-ttu-id="3a633-117">Registr identit služby IoT Hub zpřístupní následující operace:</span><span class="sxs-lookup"><span data-stu-id="3a633-117">The IoT Hub identity registry exposes the following operations:</span></span>

* <span data-ttu-id="3a633-118">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-118">Create device identity</span></span>
* <span data-ttu-id="3a633-119">Aktualizovat identita zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-119">Update device identity</span></span>
* <span data-ttu-id="3a633-120">Načíst identitu zařízení podle ID</span><span class="sxs-lookup"><span data-stu-id="3a633-120">Retrieve device identity by ID</span></span>
* <span data-ttu-id="3a633-121">Odstranit identita zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-121">Delete device identity</span></span>
* <span data-ttu-id="3a633-122">Seznam až 1 000 identit</span><span class="sxs-lookup"><span data-stu-id="3a633-122">List up to 1000 identities</span></span>
* <span data-ttu-id="3a633-123">Exportovat všechny identity do Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="3a633-123">Export all identities to Azure blob storage</span></span>
* <span data-ttu-id="3a633-124">Import identit z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="3a633-124">Import identities from Azure blob storage</span></span>

<span data-ttu-id="3a633-125">Všechny tyto operace můžete použít optimistickou metodu souběžného, jak je uvedeno v [RFC7232][lnk-rfc7232].</span><span class="sxs-lookup"><span data-stu-id="3a633-125">All these operations can use optimistic concurrency, as specified in [RFC7232][lnk-rfc7232].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a633-126">Jediný způsob, jak načíst všechny identity v registru identit služby IoT hub je použití [exportovat] [ lnk-export] funkce.</span><span class="sxs-lookup"><span data-stu-id="3a633-126">The only way to retrieve all identities in an IoT hub's identity registry is to use the [Export][lnk-export] functionality.</span></span>

<span data-ttu-id="3a633-127">Registr identit IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="3a633-127">An IoT Hub identity registry:</span></span>

* <span data-ttu-id="3a633-128">Neobsahuje žádné metadata aplikace.</span><span class="sxs-lookup"><span data-stu-id="3a633-128">Does not contain any application metadata.</span></span>
* <span data-ttu-id="3a633-129">Je přístupný jako slovník, pomocí **deviceId** jako klíč.</span><span class="sxs-lookup"><span data-stu-id="3a633-129">Can be accessed like a dictionary, by using the **deviceId** as the key.</span></span>
* <span data-ttu-id="3a633-130">Nepodporuje výrazovou dotazů.</span><span class="sxs-lookup"><span data-stu-id="3a633-130">Does not support expressive queries.</span></span>

<span data-ttu-id="3a633-131">Řešení IoT obvykle obsahuje samostatné úložiště specifické pro řešení, který obsahuje metadata specifické pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3a633-131">An IoT solution typically has a separate solution-specific store that contains application-specific metadata.</span></span> <span data-ttu-id="3a633-132">Konkrétní řešení úložiště v inteligentní sestavování řešení by například záznam místnosti, ve kterém je nasazená teplotní snímač.</span><span class="sxs-lookup"><span data-stu-id="3a633-132">For example, the solution-specific store in a smart building solution would record the room in which a temperature sensor is deployed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3a633-133">Registr identity lze použijte pouze pro správu zařízení a zajišťování operace.</span><span class="sxs-lookup"><span data-stu-id="3a633-133">Only use the identity registry for device management and provisioning operations.</span></span> <span data-ttu-id="3a633-134">Vysoké propustnosti operace v době běhu nesmí závisí na provádění operací v registru identit.</span><span class="sxs-lookup"><span data-stu-id="3a633-134">High throughput operations at run time should not depend on performing operations in the identity registry.</span></span> <span data-ttu-id="3a633-135">Například kontrola stavu připojení zařízení před odesláním příkazu není podporované vzor.</span><span class="sxs-lookup"><span data-stu-id="3a633-135">For example, checking the connection state of a device before sending a command is not a supported pattern.</span></span> <span data-ttu-id="3a633-136">Nezapomeňte zaškrtnout [míru omezení] [ lnk-quotas] pro registr identit a [prezenčního signálu zařízení] [ lnk-guidance-heartbeat] vzor.</span><span class="sxs-lookup"><span data-stu-id="3a633-136">Make sure to check the [throttling rates][lnk-quotas] for the identity registry, and the [device heartbeat][lnk-guidance-heartbeat] pattern.</span></span>

## <a name="disable-devices"></a><span data-ttu-id="3a633-137">Zakažte zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-137">Disable devices</span></span>

<span data-ttu-id="3a633-138">Zařízení můžete zakázat aktualizací **stav** vlastnost identity v registru identit.</span><span class="sxs-lookup"><span data-stu-id="3a633-138">You can disable devices by updating the **status** property of an identity in the identity registry.</span></span> <span data-ttu-id="3a633-139">Tato vlastnost se obvykle používá ve dvou scénářích:</span><span class="sxs-lookup"><span data-stu-id="3a633-139">Typically, you use this property in two scenarios:</span></span>

* <span data-ttu-id="3a633-140">Během procesu zřizování orchestration.</span><span class="sxs-lookup"><span data-stu-id="3a633-140">During a provisioning orchestration process.</span></span> <span data-ttu-id="3a633-141">Další informace najdete v tématu [zřizování zařízení][lnk-guidance-provisioning].</span><span class="sxs-lookup"><span data-stu-id="3a633-141">For more information, see [Device Provisioning][lnk-guidance-provisioning].</span></span>
* <span data-ttu-id="3a633-142">Pokud z nějakého důvodu byste zvážit, dojde k ohrožení bezpečnosti nebo se staly neoprávněné zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-142">If, for any reason, you consider a device is compromised or has become unauthorized.</span></span>

## <a name="import-and-export-device-identities"></a><span data-ttu-id="3a633-143">Import a export identit zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-143">Import and export device identities</span></span>

<span data-ttu-id="3a633-144">Identit zařízení hromadné z registru identit služby IoT hub, můžete exportovat pomocí asynchronních operací [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="3a633-144">You can export device identities in bulk from an IoT hub's identity registry, by using asynchronous operations on the [IoT Hub resource provider endpoint][lnk-endpoints].</span></span> <span data-ttu-id="3a633-145">Exportuje jsou dlouho běžící úlohy, které používají kontejner objektů blob zadané zákazníka k uložení dat identity zařízení přečíst z registru identit.</span><span class="sxs-lookup"><span data-stu-id="3a633-145">Exports are long-running jobs that use a customer-supplied blob container to save device identity data read from the identity registry.</span></span>

<span data-ttu-id="3a633-146">Identit zařízení hromadné můžete importovat do registru identit služby IoT hub, pomocí asynchronních operací [koncový bod zprostředkovatele prostředků služby IoT Hub][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="3a633-146">You can import device identities in bulk to an IoT hub's identity registry, by using asynchronous operations on the [IoT Hub resource provider endpoint][lnk-endpoints].</span></span> <span data-ttu-id="3a633-147">Importy jsou dlouho běžící úlohy, které používají data v kontejneru objektů blob zadané zákazníka k zápisu dat identity zařízení do registru identit.</span><span class="sxs-lookup"><span data-stu-id="3a633-147">Imports are long-running jobs that use data in a customer-supplied blob container to write device identity data into the identity registry.</span></span>

* <span data-ttu-id="3a633-148">Podrobné informace o importu a exportu rozhraní API najdete v tématu [zprostředkovatele prostředků služby IoT Hub rozhraní REST API][lnk-resource-provider-apis].</span><span class="sxs-lookup"><span data-stu-id="3a633-148">For detailed information about the import and export APIs, see [IoT Hub resource provider REST APIs][lnk-resource-provider-apis].</span></span>
* <span data-ttu-id="3a633-149">Další informace o spouštění import a export úloh najdete v tématu [hromadné správu identit zařízení IoT Hub][lnk-bulk-identity].</span><span class="sxs-lookup"><span data-stu-id="3a633-149">To learn more about running import and export jobs, see [Bulk management of IoT Hub device identities][lnk-bulk-identity].</span></span>

## <a name="device-provisioning"></a><span data-ttu-id="3a633-150">Zřizování zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-150">Device provisioning</span></span>

<span data-ttu-id="3a633-151">Data zařízení, která ukládá daného řešení IoT, závisí na konkrétní požadavky tohoto řešení.</span><span class="sxs-lookup"><span data-stu-id="3a633-151">The device data that a given IoT solution stores depends on the specific requirements of that solution.</span></span> <span data-ttu-id="3a633-152">Ale minimálně, musí řešení úložiště identit zařízení a ověřovací klíče.</span><span class="sxs-lookup"><span data-stu-id="3a633-152">But, as a minimum, a solution must store device identities and authentication keys.</span></span> <span data-ttu-id="3a633-153">Azure IoT Hub obsahuje registr identity, který může ukládat hodnoty pro každé zařízení, jako je například ID ověřovací klíče a stavové kódy.</span><span class="sxs-lookup"><span data-stu-id="3a633-153">Azure IoT Hub includes an identity registry that can store values for each device such as IDs, authentication keys, and status codes.</span></span> <span data-ttu-id="3a633-154">Řešení můžete použít jinými službami Azure, jako je například úložiště table, úložiště objektů blob nebo Cosmos DB k ukládání dat další zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-154">A solution can use other Azure services such as table storage, blob storage, or Cosmos DB to store any additional device data.</span></span>

<span data-ttu-id="3a633-155">*Zřizování zařízení* je proces přidávání počáteční zařízení data do úložiště v řešení.</span><span class="sxs-lookup"><span data-stu-id="3a633-155">*Device provisioning* is the process of adding the initial device data to the stores in your solution.</span></span> <span data-ttu-id="3a633-156">Pokud chcete povolit nové zařízení pro připojení do vašeho centra, musíte přidat ID zařízení a klíče do registru identit služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-156">To enable a new device to connect to your hub, you must add a device ID and keys to the IoT Hub identity registry.</span></span> <span data-ttu-id="3a633-157">Jako součást procesu zřizování může být potřeba inicializovat data specifická pro zařízení v jiné řešení úložiště.</span><span class="sxs-lookup"><span data-stu-id="3a633-157">As part of the provisioning process, you might need to initialize device-specific data in other solution stores.</span></span>

## <a name="device-heartbeat"></a><span data-ttu-id="3a633-158">Prezenční signál zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-158">Device heartbeat</span></span>

<span data-ttu-id="3a633-159">Registr identit služby IoT Hub obsahuje pole s názvem **connectionState**.</span><span class="sxs-lookup"><span data-stu-id="3a633-159">The IoT Hub identity registry contains a field called **connectionState**.</span></span> <span data-ttu-id="3a633-160">Použít pouze **connectionState** pole při vývoji a ladění.</span><span class="sxs-lookup"><span data-stu-id="3a633-160">Only use the **connectionState** field during development and debugging.</span></span> <span data-ttu-id="3a633-161">Řešení IoT by neměl dotaz na pole v době běhu.</span><span class="sxs-lookup"><span data-stu-id="3a633-161">IoT solutions should not query the field at run time.</span></span> <span data-ttu-id="3a633-162">Například dotazování **connectionState** pole ke kontrole, pokud je zařízení připojené před odesláním zprávy typu cloud zařízení nebo serveru služby SMS.</span><span class="sxs-lookup"><span data-stu-id="3a633-162">For example, do not query the **connectionState** field to check if a device is connected before you send a cloud-to-device message or an SMS.</span></span>

<span data-ttu-id="3a633-163">Pokud je potřeba vědět, pokud je zařízení připojené, měli byste implementovat řešení IoT *prezenčního signálu vzor*.</span><span class="sxs-lookup"><span data-stu-id="3a633-163">If your IoT solution needs to know if a device is connected, you should implement the *heartbeat pattern*.</span></span>

<span data-ttu-id="3a633-164">Ve vzoru prezenčního signálu zařízení odesílá zprávy typu zařízení cloud alespoň jednou každých pevné množství času (například alespoň jednou za hodinu).</span><span class="sxs-lookup"><span data-stu-id="3a633-164">In the heartbeat pattern, the device sends device-to-cloud messages at least once every fixed amount of time (for example, at least once every hour).</span></span> <span data-ttu-id="3a633-165">Proto i v případě, že zařízení nemá žádná data k odeslání, stále odešle zprávu typu zařízení cloud prázdný (obvykle s vlastnost, která ji identifikuje jako prezenční signál).</span><span class="sxs-lookup"><span data-stu-id="3a633-165">Therefore, even if a device does not have any data to send, it still sends an empty device-to-cloud message (usually with a property that identifies it as a heartbeat).</span></span> <span data-ttu-id="3a633-166">Na straně služby řešení udržuje mapu s poslední prezenční signál pro každé zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-166">On the service side, the solution maintains a map with the last heartbeat received for each device.</span></span> <span data-ttu-id="3a633-167">Pokud řešení neobdrží zprávu prezenčního signálu v očekávaném čase ze zařízení, předpokládá, že došlo k potížím se zařízením.</span><span class="sxs-lookup"><span data-stu-id="3a633-167">If the solution does not receive a heartbeat message within the expected time from the device, it assumes that there is a problem with the device.</span></span>

<span data-ttu-id="3a633-168">Složitější implementace může obsahovat informace z [operations monitorování] [ lnk-devguide-opmon] k identifikaci zařízení, která se snaží připojit nebo komunikovat, ale selhání.</span><span class="sxs-lookup"><span data-stu-id="3a633-168">A more complex implementation could include the information from [operations monitoring][lnk-devguide-opmon] to identify devices that are trying to connect or communicate but failing.</span></span> <span data-ttu-id="3a633-169">Pokud implementujete vzoru prezenčního signálu, nezapomeňte zaškrtnout [IoT Hub kvóty a omezení][lnk-quotas].</span><span class="sxs-lookup"><span data-stu-id="3a633-169">When you implement the heartbeat pattern, make sure to check [IoT Hub Quotas and Throttles][lnk-quotas].</span></span>

> [!NOTE]
> <span data-ttu-id="3a633-170">Pokud řešení IoT používá výhradně k určení, zda se k odesílání zpráv typu cloud zařízení stav připojení a zprávy nejsou všesměrové vysílání pro velké sady zařízení, zvažte použití jednodušší *krátkodobých čas vypršení platnosti* vzor.</span><span class="sxs-lookup"><span data-stu-id="3a633-170">If an IoT solution uses the connection state solely to determine whether to send cloud-to-device messages, and messages are not broadcast to large sets of devices, consider using the simpler *short expiry time* pattern.</span></span> <span data-ttu-id="3a633-171">Tento vzor dosáhne stejného výsledku jako zachování registru stav připojení zařízení pomocí vzoru prezenčního signálu, aniž by byly efektivnější.</span><span class="sxs-lookup"><span data-stu-id="3a633-171">This pattern achieves the same result as maintaining a device connection state registry using the heartbeat pattern, while being more efficient.</span></span> <span data-ttu-id="3a633-172">Pokud budete požadovat potvrzení zprávy, IoT Hub vás může upozornit, které jsou o zařízení, která může přijímat zprávy, které nejsou.</span><span class="sxs-lookup"><span data-stu-id="3a633-172">If you request message acknowledgements, IoT Hub can notify you about which devices are able to receive messages and which are not.</span></span>

## <a name="device-lifecycle-notifications"></a><span data-ttu-id="3a633-173">Oznámení životního cyklu zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-173">Device lifecycle notifications</span></span>

<span data-ttu-id="3a633-174">IoT Hub můžete řešení IoT upozornit, když je vytvořené nebo odstraněné zasláním oznámení životního cyklu zařízení identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-174">IoT Hub can notify your IoT solution when a device identity is created or deleted by sending device lifecycle notifications.</span></span> <span data-ttu-id="3a633-175">Uděláte to tak, musí vaše řešení IoT vytvořit trasu a nastavte zdroj dat na hodnotu *DeviceLifecycleEvents*.</span><span class="sxs-lookup"><span data-stu-id="3a633-175">To do so, your IoT solution needs to create a route and to set the Data Source equal to *DeviceLifecycleEvents*.</span></span> <span data-ttu-id="3a633-176">Ve výchozím nastavení jsou odeslána žádná oznámení životního cyklu, tedy předem neexistuje žádný takový trasy.</span><span class="sxs-lookup"><span data-stu-id="3a633-176">By default, no lifecycle notifications are sent, that is, no such routes pre-exist.</span></span> <span data-ttu-id="3a633-177">Oznámení obsahuje vlastnosti a text.</span><span class="sxs-lookup"><span data-stu-id="3a633-177">The notification message includes properties, and body.</span></span>

<span data-ttu-id="3a633-178">Vlastnosti: Vlastnosti zprávu systému mají předponu `'$'` symbol.</span><span class="sxs-lookup"><span data-stu-id="3a633-178">Properties: Message system properties are prefixed with the `'$'` symbol.</span></span>

| <span data-ttu-id="3a633-179">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="3a633-179">Name</span></span> | <span data-ttu-id="3a633-180">Hodnota</span><span class="sxs-lookup"><span data-stu-id="3a633-180">Value</span></span> |
| --- | --- |
<span data-ttu-id="3a633-181">$content – typ</span><span class="sxs-lookup"><span data-stu-id="3a633-181">$content-type</span></span> | <span data-ttu-id="3a633-182">application/json</span><span class="sxs-lookup"><span data-stu-id="3a633-182">application/json</span></span> |
<span data-ttu-id="3a633-183">$iothub-enqueuedtime</span><span class="sxs-lookup"><span data-stu-id="3a633-183">$iothub-enqueuedtime</span></span> |  <span data-ttu-id="3a633-184">Čas odeslání oznámení.</span><span class="sxs-lookup"><span data-stu-id="3a633-184">Time when the notification was sent</span></span> |
<span data-ttu-id="3a633-185">$iothub – zpráva – zdroj</span><span class="sxs-lookup"><span data-stu-id="3a633-185">$iothub-message-source</span></span> | <span data-ttu-id="3a633-186">deviceLifecycleEvents</span><span class="sxs-lookup"><span data-stu-id="3a633-186">deviceLifecycleEvents</span></span> |
<span data-ttu-id="3a633-187">$content – kódování</span><span class="sxs-lookup"><span data-stu-id="3a633-187">$content-encoding</span></span> | <span data-ttu-id="3a633-188">znakové sady UTF-8</span><span class="sxs-lookup"><span data-stu-id="3a633-188">utf-8</span></span> |
<span data-ttu-id="3a633-189">opType</span><span class="sxs-lookup"><span data-stu-id="3a633-189">opType</span></span> | <span data-ttu-id="3a633-190">**createDeviceIdentity** nebo **deleteDeviceIdentity**</span><span class="sxs-lookup"><span data-stu-id="3a633-190">**createDeviceIdentity** or **deleteDeviceIdentity**</span></span> |
<span data-ttu-id="3a633-191">hubName</span><span class="sxs-lookup"><span data-stu-id="3a633-191">hubName</span></span> | <span data-ttu-id="3a633-192">Název centra IoT</span><span class="sxs-lookup"><span data-stu-id="3a633-192">Name of IoT Hub</span></span> |
<span data-ttu-id="3a633-193">deviceId</span><span class="sxs-lookup"><span data-stu-id="3a633-193">deviceId</span></span> | <span data-ttu-id="3a633-194">ID zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-194">ID of the device</span></span> |
<span data-ttu-id="3a633-195">operationTimestamp</span><span class="sxs-lookup"><span data-stu-id="3a633-195">operationTimestamp</span></span> | <span data-ttu-id="3a633-196">Časové razítko ISO8601 operace</span><span class="sxs-lookup"><span data-stu-id="3a633-196">ISO8601 timestamp of operation</span></span> |
<span data-ttu-id="3a633-197">schéma zprávy iothub</span><span class="sxs-lookup"><span data-stu-id="3a633-197">iothub-message-schema</span></span> | <span data-ttu-id="3a633-198">deviceLifecycleNotification</span><span class="sxs-lookup"><span data-stu-id="3a633-198">deviceLifecycleNotification</span></span> |

<span data-ttu-id="3a633-199">Text: V této části je ve formátu JSON a představuje twin identity vytvořený zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-199">Body: This section is in JSON format and represents the twin of the created device identity.</span></span> <span data-ttu-id="3a633-200">Například:</span><span class="sxs-lookup"><span data-stu-id="3a633-200">For example,</span></span>

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
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

## <a name="reference-topics"></a><span data-ttu-id="3a633-201">Témata odkazů:</span><span class="sxs-lookup"><span data-stu-id="3a633-201">Reference topics:</span></span>

<span data-ttu-id="3a633-202">Následující referenční témata poskytují další informace o registru identit.</span><span class="sxs-lookup"><span data-stu-id="3a633-202">The following reference topics provide you with more information about the identity registry.</span></span>

## <a name="device-identity-properties"></a><span data-ttu-id="3a633-203">Vlastnosti identity zařízení</span><span class="sxs-lookup"><span data-stu-id="3a633-203">Device identity properties</span></span>

<span data-ttu-id="3a633-204">Identit zařízení jsou reprezentovány jako dokumenty JSON s následujícími vlastnostmi:</span><span class="sxs-lookup"><span data-stu-id="3a633-204">Device identities are represented as JSON documents with the following properties:</span></span>

| <span data-ttu-id="3a633-205">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="3a633-205">Property</span></span> | <span data-ttu-id="3a633-206">Možnosti</span><span class="sxs-lookup"><span data-stu-id="3a633-206">Options</span></span> | <span data-ttu-id="3a633-207">Popis</span><span class="sxs-lookup"><span data-stu-id="3a633-207">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3a633-208">deviceId</span><span class="sxs-lookup"><span data-stu-id="3a633-208">deviceId</span></span> |<span data-ttu-id="3a633-209">aktualizace požadované, jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-209">required, read-only on updates</span></span> |<span data-ttu-id="3a633-210">Řetězec malá a velká písmena (až 128 znaků.) z alfanumerických znaků ASCII 7bitového + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span><span class="sxs-lookup"><span data-stu-id="3a633-210">A case-sensitive string (up to 128 characters long) of ASCII 7-bit alphanumeric characters + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span></span> |
| <span data-ttu-id="3a633-211">generationId</span><span class="sxs-lookup"><span data-stu-id="3a633-211">generationId</span></span> |<span data-ttu-id="3a633-212">vyžaduje jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-212">required, read-only</span></span> |<span data-ttu-id="3a633-213">IoT hub generovaných, malá a velká písmena řetězec až 128 znaků.</span><span class="sxs-lookup"><span data-stu-id="3a633-213">An IoT hub-generated, case-sensitive string up to 128 characters long.</span></span> <span data-ttu-id="3a633-214">Tato hodnota se používá k rozlišení zařízení se stejným **deviceId**, pokud byla odstraněna a znovu vytvořena.</span><span class="sxs-lookup"><span data-stu-id="3a633-214">This value is used to distinguish devices with the same **deviceId**, when they have been deleted and re-created.</span></span> |
| <span data-ttu-id="3a633-215">Značka Etag</span><span class="sxs-lookup"><span data-stu-id="3a633-215">etag</span></span> |<span data-ttu-id="3a633-216">vyžaduje jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-216">required, read-only</span></span> |<span data-ttu-id="3a633-217">Řetězec představující na slabou značku ETag pro identitu zařízení dle [RFC7232][lnk-rfc7232].</span><span class="sxs-lookup"><span data-stu-id="3a633-217">A string representing a weak ETag for the device identity, as per [RFC7232][lnk-rfc7232].</span></span> |
| <span data-ttu-id="3a633-218">ověřování</span><span class="sxs-lookup"><span data-stu-id="3a633-218">auth</span></span> |<span data-ttu-id="3a633-219">Volitelné</span><span class="sxs-lookup"><span data-stu-id="3a633-219">optional</span></span> |<span data-ttu-id="3a633-220">Složené objekt obsahující informace a zabezpečení materiály ověřování.</span><span class="sxs-lookup"><span data-stu-id="3a633-220">A composite object containing authentication information and security materials.</span></span> |
| <span data-ttu-id="3a633-221">auth.symkey</span><span class="sxs-lookup"><span data-stu-id="3a633-221">auth.symkey</span></span> |<span data-ttu-id="3a633-222">Volitelné</span><span class="sxs-lookup"><span data-stu-id="3a633-222">optional</span></span> |<span data-ttu-id="3a633-223">Objekt složený obsahující primární a sekundární klíč uložený ve formátu base64.</span><span class="sxs-lookup"><span data-stu-id="3a633-223">A composite object containing a primary and a secondary key, stored in base64 format.</span></span> |
| <span data-ttu-id="3a633-224">status</span><span class="sxs-lookup"><span data-stu-id="3a633-224">status</span></span> |<span data-ttu-id="3a633-225">Požadované</span><span class="sxs-lookup"><span data-stu-id="3a633-225">required</span></span> |<span data-ttu-id="3a633-226">Slouží jako ukazatel přístup.</span><span class="sxs-lookup"><span data-stu-id="3a633-226">An access indicator.</span></span> <span data-ttu-id="3a633-227">Může být **povoleno** nebo **zakázané**.</span><span class="sxs-lookup"><span data-stu-id="3a633-227">Can be **Enabled** or **Disabled**.</span></span> <span data-ttu-id="3a633-228">Pokud **povoleno**, zařízení se může připojit.</span><span class="sxs-lookup"><span data-stu-id="3a633-228">If **Enabled**, the device is allowed to connect.</span></span> <span data-ttu-id="3a633-229">Pokud **zakázané**, toto zařízení nemá přístup k žádný koncový bod směřujících zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-229">If **Disabled**, this device cannot access any device-facing endpoint.</span></span> |
| <span data-ttu-id="3a633-230">statusReason</span><span class="sxs-lookup"><span data-stu-id="3a633-230">statusReason</span></span> |<span data-ttu-id="3a633-231">Volitelné</span><span class="sxs-lookup"><span data-stu-id="3a633-231">optional</span></span> |<span data-ttu-id="3a633-232">128 znaků dlouhý řetězec, který ukládá důvod stavu identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-232">A 128 character-long string that stores the reason for the device identity status.</span></span> <span data-ttu-id="3a633-233">Jsou povoleny všechny znaky UTF-8.</span><span class="sxs-lookup"><span data-stu-id="3a633-233">All UTF-8 characters are allowed.</span></span> |
| <span data-ttu-id="3a633-234">statusUpdateTime</span><span class="sxs-lookup"><span data-stu-id="3a633-234">statusUpdateTime</span></span> |<span data-ttu-id="3a633-235">jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-235">read-only</span></span> |<span data-ttu-id="3a633-236">Dočasné ukazatele zobrazuje datum a čas poslední aktualizace stavu.</span><span class="sxs-lookup"><span data-stu-id="3a633-236">A temporal indicator, showing the date and time of the last status update.</span></span> |
| <span data-ttu-id="3a633-237">Hodnota connectionState</span><span class="sxs-lookup"><span data-stu-id="3a633-237">connectionState</span></span> |<span data-ttu-id="3a633-238">jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-238">read-only</span></span> |<span data-ttu-id="3a633-239">Pole, která určuje stav připojení: buď **připojeno** nebo **odpojeno**.</span><span class="sxs-lookup"><span data-stu-id="3a633-239">A field indicating connection status: either **Connected** or **Disconnected**.</span></span> <span data-ttu-id="3a633-240">Toto pole představuje IoT Hub pohled na stav připojení zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-240">This field represents the IoT Hub view of the device connection status.</span></span> <span data-ttu-id="3a633-241">**Důležité**: Toto pole by měl použít pouze pro účely ladění nebo vývoj.</span><span class="sxs-lookup"><span data-stu-id="3a633-241">**Important**: This field should be used only for development/debugging purposes.</span></span> <span data-ttu-id="3a633-242">Stav připojení je aktualizovat jenom pro zařízení pomocí MQTT nebo AMQP.</span><span class="sxs-lookup"><span data-stu-id="3a633-242">The connection state is updated only for devices using MQTT or AMQP.</span></span> <span data-ttu-id="3a633-243">Navíc je založena na úrovni protokolu příkazy ping (příkazy ping MQTT nebo AMQP příkazy ping) a může mít maximální zpoždění jenom 5 minut.</span><span class="sxs-lookup"><span data-stu-id="3a633-243">Also, it is based on protocol-level pings (MQTT pings, or AMQP pings), and it can have a maximum delay of only 5 minutes.</span></span> <span data-ttu-id="3a633-244">Z těchto důvodů může být falešně pozitivních zjištění, například zařízení hlášené jako připojené, ale které jsou odpojené.</span><span class="sxs-lookup"><span data-stu-id="3a633-244">For these reasons, there can be false positives, such as devices reported as connected but that are disconnected.</span></span> |
| <span data-ttu-id="3a633-245">connectionStateUpdatedTime</span><span class="sxs-lookup"><span data-stu-id="3a633-245">connectionStateUpdatedTime</span></span> |<span data-ttu-id="3a633-246">jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-246">read-only</span></span> |<span data-ttu-id="3a633-247">Byl aktualizován na dočasné ukazatel zobrazuje datum a čas posledního stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="3a633-247">A temporal indicator, showing the date and last time the connection state was updated.</span></span> |
| <span data-ttu-id="3a633-248">lastActivityTime</span><span class="sxs-lookup"><span data-stu-id="3a633-248">lastActivityTime</span></span> |<span data-ttu-id="3a633-249">jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3a633-249">read-only</span></span> |<span data-ttu-id="3a633-250">Dočasné indikátor zobrazuje datum a čas poslední zařízení připojené, přijatých nebo odeslaných zprávu.</span><span class="sxs-lookup"><span data-stu-id="3a633-250">A temporal indicator, showing the date and last time the device connected, received, or sent a message.</span></span> |

> [!NOTE]
> <span data-ttu-id="3a633-251">Stav připojení může představovat pouze IoT Hub zobrazení stavu připojení.</span><span class="sxs-lookup"><span data-stu-id="3a633-251">Connection state can only represent the IoT Hub view of the status of the connection.</span></span> <span data-ttu-id="3a633-252">Aktualizace tohoto stavu může zpozdit, v závislosti na stavu sítě a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3a633-252">Updates to this state may be delayed, depending on network conditions and configurations.</span></span>

## <a name="additional-reference-material"></a><span data-ttu-id="3a633-253">Odkaz na další materiály</span><span class="sxs-lookup"><span data-stu-id="3a633-253">Additional reference material</span></span>

<span data-ttu-id="3a633-254">Další témata referenční příručka vývojáře IoT Hub patří:</span><span class="sxs-lookup"><span data-stu-id="3a633-254">Other reference topics in the IoT Hub developer guide include:</span></span>

* <span data-ttu-id="3a633-255">[Koncové body centra IoT] [ lnk-endpoints] popisuje různé koncových bodů, které každý IoT hub zpřístupní pro spuštění a management operace.</span><span class="sxs-lookup"><span data-stu-id="3a633-255">[IoT Hub endpoints][lnk-endpoints] describes the various endpoints that each IoT hub exposes for run-time and management operations.</span></span>
* <span data-ttu-id="3a633-256">[Omezování a kvóty] [ lnk-quotas] popisuje kvóty a omezení chování, které se vztahují ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="3a633-256">[Throttling and quotas][lnk-quotas] describes the quotas and throttling behaviors that apply to the IoT Hub service.</span></span>
* <span data-ttu-id="3a633-257">[Azure IoT zařízení a služby sady SDK] [ lnk-sdks] uvádí různé jazykové sady SDK můžete použít při vývoji aplikace zařízení a služby, které interakci s centrem IoT.</span><span class="sxs-lookup"><span data-stu-id="3a633-257">[Azure IoT device and service SDKs][lnk-sdks] lists the various language SDKs you can use when you develop both device and service apps that interact with IoT Hub.</span></span>
* <span data-ttu-id="3a633-258">[IoT Hub dotazovací jazyk] [ lnk-query] popisuje dotazovací jazyk, můžete použít k načtení informací ze služby IoT Hub o úlohách a dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="3a633-258">[IoT Hub query language][lnk-query] describes the query language you can use to retrieve information from IoT Hub about your device twins and jobs.</span></span>
* <span data-ttu-id="3a633-259">[Podpora IoT Hub MQTT] [ lnk-devguide-mqtt] poskytuje další informace o podpoře služby IoT Hub pro protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="3a633-259">[IoT Hub MQTT support][lnk-devguide-mqtt] provides more information about IoT Hub support for the MQTT protocol.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a633-260">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3a633-260">Next steps</span></span>

<span data-ttu-id="3a633-261">Nyní jste se naučili použití registru identit služby IoT Hub, může zajímat v následujících tématech Příručka vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="3a633-261">Now you have learned how to use the IoT Hub identity registry, you may be interested in the following IoT Hub developer guide topics:</span></span>

* <span data-ttu-id="3a633-262">[Řízení přístupu ke službě IoT Hub][lnk-devguide-security]</span><span class="sxs-lookup"><span data-stu-id="3a633-262">[Control access to IoT Hub][lnk-devguide-security]</span></span>
* <span data-ttu-id="3a633-263">[Pomocí dvojčata zařízení synchronizovat stavu a konfigurace][lnk-devguide-device-twins]</span><span class="sxs-lookup"><span data-stu-id="3a633-263">[Use device twins to synchronize state and configurations][lnk-devguide-device-twins]</span></span>
* <span data-ttu-id="3a633-264">[Volání metody přímé na zařízení][lnk-devguide-directmethods]</span><span class="sxs-lookup"><span data-stu-id="3a633-264">[Invoke a direct method on a device][lnk-devguide-directmethods]</span></span>
* <span data-ttu-id="3a633-265">[Plánování úloh na několika zařízeních][lnk-devguide-jobs]</span><span class="sxs-lookup"><span data-stu-id="3a633-265">[Schedule jobs on multiple devices][lnk-devguide-jobs]</span></span>

<span data-ttu-id="3a633-266">Pokud chcete vyzkoušet některé konceptů popsaných v tomto článku, může zajímat v následujícím kurzu IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="3a633-266">If you would like to try out some of the concepts described in this article, you may be interested in the following IoT Hub tutorial:</span></span>

* <span data-ttu-id="3a633-267">[Začínáme s Azure IoT Hub][lnk-getstarted-tutorial]</span><span class="sxs-lookup"><span data-stu-id="3a633-267">[Get started with Azure IoT Hub][lnk-getstarted-tutorial]</span></span>

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
