---
title: "zasílání zpráv s platformou iothub-explorer cloud zařízení Azure IoT Hub pro aaaManage | Microsoft Docs"
description: "Zjistěte, jak toouse hello zprávy iothub-explorer rozhraní příkazového řádku nástroje toomonitor zařízení toocloud (D2C) a odesílat zprávy toodevice (C2D) cloudu v Azure IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "iothub Průzkumníku cloudu zařízení zasílání zpráv a toodevice cloudové Centrum iot, cloudu toodevice zasílání zpráv"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a><span data-ttu-id="04742-104">Pomocí Průzkumníka iothub toosend a příjem zpráv mezi zařízením a IoT Hub</span><span class="sxs-lookup"><span data-stu-id="04742-104">Use iothub-explorer toosend and receive messages between your device and IoT Hub</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="04742-106">[iothub-explorer](https://github.com/azure/iothub-explorer) má několik příkazů, který usnadňuje správu IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="04742-106">[iothub-explorer](https://github.com/azure/iothub-explorer) has a handful of commands that makes IoT Hub management easier.</span></span> <span data-ttu-id="04742-107">Tento kurz se zaměřuje na toouse iothub-explorer toosend a příjem zpráv mezi zařízením a služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="04742-107">This tutorial focuses on how toouse iothub-explorer toosend and receive messages between your device and your IoT hub.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="04742-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="04742-108">What you will learn</span></span>

<span data-ttu-id="04742-109">Zjistíte, jak toouse iothub-explorer toomonitor zařízení cloud zprávy a zprávy toosend cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="04742-109">You learn how toouse iothub-explorer toomonitor device-to-cloud messages and toosend cloud-to-device messages.</span></span> <span data-ttu-id="04742-110">Zprávy typu zařízení cloud může být data snímačů, aby vaše zařízení shromažďuje a poté odešle tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="04742-110">Device-to-cloud messages could be sensor data that your device collects and then sends tooyour IoT hub.</span></span> <span data-ttu-id="04742-111">Zprávy typu cloud zařízení může být příkazy odešle službě IoT hub tooyour zařízení tooblink DIODU, který je připojený tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="04742-111">Cloud-to-device messages could be commands that your IoT hub sends tooyour device tooblink an LED that is connected tooyour device.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="04742-112">Co provedete</span><span class="sxs-lookup"><span data-stu-id="04742-112">What you will do</span></span>

- <span data-ttu-id="04742-113">Pomocí zpráv typu zařízení cloud toomonitor iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="04742-113">Use iothub-explorer toomonitor device-to-cloud messages.</span></span>
- <span data-ttu-id="04742-114">Pomocí zpráv typu cloud zařízení toosend iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="04742-114">Use iothub-explorer toosend cloud-to-device messages.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="04742-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="04742-115">What you need</span></span>

- <span data-ttu-id="04742-116">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="04742-116">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="04742-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="04742-117">An active Azure subscription.</span></span>
  - <span data-ttu-id="04742-118">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="04742-118">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="04742-119">Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="04742-119">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="04742-120">iothub-explorer.</span><span class="sxs-lookup"><span data-stu-id="04742-120">iothub-explorer.</span></span> <span data-ttu-id="04742-121">([Nainstalovat iothub-explorer](https://github.com/azure/iothub-explorer))</span><span class="sxs-lookup"><span data-stu-id="04742-121">([Install iothub-explorer](https://github.com/azure/iothub-explorer))</span></span>

## <a name="monitor-device-to-cloud-messages"></a><span data-ttu-id="04742-122">Sledování zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="04742-122">Monitor device-to-cloud messages</span></span>

<span data-ttu-id="04742-123">toomonitor zprávy, které jsou odesílány z tooyour zařízení IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="04742-123">toomonitor messages that are sent from your device tooyour IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="04742-124">Otevřete okno konzoly.</span><span class="sxs-lookup"><span data-stu-id="04742-124">Open a console window.</span></span>
1. <span data-ttu-id="04742-125">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="04742-125">Run hello following command:</span></span>

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > <span data-ttu-id="04742-126">Získat `<device-id>` a `<IoTHubConnectionString>` ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="04742-126">Get `<device-id>` and `<IoTHubConnectionString>` from your IoT hub.</span></span> <span data-ttu-id="04742-127">Ujistěte se, že jste dokončili kurz předchozí hello.</span><span class="sxs-lookup"><span data-stu-id="04742-127">Make sure you've finished hello previous tutorial.</span></span> <span data-ttu-id="04742-128">Nebo můžete zkusit toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Pokud máte `HostName`, `SharedAccessKeyName` a `SharedAccessKey`.</span><span class="sxs-lookup"><span data-stu-id="04742-128">Or you can try toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` if you have `HostName`, `SharedAccessKeyName` and `SharedAccessKey`.</span></span>

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="04742-129">Odesílání zpráv z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="04742-129">Send cloud-to-device messages</span></span>

<span data-ttu-id="04742-130">toosend zprávy ze zařízení IoT hub tooyour postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="04742-130">toosend a message from your IoT hub tooyour device, follow these steps:</span></span>

1. <span data-ttu-id="04742-131">Otevřete okno konzoly.</span><span class="sxs-lookup"><span data-stu-id="04742-131">Open a console window.</span></span>
1. <span data-ttu-id="04742-132">Spusťte relaci ve službě IoT hub spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="04742-132">Start a session on your IoT hub by running hello following command:</span></span>

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. <span data-ttu-id="04742-133">Odeslat zprávu tooyour zařízení tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="04742-133">Send a message tooyour device by running hello following command:</span></span>

   ```bash
   iothub-explorer send <device-id> <message>
   ```

<span data-ttu-id="04742-134">příkaz Hello bliká hello DIODU tooyour připojených zařízení a odešle hello zpráva tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="04742-134">hello command blinks hello LED that is connected tooyour device and sends hello message tooyour device.</span></span>

> [!Note]
> <span data-ttu-id="04742-135">Není nutné pro hello zařízení toosend samostatné potvrzení příkazu back tooyour IoT hub po přijetí zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="04742-135">There is no need for hello device toosend a separate ack command back tooyour IoT hub upon receiving hello message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04742-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04742-136">Next steps</span></span>

<span data-ttu-id="04742-137">Když jste se naučili jak toomonitor zařízení cloud zprávy a odesílání zpráv typu cloud zařízení mezi zařízení IoT a Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="04742-137">You’ve learned how toomonitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
