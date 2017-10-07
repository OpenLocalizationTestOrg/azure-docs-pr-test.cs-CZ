---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 3: Přečtěte si zprávy | Microsoft Docs"
description: "Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "data v cloudu hello, shromažďování dat v cloudu, Cloudová služba iot, iot dat"
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
ms.openlocfilehash: d3ffbe2e83f9d61c0088b8876a7f0eea62c1fbe1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

## <a name="what-you-will-do"></a>Co provedete

- Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

Jak toouse hello gulp nástroj tooread zprávy ze služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Hello zakázat ukázkovou aplikaci, která jste v lekci 3 proběhla úspěšně.

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce

Hello zařízení připojovací řetězec se používá ve službě IoT hub tooyour tooconnect zařízení (TI SensorTag nebo simulované zařízení). Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub.

- Seznam všech centra IoT ve vaší skupině prostředků spuštěním hello následující příkaz:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.
- Získáte hello IoT hub, připojovací řetězec tak, že spustíte následující příkaz hello:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`je hello název, který jste zadali v lekci 2.

## <a name="configure-hello-device-connection-for-hello-sample-code"></a>Konfigurace připojení k zařízení hello pro hello ukázkový kód

Aktualizace hello zařízení konfigurační soubor `config-azure.json` tak, aby bylo možné číst zprávy ze služby IoT hub v hostitelském počítači. toodo, postupujte takto:

1. Otevřete `config-azure.json` ve Visual Studio Code tak, že spustíte následující příkaz v okně konzoly hello:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Ujistěte se, hello následující nahrazení v hello `config-azure.json` souboru:

   ![snímek obrazovky konfigurace azure](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Nahraďte `[IoT hub connection string]` s hello IoT hub připojovací řetězec, který jste získali.

## <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT

Pokud máte TI SensorTag, ujistěte se, že máte už zapnutý vaší SensorTag. Spuštění ukázkové aplikace hello brány a číst zprávy služby IoT Hub hello následující příkaz:

```bash
gulp run --iot-hub
```

příkaz Hello spustí hello zakázat ukázkovou aplikaci, která čte a balíčky teploty data z SensorTag nebo simulované zařízení a odešle zprávu hello tooyour služby IoT hub každé 2 sekundy. Vytvoří také zprávu podřízený proces tooreceive hello.

Hello zprávy, které jsou odesílány a přijaté jsou všechny zobrazené okamžitě na hello stejné konzole okno v hello hostitelský počítač. instance aplikace Hello ukázka přeruší automaticky v 40 sekund.

![Zakázat ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a>Souhrn

Zprávy ukázkový kód tooread jste spustit ze služby IoT hub. Jste připravené tooread hello zprávy, které jsou uložené ve službě Azure table storage.

## <a name="next-steps"></a>Další kroky
[Vytvoření Azure Function App a účtu Azure Storage](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


