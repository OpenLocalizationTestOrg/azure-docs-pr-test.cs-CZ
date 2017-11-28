---
title: 'Connect Arduino (C) tooAzure IoT - Lekce 3: Table storage | Microsoft Docs'
description: "Zprávy typu zařízení cloud hello sledovat, jak jsou napsány tooyour Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 386083e0-0dbb-48c0-9ac2-4f8fb4590772
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fef18bc9e780e78d95f0c643a5f193125130a5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="714a1-104">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="714a1-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="714a1-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="714a1-105">What you will do</span></span>
<span data-ttu-id="714a1-106">Zprávy typu zařízení cloud hello monitorování odeslané z centra IoT Adafruit prolnutí M0 Wi-Fi Arduino Tabule tooyour jako hello zprávy jsou zapsány tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="714a1-106">Monitor hello device-to-cloud messages that are sent from your Adafruit Feather M0 WiFi Arduino board tooyour IoT hub as hello messages are written tooyour Azure Table storage.</span></span>

<span data-ttu-id="714a1-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="714a1-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="714a1-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="714a1-108">What you will learn</span></span>
<span data-ttu-id="714a1-109">V tomto článku se dozvíte, jak trvalý toouse hello gulp úloha číst zprávy tooread zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="714a1-109">In this article, you will learn how toouse hello gulp read-message task tooread messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="714a1-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="714a1-110">What you need</span></span>
<span data-ttu-id="714a1-111">Před zahájením tohoto procesu se musí úspěšně jste dokončili [spustit hello Azure blikání ukázkovou aplikaci na vaší kartě Arduino][run-blink-application].</span><span class="sxs-lookup"><span data-stu-id="714a1-111">Before starting this process, you must have successfully completed [Run hello Azure blink sample application on your Arduino board][run-blink-application].</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="714a1-112">Přečtěte si nové zprávy z vašeho účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="714a1-112">Read new messages from your storage account</span></span>
<span data-ttu-id="714a1-113">V předchozím článku hello jste spustili ukázkovou aplikaci na vaší kartě Arduino.</span><span class="sxs-lookup"><span data-stu-id="714a1-113">In hello previous article, you ran a sample application on your Arduino board.</span></span> <span data-ttu-id="714a1-114">Ukázková aplikace Hello odeslané zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="714a1-114">hello sample application sent messages tooyour Azure IoT hub.</span></span> <span data-ttu-id="714a1-115">Hello zprávy odeslané tooyour IoT hub jsou uloženy do úložiště Azure Table prostřednictvím aplikace Azure funkce hello.</span><span class="sxs-lookup"><span data-stu-id="714a1-115">hello messages sent tooyour IoT hub are stored into your Azure Table storage via hello Azure function app.</span></span> <span data-ttu-id="714a1-116">Je nutné hello úložiště Azure připojovací řetězec tooread zprávy z Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="714a1-116">You need hello Azure storage connection string tooread messages from your Azure Table storage.</span></span>

<span data-ttu-id="714a1-117">zprávy tooread uložené ve službě Azure Table storage, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="714a1-117">tooread messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="714a1-118">Spustit hello následující příkazy a získáte hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="714a1-118">Get hello connection string by running hello following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="714a1-119">První příkaz Hello načte hello `storage name` , který se používá v hello druhý příkaz tooget hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="714a1-119">hello first command retrieves hello `storage name` that is used in hello second command tooget hello connection string.</span></span> <span data-ttu-id="714a1-120">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="714a1-120">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
2. <span data-ttu-id="714a1-121">Konfigurační soubor otevřete hello `config-arduino.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="714a1-121">Open hello configuration file `config-arduino.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. <span data-ttu-id="714a1-122">Nahraďte `[Azure storage connection string]` s řetězcem připojení hello jste získali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="714a1-122">Replace `[Azure storage connection string]` with hello connection string you got in step 1.</span></span>
4. <span data-ttu-id="714a1-123">Uložit hello `config-arduino.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="714a1-123">Save hello `config-arduino.json` file.</span></span>
5. <span data-ttu-id="714a1-124">Odesílání zpráv znovu a číst je z Azure Table storage spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="714a1-124">Send messages again and read them from your Azure Table storage by running hello following command:</span></span>

   ```bash
   gulp run --read-storage

   # You can monitor hello serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   <span data-ttu-id="714a1-125">Hello logiku pro čtení z úložiště tabulek Azure je v hello `azure-table.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="714a1-125">hello logic for reading from Azure Table storage is in hello `azure-table.js` file.</span></span>

   ![spuštění – gulp úložiště pro čtení][gulp-run]

## <a name="summary"></a><span data-ttu-id="714a1-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="714a1-127">Summary</span></span>
<span data-ttu-id="714a1-128">Úspěšně jste připojené centra IoT tooyour Tabule Arduino hello cloudu a použít hello blikání ukázkové aplikace toosend zařízení cloud zprávy.</span><span class="sxs-lookup"><span data-stu-id="714a1-128">You've successfully connected your Arduino board tooyour IoT hub in hello cloud and used hello blink sample application toosend device-to-cloud messages.</span></span> <span data-ttu-id="714a1-129">Můžete také použít hello Azure funkce aplikace toostore příchozí IoT hub zprávy tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="714a1-129">You also used hello Azure function app toostore incoming IoT hub messages tooyour Azure Table storage.</span></span> <span data-ttu-id="714a1-130">Můžete teď odesílání zpráv typu cloud zařízení z vaší IoT hub tooyour Arduino panelu.</span><span class="sxs-lookup"><span data-stu-id="714a1-130">You can now send cloud-to-device messages from your IoT hub tooyour Arduino board.</span></span>

## <a name="next-steps"></a><span data-ttu-id="714a1-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="714a1-131">Next steps</span></span>
<span data-ttu-id="714a1-132">[Odesílání zpráv typu cloud zařízení][send-cloud-to-device-messages]</span><span class="sxs-lookup"><span data-stu-id="714a1-132">[Send cloud-to-device messages][send-cloud-to-device-messages]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md