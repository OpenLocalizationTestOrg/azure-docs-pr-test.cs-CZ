---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 3: sledování zpráv | Microsoft Docs"
description: "Zprávy typu zařízení cloud hello sledovat, jak jsou napsány tooyour Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: cad545c3-dd88-486c-a663-d587a924ccd4
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2679b22f2987f77ecd1eea03044ed8ea03bf73f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a><span data-ttu-id="e1c0e-104">Čtení zprávy uchovávané v úložišti Azure</span><span class="sxs-lookup"><span data-stu-id="e1c0e-104">Read messages persisted in Azure Storage</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="e1c0e-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e1c0e-105">What you will do</span></span>
<span data-ttu-id="e1c0e-106">Zprávy typu zařízení cloud hello monitorování odesílaných ze služby IoT hub Intel Edison tooyour jako hello zprávy jsou zapsány tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-106">Monitor hello device-to-cloud messages that are sent from Intel Edison tooyour IoT hub as hello messages are written tooyour Azure Table storage.</span></span> <span data-ttu-id="e1c0e-107">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="e1c0e-107">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e1c0e-108">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e1c0e-108">What you will learn</span></span>
<span data-ttu-id="e1c0e-109">V tomto článku se dozvíte, jak trvalý toouse hello gulp úloha číst zprávy tooread zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-109">In this article, you will learn how toouse hello gulp read-message task tooread messages persisted in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e1c0e-110">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e1c0e-110">What you need</span></span>
<span data-ttu-id="e1c0e-111">Před zahájením tohoto procesu se musí úspěšně jste dokončili [hello Azure blikání ukázkovou aplikaci spustili Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].</span><span class="sxs-lookup"><span data-stu-id="e1c0e-111">Before starting this process, you must have successfully completed [Run hello Azure blink sample application on Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].</span></span>

## <a name="read-new-messages-from-your-storage-account"></a><span data-ttu-id="e1c0e-112">Přečtěte si nové zprávy z vašeho účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="e1c0e-112">Read new messages from your storage account</span></span>
<span data-ttu-id="e1c0e-113">V předchozím článku hello jste spustili ukázkovou aplikaci na Edison.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-113">In hello previous article, you ran a sample application on Edison.</span></span> <span data-ttu-id="e1c0e-114">Ukázková aplikace Hello odeslané zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-114">hello sample application sent messages tooyour Azure IoT hub.</span></span> <span data-ttu-id="e1c0e-115">Hello zprávy odeslané tooyour IoT hub jsou uloženy do úložiště Azure Table prostřednictvím aplikace Azure funkce hello.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-115">hello messages sent tooyour IoT hub are stored into your Azure Table storage via hello Azure function app.</span></span> <span data-ttu-id="e1c0e-116">Je nutné hello úložiště Azure připojovací řetězec tooread zprávy z Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-116">You need hello Azure storage connection string tooread messages from your Azure Table storage.</span></span>

<span data-ttu-id="e1c0e-117">zprávy tooread uložené ve službě Azure Table storage, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e1c0e-117">tooread messages stored in your Azure Table storage, follow these steps:</span></span>

1. <span data-ttu-id="e1c0e-118">Spustit hello následující příkazy a získáte hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="e1c0e-118">Get hello connection string by running hello following commands:</span></span>

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   <span data-ttu-id="e1c0e-119">První příkaz Hello načte hello `storage name` , který se používá v hello druhý příkaz tooget hello připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-119">hello first command retrieves hello `storage name` that is used in hello second command tooget hello connection string.</span></span> <span data-ttu-id="e1c0e-120">Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-120">Use `iot-sample` as hello value of `{resource group name}` if you didn't change hello value.</span></span>
2. <span data-ttu-id="e1c0e-121">Konfigurační soubor otevřete hello `config-edison.json` ve Visual Studio Code spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e1c0e-121">Open hello configuration file `config-edison.json` in Visual Studio Code by running hello following command:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. <span data-ttu-id="e1c0e-122">Nahraďte `[Azure storage connection string]` s řetězcem připojení hello jste získali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-122">Replace `[Azure storage connection string]` with hello connection string you got in step 1.</span></span>
4. <span data-ttu-id="e1c0e-123">Uložit hello `config-edison.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-123">Save hello `config-edison.json` file.</span></span>
5. <span data-ttu-id="e1c0e-124">Odesílání zpráv znovu a číst je z Azure Table storage spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e1c0e-124">Send messages again and read them from your Azure Table storage by running hello following command:</span></span>

   ```bash
   gulp run --read-storage
   ```

   <span data-ttu-id="e1c0e-125">Hello logiku pro čtení z úložiště tabulek Azure je v hello `azure-table.js` souboru.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-125">hello logic for reading from Azure Table storage is in hello `azure-table.js` file.</span></span>

   ![spuštění – gulp úložiště pro čtení][gulp run]

## <a name="summary"></a><span data-ttu-id="e1c0e-127">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e1c0e-127">Summary</span></span>
<span data-ttu-id="e1c0e-128">Úspěšně jste připojené Edison tooyour IoT hub v hello cloudu a použít hello blikání ukázkové aplikace toosend zařízení cloud zprávy.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-128">You've successfully connected Edison tooyour IoT hub in hello cloud and used hello blink sample application toosend device-to-cloud messages.</span></span> <span data-ttu-id="e1c0e-129">Můžete také použít hello Azure funkce aplikace toostore příchozí IoT hub zprávy tooyour Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-129">You also used hello Azure function app toostore incoming IoT hub messages tooyour Azure Table storage.</span></span> <span data-ttu-id="e1c0e-130">Můžete teď odesílání zpráv typu cloud zařízení z vaší tooEdison centra IoT.</span><span class="sxs-lookup"><span data-stu-id="e1c0e-130">You can now send cloud-to-device messages from your IoT hub tooEdison.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1c0e-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e1c0e-131">Next steps</span></span>
<span data-ttu-id="e1c0e-132">[Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení][receive-cloud-to-device-messages]</span><span class="sxs-lookup"><span data-stu-id="e1c0e-132">[Run a sample application tooreceive cloud-to-device messages][receive-cloud-to-device-messages]</span></span>
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md