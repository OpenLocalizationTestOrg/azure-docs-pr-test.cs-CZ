---
title: "Simulované zařízení & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
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
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

## <a name="what-you-will-do"></a>Co provedete

- Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak toouse hello gulp nástroj tooread zprávy ze služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Ukázka simulované zařízení Hello v [konfigurace a spustit Simulovaná zařízení cloud nahrát ukázková aplikace](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce

Hello zařízení připojovací řetězec se používá ve službě IoT hub tooyour tooconnect simulované zařízení. Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.

- Seznam všech centra IoT ve vaší skupině prostředků spuštěním hello následující příkaz:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud nebylo ji změnit.
- Získáte hello IoT hub, připojovací řetězec tak, že spustíte následující příkaz hello:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`je hello název, který jste zadali v lekci 2.

## <a name="configure-hello-device-connection-for-hello-sample-code"></a>Konfigurace připojení k zařízení hello pro hello ukázkový kód

Aktualizovat IoT hub a zařízení konfigurace připojení v `config-azure.json` provedením hello následující kroky:

1. Otevřete `config-azure.json` ve Visual Studio Code tak, že spustíte následující příkaz v okně konzoly hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Ujistěte se, hello následující nahrazení v hello `config-azure.json` souboru:

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Nahraďte `[IoT hub connection string]` s hello IoT hub připojovací řetězec.

## <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

Spuštění ukázkové aplikace hello simulované zařízení a číst zprávy služby IoT Hub hello následující příkaz:

```bash
gulp run --iot-hub
```

příkaz Hello spustí hello aplikace, která odesílá zprávy tooyour IoT hub každé 2 sekundy. Vytvoří také zprávu podřízený proces tooreceive hello.

Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač. aplikace Hello bude ukončena v 40 sekund.

![Simulované ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a>Souhrn

Úspěšně jste spuštění hello ukázkové aplikace toosend dat tooyour IoT hub pomocí simulovaného zařízení. Také jste si přečetli hello zprávy, které byly odeslány tooyour IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření Azure Function App a účtu Azure Storage](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


