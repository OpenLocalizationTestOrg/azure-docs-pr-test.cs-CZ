---
title: "Pochopení koncové body Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře – referenční informace o IoT Hub směřujících zařízení a služby přístupem koncových bodů."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 93ada731fe70cf7d294537241f8104c0b89940ed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="reference---iot-hub-endpoints"></a><span data-ttu-id="bd0fd-103">Odkaz – koncové body centra IoT</span><span class="sxs-lookup"><span data-stu-id="bd0fd-103">Reference - IoT Hub endpoints</span></span>

## <a name="iot-hub-names"></a><span data-ttu-id="bd0fd-104">Názvy IoT Hub</span><span class="sxs-lookup"><span data-stu-id="bd0fd-104">IoT Hub names</span></span>

<span data-ttu-id="bd0fd-105">Název služby IoT hub, který je hostitelem koncové body na portálu na můžete najít **přehled** okno.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-105">You can find the name of the IoT hub that hosts your endpoints in the portal on the **Overview** blade.</span></span> <span data-ttu-id="bd0fd-106">Ve výchozím nastavení, jako název DNS služby IoT hub vypadá takto: `{your iot hub name}.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-106">By default, the DNS name of an IoT hub looks like: `{your iot hub name}.azure-devices.net`.</span></span>

<span data-ttu-id="bd0fd-107">Azure DNS můžete použít k vytvoření vlastní název DNS pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-107">You can use Azure DNS to create a custom DNS name for your IoT hub.</span></span> <span data-ttu-id="bd0fd-108">Další informace najdete v tématu [Azure DNS používá k poskytování nastavení vlastní domény pro službu Azure](../dns/dns-custom-domain.md#azure-iot).</span><span class="sxs-lookup"><span data-stu-id="bd0fd-108">For more information, see [Use Azure DNS to provide custom domain settings for an Azure service](../dns/dns-custom-domain.md#azure-iot).</span></span>

## <a name="list-of-built-in-iot-hub-endpoints"></a><span data-ttu-id="bd0fd-109">Seznam předdefinovaných koncové body centra IoT</span><span class="sxs-lookup"><span data-stu-id="bd0fd-109">List of built-in IoT Hub endpoints</span></span>

<span data-ttu-id="bd0fd-110">Azure IoT Hub je víceklientské služby, který zveřejňuje jeho funkce pro různé účastníky.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-110">Azure IoT Hub is a multi-tenant service that exposes its functionality to various actors.</span></span> <span data-ttu-id="bd0fd-111">Následující diagram znázorňuje různé koncových bodů, aby IoT Hub zpřístupní.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-111">The following diagram shows the various endpoints that IoT Hub exposes.</span></span>

![Koncové body IoT Hubu][img-endpoints]

<span data-ttu-id="bd0fd-113">Následující seznam popisuje koncových bodů:</span><span class="sxs-lookup"><span data-stu-id="bd0fd-113">The following list describes the endpoints:</span></span>

