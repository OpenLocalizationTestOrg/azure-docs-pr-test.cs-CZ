---
title: "Správa zařízení IoT pomocí Průzkumníka iothub aaaAzure | Microsoft Docs"
description: "Nástroj hello iothub-explorer rozhraní příkazového řádku pro správu zařízení Azure IoT Hub, poskytuje funkci hello přímé metody a možnosti správy hello Twin požadované vlastnosti."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Správa zařízení Azure iot, správou zařízení azure iot hub, iot správy zařízení, správou zařízení iot hub"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="8c5c0-104">Použití iothub Průzkumníka pro správu zařízení Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="8c5c0-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="8c5c0-106">[iothub-explorer](https://github.com/azure/iothub-explorer) je nástroj rozhraní příkazového řádku, který jste spustili identit zařízení toomanage počítač na hostitele v registru centra IoT.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer toomanage device identities in your IoT hub registry.</span></span> <span data-ttu-id="8c5c0-107">Se dodává s možností správy, které můžete použít tooperform různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-107">It comes with management options that you can use tooperform various tasks.</span></span>

| <span data-ttu-id="8c5c0-108">Možnost správy</span><span class="sxs-lookup"><span data-stu-id="8c5c0-108">Management option</span></span>          | <span data-ttu-id="8c5c0-109">Úkol</span><span class="sxs-lookup"><span data-stu-id="8c5c0-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8c5c0-110">Přímé metody</span><span class="sxs-lookup"><span data-stu-id="8c5c0-110">Direct methods</span></span>             | <span data-ttu-id="8c5c0-111">Nastavit zařízení fungovat jako je například spuštění nebo zastavení odesílání zpráv nebo restartování zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-111">Make a device act such as starting or stopping sending messages or rebooting hello device.</span></span>                                        |
| <span data-ttu-id="8c5c0-112">Vlastnosti Twin potřeby</span><span class="sxs-lookup"><span data-stu-id="8c5c0-112">Twin desired properties</span></span>    | <span data-ttu-id="8c5c0-113">Umístit zařízení do určité stavy, jako je třeba nastavení DIODU toogreen nebo nastavení telemetrie hello poslat too30 minut určený v intervalu.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-113">Put a device into certain states, such as setting an LED toogreen or setting hello telemetry send interval too30 minutes.</span></span>         |
| <span data-ttu-id="8c5c0-114">Twin hlášené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="8c5c0-114">Twin reported properties</span></span>   | <span data-ttu-id="8c5c0-115">Získat hello hlášené stav zařízení.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-115">Get hello reported state of a device.</span></span> <span data-ttu-id="8c5c0-116">Například hello zařízení hlásí hello je nyní blikat Indikátor.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-116">For example, hello device reports hello LED is blinking now.</span></span>                                    |
| <span data-ttu-id="8c5c0-117">Značky Twin</span><span class="sxs-lookup"><span data-stu-id="8c5c0-117">Twin tags</span></span>                  | <span data-ttu-id="8c5c0-118">Metadata specifická pro zařízení ukládat v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-118">Store device-specific metadata in hello cloud.</span></span> <span data-ttu-id="8c5c0-119">Například hello počítače prodejních umístění nasazení.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-119">For example, hello deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="8c5c0-120">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="8c5c0-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="8c5c0-121">Odešlete oznámení tooa zařízení.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-121">Send notifications tooa device.</span></span> <span data-ttu-id="8c5c0-122">Například "je velmi pravděpodobně toorain ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-122">For example, "It is very likely toorain today.</span></span> <span data-ttu-id="8c5c0-123">Nezapomeňte toobring zastřešující."</span><span class="sxs-lookup"><span data-stu-id="8c5c0-123">Don't forget toobring an umbrella."</span></span>              |
| <span data-ttu-id="8c5c0-124">Dotazy twin zařízení</span><span class="sxs-lookup"><span data-stu-id="8c5c0-124">Device twin queries</span></span>        | <span data-ttu-id="8c5c0-125">Dotaz všechny tooretrieve dvojčata zařízení ty s libovolné podmínky, jako je identifikace hello zařízení, které jsou k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-125">Query all device twins tooretrieve those with arbitrary conditions, such as identifying hello devices that are available for use.</span></span> |

<span data-ttu-id="8c5c0-126">Podrobnější vysvětlení na hello rozdíly a pokyny k použití těchto možností najdete v článku [pokyny komunikace zařízení cloud](iot-hub-devguide-d2c-guidance.md) a [Cloud zařízení komunikace pokyny](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="8c5c0-126">For more detailed explanation on hello differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8c5c0-127">Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="8c5c0-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="8c5c0-128">IoT Hub trvá dvojče zařízení pro každé zařízení, která se připojuje tooit.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-128">IoT Hub persists a device twin for each device that connects tooit.</span></span> <span data-ttu-id="8c5c0-129">Další informace o dvojčata zařízení najdete v tématu [začít pracovat s dvojčata zařízení](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="8c5c0-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="8c5c0-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="8c5c0-130">What you learn</span></span>

<span data-ttu-id="8c5c0-131">Zjistíte pomocí různých možností správy na vývojovém počítači iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="8c5c0-132">Co dělat</span><span class="sxs-lookup"><span data-stu-id="8c5c0-132">What you do</span></span>

<span data-ttu-id="8c5c0-133">Spusťte Průzkumníka iothub s různé možnosti správy.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="8c5c0-134">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="8c5c0-134">What you need</span></span>

- <span data-ttu-id="8c5c0-135">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="8c5c0-136">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="8c5c0-137">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="8c5c0-138">Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-138">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="8c5c0-139">Zajistěte, aby že vaše zařízení se systémem s hello klientská aplikace během tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-139">Make sure your device is running with hello client application during this tutorial.</span></span>
- <span data-ttu-id="8c5c0-140">iothub-explorer [nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer) na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-tooyour-iot-hub"></a><span data-ttu-id="8c5c0-141">Připojit tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="8c5c0-141">Connect tooyour IoT hub</span></span>

<span data-ttu-id="8c5c0-142">Připojte tooyour IoT hub spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-142">Connect tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="8c5c0-143">Pomocí Průzkumníka iothub přímé metody</span><span class="sxs-lookup"><span data-stu-id="8c5c0-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="8c5c0-144">Vyvolání hello `start` metoda hello zařízení aplikaci toosend zprávy tooyour IoT hub spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-144">Invoke hello `start` method in hello device app toosend messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="8c5c0-145">Vyvolání hello `stop` metoda v hello zařízení aplikaci toostop odesílání zpráv tooyour IoT hub spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-145">Invoke hello `stop` method in hello device app toostop sending messages tooyour IoT hub by running hello following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="8c5c0-146">Pomocí Průzkumníka iothub požadované vlastnosti pro dvojici</span><span class="sxs-lookup"><span data-stu-id="8c5c0-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="8c5c0-147">Nastavení intervalu požadovanou vlastnost = 3000 spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-147">Set a desired property interval = 3000 by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="8c5c0-148">Tuto vlastnost lze číst zařízením.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="8c5c0-149">Použití iothub Průzkumníka s vlastnostmi hlášené pro dvojici</span><span class="sxs-lookup"><span data-stu-id="8c5c0-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="8c5c0-150">Získat hello hlášené vlastnosti zařízení hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-150">Get hello reported properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="8c5c0-151">Jedna z vlastností hello je $metadata. $lastUpdated které ukazuje hello čas poslední toto zařízení odeslání nebo přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-151">One of hello properties is $metadata.$lastUpdated which shows hello last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="8c5c0-152">Použití iothub Průzkumníka pomocí značek pro dvojici</span><span class="sxs-lookup"><span data-stu-id="8c5c0-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="8c5c0-153">Zobrazení značek hello a vlastnosti zařízení hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-153">Display hello tags and properties of hello device by running hello following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="8c5c0-154">Přidání role pole = teploty a vlhkosti toohello zařízení tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-154">Add a field role = temperature&humidity toohello device by running hello following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="8c5c0-155">Pomocí Průzkumníka iothub zprávy typu Cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="8c5c0-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="8c5c0-156">Odeslat zařízení toohello zprávu "Hello, World" tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-156">Send a "Hello World" message toohello device by running hello following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="8c5c0-157">V tématu [použijte toosend iothub-explorer a příjem zpráv mezi zařízením a IoT Hub](iot-hub-explorer-cloud-device-messaging.md) pro skutečné scénář použití tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-157">See [Use iothub-explorer toosend and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="8c5c0-158">Použití iothub Průzkumníka s dotazy dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="8c5c0-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="8c5c0-159">Dotaz na zařízení s značkou role = 'teploty a vlhkosti' spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-159">Query devices with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="8c5c0-160">Dotaz na všechna zařízení kromě těch, které se značkou role = 'teploty a vlhkosti' spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8c5c0-160">Query all devices except those with a tag of role = 'temperature&humidity' by running hello following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="8c5c0-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c5c0-161">Next steps</span></span>

<span data-ttu-id="8c5c0-162">Když jste se naučili jak toouse iothub-explorer se různé možnosti správy.</span><span class="sxs-lookup"><span data-stu-id="8c5c0-162">You've learned how toouse iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
