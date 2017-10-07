---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 3: odesílání zpráv | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooIntel Edison, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 12672b64-795a-4dfc-86fd-df53ed3eeef7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 83615335027cc792b89d3894578fc3f7a55e5c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a><span data-ttu-id="eaa80-104">Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud</span><span class="sxs-lookup"><span data-stu-id="eaa80-104">Run a sample application toosend device-to-cloud messages</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="eaa80-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="eaa80-105">What you will do</span></span>
<span data-ttu-id="eaa80-106">Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na Edison Intel, který odesílá zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eaa80-106">This article will show you how toodeploy and run a sample application on Intel Edison that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="eaa80-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="eaa80-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="eaa80-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="eaa80-108">What you will learn</span></span>
<span data-ttu-id="eaa80-109">Bude zjistěte, jak toouse hello gulp toodeploy nástroje a spusťte hello ukázkové C aplikace na Edison.</span><span class="sxs-lookup"><span data-stu-id="eaa80-109">You will learn how toouse hello gulp tool toodeploy and run hello sample C application on Edison.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="eaa80-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="eaa80-110">What you need</span></span>
* <span data-ttu-id="eaa80-111">Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="eaa80-111">Before you start this task, you must have successfully completed [Create an Azure function app and a storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>

## <a name="get-your-iot-hub-and-device-connection-strings"></a><span data-ttu-id="eaa80-112">Získat IoT hub a zařízení připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="eaa80-112">Get your IoT hub and device connection strings</span></span>
<span data-ttu-id="eaa80-113">Hello zařízení připojovací řetězec je použité tooconnect Edison tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eaa80-113">hello device connection string is used tooconnect Edison tooyour IoT hub.</span></span> <span data-ttu-id="eaa80-114">Hello IoT hub připojovací řetězec je použité tooconnect vaše IoT hub toohello zařízení identita, která představuje Edison hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eaa80-114">hello IoT hub connection string is used tooconnect your IoT hub toohello device identity that represents Edison in hello IoT hub.</span></span>

* <span data-ttu-id="eaa80-115">Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:</span><span class="sxs-lookup"><span data-stu-id="eaa80-115">List all your IoT hubs in your resource group by running hello following Azure CLI command:</span></span>

```bash
az iot hub list -g iot-sample --query [].name
```

<span data-ttu-id="eaa80-116">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="eaa80-116">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>

* <span data-ttu-id="eaa80-117">Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="eaa80-117">Get hello IoT hub connection string by running hello following Azure CLI command:</span></span>

```bash
az iot hub show-connection-string --name {my hub name}
```

<span data-ttu-id="eaa80-118">`{my hub name}`je hello název, který jste zadali při vytvoření služby IoT hub a registraci Edison.</span><span class="sxs-lookup"><span data-stu-id="eaa80-118">`{my hub name}` is hello name that you specified when you created your IoT hub and registered Edison.</span></span>

* <span data-ttu-id="eaa80-119">Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="eaa80-119">Get hello device connection string by running hello following command:</span></span>

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

<span data-ttu-id="eaa80-120">Použití `myinteledison` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="eaa80-120">Use `myinteledison` as hello value of `{device id}` if you didn't change hello value.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="eaa80-121">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="eaa80-121">Configure hello device connection</span></span>
1. <span data-ttu-id="eaa80-122">Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="eaa80-122">Initialize hello configuration file by running hello following commands:</span></span>

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > <span data-ttu-id="eaa80-123">Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.</span><span class="sxs-lookup"><span data-stu-id="eaa80-123">Run **gulp install-tools** as well, if you haven't done it in Lesson 1.</span></span>

2. <span data-ttu-id="eaa80-124">Otevřete hello zařízení konfigurační soubor `config-edison.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="eaa80-124">Open hello device configuration file `config-edison.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. <span data-ttu-id="eaa80-126">Ujistěte se, hello následující nahrazení v hello `config-edison.json` souboru:</span><span class="sxs-lookup"><span data-stu-id="eaa80-126">Make hello following replacements in hello `config-edison.json` file:</span></span>

   * <span data-ttu-id="eaa80-127">Nahraďte **[zařízení hostitele nebo IP adresa]** s adresou IP zařízení hello označena dolů, při konfiguraci zařízení.</span><span class="sxs-lookup"><span data-stu-id="eaa80-127">Replace **[device hostname or IP address]** with hello device IP address you marked down when you configured your device.</span></span>
   * <span data-ttu-id="eaa80-128">Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="eaa80-128">Replace **[IoT device connection string]** with hello `device connection string` you obtained.</span></span>
   * <span data-ttu-id="eaa80-129">Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.</span><span class="sxs-lookup"><span data-stu-id="eaa80-129">Replace **[IoT hub connection string]** with hello `iot hub connection string` you obtained.</span></span>

   > [!NOTE]
   > <span data-ttu-id="eaa80-130">Nepotřebujete `azure_storage_connection_string` v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="eaa80-130">You don't need `azure_storage_connection_string` in this article.</span></span> <span data-ttu-id="eaa80-131">Dodržte jako je.</span><span class="sxs-lookup"><span data-stu-id="eaa80-131">Keep it as is.</span></span>

## <a name="deploy-and-run-hello-sample-application"></a><span data-ttu-id="eaa80-132">Nasazení a spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="eaa80-132">Deploy and run hello sample application</span></span>
<span data-ttu-id="eaa80-133">Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="eaa80-133">Deploy and run hello sample application on Edison by running hello following command:</span></span>

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a><span data-ttu-id="eaa80-134">Ověřte, že funguje hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="eaa80-134">Verify that hello sample application works</span></span>
<span data-ttu-id="eaa80-135">Měli byste vidět hello DIODU, který je připojený tooEdison blikat každé dvě sekundy.</span><span class="sxs-lookup"><span data-stu-id="eaa80-135">You should see hello LED that is connected tooEdison blinking every two seconds.</span></span> <span data-ttu-id="eaa80-136">Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eaa80-136">Every time hello LED blinks, hello sample application sends a message tooyour IoT hub and verifies that hello message has been successfully sent tooyour IoT hub.</span></span> <span data-ttu-id="eaa80-137">Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="eaa80-137">In addition, each message received by hello IoT hub is printed in hello console window.</span></span> <span data-ttu-id="eaa80-138">Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.</span><span class="sxs-lookup"><span data-stu-id="eaa80-138">hello sample application terminates automatically after sending 20 messages.</span></span>

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a><span data-ttu-id="eaa80-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="eaa80-140">Summary</span></span>
<span data-ttu-id="eaa80-141">Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci na Edison toosend zpráv typu zařízení cloud tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="eaa80-141">You've deployed and run hello new blink sample application on Edison toosend device-to-cloud messages tooyour IoT hub.</span></span> <span data-ttu-id="eaa80-142">Vaše zprávy se teď sledovat, jak jsou napsány toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="eaa80-142">You now monitor your messages as they are written toohello storage account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaa80-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eaa80-143">Next steps</span></span>
<span data-ttu-id="eaa80-144">[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]</span><span class="sxs-lookup"><span data-stu-id="eaa80-144">[Read messages persisted in Azure Storage][read-messages-persisted-in-azure-storage]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md