* <span data-ttu-id="bd0fd-114">**Poskytovatel prostředků**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-114">**Resource provider**.</span></span> <span data-ttu-id="bd0fd-115">Zprostředkovatel prostředků služby IoT Hub zpřístupní [Azure Resource Manager] [ lnk-arm] rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-115">The IoT Hub resource provider exposes an [Azure Resource Manager][lnk-arm] interface.</span></span> <span data-ttu-id="bd0fd-116">Toto rozhraní umožňuje vlastníky předplatného Azure k vytváření a odstraňování centra IoT a k aktualizaci vlastností centra IoT.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-116">This interface enables Azure subscription owners to create and delete IoT hubs, and to update IoT hub properties.</span></span> <span data-ttu-id="bd0fd-117">Vlastnosti služby IoT Hub řídí [zásad zabezpečení na úrovni centra][lnk-accesscontrol], na rozdíl od řízení přístupu na úrovni zařízení a funkční nastavení pro zasílání zpráv typu cloud zařízení a zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-117">IoT Hub properties govern [hub-level security policies][lnk-accesscontrol], as opposed to device-level access control, and functional options for cloud-to-device and device-to-cloud messaging.</span></span> <span data-ttu-id="bd0fd-118">Zprostředkovatel prostředků služby IoT Hub můžete také [exportovat identit zařízení][lnk-importexport].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-118">The IoT Hub resource provider also enables you to [export device identities][lnk-importexport].</span></span>
* <span data-ttu-id="bd0fd-119">**Správa identit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-119">**Device identity management**.</span></span> <span data-ttu-id="bd0fd-120">Každé centrum IoT zpřístupňuje řadu koncové body HTTP REST pro správu identit zařízení (vytvořit, získat, aktualizovat a odstranit).</span><span class="sxs-lookup"><span data-stu-id="bd0fd-120">Each IoT hub exposes a set of HTTP REST endpoints to manage device identities (create, retrieve, update, and delete).</span></span> <span data-ttu-id="bd0fd-121">[Identit zařízení] [ lnk-device-identities] se používají pro řízení přístupu a ověřování a zařízení.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-121">[Device identities][lnk-device-identities] are used for device authentication and access control.</span></span>
* <span data-ttu-id="bd0fd-122">**Správa zařízení twin**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-122">**Device twin management**.</span></span> <span data-ttu-id="bd0fd-123">Každé centrum IoT zpřístupňuje řadu koncového bodu služby směřujících HTTP REST dotazů a aktualizace [dvojčata zařízení] [ lnk-twins] (aktualizace značky a vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="bd0fd-123">Each IoT hub exposes a set of service-facing HTTP REST endpoint to query and update [device twins][lnk-twins] (update tags and properties).</span></span>
* <span data-ttu-id="bd0fd-124">**Úlohy správy**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-124">**Jobs management**.</span></span> <span data-ttu-id="bd0fd-125">Každý IoT hub zpřístupní koncový bod HTTP REST služby směřujících k dotazování a správu sadu [úlohy][lnk-jobs].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-125">Each IoT hub exposes a set of service-facing HTTP REST endpoint to query and manage [jobs][lnk-jobs].</span></span>
* <span data-ttu-id="bd0fd-126">**Koncové body zařízení**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-126">**Device endpoints**.</span></span> <span data-ttu-id="bd0fd-127">Pro každé zařízení v registru identit služby IoT Hub zpřístupní sada koncových bodů:</span><span class="sxs-lookup"><span data-stu-id="bd0fd-127">For each device in the identity registry, IoT Hub exposes a set of endpoints:</span></span>

  * <span data-ttu-id="bd0fd-128">*Odesílání zpráv typu zařízení cloud*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-128">*Send device-to-cloud messages*.</span></span> <span data-ttu-id="bd0fd-129">Zařízení používá tento koncový bod pro [odesílání zpráv typu zařízení cloud][lnk-d2c].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-129">A device uses this endpoint to [send device-to-cloud messages][lnk-d2c].</span></span>
  * <span data-ttu-id="bd0fd-130">*Příjem zpráv typu cloud zařízení*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-130">*Receive cloud-to-device messages*.</span></span> <span data-ttu-id="bd0fd-131">Zařízení používá tento koncový bod pro příjem cílové [zprávy typu cloud zařízení][lnk-c2d].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-131">A device uses this endpoint to receive targeted [cloud-to-device messages][lnk-c2d].</span></span>
  * <span data-ttu-id="bd0fd-132">*Zahájení nahrávání souborů*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-132">*Initiate file uploads*.</span></span> <span data-ttu-id="bd0fd-133">Zařízení používá tento koncový bod pro příjem identifikátor URI SAS úložiště Azure ze služby IoT Hub na [nahrát soubor][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-133">A device uses this endpoint to receive an Azure Storage SAS URI from IoT Hub to [upload a file][lnk-upload].</span></span>
  * <span data-ttu-id="bd0fd-134">*Načíst a aktualizovat vlastnosti twin zařízení*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-134">*Retrieve and update device twin properties*.</span></span> <span data-ttu-id="bd0fd-135">Zařízení používá tento koncový bod pro přístup k jeho [dvojče zařízení][lnk-twins]na vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-135">A device uses this endpoint to access its [device twin][lnk-twins]'s properties.</span></span>
  * <span data-ttu-id="bd0fd-136">*Přijímat požadavky přímá metoda*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-136">*Receive direct method requests*.</span></span> <span data-ttu-id="bd0fd-137">Zařízení používá k naslouchání pro tento koncový bod [přímá metoda][lnk-methods]na požadavky.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-137">A device uses this endpoint to listen for [direct method][lnk-methods]'s requests.</span></span>

    <span data-ttu-id="bd0fd-138">Tyto koncové body jsou vystavené pomocí [protokoly MQTT v3.1.1][lnk-mqtt], HTTP 1.1 a [protokolu AMQP 1.0] [ lnk-amqp] protokoly.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-138">These endpoints are exposed using [MQTT v3.1.1][lnk-mqtt], HTTP 1.1, and [AMQP 1.0][lnk-amqp] protocols.</span></span> <span data-ttu-id="bd0fd-139">Je také k dispozici prostřednictvím protokolu AMQP [Websocket] [ lnk-websockets] na portu 443.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-139">AMQP is also available over [WebSockets][lnk-websockets] on port 443.</span></span>

    <span data-ttu-id="bd0fd-140">Koncové body pro metody a dvojčata zařízení jsou k dispozici pouze při použití [protokoly MQTT v3.1.1] [ lnk-mqtt] protokolu.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-140">The device twins and methods endpoints are available only when you use the [MQTT v3.1.1][lnk-mqtt] protocol.</span></span>

