---
title: "Připojení k Azure IoT - lekci 3 malin platformy (uzel): ukázku spustit | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace na 3 malin platformy, která odesílá zprávy do služby IoT hub a bliká Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "blikání vedla cloudové platformy, vedla blikání z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 427d8e5e-8af8-4249-8607-44edc90a4972
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1c03283ee276a954f822d6eca5f0a3d5f93ec64b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak nasadit a spustit ukázkovou aplikaci na 3 malin platformy, která odesílá zprávy do služby IoT hub. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
Se dozvíte, jak používat nástroj gulp k nasazení a spuštění ukázkové aplikace Node.js na pí.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a účet úložiště pro zpracování a ukládání IoT Centrum zpráv](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
Zařízení připojovací řetězec se používá ve vašem platformy pro připojení do služby IoT hub. IoT hub připojovací řetězec se používá pro připojení k registru identit ve službě IoT hub pro správu zařízení, které se mohou připojit ke službě IoT hub. 

* Seznam všech centra IoT ve vaší skupině prostředků spuštěním následujícího příkazu příkazového řádku Azure CLI:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI získáte připojovací řetězec centra IoT:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`je název, který jste zadali při vytvoření služby IoT hub a registraci pí.

* Získáte připojovací řetězec zařízení tak, že spustíte následující příkaz:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Použití `myraspberrypi` jako hodnotu `{device id}` Pokud nebylo změňte hodnotu.

## <a name="configure-the-device-connection"></a>Nakonfigurujte připojení zařízení
1. Inicializace konfiguračního souboru spuštěním následujících příkazů:
   
   ```bash
   npm install
   gulp init
   ```
2. Otevřete konfigurační soubor zařízení `config-raspberrypi.json` ve Visual Studio Code spuštěním následujícího příkazu:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Zkontrolujte následující nahrazení v `config-raspberrypi.json` souboru:
   
   * Nahraďte **[zařízení hostitele nebo IP adresa]** se zařízení IP adresu nebo název hostitele, který jste získali z `device-discovery-cli` nebo s hodnotou zděděná při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** s `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s `iot hub connection string` jste získali.

Aktualizace `config-raspberrypi.json` souborů, abyste mohli nasadit ukázkovou aplikaci z vašeho počítače.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na platformy spuštěním následujícího příkazu:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Ověřte, že funguje ukázkové aplikace
Měli byste vidět DIODU, která je připojena k pí blikat každé dvě sekundy. Pokaždé, když DIODU bliká, ukázkové aplikace odešle zprávu do služby IoT hub a ověřuje, že zpráva byla úspěšně odeslána do služby IoT hub. Kromě toho každou zprávu přijme služby IoT hub je vytištěna v okně konzoly. Ukázkové aplikace automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a>Souhrn
Jste nasazení a spuštění nové aplikace ukázka blikání na platformy k odesílání zpráv typu zařízení cloud do služby IoT hub. Nyní můžete sledovat vaše zprávy jako jsou zapsány do účtu úložiště.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)

