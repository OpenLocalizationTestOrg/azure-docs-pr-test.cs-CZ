---
title: "Pochopení formát zprávy Azure IoT Hub | Microsoft Docs"
description: "Příručka vývojáře - popisuje formát a očekávaný obsah zprávy IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: e6eafb1a0030b022da2b5d0b787e092f3067c99f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-and-read-iot-hub-messages"></a><span data-ttu-id="7e1d8-103">Vytvoření a čtení zpráv služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="7e1d8-103">Create and read IoT Hub messages</span></span>

<span data-ttu-id="7e1d8-104">Pro podporu bezproblémové vzájemná funkční spolupráce mezi protokoly, definuje IoT Hub běžný formát zprávy pro všechny protokoly zařízení přístupem.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-104">To support seamless interoperability across protocols, IoT Hub defines a common message format for all device-facing protocols.</span></span> <span data-ttu-id="7e1d8-105">Používá se tento formát zprávy pro [zařízení cloud] [ lnk-d2c] a [cloud zařízení] [ lnk-c2d] zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-105">This message format is used for both [device-to-cloud][lnk-d2c] and [cloud-to-device][lnk-c2d] messages.</span></span> <span data-ttu-id="7e1d8-106">[IoT Hub zpráva] [ lnk-messaging] se skládá z:</span><span class="sxs-lookup"><span data-stu-id="7e1d8-106">An [IoT Hub message][lnk-messaging] consists of:</span></span>

* <span data-ttu-id="7e1d8-107">Sadu *vlastnosti systému*.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-107">A set of *system properties*.</span></span> <span data-ttu-id="7e1d8-108">Vlastnosti, které Centrum IoT interpretuje nebo nastaví.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-108">Properties that IoT Hub interprets or sets.</span></span> <span data-ttu-id="7e1d8-109">Tato sada je předem určený.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-109">This set is predetermined.</span></span>
* <span data-ttu-id="7e1d8-110">Sadu *vlastnosti aplikace*.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-110">A set of *application properties*.</span></span> <span data-ttu-id="7e1d8-111">Slovník vlastnosti řetězce, které aplikace můžete definovat a přístup, aniž by museli deserializaci textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-111">A dictionary of string properties that the application can define and access, without needing to deserialize the message body.</span></span> <span data-ttu-id="7e1d8-112">IoT Hub nikdy upravuje tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-112">IoT Hub never modifies these properties.</span></span>
* <span data-ttu-id="7e1d8-113">Neprůhledné binární text.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-113">An opaque binary body.</span></span>

