---
title: "Connect Intel Edison (C) k Azure IoT - Lekce 3: odesílání zpráv | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace pro Edison Intel, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data do cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 12672b64-795a-4dfc-86fd-df53ed3eeef7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d104618ebb37a19c83f161beceb5c71bc89bbb56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na Edison Intel, který odesílá zprávy do služby IoT hub. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace C na Edison.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
Připojovací řetězec zařízení slouží k připojení Edison do služby IoT hub. IoT hub připojovací řetězec se používá pro připojení služby IoT hub pro identitu zařízení, která představuje Edison ve službě IoT hub.

* Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci Edison.

* Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Použití `myinteledison` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.

## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení
1. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.

2. Otevřete konfigurační soubor zařízení `config-edison.json` ve Visual Studio Code spuštěním následujícího příkazu:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. Zkontrolujte následující nahrazení v `config-edison.json` souboru:

   * Nahraďte **[zařízení hostitele nebo IP adresa]** s IP adresou zařízení označit dolů, při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.

   > [!NOTE]
   > Nepotřebujete `azure_storage_connection_string` v tomto článku. Dodržte jako je.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na Edison spuštěním následujícího příkazu:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Ověřte, že funguje ukázkové aplikace
Měli byste vidět DIODU, která je připojena k Edison blikat každé dvě sekundy. Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub. Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly. Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Souhrn
Jste nasazení a spuštění nové aplikace ukázka blikání na Edison k odesílání zpráv typu zařízení cloud do služby IoT hub. Zprávy si můžete nyní sledovat, jak se zapisují na účet úložiště.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md