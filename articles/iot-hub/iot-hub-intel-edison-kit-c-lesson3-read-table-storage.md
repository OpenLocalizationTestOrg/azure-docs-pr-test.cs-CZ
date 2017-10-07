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
# <a name="read-messages-persisted-in-azure-storage"></a>Čtení zprávy uchovávané v úložišti Azure
## <a name="what-you-will-do"></a>Co provedete
Zprávy typu zařízení cloud hello monitorování odesílaných ze služby IoT hub Intel Edison tooyour jako hello zprávy jsou zapsány tooyour Azure Table storage. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte, jak trvalý toouse hello gulp úloha číst zprávy tooread zpráv ve službě Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete
Před zahájením tohoto procesu se musí úspěšně jste dokončili [hello Azure blikání ukázkovou aplikaci spustili Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].

## <a name="read-new-messages-from-your-storage-account"></a>Přečtěte si nové zprávy z vašeho účtu úložiště
V předchozím článku hello jste spustili ukázkovou aplikaci na Edison. Ukázková aplikace Hello odeslané zprávy tooyour Azure IoT hub. Hello zprávy odeslané tooyour IoT hub jsou uloženy do úložiště Azure Table prostřednictvím aplikace Azure funkce hello. Je nutné hello úložiště Azure připojovací řetězec tooread zprávy z Azure Table storage.

zprávy tooread uložené ve službě Azure Table storage, postupujte takto:

1. Spustit hello následující příkazy a získáte hello připojovací řetězec:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   První příkaz Hello načte hello `storage name` , který se používá v hello druhý příkaz tooget hello připojovací řetězec. Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.
2. Konfigurační soubor otevřete hello `config-edison.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. Nahraďte `[Azure storage connection string]` s řetězcem připojení hello jste získali v kroku 1.
4. Uložit hello `config-edison.json` souboru.
5. Odesílání zpráv znovu a číst je z Azure Table storage spuštěním hello následující příkaz:

   ```bash
   gulp run --read-storage
   ```

   Hello logiku pro čtení z úložiště tabulek Azure je v hello `azure-table.js` souboru.

   ![spuštění – gulp úložiště pro čtení][gulp run]

## <a name="summary"></a>Souhrn
Úspěšně jste připojené Edison tooyour IoT hub v hello cloudu a použít hello blikání ukázkové aplikace toosend zařízení cloud zprávy. Můžete také použít hello Azure funkce aplikace toostore příchozí IoT hub zprávy tooyour Azure Table storage. Můžete teď odesílání zpráv typu cloud zařízení z vaší tooEdison centra IoT.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message_c.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md