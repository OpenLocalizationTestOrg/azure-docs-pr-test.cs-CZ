---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 45f3595c4848d5c283cdf95604adf8d2c8d6a809
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="e38b9-104">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="e38b9-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e38b9-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e38b9-105">What you will do</span></span>

- <span data-ttu-id="e38b9-106">Spustíte ukázkový kód v hostitelském počítači umožní číst zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e38b9-106">Run sample code on your host computer to read messages from your IoT hub.</span></span>

<span data-ttu-id="e38b9-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e38b9-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e38b9-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e38b9-108">What you will learn</span></span>

<span data-ttu-id="e38b9-109">Jak používat nástroj gulp ke čtení zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e38b9-109">How to use the gulp tool to read messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e38b9-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e38b9-110">What you need</span></span>

- <span data-ttu-id="e38b9-111">Zakázat ukázkovou aplikaci, která jste v lekci 3 proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="e38b9-111">The BLE sample application that you ran successfully in Lesson 3.</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="e38b9-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="e38b9-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="e38b9-113">Zařízení připojovací řetězec se používá zařízením (TI SensorTag nebo simulované zařízení) pro připojení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e38b9-113">The device connection string is used by your device (TI SensorTag or simulated device) to connect to your IoT hub.</span></span> <span data-ttu-id="e38b9-114">IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e38b9-114">The IoT hub connection string is used to connect to the identity registry in your IoT hub to manage the devices that are allowed to connect to your IoT hub.</span></span>

- <span data-ttu-id="e38b9-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e38b9-115">List all your IoT hubs in your resource group by running the following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="e38b9-116">Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e38b9-116">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value.</span></span>
- <span data-ttu-id="e38b9-117">Získáte připojovací řetězec centra IoT tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e38b9-117">Get the IoT hub connection string by running the following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="e38b9-118">`{my hub name}`je název, který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="e38b9-118">`{my hub name}` is the name that you specified in Lesson 2.</span></span>

## <a name="configure-the-device-connection-for-the-sample-code"></a><span data-ttu-id="e38b9-119">Konfigurace připojení zařízení pro ukázkového kódu</span><span class="sxs-lookup"><span data-stu-id="e38b9-119">Configure the device connection for the sample code</span></span>

<span data-ttu-id="e38b9-120">Aktualizovat konfigurační soubor zařízení `config-azure.json` tak, aby bylo možné číst zprávy ze služby IoT hub v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="e38b9-120">Update the device configuration file `config-azure.json` so that you can read messages from your IoT hub on your host computer.</span></span> <span data-ttu-id="e38b9-121">Chcete-li to provést, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e38b9-121">To do this, follow these steps:</span></span>

1. <span data-ttu-id="e38b9-122">Otevřete `config-azure.json` ve Visual Studio Code spuštěním následujícího příkazu v okně konzoly:</span><span class="sxs-lookup"><span data-stu-id="e38b9-122">Open `config-azure.json` in Visual Studio Code by running the following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="e38b9-123">Zkontrolujte následující nahrazení v `config-azure.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="e38b9-123">Make the following replacements in the `config-azure.json` file:</span></span>

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="e38b9-125">Nahraďte `[IoT hub connection string]` s IoT hub připojovací řetězec, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="e38b9-125">Replace `[IoT hub connection string]` with the IoT hub connection string that you obtained.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="e38b9-126">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="e38b9-126">Read messages from your IoT hub</span></span>

<span data-ttu-id="e38b9-127">Pokud máte TI SensorTag, ujistěte se, že máte už zapnutý vaší SensorTag.</span><span class="sxs-lookup"><span data-stu-id="e38b9-127">If you have a TI SensorTag, make sure you have already powered on your SensorTag.</span></span> <span data-ttu-id="e38b9-128">Spuštění ukázkové aplikace brány a číst zprávy služby IoT Hub následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e38b9-128">Run the gateway sample application and read IoT Hub messages by the following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="e38b9-129">Příkaz spustí zakázat ukázkovou aplikaci, která čte a balíčky teploty data z SensorTag nebo simulované zařízení a odešle zprávu do služby IoT hub každé 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="e38b9-129">The command runs the BLE sample application that reads and packages temperature data from your SensorTag or simulated device and sends the message to your IoT hub every 2 seconds.</span></span> <span data-ttu-id="e38b9-130">Vytvoří také podřízený proces zpráva.</span><span class="sxs-lookup"><span data-stu-id="e38b9-130">It also spawns a child process to receive the message.</span></span>

<span data-ttu-id="e38b9-131">Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="e38b9-131">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="e38b9-132">Instance aplikace na ukázkové přeruší automaticky v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="e38b9-132">The sample application instance will terminate automatically in 40 seconds.</span></span>

![Zakázat ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a><span data-ttu-id="e38b9-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e38b9-134">Summary</span></span>

<span data-ttu-id="e38b9-135">Spustíte ukázkový kód ke čtení zpráv ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e38b9-135">You've run a sample code to read messages from your IoT hub.</span></span> <span data-ttu-id="e38b9-136">Jste připravení číst zprávy, které jsou uložené ve službě Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="e38b9-136">You're ready to read the messages that are stored in your Azure table storage.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e38b9-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e38b9-137">Next steps</span></span>
[<span data-ttu-id="e38b9-138">Vytvoření Azure Function App a účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e38b9-138">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


