---
featureFlags: usabilla
title: "Připojení k Azure IoT - lekci 3 malin platformy (uzel): Table storage | Microsoft Docs"
description: "Monitorování zprávy typu zařízení cloud, jako jsou zapsány do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "načtení dat z cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 9965bd54-61da-479b-aaad-5604850a2be5
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 60084906c05ff9e5396f8e2378d73f7ac939d8df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="edb52-104">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="edb52-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="edb52-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="edb52-105">What you will do</span></span>
<span data-ttu-id="edb52-106">Monitorování zprávy typu zařízení cloud, které jsou odesílány z malin pí 3 do služby IoT hub jako zprávy jsou zapsány do Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="edb52-106">Monitor the device-to-cloud messages that are sent from Raspberry Pi 3 to your IoT hub as the messages are written to your Azure Table storage.</span></span> <span data-ttu-id="edb52-107">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="edb52-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="edb52-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="edb52-108">What you will learn</span></span>
<span data-ttu-id="edb52-109">V tomto článku se dozvíte, jak pomocí úloh pro čtení zpráv gulp číst zprávy uchovávané v Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="edb52-109">In this article, you will learn how to use the gulp read-message task to read messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="edb52-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="edb52-110">What you need</span></span>
<span data-ttu-id="edb52-111">Před zahájením tohoto procesu se musí úspěšně jste dokončili [spuštění ukázkové aplikace Azure blikání na malin pí 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).</span><span class="sxs-lookup"><span data-stu-id="edb52-111">Before starting this process, you must have successfully completed [Run the Azure blink sample application on Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="edb52-112">Přečtěte si nové zprávy z vašeho účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="edb52-112">Read new messages from your storage account</span></span>
<span data-ttu-id="edb52-113">V předchozím článku jste spustili ukázkovou aplikaci na pí.</span><span class="sxs-lookup"><span data-stu-id="edb52-113">In the previous article, you ran a sample application on Pi.</span></span> <span data-ttu-id="edb52-114">Ukázkové aplikace odeslané zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="edb52-114">The sample application sent messages to your Azure IoT hub.</span></span> <span data-ttu-id="edb52-115">Zprávy odeslané do služby IoT hub jsou uloženy do úložiště Azure Table prostřednictvím Azure funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="edb52-115">The messages sent to your IoT hub are stored into your Azure Table storage via the Azure function app.</span></span> <span data-ttu-id="edb52-116">Je třeba připojovacího řetězce úložiště Azure pro čtení zpráv z Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="edb52-116">You need the Azure storage connection string to read messages from your Azure Table storage.</span></span>

<span data-ttu-id="edb52-117">Umožní číst zprávy, které jsou uložené ve službě Azure Table storage, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="edb52-117">To read messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="edb52-118">Spustit následující příkazy a získáte připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="edb52-118">Get the connection string by running the following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="edb52-119">Načte první příkaz `storage name` který se používá v druhém příkazu získat připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="edb52-119">The first command retrieves the `storage name` that is used in the second command to get the connection string.</span></span> <span data-ttu-id="edb52-120">Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.</span><span class="sxs-lookup"><span data-stu-id="edb52-120">Use `iot-sample` as the value of `{resource group name}` if you didn't change the value.</span></span>
2. <span data-ttu-id="edb52-121">Otevřete konfigurační soubor `config-raspberrypi.json` ve Visual Studio Code spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="edb52-121">Open the configuration file `config-raspberrypi.json` in Visual Studio Code by running the following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. <span data-ttu-id="edb52-122">Nahraďte `[Azure storage connection string]` připojovacím řetězcem, který jste získali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="edb52-122">Replace `[Azure storage connection string]` with the connection string you got in step 1.</span></span>
4. <span data-ttu-id="edb52-123">Uložit `config-raspberrypi.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="edb52-123">Save the `config-raspberrypi.json` file.</span></span>
5. <span data-ttu-id="edb52-124">Odesílání zpráv znovu a číst je z Azure Table storage spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="edb52-124">Send messages again and read them from your Azure Table storage by running the following command:</span></span>
   
   ```bash
   gulp run --read-storage
   ```
   
   <span data-ttu-id="edb52-125">Probíhá logiku pro čtení z úložiště tabulek Azure `azure-table.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="edb52-125">The logic for reading from Azure Table storage is in the `azure-table.js` file.</span></span>
   
    ![spuštění – gulp úložiště pro čtení](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a><span data-ttu-id="edb52-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="edb52-127">Summary</span></span>
<span data-ttu-id="edb52-128">Úspěšně jste připojený pí do služby IoT hub v cloudu a používá ukázkovou aplikaci blikání k odesílání zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="edb52-128">You've successfully connected Pi to your IoT hub in the cloud and used the blink sample application to send device-to-cloud messages.</span></span> <span data-ttu-id="edb52-129">Aplikace Azure funkce se také používají k ukládání příchozích zpráv IoT hub do úložiště Azure Table.</span><span class="sxs-lookup"><span data-stu-id="edb52-129">You also used the Azure function app to store incoming IoT hub messages to your Azure Table storage.</span></span> <span data-ttu-id="edb52-130">Pi nyní možné odesílat zprávy typu cloud zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="edb52-130">You can now send cloud-to-device messages from your IoT hub to Pi.</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb52-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="edb52-131">Next steps</span></span>
[<span data-ttu-id="edb52-132">Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="edb52-132">Run the sample application to receive cloud-to-device messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