* <span data-ttu-id="bd0fd-141">**Koncové body služby**.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-141">**Service endpoints**.</span></span> <span data-ttu-id="bd0fd-142">Každé centrum IoT zpřístupňuje řadu koncové body pro váš back-end řešení pro komunikaci ve vašich zařízeních.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-142">Each IoT hub exposes a set of endpoints  for your solution back end to communicate with your devices.</span></span> <span data-ttu-id="bd0fd-143">S jednou výjimkou těchto koncových bodů jsou přístupné pouze pomocí [AMQP] [ lnk-amqp] protokolu.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-143">With one exception, these endpoints are only exposed using the [AMQP][lnk-amqp] protocol.</span></span> <span data-ttu-id="bd0fd-144">Koncový bod volání metoda vystavený přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-144">The method invocation endpoint is exposed over the HTTP protocol.</span></span>
  
  * <span data-ttu-id="bd0fd-145">*Příjem zpráv typu zařízení cloud*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-145">*Receive device-to-cloud messages*.</span></span> <span data-ttu-id="bd0fd-146">Tento koncový bod je kompatibilní s [Azure Event Hubs][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-146">This endpoint is compatible with [Azure Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="bd0fd-147">Back-end služby můžete použít ke čtení [zpráv typu zařízení cloud] [ lnk-d2c] poslal zařízení.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-147">A back-end service can use it to read the [device-to-cloud messages][lnk-d2c] sent by your devices.</span></span> <span data-ttu-id="bd0fd-148">Můžete vytvořit vlastní koncové body ve službě IoT hub kromě tento předdefinovaný koncový bod.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-148">You can create custom endpoints on your IoT hub in addition to this built-in endpoint.</span></span>
  * <span data-ttu-id="bd0fd-149">*Zprávy typu cloud zařízení odesílat a přijímat potvrzování doručení*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-149">*Send cloud-to-device messages and receive delivery acknowledgments*.</span></span> <span data-ttu-id="bd0fd-150">Tyto koncové body povolit back-end vašeho řešení odeslat spolehlivé [zprávy typu cloud zařízení][lnk-c2d]a získat odpovídající doručení nebo vypršení platnosti potvrzování.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-150">These endpoints enable your solution back end to send reliable [cloud-to-device messages][lnk-c2d], and to receive the corresponding delivery or expiration acknowledgments.</span></span>
  * <span data-ttu-id="bd0fd-151">*Přijímat oznámení souboru*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-151">*Receive file notifications*.</span></span> <span data-ttu-id="bd0fd-152">Tento koncový bod zasílání zpráv umožňuje dostávat oznámení o, když vaše zařízení úspěšně odeslat soubor.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-152">This messaging endpoint allows you to receive notifications of when your devices successfully upload a file.</span></span> 
  * <span data-ttu-id="bd0fd-153">*Přímé volání metody*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-153">*Direct method invocation*.</span></span> <span data-ttu-id="bd0fd-154">Tento koncový bod umožňuje službě back-end pro vyvolání [přímá metoda] [ lnk-methods] na zařízení.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-154">This endpoint allows a back-end service to invoke a [direct method][lnk-methods] on a device.</span></span>
  * <span data-ttu-id="bd0fd-155">*Zobrazí operace sledování událostí*.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-155">*Receive operations monitoring events*.</span></span> <span data-ttu-id="bd0fd-156">Tento koncový bod umožňuje sledování událostí, pokud je nakonfigurované služby IoT hub pro vydávání je operace příjmu.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-156">This endpoint allows you to receive operations monitoring events if your IoT hub has been configured to emit them.</span></span> <span data-ttu-id="bd0fd-157">Další informace najdete v tématu [IoT Hub operations monitorování][lnk-operations-mon].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-157">For more information, see [IoT Hub operations monitoring][lnk-operations-mon].</span></span>

<span data-ttu-id="bd0fd-158">[SDK služby Azure IoT] [ lnk-sdks] článek popisuje různé způsoby, kterými pro přístup k těchto koncových bodů.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-158">The [Azure IoT SDKs][lnk-sdks] article describes the various ways to access these endpoints.</span></span>

<span data-ttu-id="bd0fd-159">Použít všechny koncové body centra IoT [TLS] [ lnk-tls] protokol a žádný koncový bod je někdy zveřejněné na bez šifrování nezabezpečené kanály.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-159">All IoT Hub endpoints use the [TLS][lnk-tls] protocol, and no endpoint is ever exposed on unencrypted/unsecured channels.</span></span>

## <a name="custom-endpoints"></a><span data-ttu-id="bd0fd-160">Vlastní koncové body</span><span class="sxs-lookup"><span data-stu-id="bd0fd-160">Custom endpoints</span></span>

<span data-ttu-id="bd0fd-161">Do služby IoT hub tak, aby fungoval jako koncové body pro směrování zpráv můžete propojit stávající služby Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-161">You can link existing Azure services in your subscription to your IoT hub to act as endpoints for message routing.</span></span> <span data-ttu-id="bd0fd-162">Tyto koncové body fungovat jako koncové body služby a jsou použity jako jímky pro směrování zpráv.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-162">These endpoints act as service endpoints and are used as sinks for message routes.</span></span> <span data-ttu-id="bd0fd-163">Zařízení nemůže zapisovat přímo do další koncové body.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-163">Devices cannot write directly to the additional endpoints.</span></span> <span data-ttu-id="bd0fd-164">Další informace o směrování zpráv, naleznete v příspěvku Příručka vývojáře na [odesílání a přijímání zpráv službou IoT hub][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-164">To learn more about message routes, see the developer guide entry on [sending and receiving messages with IoT hub][lnk-devguide-messaging].</span></span>

<span data-ttu-id="bd0fd-165">Následující služby Azure IoT Hub aktuálně podporuje jako další koncové body:</span><span class="sxs-lookup"><span data-stu-id="bd0fd-165">IoT Hub currently supports the following Azure services as additional endpoints:</span></span>

* <span data-ttu-id="bd0fd-166">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="bd0fd-166">Event Hubs</span></span>
* <span data-ttu-id="bd0fd-167">Fronty služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="bd0fd-167">Service Bus Queues</span></span>
* <span data-ttu-id="bd0fd-168">Témata služby Service Bus</span><span class="sxs-lookup"><span data-stu-id="bd0fd-168">Service Bus Topics</span></span>

<span data-ttu-id="bd0fd-169">IoT Hub potřebuje přístup k zápisu do těchto koncových bodů služby pro směrování zpráv pro práci.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-169">IoT Hub needs write access to these service endpoints for message routing to work.</span></span> <span data-ttu-id="bd0fd-170">Pokud nakonfigurujete koncové body prostřednictvím portálu Azure, jsou pro vás přidat potřebná oprávnění.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-170">If you configure your endpoints through the Azure portal, the necessary permissions are added for you.</span></span> <span data-ttu-id="bd0fd-171">Zajistěte, aby že konfigurace vaší služby pro podporu očekávané propustnost.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-171">Make sure you configure your services to support the expected throughput.</span></span> <span data-ttu-id="bd0fd-172">Při první konfiguraci řešení IoT, můžete sledovat další koncové body a průběhu skutečné zátěže.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-172">When you first configure your IoT solution, you may need to monitor your additional endpoints and make any necessary adjustments for the actual load.</span></span>

<span data-ttu-id="bd0fd-173">Pokud zpráva odpovídá více tras všechny přejděte na stejný koncový bod, IoT Hub zajišťuje zprávy do tohoto koncového bodu pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-173">If a message matches multiple routes that all point to the same endpoint, IoT Hub delivers message to that endpoint only once.</span></span> <span data-ttu-id="bd0fd-174">Proto není potřeba konfigurace odstranění duplicitních dat na fronty sběrnice nebo téma.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-174">Therefore, you do not need to configure deduplication on your Service Bus queue or topic.</span></span> <span data-ttu-id="bd0fd-175">Do oddílů front oddílu spřažení zaručuje, řazení zpráv.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-175">In partitioned queues, partition affinity guarantees message ordering.</span></span>

> [!NOTE]
> <span data-ttu-id="bd0fd-176">Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-176">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="bd0fd-177">Pokud některá z těchto možností jsou povolené, koncový bod se zobrazí jako **Unreachable** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-177">If either of those options are enabled, the endpoint appears as **Unreachable** in the Azure portal.</span></span>

<span data-ttu-id="bd0fd-178">Omezení pro počet koncových bodů můžete přidat, naleznete v části [kvóty a omezení][lnk-devguide-quotas].</span><span class="sxs-lookup"><span data-stu-id="bd0fd-178">For the limits on the number of endpoints you can add, see [Quotas and throttling][lnk-devguide-quotas].</span></span>

## <a name="field-gateways"></a><span data-ttu-id="bd0fd-179">Pole brány</span><span class="sxs-lookup"><span data-stu-id="bd0fd-179">Field gateways</span></span>

<span data-ttu-id="bd0fd-180">V řešení IoT *brána pole* nachází mezi zařízeními a vaše koncové body centra IoT.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-180">In an IoT solution, a *field gateway* sits between your devices and your IoT Hub endpoints.</span></span> <span data-ttu-id="bd0fd-181">Je obvykle nachází blízko zařízení.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-181">It is typically located close to your devices.</span></span> <span data-ttu-id="bd0fd-182">Vaše zařízení komunikují přímo s brány pole pomocí protokol podporovaný zařízení.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-182">Your devices communicate directly with the field gateway by using a protocol supported by the devices.</span></span> <span data-ttu-id="bd0fd-183">Brána pole se připojí k koncový bod služby IoT Hub pomocí protokolu, který je podporován službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-183">The field gateway connects to an IoT Hub endpoint using a protocol that is supported by IoT Hub.</span></span> <span data-ttu-id="bd0fd-184">Brána pole může být vyhrazené hardwarové zařízení nebo počítač úsporného režimu softwarem vlastní brány.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-184">A field gateway might be a dedicated hardware device or a low-power computer running custom gateway software.</span></span>

<span data-ttu-id="bd0fd-185">Můžete použít [Azure IoT Edge] [ lnk-iot-edge] implementovat brána pole.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-185">You can use [Azure IoT Edge][lnk-iot-edge] to implement a field gateway.</span></span> <span data-ttu-id="bd0fd-186">Okraj IoT nabízí funkce, jako je multiplexní komunikaci od více zařízení do stejné připojení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bd0fd-186">IoT Edge offers functionality such as multiplexing communications from multiple devices onto the same IoT Hub connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd0fd-187">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bd0fd-187">Next steps</span></span>

<span data-ttu-id="bd0fd-188">Zahrnout další referenční témata v této příručce pro vývojáře IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="bd0fd-188">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="bd0fd-189">[IoT Hub dotazovacího jazyka pro dvojčata zařízení, úlohy a směrování zpráv][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="bd0fd-189">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="bd0fd-190">[Kvóty a omezení][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="bd0fd-190">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="bd0fd-191">[Podpora MQTT centra IoT][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="bd0fd-191">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
