---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fbf7958e2437d274f2692dbc235ac8147bdfa63
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="5b678-104">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="5b678-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="5b678-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5b678-105">What you will do</span></span>

- <span data-ttu-id="5b678-106">Spustíte ukázkový kód v hostitelském počítači umožní číst zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-106">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="5b678-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5b678-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5b678-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5b678-108">What you will learn</span></span>

<span data-ttu-id="5b678-109">Jak používat nástroj gulp ke čtení zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-109">How to use the gulp tool to read messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5b678-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5b678-110">What you need</span></span>

- <span data-ttu-id="5b678-111">Ukázka simulované zařízení v [konfigurace a spustit Simulovaná zařízení cloud nahrát ukázková aplikace](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="5b678-111">The simulated device sample in [Configure and run a simulated device cloud upload sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="5b678-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="5b678-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="5b678-113">Zařízení připojovací řetězec se používá simulovaným zařízením pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-113">The device connection string is used by your simulated device to connect to your IoT hub.</span></span> <span data-ttu-id="5b678-114">IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span>

- <span data-ttu-id="5b678-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b678-115">List all your IoT hubs in your resource group by running the following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="5b678-116">Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo ji změnit.</span><span class="sxs-lookup"><span data-stu-id="5b678-116">Use `iot-gateway` as the value of `{resource group name}` if you didn't change it.</span></span>
- <span data-ttu-id="5b678-117">Získáte připojovací řetězec centra IoT tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b678-117">Get the IoT hub connection string by running the following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="5b678-118">`{my hub name}`je název, který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="5b678-118">`{my hub name}` is the name that you specified in Lesson 2.</span></span>

## <a name="configure-the-device-connection-for-the-sample-code"></a><span data-ttu-id="5b678-119">Konfigurace připojení zařízení pro ukázkového kódu</span><span class="sxs-lookup"><span data-stu-id="5b678-119">Configure the device connection for the sample code</span></span>

<span data-ttu-id="5b678-120">Aktualizovat IoT hub a zařízení konfigurace připojení v `config-azure.json` provedením následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="5b678-120">Update IoT hub and device connection configurations in `config-azure.json` by performing the following steps:</span></span>

1. <span data-ttu-id="5b678-121">Otevřete `config-azure.json` ve Visual Studio Code spuštěním následujícího příkazu v okně konzoly:</span><span class="sxs-lookup"><span data-stu-id="5b678-121">Open `config-azure.json` in Visual Studio Code by running the following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="5b678-122">Zkontrolujte následující nahrazení v `config-azure.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="5b678-122">Make the following replacements in the `config-azure.json` file:</span></span>

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="5b678-124">Nahraďte `[IoT hub connection string]` připojovacím řetězcem IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-124">Replace `[IoT hub connection string]` with the IoT hub connection string.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="5b678-125">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="5b678-125">Read messages from your IoT hub</span></span>

<span data-ttu-id="5b678-126">Spuštění ukázkové aplikace simulovaného zařízení a číst zprávy služby IoT Hub následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b678-126">Run the simulated device sample application and read IoT Hub messages by the following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="5b678-127">Příkaz spustí aplikaci, která odesílá zprávy do služby IoT hub každé 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="5b678-127">The command runs the application that sends messages to your IoT hub every 2 seconds.</span></span> <span data-ttu-id="5b678-128">Vytvoří také podřízený proces zpráva.</span><span class="sxs-lookup"><span data-stu-id="5b678-128">It also spawns a child process to receive the message.</span></span>

<span data-ttu-id="5b678-129">Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="5b678-129">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="5b678-130">Aplikace se zavře v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="5b678-130">The application will exit in 40 seconds.</span></span>

![Simulované ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a><span data-ttu-id="5b678-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5b678-132">Summary</span></span>

<span data-ttu-id="5b678-133">Úspěšně jste spusťte ukázkovou aplikaci k odesílání dat do služby IoT hub s simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="5b678-133">You've successfully run the sample application to send data to your IoT hub with simulated device.</span></span> <span data-ttu-id="5b678-134">Také jste si přečetli zprávy, které byly odeslány do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b678-134">You've also read the messages that have been sent to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b678-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b678-135">Next steps</span></span>
[<span data-ttu-id="5b678-136">Vytvoření Azure Function App a účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5b678-136">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


