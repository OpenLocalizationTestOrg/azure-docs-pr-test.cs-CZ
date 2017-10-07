---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 3: odesílání zpráv | Microsoft Docs"
description: "Nasazení a spuštění ukázkové aplikace tooIntel Edison, která odesílá zprávy tooyour IoT hub a bliká DIODU hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloudové služby IOT, arduino odesílat data toocloud"
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
ms.openlocfilehash: 83615335027cc792b89d3894578fc3f7a55e5c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud
## <a name="what-you-will-do"></a>Co provedete
Tento článek vám ukáže, jak toodeploy a spusťte ukázkové aplikace na Edison Intel, který odesílá zprávy tooyour IoT hub. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
Bude zjistěte, jak toouse hello gulp toodeploy nástroje a spusťte hello ukázkové C aplikace na Edison.

## <a name="what-you-need"></a>Co potřebujete
* Než začnete tuto úlohu, musí úspěšně jste dokončili [vytvořit aplikaci Azure funkce a úložiště účtu úložiště a tooprocess IoT hub zprávy][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Získat IoT hub a zařízení připojovací řetězce
Hello zařízení připojovací řetězec je použité tooconnect Edison tooyour IoT hub. Hello IoT hub připojovací řetězec je použité tooconnect vaše IoT hub toohello zařízení identita, která představuje Edison hello IoT hub.

* Seznam všech centra IoT ve vaší skupině prostředků tak, že spustíte následující příkaz rozhraní příkazového řádku Azure hello:

```bash
az iot hub list -g iot-sample --query [].name
```

Použití `iot-sample` jako hodnota hello `{resource group name}` Pokud hodnota hello nebyla změněna.

* Spuštěním následujícího příkazu příkazového řádku Azure CLI hello získáte hello IoT hub, připojovací řetězec:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`je hello název, který jste zadali při vytvoření služby IoT hub a registraci Edison.

* Získáte hello zařízení připojovací řetězec tak, že spustíte následující příkaz hello:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Použití `myinteledison` jako hodnota hello `{device id}` Pokud hodnota hello nebyla změněna.

## <a name="configure-hello-device-connection"></a>Nakonfigurujte připojení zařízení hello
1. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.

2. Otevřete hello zařízení konfigurační soubor `config-edison.json` ve Visual Studio Code spuštěním hello následující příkaz:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. Ujistěte se, hello následující nahrazení v hello `config-edison.json` souboru:

   * Nahraďte **[zařízení hostitele nebo IP adresa]** s adresou IP zařízení hello označena dolů, při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** s hello `device connection string` jste získali.
   * Nahraďte **[IoT hub, připojovací řetězec]** s hello `iot hub connection string` jste získali.

   > [!NOTE]
   > Nepotřebujete `azure_storage_connection_string` v tomto článku. Dodržte jako je.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkaz:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Ověřte, že funguje hello ukázkové aplikace
Měli byste vidět hello DIODU, který je připojený tooEdison blikat každé dvě sekundy. Pokaždé, když hello DIODU bliká, hello ukázkové aplikace odešle zpráva tooyour IoT hub a ověřuje, zda je že tento hello byla úspěšně odeslána zpráva tooyour IoT hub. Kromě toho každou zprávu přijme hello IoT hub je vytištěna v okně konzoly hello. Ukázková aplikace Hello automaticky ukončí po odeslání 20 zpráv.

![Ukázkovou aplikaci s odeslané a přijaté zprávy][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Souhrn
Jste nasazení a spusťte hello nové blikání ukázkovou aplikaci na Edison toosend zpráv typu zařízení cloud tooyour IoT hub. Vaše zprávy se teď sledovat, jak jsou napsány toohello účet úložiště.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md