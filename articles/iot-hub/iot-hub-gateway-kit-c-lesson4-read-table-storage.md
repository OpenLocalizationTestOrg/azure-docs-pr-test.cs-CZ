---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 4: Table storage | Microsoft Docs"
description: "Ukládat zprávy z Intel NUC do služby IoT hub, zapisuje je do úložiště Azure Table a číst je z cloudu."
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
ms.openlocfilehash: 72659ef3a7fd2f6011590d37176fd05503269aff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-table-storage"></a>Čtení zprávy uchovávané v úložišti Azure Table

## <a name="what-you-will-do"></a>Co provedete

- Spuštění ukázkové aplikace brány na bránu, která odesílá zprávy do služby IoT hub.
- Potom spusťte ukázkový kód v hostitelském počítači ke čtení zpráv ve službě Azure Table storage. 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak používat nástroj gulp spustit ukázkový kód ke čtení zpráv ve službě Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete

Úspěšně jste provést následující úlohy:

- [Vytvoření účtu úložiště Azure a Azure funkce aplikace](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md).
- [Spuštění ukázkové aplikace brány](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md).
- [Čtení zpráv z centra IoT](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md).

## <a name="get-your-azure-storage-connection-strings"></a>Získat vaše připojovacích řetězců Azure storage

Časná v této lekci úspěšně jste vytvořili účet úložiště Azure. Chcete-li získat připojovací řetězec účtu úložiště Azure, spusťte následující příkazy:

* Zobrazí seznam všech účtů úložišť.

```bash
az storage account list -g iot-gateway --query [].name
```

* Získáte připojovací řetězec úložiště azure.

```bash
az storage account show-connection-string -g iot-gateway -n {storage name}
```

Použít iot brány jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu v lekci 2.

## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení

Aktualizace `config-azure.json` tak, aby ukázkový kód, který běží v hostitelském počítači může číst zprávy ve službě Azure Table storage. Konfigurace připojení zařízení, postupujte takto:

1. Otevřete konfigurační soubor zařízení `config-azure.json` spuštěním následujících příkazů:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

   ![konfigurace](media/iot-hub-gateway-kit-lessons/lesson4/config_azure.png)

2. Nahraďte `[Azure storage connection string]` připojovacím řetězcem úložiště Azure, který jste získali.

   `[IoT hub connection string]`by měla být nahrazená již v části [čtení zpráv z Azure IoT Hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md) v Lesson3.

## <a name="read-messages-in-your-azure-table-storage"></a>Čtení zpráv ve službě Azure Table storage

Spuštění ukázkové aplikace brány a čtení zpráv úložiště Azure Table pomocí následujícího příkazu:

```bash
gulp run --table-storage
```

Služby IoT hub aktivuje aplikaci funkce Azure pro uložení zpráv do úložiště Azure Table, pokud dorazí novou zprávu.
`gulp run` Příkaz spustí brány ukázkovou aplikaci, která odesílá zprávy do služby IoT hub. S `table-storage` parametr, je také vytvoří podřízený proces pro příjem uložené zprávy ve službě Azure Table storage.

Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači. Instance aplikace na ukázkové přeruší automaticky v 40 sekund.

   ![gulp pro čtení](media/iot-hub-gateway-kit-lessons/lesson4/gulp_run_read_table.png)


## <a name="summary"></a>Souhrn

Spustíte ukázkový kód ke čtení zpráv ve službě Azure Table storage, uloží prostřednictvím funkce Azure aplikace.