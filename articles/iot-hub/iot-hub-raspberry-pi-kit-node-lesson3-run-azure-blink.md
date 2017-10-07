---
title: "Připojit malin platformy (uzel) tooAzure IoT - Lekce 3: spuštění ukázkových | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooRaspberry pí 3, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
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
ms.openlocfilehash: 991c64e0b1b6f965b029560cdc6403e557841e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na 3 malin platformy, která odesílá zprávy tooyour IoT hub. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
Se dozvíte, jak toouse hello gulp toodeploy nástroje a spusťte aplikaci Node.js hello ukázka na platformy.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
řetězec připojení zařízení Hello používá pí tooconnect tooyour IoT hub. Hello IoT hub připojovací řetězec je použité tooconnect toohello registru identit ve vaší IoT hub toomanage hello zařízení, které jsou povolená tooconnect tooyour IoT hub. 

* Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`je hello název, který jste zadali při vytvoření služby IoT hub a registraci pí.

* Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Použití `myraspberrypi` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.

## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello
1. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:
   
   ```bash
   npm install
   gulp init
   ```
2. Otevřete hello zařízení konfigurační soubor `config-raspberrypi.json` ve Visual Studio Code spuštěním hello následující příkaz:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Ujistěte se, hello následující nahrazení v hello `config-raspberrypi.json` souboru:
   
   * Nahraďte **[zařízení hostitele nebo IP adresa]** s hello zařízení IP adresu nebo název hostitele, který jste získali z `device-discovery-cli` nebo s hodnotou hello zděděná při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.

Aktualizace hello `config-raspberrypi.json` souborů, abyste mohli nasadit hello ukázkovou aplikaci z vašeho počítače.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Ověřte, že funguje hello ukázkové aplikace
Měli byste vidět hello DIODU, který je připojený tooPi blikat každé dvě sekundy. Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub. Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello. Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a>Souhrn
Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci platformy toosend zpráv typu zařízení cloud tooyour IoT hub. Nyní můžete sledovat vaše zprávy jako toohello účet úložiště, jak jsou napsány.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)

