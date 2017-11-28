---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
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
ms.openlocfilehash: d3ffbe2e83f9d61c0088b8876a7f0eea62c1fbe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="51943-104">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="51943-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="51943-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="51943-105">What you will do</span></span>

- <span data-ttu-id="51943-106">Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="51943-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="51943-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="51943-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="51943-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="51943-108">What you will learn</span></span>

<span data-ttu-id="51943-109">Jak toouse hello gulp nástroj tooread zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="51943-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="51943-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="51943-110">What you need</span></span>

- <span data-ttu-id="51943-111">Hello zakázat ukázkovou aplikaci, která jste v lekci 3 proběhla úspěšně.</span><span class="sxs-lookup"><span data-stu-id="51943-111">hello BLE sample application that you ran successfully in Lesson 3.</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="51943-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="51943-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="51943-113">Hello zařízení připojovací řetězec se používá ve službě IoT hub tooyour tooconnect zařízení (TI SensorTag nebo simulované zařízení).</span><span class="sxs-lookup"><span data-stu-id="51943-113">hello device connection string is used by your device (TI SensorTag or simulated device) tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="51943-114">Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="51943-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="51943-115">Seznam všech centra IoT ve vaší skupině prostředků spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51943-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="51943-116">Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="51943-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
- <span data-ttu-id="51943-117">Získáte hello IoT hub, připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="51943-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="51943-118">`{my hub name}`je hello název, který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="51943-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="51943-119">Konfigurace připojení k zařízení hello pro hello ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="51943-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="51943-120">Aktualizace hello zařízení konfigurační soubor `config-azure.json` tak, aby bylo možné číst zprávy ze služby IoT hub v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="51943-120">Update hello device configuration file `config-azure.json` so that you can read messages from your IoT hub on your host computer.</span></span> <span data-ttu-id="51943-121">toodo, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="51943-121">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="51943-122">Otevřete `config-azure.json` ve Visual Studio Code tak, že spustíte následující příkaz v okně konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="51943-122">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="51943-123">Ujistěte se, hello následující nahrazení v hello `config-azure.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="51943-123">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="51943-125">Nahraďte `[IoT hub connection string]` s hello IoT hub připojovací řetězec, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="51943-125">Replace `[IoT hub connection string]` with hello IoT hub connection string that you obtained.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="51943-126">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="51943-126">Read messages from your IoT hub</span></span>

<span data-ttu-id="51943-127">Pokud máte TI SensorTag, ujistěte se, že máte už zapnutý vaší SensorTag.</span><span class="sxs-lookup"><span data-stu-id="51943-127">If you have a TI SensorTag, make sure you have already powered on your SensorTag.</span></span> <span data-ttu-id="51943-128">Spuštění ukázkové aplikace hello brány a číst zprávy služby IoT Hub hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="51943-128">Run hello gateway sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="51943-129">příkaz Hello spustí hello zakázat ukázkovou aplikaci, která čte a balíčky teploty data z SensorTag nebo simulované zařízení a odešle zprávu hello tooyour služby IoT hub každé 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="51943-129">hello command runs hello BLE sample application that reads and packages temperature data from your SensorTag or simulated device and sends hello message tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="51943-130">Vytvoří také zprávu podřízený proces tooreceive hello.</span><span class="sxs-lookup"><span data-stu-id="51943-130">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="51943-131">Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="51943-131">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="51943-132">instance aplikace Hello ukázka přeruší automaticky v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="51943-132">hello sample application instance will terminate automatically in 40 seconds.</span></span>

![Zakázat ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a><span data-ttu-id="51943-134">Souhrn</span><span class="sxs-lookup"><span data-stu-id="51943-134">Summary</span></span>

<span data-ttu-id="51943-135">Zprávy ukázkový kód tooread jste spustit ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="51943-135">You've run a sample code tooread messages from your IoT hub.</span></span> <span data-ttu-id="51943-136">Jste připravené tooread hello zprávy, které jsou uložené ve službě Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="51943-136">You're ready tooread hello messages that are stored in your Azure table storage.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51943-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="51943-137">Next steps</span></span>
[<span data-ttu-id="51943-138">Vytvoření Azure Function App a účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="51943-138">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


