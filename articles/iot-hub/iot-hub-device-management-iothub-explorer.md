---
title: "Správa zařízení Azure IoT pomocí Průzkumníka iothub | Microsoft Docs"
description: "Použijte nástroj iothub Průzkumník rozhraní příkazového řádku pro správu zařízení Azure IoT Hub, poskytuje funkci přímé metody a možnosti správy Twin požadované vlastnosti."
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
ms.openlocfilehash: 5b7a5057bdfb5920fbb5759bed1f5561cfa1d7e0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a><span data-ttu-id="54cfc-104">Použití iothub Průzkumníka pro správu zařízení Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="54cfc-104">Use iothub-explorer for Azure IoT Hub device management</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="54cfc-106">[iothub-explorer](https://github.com/azure/iothub-explorer) je nástroj příkazového řádku, který spustíte na hostiteli pro správu identit zařízení v registru centra IoT.</span><span class="sxs-lookup"><span data-stu-id="54cfc-106">[iothub-explorer](https://github.com/azure/iothub-explorer) is a CLI tool that you run on a host computer to manage device identities in your IoT hub registry.</span></span> <span data-ttu-id="54cfc-107">Součástí možnosti správy, které můžete provádět různé úlohy.</span><span class="sxs-lookup"><span data-stu-id="54cfc-107">It comes with management options that you can use to perform various tasks.</span></span>

| <span data-ttu-id="54cfc-108">Možnost správy</span><span class="sxs-lookup"><span data-stu-id="54cfc-108">Management option</span></span>          | <span data-ttu-id="54cfc-109">Úkol</span><span class="sxs-lookup"><span data-stu-id="54cfc-109">Task</span></span>                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="54cfc-110">Přímé metody</span><span class="sxs-lookup"><span data-stu-id="54cfc-110">Direct methods</span></span>             | <span data-ttu-id="54cfc-111">Nastavit zařízení fungovat jako je například spuštění nebo zastavení odesílání zpráv nebo restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="54cfc-111">Make a device act such as starting or stopping sending messages or rebooting the device.</span></span>                                        |
| <span data-ttu-id="54cfc-112">Vlastnosti Twin potřeby</span><span class="sxs-lookup"><span data-stu-id="54cfc-112">Twin desired properties</span></span>    | <span data-ttu-id="54cfc-113">Umístěte zařízení do určité stavy, například nastavit DIODU na zelenou nebo nastavení intervalu odeslání telemetrie do 30 minut.</span><span class="sxs-lookup"><span data-stu-id="54cfc-113">Put a device into certain states, such as setting an LED to green or setting the telemetry send interval to 30 minutes.</span></span>         |
| <span data-ttu-id="54cfc-114">Twin hlášené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="54cfc-114">Twin reported properties</span></span>   | <span data-ttu-id="54cfc-115">Zjištění hlášené stavu zařízení.</span><span class="sxs-lookup"><span data-stu-id="54cfc-115">Get the reported state of a device.</span></span> <span data-ttu-id="54cfc-116">Například zařízení ohlásí, že nyní bliká Indikátor.</span><span class="sxs-lookup"><span data-stu-id="54cfc-116">For example, the device reports the LED is blinking now.</span></span>                                    |
| <span data-ttu-id="54cfc-117">Značky Twin</span><span class="sxs-lookup"><span data-stu-id="54cfc-117">Twin tags</span></span>                  | <span data-ttu-id="54cfc-118">Metadata specifická pro zařízení ukládat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="54cfc-118">Store device-specific metadata in the cloud.</span></span> <span data-ttu-id="54cfc-119">Například nasazení umístění prodejních počítače.</span><span class="sxs-lookup"><span data-stu-id="54cfc-119">For example, the deployment location of a vending machine.</span></span>                         |
| <span data-ttu-id="54cfc-120">Zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="54cfc-120">Cloud-to-device messages</span></span>   | <span data-ttu-id="54cfc-121">Odesílání oznámení do zařízení.</span><span class="sxs-lookup"><span data-stu-id="54cfc-121">Send notifications to a device.</span></span> <span data-ttu-id="54cfc-122">Například "je velmi pravděpodobně déšť ještě dnes.</span><span class="sxs-lookup"><span data-stu-id="54cfc-122">For example, "It is very likely to rain today.</span></span> <span data-ttu-id="54cfc-123">Nezapomeňte uvést zastřešující."</span><span class="sxs-lookup"><span data-stu-id="54cfc-123">Don't forget to bring an umbrella."</span></span>              |
| <span data-ttu-id="54cfc-124">Dotazy twin zařízení</span><span class="sxs-lookup"><span data-stu-id="54cfc-124">Device twin queries</span></span>        | <span data-ttu-id="54cfc-125">Dotaz na všechny dvojčata zařízení k načtení ty s libovolné podmínky, jako je identifikace zařízení, která jsou k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="54cfc-125">Query all device twins to retrieve those with arbitrary conditions, such as identifying the devices that are available for use.</span></span> |

<span data-ttu-id="54cfc-126">Podrobnější vysvětlení na rozdíly a pokyny k použití těchto možností najdete v článku [pokyny komunikace zařízení cloud](iot-hub-devguide-d2c-guidance.md) a [Cloud zařízení komunikace pokyny](iot-hub-devguide-c2d-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="54cfc-126">For more detailed explanation on the differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).</span></span>

> [!NOTE]
> <span data-ttu-id="54cfc-127">Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="54cfc-127">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="54cfc-128">IoT Hub trvá dvojče zařízení pro každé zařízení, která se k němu připojuje.</span><span class="sxs-lookup"><span data-stu-id="54cfc-128">IoT Hub persists a device twin for each device that connects to it.</span></span> <span data-ttu-id="54cfc-129">Další informace o dvojčata zařízení najdete v tématu [začít pracovat s dvojčata zařízení](iot-hub-node-node-twin-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="54cfc-129">For more information about device twins, see [Get started with device twins](iot-hub-node-node-twin-getstarted.md).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="54cfc-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="54cfc-130">What you learn</span></span>

<span data-ttu-id="54cfc-131">Zjistíte pomocí různých možností správy na vývojovém počítači iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="54cfc-131">You learn using iothub-explorer with various management options on your development machine.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="54cfc-132">Co dělat</span><span class="sxs-lookup"><span data-stu-id="54cfc-132">What you do</span></span>

<span data-ttu-id="54cfc-133">Spusťte Průzkumníka iothub s různé možnosti správy.</span><span class="sxs-lookup"><span data-stu-id="54cfc-133">Run iothub-explorer with various management options.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54cfc-134">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="54cfc-134">What you need</span></span>

- <span data-ttu-id="54cfc-135">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="54cfc-135">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="54cfc-136">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="54cfc-136">An active Azure subscription.</span></span>
  - <span data-ttu-id="54cfc-137">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cfc-137">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="54cfc-138">Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cfc-138">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="54cfc-139">Zajistěte, aby že vaše zařízení se systémem s klienta aplikace v průběhu tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="54cfc-139">Make sure your device is running with the client application during this tutorial.</span></span>
- <span data-ttu-id="54cfc-140">iothub-explorer [nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer) na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="54cfc-140">iothub-explorer, [Install iothub-explorer](https://github.com/azure/iothub-explorer) on your development machine.</span></span>

## <a name="connect-to-your-iot-hub"></a><span data-ttu-id="54cfc-141">Připojení ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54cfc-141">Connect to your IoT hub</span></span>

<span data-ttu-id="54cfc-142">Připojení do služby IoT hub spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-142">Connect to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a><span data-ttu-id="54cfc-143">Pomocí Průzkumníka iothub přímé metody</span><span class="sxs-lookup"><span data-stu-id="54cfc-143">Use iothub-explorer with direct methods</span></span>

<span data-ttu-id="54cfc-144">Vyvolání `start` metoda v aplikaci zařízení k odesílání zpráv do služby IoT hub spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-144">Invoke the `start` method in the device app to send messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> start
```

<span data-ttu-id="54cfc-145">Vyvolání `stop` metoda v aplikaci zařízení zastavit odesílání zpráv do služby IoT hub spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-145">Invoke the `stop` method in the device app to stop sending messages to your IoT hub by running the following command:</span></span>

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a><span data-ttu-id="54cfc-146">Pomocí Průzkumníka iothub požadované vlastnosti pro dvojici</span><span class="sxs-lookup"><span data-stu-id="54cfc-146">Use iothub-explorer with twin’s desired properties</span></span>

<span data-ttu-id="54cfc-147">Nastavení intervalu požadovanou vlastnost = 3000 spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-147">Set a desired property interval = 3000 by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

<span data-ttu-id="54cfc-148">Tuto vlastnost lze číst zařízením.</span><span class="sxs-lookup"><span data-stu-id="54cfc-148">This property can be read by your device.</span></span>

## <a name="use-iothub-explorer-with-twins-reported-properties"></a><span data-ttu-id="54cfc-149">Použití iothub Průzkumníka s vlastnostmi hlášené pro dvojici</span><span class="sxs-lookup"><span data-stu-id="54cfc-149">Use iothub-explorer with twin’s reported properties</span></span>

<span data-ttu-id="54cfc-150">Umožňuje získáte vlastnosti hlášených zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54cfc-150">Get the reported properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="54cfc-151">Jedna z vlastností je $metadata. $lastUpdated který ukazuje čas poslední toto zařízení odeslání nebo přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="54cfc-151">One of the properties is $metadata.$lastUpdated which shows the last time this device sends or receives a message.</span></span>

## <a name="use-iothub-explorer-with-twins-tags"></a><span data-ttu-id="54cfc-152">Použití iothub Průzkumníka pomocí značek pro dvojici</span><span class="sxs-lookup"><span data-stu-id="54cfc-152">Use iothub-explorer with twin’s tags</span></span>

<span data-ttu-id="54cfc-153">Zobrazení značek a vlastnosti zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54cfc-153">Display the tags and properties of the device by running the following command:</span></span>

```bash
iothub-explorer get-twin <your device id>
```

<span data-ttu-id="54cfc-154">Přidání role pole = teploty a vlhkosti do zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54cfc-154">Add a field role = temperature&humidity to the device by running the following command:</span></span>

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a><span data-ttu-id="54cfc-155">Pomocí Průzkumníka iothub zprávy typu Cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="54cfc-155">Use iothub-explorer with Cloud-to-device messages</span></span>

<span data-ttu-id="54cfc-156">Odeslat zprávu "Hello World" do zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54cfc-156">Send a "Hello World" message to the device by running the following command:</span></span>

```bash
iothub-explorer send <device-id> "Hello World"
```

<span data-ttu-id="54cfc-157">V tématu [Průzkumníkovi iothub-odesílat a přijímat zprávy mezi zařízením a IoT Hub](iot-hub-explorer-cloud-device-messaging.md) pro skutečné scénář použití tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="54cfc-157">See [Use iothub-explorer to send and receive messages between your device and IoT Hub](iot-hub-explorer-cloud-device-messaging.md) for a real scenario of using this command.</span></span>

## <a name="use-iothub-explorer-with-device-twins-queries"></a><span data-ttu-id="54cfc-158">Použití iothub Průzkumníka s dotazy dvojčata zařízení</span><span class="sxs-lookup"><span data-stu-id="54cfc-158">Use iothub-explorer with device twins queries</span></span>

<span data-ttu-id="54cfc-159">Dotaz na zařízení s značkou role = 'teploty a vlhkosti' spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-159">Query devices with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

<span data-ttu-id="54cfc-160">Dotaz na všechna zařízení kromě těch, které se značkou role = 'teploty a vlhkosti' spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="54cfc-160">Query all devices except those with a tag of role = 'temperature&humidity' by running the following command:</span></span>

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a><span data-ttu-id="54cfc-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54cfc-161">Next steps</span></span>

<span data-ttu-id="54cfc-162">Když jste se naučili použití iothub Průzkumníka pomocí různých možností správy.</span><span class="sxs-lookup"><span data-stu-id="54cfc-162">You've learned how to use iothub-explorer with various management options.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
