---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
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
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="cfd26-104">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="cfd26-104">Read messages from your IoT hub</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="cfd26-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="cfd26-105">What you will do</span></span>

- <span data-ttu-id="cfd26-106">Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cfd26-106">Run sample code on your host computer tooread messages from your IoT hub.</span></span>

<span data-ttu-id="cfd26-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cfd26-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="cfd26-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="cfd26-108">What you will learn</span></span>

<span data-ttu-id="cfd26-109">Jak toouse hello gulp nástroj tooread zprávy ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cfd26-109">How toouse hello gulp tool tooread messages from your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="cfd26-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="cfd26-110">What you need</span></span>

- <span data-ttu-id="cfd26-111">Ukázka simulované zařízení Hello v [konfigurace a spustit Simulovaná zařízení cloud nahrát ukázková aplikace](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="cfd26-111">hello simulated device sample in [Configure and run a simulated device cloud upload sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="cfd26-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="cfd26-112">Get your IoT hub and device connection strings</span></span>

<span data-ttu-id="cfd26-113">Hello zařízení připojovací řetězec se používá ve službě IoT hub tooyour tooconnect simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="cfd26-113">hello device connection string is used by your simulated device tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="cfd26-114">Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cfd26-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span>

- <span data-ttu-id="cfd26-115">Seznam všech centra IoT ve vaší skupině prostředků spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cfd26-115">List all your IoT hubs in your resource group by running hello following command:</span></span>

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   <span data-ttu-id="cfd26-116">Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud nebylo ji změnit.</span><span class="sxs-lookup"><span data-stu-id="cfd26-116">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change it.</span></span>
- <span data-ttu-id="cfd26-117">Získáte hello IoT hub, připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="cfd26-117">Get hello IoT hub connection string by running hello following command:</span></span>

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   <span data-ttu-id="cfd26-118">`{my hub name}`je hello název, který jste zadali v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="cfd26-118">`{my hub name}` is hello name that you specified in Lesson 2.</span></span>

## <a name="configure-hello-device-connection-for-hello-sample-code"></a><span data-ttu-id="cfd26-119">Konfigurace připojení k zařízení hello pro hello ukázkový kód</span><span class="sxs-lookup"><span data-stu-id="cfd26-119">Configure hello device connection for hello sample code</span></span>

<span data-ttu-id="cfd26-120">Aktualizovat IoT hub a zařízení konfigurace připojení v `config-azure.json` provedením hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cfd26-120">Update IoT hub and device connection configurations in `config-azure.json` by performing hello following steps:</span></span>

1. <span data-ttu-id="cfd26-121">Otevřete `config-azure.json` ve Visual Studio Code tak, že spustíte následující příkaz v okně konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="cfd26-121">Open `config-azure.json` in Visual Studio Code by running hello following command in a console window:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. <span data-ttu-id="cfd26-122">Ujistěte se, hello následující nahrazení v hello `config-azure.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="cfd26-122">Make hello following replacements in hello `config-azure.json` file:</span></span>

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   <span data-ttu-id="cfd26-124">Nahraďte `[IoT hub connection string]` s hello IoT hub připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="cfd26-124">Replace `[IoT hub connection string]` with hello IoT hub connection string.</span></span>

## <a name="read-messages-from-your-iot-hub"></a><span data-ttu-id="cfd26-125">Čtení zpráv z centra IoT</span><span class="sxs-lookup"><span data-stu-id="cfd26-125">Read messages from your IoT hub</span></span>

<span data-ttu-id="cfd26-126">Spuštění ukázkové aplikace hello simulované zařízení a číst zprávy služby IoT Hub hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cfd26-126">Run hello simulated device sample application and read IoT Hub messages by hello following command:</span></span>

```bash
gulp run --iot-hub
```

<span data-ttu-id="cfd26-127">příkaz Hello spustí hello aplikace, která odesílá zprávy tooyour IoT hub každé 2 sekundy.</span><span class="sxs-lookup"><span data-stu-id="cfd26-127">hello command runs hello application that sends messages tooyour IoT hub every 2 seconds.</span></span> <span data-ttu-id="cfd26-128">Vytvoří také zprávu podřízený proces tooreceive hello.</span><span class="sxs-lookup"><span data-stu-id="cfd26-128">It also spawns a child process tooreceive hello message.</span></span>

<span data-ttu-id="cfd26-129">Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="cfd26-129">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="cfd26-130">aplikace Hello bude ukončena v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="cfd26-130">hello application will exit in 40 seconds.</span></span>

![Simulované ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a><span data-ttu-id="cfd26-132">Souhrn</span><span class="sxs-lookup"><span data-stu-id="cfd26-132">Summary</span></span>

<span data-ttu-id="cfd26-133">Úspěšně jste spuštění hello ukázkové aplikace toosend dat tooyour IoT hub pomocí simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="cfd26-133">You've successfully run hello sample application toosend data tooyour IoT hub with simulated device.</span></span> <span data-ttu-id="cfd26-134">Také jste si přečetli hello zprávy, které byly odeslány tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="cfd26-134">You've also read hello messages that have been sent tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfd26-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfd26-135">Next steps</span></span>
[<span data-ttu-id="cfd26-136">Vytvoření Azure Function App a účtu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="cfd26-136">Create an Azure function app and Azure Storage account</span></span>](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