<span data-ttu-id="7e1d8-114">Názvy a hodnoty vlastností mohou obsahovat pouze alfanumerické znaky ASCII, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`\` při můžete:</span><span class="sxs-lookup"><span data-stu-id="7e1d8-114">Property names and values can only contain ASCII alphanumeric characters, plus ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`\` when you:</span></span>

* <span data-ttu-id="7e1d8-115">Odesílání zpráv typu zařízení cloud pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-115">Send device-to-cloud messages using the HTTP protocol.</span></span>
* <span data-ttu-id="7e1d8-116">Odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-116">Send cloud-to-device messages.</span></span>

<span data-ttu-id="7e1d8-117">Další informace o tom, jak kódování a dekódování zprávy odeslané pomocí různých protokolů najdete v tématu [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="7e1d8-117">For more information about how to encode and decode messages sent using different protocols, see [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="7e1d8-118">Následující tabulka uvádí sadu vlastností systému ve zprávách služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-118">The following table lists the set of system properties in IoT Hub messages.</span></span>

| <span data-ttu-id="7e1d8-119">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="7e1d8-119">Property</span></span> | <span data-ttu-id="7e1d8-120">Popis</span><span class="sxs-lookup"><span data-stu-id="7e1d8-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7e1d8-121">messageId</span><span class="sxs-lookup"><span data-stu-id="7e1d8-121">MessageId</span></span> |<span data-ttu-id="7e1d8-122">Identifikátor uživatele nastavit zprávě použité pro vzory požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-122">A user-settable identifier for the message used for request-reply patterns.</span></span> <span data-ttu-id="7e1d8-123">Formát: Malá a velká písmena řetězec (až 128 znaků.) z alfanumerických znaků ASCII 7bitového + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-123">Format: A case-sensitive string (up to 128 characters long) of ASCII 7-bit alphanumeric characters + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`.</span></span> |
| <span data-ttu-id="7e1d8-124">Pořadové číslo</span><span class="sxs-lookup"><span data-stu-id="7e1d8-124">Sequence number</span></span> |<span data-ttu-id="7e1d8-125">Číslo (jedinečný pro každé zařízení fronty) přiřazené službou IoT Hub každou zprávu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-125">A number (unique per device-queue) assigned by IoT Hub to each cloud-to-device message.</span></span> |
| <span data-ttu-id="7e1d8-126">až</span><span class="sxs-lookup"><span data-stu-id="7e1d8-126">To</span></span> |<span data-ttu-id="7e1d8-127">Cíl zadaný v [Cloud-zařízení] [ lnk-c2d] zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-127">A destination specified in [Cloud-to-Device][lnk-c2d] messages.</span></span> |
| <span data-ttu-id="7e1d8-128">ExpiryTimeUtc</span><span class="sxs-lookup"><span data-stu-id="7e1d8-128">ExpiryTimeUtc</span></span> |<span data-ttu-id="7e1d8-129">Datum a čas vypršení platnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-129">Date and time of message expiration.</span></span> |
| <span data-ttu-id="7e1d8-130">EnqueuedTime</span><span class="sxs-lookup"><span data-stu-id="7e1d8-130">EnqueuedTime</span></span> |<span data-ttu-id="7e1d8-131">Datum a čas [Cloud-zařízení] [ lnk-c2d] zpráva byla přijata službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-131">Date and time the [Cloud-to-Device][lnk-c2d] message was received by IoT Hub.</span></span> |
| <span data-ttu-id="7e1d8-132">CorrelationId</span><span class="sxs-lookup"><span data-stu-id="7e1d8-132">CorrelationId</span></span> |<span data-ttu-id="7e1d8-133">Vlastnosti řetězce v zprávu odpovědi, která obvykle obsahuje MessageId žádosti v vzory požadavku a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-133">A string property in a response message that typically contains the MessageId of the request, in request-reply patterns.</span></span> |
| <span data-ttu-id="7e1d8-134">ID uživatele</span><span class="sxs-lookup"><span data-stu-id="7e1d8-134">UserId</span></span> |<span data-ttu-id="7e1d8-135">ID používané k určení původu zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-135">An ID used to specify the origin of messages.</span></span> <span data-ttu-id="7e1d8-136">Zprávy generované IoT Hub, je nastavený na `{iot hub name}`.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-136">When messages are generated by IoT Hub, it is set to `{iot hub name}`.</span></span> |
| <span data-ttu-id="7e1d8-137">Potvrzení</span><span class="sxs-lookup"><span data-stu-id="7e1d8-137">Ack</span></span> |<span data-ttu-id="7e1d8-138">Generátor zpráva zpětnou vazbu.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-138">A feedback message generator.</span></span> <span data-ttu-id="7e1d8-139">Tato vlastnost je v zprávy typu cloud zařízení slouží k vyžádání IoT Hub, která generují zprávy zpětné vazby v důsledku spotřeby zprávy zařízení.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-139">This property is used in cloud-to-device messages to request IoT Hub to generate feedback messages as a result of the consumption of the message by the device.</span></span> <span data-ttu-id="7e1d8-140">Možné hodnoty: **žádné** (výchozí): je vygenerována žádná zpráva zpětnou vazbu, **kladné**: zobrazí zpráva zpětné vazby, pokud zpráva byla dokončena, **záporné**: přijímat zpráva zpětné vazby, pokud platnost zprávy (nebo bylo dosaženo maximální doručení počet) bez jeho dokončení zařízení, nebo **úplné**: kladné a záporné.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-140">Possible values: **none** (default): no feedback message is generated, **positive**: receive a feedback message if the message was completed, **negative**: receive a feedback message if the message expired (or maximum delivery count was reached) without being completed by the device, or **full**: both positive and negative.</span></span> <span data-ttu-id="7e1d8-141">Další informace najdete v tématu [zprávy zpětné vazby][lnk-feedback].</span><span class="sxs-lookup"><span data-stu-id="7e1d8-141">For more information, see [Message feedback][lnk-feedback].</span></span> |
| <span data-ttu-id="7e1d8-142">ConnectionDeviceId</span><span class="sxs-lookup"><span data-stu-id="7e1d8-142">ConnectionDeviceId</span></span> |<span data-ttu-id="7e1d8-143">ID nastavit službou IoT Hub na zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-143">An ID set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="7e1d8-144">Obsahuje **deviceId** zařízení, která zprávu odeslala.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-144">It contains the **deviceId** of the device that sent the message.</span></span> |
| <span data-ttu-id="7e1d8-145">ConnectionDeviceGenerationId</span><span class="sxs-lookup"><span data-stu-id="7e1d8-145">ConnectionDeviceGenerationId</span></span> |<span data-ttu-id="7e1d8-146">ID nastavit službou IoT Hub na zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-146">An ID set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="7e1d8-147">Obsahuje **generationId** (dle [vlastnosti identity zařízení][lnk-device-properties]) zařízení, která zprávu odeslala.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-147">It contains the **generationId** (as per [Device identity properties][lnk-device-properties]) of the device that sent the message.</span></span> |
| <span data-ttu-id="7e1d8-148">ConnectionAuthMethod</span><span class="sxs-lookup"><span data-stu-id="7e1d8-148">ConnectionAuthMethod</span></span> |<span data-ttu-id="7e1d8-149">Metoda ověřování, nastavit službou IoT Hub na zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-149">An authentication method set by IoT Hub on device-to-cloud messages.</span></span> <span data-ttu-id="7e1d8-150">Tato vlastnost obsahuje informace o metodu ověřování k ověření zařízení odesílá zprávy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-150">This property contains information about the authentication method used to authenticate the device sending the message.</span></span> <span data-ttu-id="7e1d8-151">Další informace najdete v tématu [zařízení do cloudu ochranu proti falšování][lnk-antispoofing].</span><span class="sxs-lookup"><span data-stu-id="7e1d8-151">For more information, see [Device to cloud anti-spoofing][lnk-antispoofing].</span></span> |

