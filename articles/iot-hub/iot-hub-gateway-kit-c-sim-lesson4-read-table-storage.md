---
title: "Simulované zařízení & brány Azure IoT - Lekce 4: Table storage | Microsoft Docs"
description: "Uložení zpráv ze služby IoT hub Intel NUC tooyour, zapisuje je úložiště Table tooAzure a číst je z cloudu hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "načtení dat z cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 78e4b6ea-968d-401e-a7dc-8f9acdb3ec1a
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 43e299d63bbbe10d4d867af25e700c3a7cc07c53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="54584-104">Čtení zprávy uchovávané v úložišti Azure Table</span><span class="sxs-lookup"><span data-stu-id="54584-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="54584-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="54584-105">What you will do</span></span>

- <span data-ttu-id="54584-106">Spusťte hello brány ukázkovou aplikaci na bránu, která odesílá zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54584-106">Run hello gateway sample application on your gateway that sends messages tooyour IoT hub.</span></span>
- <span data-ttu-id="54584-107">Spusťte ukázkový kód na hostitele počítače tooread zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="54584-107">Run sample code on your host computer tooread messages in your Azure Table storage.</span></span>

<span data-ttu-id="54584-108">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="54584-108">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="54584-109">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="54584-109">What you will learn</span></span>

<span data-ttu-id="54584-110">Jak toouse hello gulp nástroj toorun hello ukázkový kód tooread zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="54584-110">How toouse hello gulp tool toorun hello sample code tooread messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="54584-111">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="54584-111">What you need</span></span>

<span data-ttu-id="54584-112">Úspěšně jste hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="54584-112">You have have successfully done hello following tasks:</span></span>

- <span data-ttu-id="54584-113">[Vytvořit aplikaci Azure funkce hello a účet úložiště Azure hello](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="54584-113">[Created hello Azure function app and hello Azure storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="54584-114">[Spuštění ukázkové aplikace hello brány](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="54584-114">[Run hello gateway sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>
- <span data-ttu-id="54584-115">[Čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="54584-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="54584-116">Získat vaše připojovacích řetězců Azure storage</span><span class="sxs-lookup"><span data-stu-id="54584-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="54584-117">Časná v této lekci úspěšně jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="54584-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="54584-118">tooget hello připojovací řetězec hello účtu úložiště Azure, spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="54584-118">tooget hello connection string of hello Azure storage account, run hello following commands:</span></span>

* <span data-ttu-id="54584-119">Zobrazí seznam všech účtů úložišť.</span><span class="sxs-lookup"><span data-stu-id="54584-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="54584-120">Získáte připojovací řetězec úložiště azure.</span><span class="sxs-lookup"><span data-stu-id="54584-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="54584-121">Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud hodnota hello v lekci 2 nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="54584-121">Use `iot-gateway` as hello value of `{resource group name}` if you didn't change hello value in Lesson 2.</span></span>

## <a name="configure-hello-device-connection"></a><span data-ttu-id="54584-122">Nakonfigurujte připojení zařízení hello</span><span class="sxs-lookup"><span data-stu-id="54584-122">Configure hello device connection</span></span>

<span data-ttu-id="54584-123">Aktualizace hello `config-azure.json` tak, aby hello ukázkový kód, který běží v hostitelském počítači hello může číst zprávy ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="54584-123">Update hello `config-azure.json` file so that hello sample code that runs on hello host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="54584-124">tooconfigure hello připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="54584-124">tooconfigure hello device connection, follow these steps:</span></span>

1. <span data-ttu-id="54584-125">Soubor konfigurace zařízení otevřete hello `config-azure.json` spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="54584-125">Open hello device configuration file `config-azure.json` by running hello following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfigurace](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="54584-127">Nahraďte `[Azure storage connection string]` s hello připojovací řetězec úložiště Azure, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="54584-127">Replace `[Azure storage connection string]` with hello Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="54584-128">`[IoT hub connection string]`by měla být nahrazená již v části [čtení zpráv z Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) v Lesson3.</span><span class="sxs-lookup"><span data-stu-id="54584-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="54584-129">Čtení zpráv ve službě Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="54584-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="54584-130">Spuštění ukázkové aplikace hello brány a číst zprávy úložiště Azure Table hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="54584-130">Run hello gateway sample application and read Azure Table storage messages by hello following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="54584-131">Služby IoT hub se aktivuje zprávu toosave aplikace funkce Azure do Azure Table storage při přijetí nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="54584-131">Your IoT hub triggers your Azure Function application toosave message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="54584-132">Hello `gulp run` příkaz spustí brány ukázkovou aplikaci, která odesílá zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="54584-132">hello `gulp run` command runs gateway sample application that sends messages tooyour IoT hub.</span></span> <span data-ttu-id="54584-133">S `table-storage` parametr, je také vytvoří hello podřízený proces tooreceive uložit zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="54584-133">With `table-storage` parameter, it also spawns a child process tooreceive hello saved message in your Azure Table storage.</span></span>

<span data-ttu-id="54584-134">Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="54584-134">hello messages that are being sent and received are all displayed instantly on hello same console window in hello host machine.</span></span> <span data-ttu-id="54584-135">instance aplikace Hello ukázka přeruší automaticky v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="54584-135">hello sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp pro čtení](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a><span data-ttu-id="54584-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="54584-137">Summary</span></span>

<span data-ttu-id="54584-138">Spustíte jste hello ukázkový kód tooread hello zpráv ve službě Azure Table storage, uloží prostřednictvím funkce Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="54584-138">You've run hello sample code tooread hello messages in your Azure Table storage saved by your Azure Function application.</span></span>
