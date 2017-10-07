---
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooRaspberry pí 3, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "blikání vedla cloudové platformy, vedla blikání z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 427d8e5e-8af8-4249-8607-44edc90a4972
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 991c64e0b1b6f965b029560cdc6403e557841e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="c246d-104">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="c246d-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="c246d-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="c246d-105">What you will do</span></span>
<span data-ttu-id="c246d-106">Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na 3 malin platformy, která odesílá zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c246d-106">This article will show you how toodeploy and run a sample application on Raspberry Pi 3 that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="c246d-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c246d-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="c246d-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="c246d-108">What you will learn</span></span>
<span data-ttu-id="c246d-109">Se dozvíte, jak toouse hello gulp toodeploy nástroje a spusťte aplikaci Node.js hello ukázka na platformy.</span><span class="sxs-lookup"><span data-stu-id="c246d-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="c246d-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="c246d-110">What you need</span></span>
* <span data-ttu-id="c246d-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="c246d-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="c246d-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="c246d-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="c246d-113">řetězec připojení zařízení Hello používá pí tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c246d-113">hello device connection string is used by your Pi tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="c246d-114">Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c246d-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span> 

* <span data-ttu-id="c246d-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="c246d-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="c246d-116">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="c246d-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="c246d-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="c246d-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="c246d-118">`{my hub name}`je hello název, který jste zadali při vytvoření služby IoT hub a registraci pí.</span><span class="sxs-lookup"><span data-stu-id="c246d-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="c246d-119">Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c246d-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="c246d-120">Použití `myraspberrypi` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="c246d-120">Use `myraspberrypi` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="c246d-121">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="c246d-121">Configure hello device connection</span></span>
1. <span data-ttu-id="c246d-122">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="c246d-122">Initialize hello configuration file by running hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```
2. <span data-ttu-id="c246d-123">Otevřete hello zařízení konfigurační soubor `config-raspberrypi.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c246d-123">Open hello device configuration file `config-raspberrypi.json` in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="c246d-125">Ujistěte se, hello následující nahrazení v hello `config-raspberrypi.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="c246d-125">Make hello following replacements in hello `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="c246d-126">Nahraďte **[zařízení hostitele nebo IP adresa]** s hello zařízení IP adresu nebo název hostitele, který jste získali z `device-discovery-cli` nebo s hodnotou hello zděděná při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="c246d-126">Replace **[device hostname or IP address]** with hello device IP address or host name you got from `device-discovery-cli` or with hello value inherited when you configured your device.</span></span>
   * <span data-ttu-id="c246d-127">Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="c246d-127">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="c246d-128">Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="c246d-128">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

<span data-ttu-id="c246d-129">Aktualizace hello `config-raspberrypi.json` souborů, abyste mohli nasadit hello ukázkovou aplikaci z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="c246d-129">Update hello `config-raspberrypi.json` file so that you can deploy hello sample application from your computer.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="c246d-130">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="c246d-130">Deploy and run hello sample application</span></span>
<span data-ttu-id="c246d-131">Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c246d-131">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="c246d-132">Ověřte, že funguje hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="c246d-132">Verify that hello sample application works</span></span>
<span data-ttu-id="c246d-133">Měli byste vidět hello DIODU, který je připojený tooPi blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="c246d-133">You should see hello LED that is connected tooPi blinking every two seconds.</span></span> <span data-ttu-id="c246d-134">Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c246d-134">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="c246d-135">Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="c246d-135">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="c246d-136">Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="c246d-136">hello sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a><span data-ttu-id="c246d-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="c246d-138">Summary</span></span>
<span data-ttu-id="c246d-139">Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci platformy toosend zpráv typu zařízení cloud tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="c246d-139">You've deployed and run hello new blink sample application on Pi toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="c246d-140">Nyní můžete sledovat vaše zprávy jako toohello účet úložiště, jak jsou napsány.</span><span class="sxs-lookup"><span data-stu-id="c246d-140">You can now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c246d-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c246d-141">Next steps</span></span>
[<span data-ttu-id="c246d-142">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="c246d-142">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)

