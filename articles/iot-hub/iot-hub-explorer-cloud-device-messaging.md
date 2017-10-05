---
title: "Správa zařízení Azure IoT Hub cloudu zasílání zpráv s platformou iothub-explorer | Microsoft Docs"
description: "Naučte se používat nástroj iothub-explorer rozhraní příkazového řádku k monitorování zařízení cloudu zprávy (D2C) a odesílat cloudu na zařízení (C2D) zpráv ve službě Azure IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "iothub Průzkumníku cloudu zařízení zasílání zpráv a iot hub cloudu do zařízení, cloudu pro zasílání zpráv zařízení"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 30151b7bdc544bc36e959cc3528d37897198fc7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-iothub-explorer-to-send-and-receive-messages-between-your-device-and-iot-hub"></a><span data-ttu-id="5990b-104">Použití iothub Průzkumníka k odesílání a přijímání zpráv mezi zařízením a IoT Hub</span><span class="sxs-lookup"><span data-stu-id="5990b-104">Use iothub-explorer to send and receive messages between your device and IoT Hub</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="5990b-106">[iothub-explorer](https://github.com/azure/iothub-explorer) má několik příkazů, který usnadňuje správu IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-106">[iothub-explorer](https://github.com/azure/iothub-explorer) has a handful of commands that makes IoT Hub management easier.</span></span> <span data-ttu-id="5990b-107">Tento kurz se zaměřuje na použití iothub Průzkumníka odesílat a přijímat zprávy mezi zařízením a službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-107">This tutorial focuses on how to use iothub-explorer to send and receive messages between your device and your IoT hub.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5990b-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5990b-108">What you will learn</span></span>

<span data-ttu-id="5990b-109">Zjistíte, jak používat iothub-explorer ke sledování zpráv typu zařízení cloud a pro odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="5990b-109">You learn how to use iothub-explorer to monitor device-to-cloud messages and to send cloud-to-device messages.</span></span> <span data-ttu-id="5990b-110">Zprávy typu zařízení cloud může být data snímačů, který shromažďuje vaše zařízení a pak odešle do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-110">Device-to-cloud messages could be sensor data that your device collects and then sends to your IoT hub.</span></span> <span data-ttu-id="5990b-111">Zprávy typu cloud zařízení může být příkazy, které služby IoT hub odešle do zařízení blikání DIODU, která je připojena k zařízení.</span><span class="sxs-lookup"><span data-stu-id="5990b-111">Cloud-to-device messages could be commands that your IoT hub sends to your device to blink an LED that is connected to your device.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="5990b-112">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5990b-112">What you will do</span></span>

- <span data-ttu-id="5990b-113">Pomocí iothub Průzkumníka sledování zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="5990b-113">Use iothub-explorer to monitor device-to-cloud messages.</span></span>
- <span data-ttu-id="5990b-114">Iothub-explorer použijte k odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="5990b-114">Use iothub-explorer to send cloud-to-device messages.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5990b-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5990b-115">What you need</span></span>

- <span data-ttu-id="5990b-116">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="5990b-116">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="5990b-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5990b-117">An active Azure subscription.</span></span>
  - <span data-ttu-id="5990b-118">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-118">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="5990b-119">Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-119">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="5990b-120">iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="5990b-120">iothub-explorer.</span></span> <span data-ttu-id="5990b-121">([Nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer))</span><span class="sxs-lookup"><span data-stu-id="5990b-121">([Install iothub-explorer](https://github.com/azure/iothub-explorer))</span></span>

## <a name="monitor-device-to-cloud-messages"></a><span data-ttu-id="5990b-122">Sledování zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="5990b-122">Monitor device-to-cloud messages</span></span>

<span data-ttu-id="5990b-123">Pokud chcete monitorovat zpráv odesílaných ze zařízení do služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5990b-123">To monitor messages that are sent from your device to your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="5990b-124">Otevřete okno konzoly.</span><span class="sxs-lookup"><span data-stu-id="5990b-124">Open a console window.</span></span>
1. <span data-ttu-id="5990b-125">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5990b-125">Run the following command:</span></span>

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > <span data-ttu-id="5990b-126">Získat `<device-id>` a `<IoTHubConnectionString>` ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-126">Get `<device-id>` and `<IoTHubConnectionString>` from your IoT hub.</span></span> <span data-ttu-id="5990b-127">Ujistěte se, že jste dokončili kurz předchozí.</span><span class="sxs-lookup"><span data-stu-id="5990b-127">Make sure you've finished the previous tutorial.</span></span> <span data-ttu-id="5990b-128">Nebo můžete zkusit použít `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Pokud máte `HostName`, `SharedAccessKeyName` a `SharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="5990b-128">Or you can try to use `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` if you have `HostName`, `SharedAccessKeyName` and `SharedAccessKey`.</span></span>

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="5990b-129">Odesílání zpráv z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="5990b-129">Send cloud-to-device messages</span></span>

<span data-ttu-id="5990b-130">Odeslat zprávu na vaše zařízení ze služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5990b-130">To send a message from your IoT hub to your device, follow these steps:</span></span>

1. <span data-ttu-id="5990b-131">Otevřete okno konzoly.</span><span class="sxs-lookup"><span data-stu-id="5990b-131">Open a console window.</span></span>
1. <span data-ttu-id="5990b-132">Spuštění relace ve službě IoT hub spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="5990b-132">Start a session on your IoT hub by running the following command:</span></span>

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. <span data-ttu-id="5990b-133">Odeslat zprávu na vaše zařízení tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5990b-133">Send a message to your device by running the following command:</span></span>

   ```bash
   iothub-explorer send <device-id> <message>
   ```

<span data-ttu-id="5990b-134">Příkaz bliká DIODU, která je připojena k zařízení a odešle zprávu do zařízení.</span><span class="sxs-lookup"><span data-stu-id="5990b-134">The command blinks the LED that is connected to your device and sends the message to your device.</span></span>

> [!Note]
> <span data-ttu-id="5990b-135">Není nutné zařízení k odeslání příkazu samostatné ack zpět do služby IoT hub po přijetí zprávy.</span><span class="sxs-lookup"><span data-stu-id="5990b-135">There is no need for the device to send a separate ack command back to your IoT hub upon receiving the message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5990b-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5990b-136">Next steps</span></span>

<span data-ttu-id="5990b-137">Když jste se naučili sledování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení mezi zařízení IoT a Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="5990b-137">You’ve learned how to monitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
