---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooRaspberry pí 3, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "blikání vedla cloudové platformy, vedla blikání z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: e38df29f-f77f-435f-9add-46814297564f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c484beb2e2d3a3cf19f071f2ba87b9a4fe41c1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="23d39-104">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="23d39-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="23d39-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="23d39-105">What you will do</span></span>
<span data-ttu-id="23d39-106">Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na 3 malin platformy, která odesílá zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23d39-106">This article will show you how toodeploy and run a sample application on Raspberry Pi 3 that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="23d39-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="23d39-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="23d39-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="23d39-108">What you will learn</span></span>
<span data-ttu-id="23d39-109">Se dozvíte, jak toouse hello gulp toodeploy nástroje a spusťte aplikaci Node.js hello ukázka na platformy.</span><span class="sxs-lookup"><span data-stu-id="23d39-109">You will learn how toouse hello gulp tool toodeploy and run hello sample Node.js application on Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="23d39-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="23d39-110">What you need</span></span>
* <span data-ttu-id="23d39-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="23d39-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="23d39-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="23d39-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="23d39-113">řetězec připojení zařízení Hello používá pí tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23d39-113">hello device connection string is used by your Pi tooconnect tooyour IoT hub.</span></span> <span data-ttu-id="23d39-114">Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23d39-114">hello IoT hub connection string is used tooconnect toohello identity registry in your IoT hub toomanage hello devices that are allowed tooconnect tooyour IoT hub.</span></span> 

* <span data-ttu-id="23d39-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="23d39-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="23d39-116">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="23d39-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="23d39-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="23d39-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

<span data-ttu-id="23d39-118">`{my hub name}`je hello název, který jste zadali při vytvoření služby IoT hub a registraci pí.</span><span class="sxs-lookup"><span data-stu-id="23d39-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Pi.</span></span>

* <span data-ttu-id="23d39-119">Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="23d39-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

<span data-ttu-id="23d39-120">Použití `myraspberrypi` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="23d39-120">Use `myraspberrypi` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="23d39-121">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="23d39-121">Configure hello device connection</span></span>
1. <span data-ttu-id="23d39-122">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="23d39-122">Initialize hello configuration file by running hello following commands:</span></span>
   
   ```bash
   npm install
   gulp init
   ```

> [!NOTE]
> <span data-ttu-id="23d39-123">Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.</span><span class="sxs-lookup"><span data-stu-id="23d39-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="23d39-124">Otevřete hello zařízení konfigurační soubor `config-raspberrypi.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="23d39-124">Open hello device configuration file `config-raspberrypi.json` in Visual Studio Code by running hello following command:</span></span>
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
   
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. <span data-ttu-id="23d39-126">Ujistěte se, hello následující nahrazení v hello `config-raspberrypi.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="23d39-126">Make hello following replacements in hello `config-raspberrypi.json` file:</span></span>
   
   * <span data-ttu-id="23d39-127">Nahraďte **[zařízení hostitele nebo IP adresa]** s hello zařízení IP adresu nebo název hostitele, který jste získali z `device-discovery-cli` nebo s hodnotou hello zděděná při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="23d39-127">Replace **[device hostname or IP address]** with hello device IP address or host name you got from `device-discovery-cli` or with hello value inherited when you configured your device.</span></span>
   * <span data-ttu-id="23d39-128">Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="23d39-128">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="23d39-129">Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="23d39-129">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

> [!NOTE]
> <span data-ttu-id="23d39-130">Nepotřebujete `azure_storage_connection_string` v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="23d39-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="23d39-131">Dodržte jako je.</span><span class="sxs-lookup"><span data-stu-id="23d39-131">Keep it as is.</span></span>

<span data-ttu-id="23d39-132">Aktualizace hello `config-raspberrypi.json` souborů, abyste mohli nasadit hello ukázkovou aplikaci z vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="23d39-132">Update hello `config-raspberrypi.json` file so that you can deploy hello sample application from your computer.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="23d39-133">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="23d39-133">Deploy and run hello sample application</span></span>
<span data-ttu-id="23d39-134">Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="23d39-134">Deploy and run hello sample application on Pi by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="23d39-135">Ověřte, že funguje hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="23d39-135">Verify that hello sample application works</span></span>
<span data-ttu-id="23d39-136">Měli byste vidět hello DIODU, který je připojený tooPi blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="23d39-136">You should see hello LED that is connected tooPi blinking every two seconds.</span></span> <span data-ttu-id="23d39-137">Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23d39-137">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="23d39-138">Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="23d39-138">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="23d39-139">Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="23d39-139">hello sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run_c.png)

## <a name="summary"></a><span data-ttu-id="23d39-141">Souhrn</span><span class="sxs-lookup"><span data-stu-id="23d39-141">Summary</span></span>
<span data-ttu-id="23d39-142">Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci platformy toosend zpráv typu zařízení cloud tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="23d39-142">You've deployed and run hello new blink sample application on Pi toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="23d39-143">Vaše zprávy se teď sledovat, jak jsou napsány toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="23d39-143">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23d39-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="23d39-144">Next steps</span></span>
[<span data-ttu-id="23d39-145">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="23d39-145">Read messages persisted in Azure Storage</span></span>](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md)

