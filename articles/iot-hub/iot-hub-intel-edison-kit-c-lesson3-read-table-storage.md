---
title: "Connect Intel Edison (C) k Azure IoT - Lekce 3: sledování zpráv | Microsoft Docs"
description: "Monitorování zprávy typu zařízení cloud, jako jsou zapsány do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
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
ms.openlocfilehash: 249b5e0e96051fa2adeedfb9befd98fc939b4d40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Čtení zprávy uchovávané v úložišti Azure
## <a name="what-you-will-do"></a>Co provedete
Monitorování zprávy typu zařízení cloud, které jsou odesílány z Intel Edison do služby IoT hub jako zprávy jsou zapsány do Azure Table storage. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte, jak pomocí úloh pro čtení zpráv gulp číst zprávy uchovávané v Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete
Před zahájením tohoto procesu se musí úspěšně jste dokončili [spuštění ukázkové aplikace Azure blikání na Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].

## <a name="read-new-messages-from-your-storage-account"></a>Přečtěte si nové zprávy z vašeho účtu úložiště
V předchozím článku jste spustili ukázkovou aplikaci na Edison. Ukázkové aplikace odeslané zprávy do služby Azure IoT hub. Zprávy odeslané do služby IoT hub jsou uloženy do úložiště Azure Table prostřednictvím Azure funkce aplikace. Je třeba připojovacího řetězce úložiště Azure pro čtení zpráv z Azure Table storage.

Umožní číst zprávy, které jsou uložené ve službě Azure Table storage, postupujte takto:

1. Spustit následující příkazy a získáte připojovací řetězec:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Načte první příkaz `storage name` který se používá v druhém příkazu získat připojovací řetězec. Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.
2. Otevřete konfigurační soubor `config-edison.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. Nahraďte `[Azure storage connection string]` připojovacím řetězcem, který jste získali v kroku 1.
4. Uložit `config-edison.json` souboru.
5. Odesílání zpráv znovu a číst je z Azure Table storage spuštěním následujícího příkazu:

   ```bash
   gulp run --read-storage
   ```

   Probíhá logiku pro čtení z úložiště tabulek Azure `azure-table.js` souboru.

   ![spuštění – gulp úložiště pro čtení][gulp run]

## <a name="summary"></a>Souhrn
Úspěšně jste připojený Edison do služby IoT hub v cloudu a používá ukázkovou aplikaci blikání k odesílání zpráv typu zařízení cloud. Aplikace Azure funkce se také používají k ukládání příchozích zpráv IoT hub do úložiště Azure Table. Můžete teď odesílání zpráv typu cloud zařízení z centra IoT do Edison.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md