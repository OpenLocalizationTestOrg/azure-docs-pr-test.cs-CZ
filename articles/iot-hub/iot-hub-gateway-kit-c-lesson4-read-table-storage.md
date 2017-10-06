---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 4: Table storage | Microsoft Docs"
description: "Uložení zpráv ze služby IoT hub Intel NUC tooyour, zapisuje je úložiště Table tooAzure a číst je z cloudu hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "načtení dat z cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 8ca78045-ad92-4a6a-90f1-05f9668e6f0e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 29525b084eb4d6e6dfcb16d9b34f78f075d30b7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Čtení zprávy uchovávané v úložišti Azure Table

## <a name="what-you-will-do"></a>Co provedete

- Spusťte hello brány ukázkovou aplikaci na bránu, která odesílá zprávy tooyour IoT hub.
- Spusťte ukázkový kód na vaši hostitele počítače tooread hello zpráv ve službě Azure Table storage. 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak toouse hello gulp nástroj toorun hello ukázkový kód tooread zpráv ve službě Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete

Úspěšně jste hello následující úlohy:

- [Vytvořit aplikaci Azure funkce hello a účet úložiště Azure hello](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).
- [Spuštění ukázkové aplikace hello brány](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).
- [Čtení zpráv z centra IoT](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Získat vaše připojovacích řetězců Azure storage

Časná v této lekci úspěšně jste vytvořili účet úložiště Azure. tooget hello připojovací řetězec hello účtu úložiště Azure, spusťte následující příkazy hello:

* Zobrazí seznam všech účtů úložišť.

```bash
az storage account list -g iot-gateway --query [].name
```

* Získáte připojovací řetězec úložiště azure.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Použít iot brány jako hodnotu hello `{resource group name}` Pokud hodnota hello v lekci 2 nebyla změněna.

## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello

Aktualizace hello `config-azure.json` tak, aby hello ukázkový kód, který běží v hostitelském počítači hello může číst zprávy ve službě Azure Table storage. tooconfigure hello připojení zařízení, postupujte takto:

1. Soubor konfigurace zařízení otevřete hello `config-azure.json` spuštěním hello následující příkazy:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfigurace](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Nahraďte `[Azure storage connection string]` s hello připojovací řetězec úložiště Azure, který jste získali.

   `[IoT hub connection string]`by měla být nahrazená již v části [čtení zpráv z Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) v Lesson3.

## <a name="read-messages-in-your-azure-table-storage"></a>Čtení zpráv ve službě Azure Table storage

Spuštění ukázkové aplikace hello brány a číst zprávy úložiště Azure Table hello následující příkaz:

```bash
gulp run --table-storage
```

Služby IoT hub se aktivuje zprávu toosave aplikace funkce Azure do Azure Table storage při přijetí nové zprávy.
Hello `gulp run` příkaz spustí brány ukázkovou aplikaci, která odesílá zprávy tooyour IoT hub. S `table-storage` parametr, je také vytvoří hello podřízený proces tooreceive uložit zpráv ve službě Azure Table storage.

Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač. instance aplikace Hello ukázka přeruší automaticky v 40 sekund.

   ![gulp pro čtení](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


## <a name="summary"></a>Souhrn

Spustíte jste hello ukázkový kód tooread hello zpráv ve službě Azure Table storage, uloží prostřednictvím funkce Azure aplikace.
