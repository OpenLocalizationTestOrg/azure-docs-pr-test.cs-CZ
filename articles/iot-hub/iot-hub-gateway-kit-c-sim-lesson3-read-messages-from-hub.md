---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fbf7958e2437d274f2692dbc235ac8147bdfa63
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

## <a name="what-you-will-do"></a>Co provedete

- Spustíte ukázkový kód v hostitelském počítači umožní číst zprávy ze služby IoT hub.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak používat nástroj gulp ke čtení zpráv ze služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Ukázka simulované zařízení v [konfigurace a spustit Simulovaná zařízení cloud nahrát ukázková aplikace](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce

Zařízení připojovací řetězec se používá simulovaným zařízením pro připojení do služby IoT hub. IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub.

- Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo ji změnit.
- Získáte připojovací řetězec centra IoT tak, že spustíte následující příkaz:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`je název, který jste zadali v lekci 2.

## <a name="configure-the-device-connection-for-the-sample-code"></a>Konfigurace připojení zařízení pro ukázkového kódu

Aktualizovat IoT hub a zařízení konfigurace připojení v `config-azure.json` provedením následujících kroků:

1. Otevřete `config-azure.json` ve Visual Studio Code spuštěním následujícího příkazu v okně konzoly:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Zkontrolujte následující nahrazení v `config-azure.json` souboru:

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Nahraďte `[IoT hub connection string]` připojovacím řetězcem IoT hub.

## <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

Spuštění ukázkové aplikace simulovaného zařízení a číst zprávy služby IoT Hub následující příkaz:

```bash
gulp run --iot-hub
```

Příkaz spustí aplikaci, která odesílá zprávy do služby IoT hub každé 2 sekundy. Vytvoří také podřízený proces zpráva.

Zprávy, které jsou odesílány a přijaté jsou všechny okamžitě zobrazí v okně konzoly v hostitelském počítači. Aplikace se zavře v 40 sekund.

![Simulované ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a>Souhrn

Úspěšně jste spusťte ukázkovou aplikaci k odesílání dat do služby IoT hub s simulované zařízení. Také jste si přečetli zprávy, které byly odeslány do služby IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření Azure Function App a účtu Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


