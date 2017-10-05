---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 45f3595c4848d5c283cdf95604adf8d2c8d6a809
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

## <a name="what-you-will-do"></a>Co provedete

- Spustíte ukázkový kód v hostitelském počítači umožní číst zprávy ze služby IoT hub.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak používat nástroj gulp ke čtení zpráv ze služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Zakázat ukázkovou aplikaci, která jste v lekci 3 proběhla úspěšně.

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce

Zařízení připojovací řetězec se používá zařízením (TI SensorTag nebo simulované zařízení) pro připojení do služby IoT hub. IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub.

- Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.
- Získáte připojovací řetězec centra IoT tak, že spustíte následující příkaz:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`je název, který jste zadali v lekci 2.

## <a name="configure-the-device-connection-for-the-sample-code"></a>Konfigurace připojení zařízení pro ukázkového kódu

Aktualizovat konfigurační soubor zařízení `config-azure.json` tak, aby bylo možné číst zprávy ze služby IoT hub v hostitelském počítači. Chcete-li to provést, postupujte takto:

1. Otevřete `config-azure.json` ve Visual Studio Code spuštěním následujícího příkazu v okně konzoly:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Zkontrolujte následující nahrazení v `config-azure.json` souboru:

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Nahraďte `[IoT hub connection string]` s IoT hub připojovací řetězec, který jste získali.

## <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

Pokud máte TI SensorTag, ujistěte se, že máte už zapnutý vaší SensorTag. Spuštění ukázkové aplikace brány a číst zprávy služby IoT Hub následující příkaz:

```bash
gulp run --iot-hub
```

Příkaz spustí zakázat ukázkovou aplikaci, která čte a balíčky teploty data z SensorTag nebo simulované zařízení a odešle zprávu do služby IoT hub každé 2 sekundy. Vytvoří také podřízený proces zpráva.

Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači. Instance aplikace na ukázkové přeruší automaticky v 40 sekund.

![Zakázat ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a>Souhrn

Spustíte ukázkový kód ke čtení zpráv ze služby IoT hub. Jste připravení číst zprávy, které jsou uložené ve službě Azure table storage.

## <a name="next-steps"></a>Další kroky
[Vytvoření Azure Function App a účtu Azure Storage](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