## <a name="message-size"></a><span data-ttu-id="7e1d8-152">Velikost zpráv</span><span class="sxs-lookup"><span data-stu-id="7e1d8-152">Message size</span></span>

<span data-ttu-id="7e1d8-153">IoT Hub měří velikost zprávy způsobem, bez ohledu na protokol, vzhledem k tomu jenom skutečné datové části.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-153">IoT Hub measures message size in a protocol-agnostic way, considering only the actual payload.</span></span> <span data-ttu-id="7e1d8-154">Velikost v bajtech se počítá jako součet z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="7e1d8-154">The size in bytes is calculated as the sum of the following:</span></span>

* <span data-ttu-id="7e1d8-155">Velikost textu v bajtech.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-155">The body size in bytes.</span></span>
* <span data-ttu-id="7e1d8-156">Velikost v bajtech všechny hodnoty vlastností zpráv systému.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-156">The size in bytes of all the values of the message system properties.</span></span>
* <span data-ttu-id="7e1d8-157">Velikost v bajtech všechny názvy vlastností uživatele a hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-157">The size in bytes of all user property names and values.</span></span>

<span data-ttu-id="7e1d8-158">Názvy a hodnoty vlastností jsou omezeny na znaky ASCII, takže délka řetězce rovná velikost v bajtech.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-158">Property names and values are limited to ASCII characters, so the length of the strings equals the size in bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e1d8-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7e1d8-159">Next steps</span></span>

<span data-ttu-id="7e1d8-160">Informace o omezení velikosti zpráv v centru IoT najdete v tématu [IoT Hub kvót a omezování][lnk-quotas].</span><span class="sxs-lookup"><span data-stu-id="7e1d8-160">For information about message size limits in IoT Hub, see [IoT Hub quotas and throttling][lnk-quotas].</span></span>

<span data-ttu-id="7e1d8-161">Zjistěte, jak vytvořit a čtení zpráv v různých programovacích jazyků IoT Hub, najdete v článku [Začínáme] [ lnk-get-started] kurzy.</span><span class="sxs-lookup"><span data-stu-id="7e1d8-161">To learn how to create and read IoT Hub messages in various programming languages, see the [Get started][lnk-get-started] tutorials.</span></span>

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
