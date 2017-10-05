---
title: "Simulované zařízení & brány Azure IoT - Lekce 4: Table storage | Microsoft Docs"
description: "Ukládat zprávy z Intel NUC do služby IoT hub, zapisuje je do úložiště Azure Table a číst je z cloudu."
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
ms.openlocfilehash: de5fae794c195132e2a487c0095845c756aa28e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a><span data-ttu-id="632eb-104">Čtení zprávy uchovávané v úložišti Azure Table</span><span class="sxs-lookup"><span data-stu-id="632eb-104">Read messages persisted in Azure Table storage</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="632eb-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="632eb-105">What you will do</span></span>

- <span data-ttu-id="632eb-106">Spuštění ukázkové aplikace brány na bránu, která odesílá zprávy do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="632eb-106">Run the gateway sample application on your gateway that sends messages to your IoT hub.</span></span>
- <span data-ttu-id="632eb-107">Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="632eb-107">Run sample code on your host computer to read messages in your Azure Table storage.</span></span>

<span data-ttu-id="632eb-108">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="632eb-108">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="632eb-109">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="632eb-109">What you will learn</span></span>

<span data-ttu-id="632eb-110">Jak používat nástroj gulp spustit ukázkový kód ke čtení zpráv ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="632eb-110">How to use the gulp tool to run the sample code to read messages in your Azure Table storage.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="632eb-111">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="632eb-111">What you need</span></span>

<span data-ttu-id="632eb-112">Úspěšně jste provést následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="632eb-112">You have have successfully done the following tasks:</span></span>

- <span data-ttu-id="632eb-113">[Vytvoření účtu úložiště Azure a Azure funkce aplikace](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="632eb-113">[Created the Azure function app and the Azure storage account](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md).</span></span>
- <span data-ttu-id="632eb-114">[Spuštění ukázkové aplikace brány](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span><span class="sxs-lookup"><span data-stu-id="632eb-114">[Run the gateway sample application](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).</span></span>
- <span data-ttu-id="632eb-115">[Čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span><span class="sxs-lookup"><span data-stu-id="632eb-115">[Read messages from your IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md).</span></span>

## <a name="get-your-azure-storage-connection-strings"></a><span data-ttu-id="632eb-116">Získat vaše připojovacích řetězců Azure storage</span><span class="sxs-lookup"><span data-stu-id="632eb-116">Get your Azure storage connection strings</span></span>

<span data-ttu-id="632eb-117">Časná v této lekci úspěšně jste vytvořili účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="632eb-117">Early in this lesson, you successfully created an Azure storage account.</span></span> <span data-ttu-id="632eb-118">Chcete-li získat připojovací řetězec účtu úložiště Azure, spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="632eb-118">To get the connection string of the Azure storage account, run the following commands:</span></span>

* <span data-ttu-id="632eb-119">Zobrazí seznam všech účtů úložišť.</span><span class="sxs-lookup"><span data-stu-id="632eb-119">List all your storage accounts.</span></span>

```bash
az storage account list -g iot-gateway --query [].name
```

* <span data-ttu-id="632eb-120">Získáte připojovací řetězec úložiště azure.</span><span class="sxs-lookup"><span data-stu-id="632eb-120">Get azure storage connection string.</span></span>

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

<span data-ttu-id="632eb-121">Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu v lekci 2.</span><span class="sxs-lookup"><span data-stu-id="632eb-121">Use `iot-gateway` as the value of `{resource group name}` if you didn't change the value in Lesson 2.</span></span>

## <a name="configure-the-device-connection"></a><span data-ttu-id="632eb-122">Nakonfigurujte připojení zařízení</span><span class="sxs-lookup"><span data-stu-id="632eb-122">Configure the device connection</span></span>

<span data-ttu-id="632eb-123">Aktualizace `config-azure.json` tak, aby ukázkový kód, který běží v hostitelském počítači může číst zprávy ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="632eb-123">Update the `config-azure.json` file so that the sample code that runs on the host computer can read message in your Azure Table storage.</span></span> <span data-ttu-id="632eb-124">Konfigurace připojení zařízení, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="632eb-124">To configure the device connection, follow these steps:</span></span>

1. <span data-ttu-id="632eb-125">Otevřete konfigurační soubor zařízení `config-azure.json` spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="632eb-125">Open the device configuration file `config-azure.json` by running the following commands:</span></span>

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfigurace](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. <span data-ttu-id="632eb-127">Nahraďte `[Azure storage connection string]` připojovacím řetězcem úložiště Azure, který jste získali.</span><span class="sxs-lookup"><span data-stu-id="632eb-127">Replace `[Azure storage connection string]` with the Azure storage connection string that you obtained.</span></span>

   <span data-ttu-id="632eb-128">`[IoT hub connection string]`by měla být nahrazená již v části [čtení zpráv z Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) v Lesson3.</span><span class="sxs-lookup"><span data-stu-id="632eb-128">`[IoT hub connection string]` should already be replaced in section [Read messages from Azure IoT Hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md) in Lesson3.</span></span>

## <a name="read-messages-in-your-azure-table-storage"></a><span data-ttu-id="632eb-129">Čtení zpráv ve službě Azure Table storage</span><span class="sxs-lookup"><span data-stu-id="632eb-129">Read messages in your Azure Table storage</span></span>

<span data-ttu-id="632eb-130">Spuštění ukázkové aplikace brány a čtení zpráv úložiště Azure Table pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="632eb-130">Run the gateway sample application and read Azure Table storage messages by the following command:</span></span>

```bash
gulp run --table-storage
```

<span data-ttu-id="632eb-131">Služby IoT hub aktivuje aplikaci funkce Azure pro uložení zpráv do úložiště Azure Table, pokud dorazí novou zprávu.</span><span class="sxs-lookup"><span data-stu-id="632eb-131">Your IoT hub triggers your Azure Function application to save message into your Azure Table storage when new message arrives.</span></span>
<span data-ttu-id="632eb-132">`gulp run` Příkaz spustí brány ukázkovou aplikaci, která odesílá zprávy do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="632eb-132">The `gulp run` command runs gateway sample application that sends messages to your IoT hub.</span></span> <span data-ttu-id="632eb-133">S `table-storage` parametr, je také vytvoří podřízený proces pro příjem uložené zprávy ve službě Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="632eb-133">With `table-storage` parameter, it also spawns a child process to receive the saved message in your Azure Table storage.</span></span>

<span data-ttu-id="632eb-134">Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="632eb-134">The messages that are being sent and received are all displayed instantly on the same console window in the host machine.</span></span> <span data-ttu-id="632eb-135">Instance aplikace na ukázkové přeruší automaticky v 40 sekund.</span><span class="sxs-lookup"><span data-stu-id="632eb-135">The sample application instance will terminate automatically in 40 seconds.</span></span>

   ![gulp pro čtení](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table_simudev.png)


## <a name="summary"></a><span data-ttu-id="632eb-137">Souhrn</span><span class="sxs-lookup"><span data-stu-id="632eb-137">Summary</span></span>

<span data-ttu-id="632eb-138">Spustíte ukázkový kód ke čtení zpráv ve službě Azure Table storage, uloží prostřednictvím funkce Azure aplikace.</span><span class="sxs-lookup"><span data-stu-id="632eb-138">You've run the sample code to read the messages in your Azure Table storage saved by your Azure Function application.</span></span>
