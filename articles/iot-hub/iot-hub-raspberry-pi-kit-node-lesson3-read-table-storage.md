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
# <a name="read-messages-persisted-in-azure-storage"></a>Čtení zprávy uchovávané v úložišti Azure
## <a name="what-you-will-do"></a>Co provedete
Monitorování zprávy typu zařízení cloud, které jsou odesílány z malin pí 3 do služby IoT hub jako zprávy jsou zapsány do Azure Table storage. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte, jak pomocí úloh pro čtení zpráv gulp číst zprávy uchovávané v Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete
Před zahájením tohoto procesu se musí úspěšně jste dokončili [spuštění ukázkové aplikace Azure blikání na malin pí 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="read-new-messages-from-your-storage-account"></a>Přečtěte si nové zprávy z vašeho účtu úložiště
V předchozím článku jste spustili ukázkovou aplikaci na pí. Ukázkové aplikace odeslané zprávy do služby Azure IoT hub. Zprávy odeslané do služby IoT hub jsou uloženy do úložiště Azure Table prostřednictvím Azure funkce aplikace. Je třeba připojovacího řetězce úložiště Azure pro čtení zpráv z Azure Table storage.

Umožní číst zprávy, které jsou uložené ve službě Azure Table storage, postupujte takto:

1. Spustit následující příkazy a získáte připojovací řetězec:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Načte první příkaz `storage name` který se používá v druhém příkazu získat připojovací řetězec. Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.
2. Otevřete konfigurační soubor `config-raspberrypi.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Nahraďte `[Azure storage connection string]` připojovacím řetězcem, který jste získali v kroku 1.
4. Uložit `config-raspberrypi.json` souboru.
5. Odesílání zpráv znovu a číst je z Azure Table storage spuštěním následujícího příkazu:
   
   ```bash
   gulp run --read-storage
   ```
   
   Probíhá logiku pro čtení z úložiště tabulek Azure `azure-table.js` souboru.
   
    ![spuštění – gulp úložiště pro čtení](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a>Souhrn
Úspěšně jste připojený pí do služby IoT hub v cloudu a používá ukázkovou aplikaci blikání k odesílání zpráv typu zařízení cloud. Aplikace Azure funkce se také používají k ukládání příchozích zpráv IoT hub do úložiště Azure Table. Pi nyní možné odesílat zprávy typu cloud zařízení ze služby IoT hub.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

