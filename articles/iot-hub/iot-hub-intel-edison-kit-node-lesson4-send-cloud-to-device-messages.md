---
title: "Připojit Intel Edison (uzel) tooAzure IoT - Lekce 4: přijímat zprávy | Microsoft Docs"
description: "Ukázková aplikace běží na Edison a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odešle tooEdison zprávy z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ovládací prvek arduino vedla z webové, arduino řízení vedla přes web"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: bc738bf6-e38d-4024-82d7-39b6c2d4bacb
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: aab0ced4810dd3d4f5ba636940b06563f1db9241
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení
V tomto článku nasadit ukázkovou aplikaci na Intel Edison. Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub. Také spustit úlohu gulp na váš počítač toosend zprávy tooEdison ze služby IoT hub. Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-do"></a>Co provedete
* Připojte hello ukázkové aplikace tooyour IoT hub.
* Nasazení a spuštění hello ukázkovou aplikaci.
* Odesílat zprávy z vaší IoT hub tooEdison tooblink hello Indikátor.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toomonitor příchozích zpráv ze služby IoT hub.
* Jak toosend cloud zařízení zprávy z vaší tooEdison centra IoT.

## <a name="what-you-need"></a>Co potřebujete
* Intel Edison, nastavte pro použití. jak zjistit, tooset až Edison, toolearn [konfigurace zařízení][configure-your-device].
* Služby IoT hub, který se vytvoří ve vašem předplatném Azure. toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Připojit hello ukázkové aplikace tooyour IoT hub
1. Ujistěte se, že jste ve složce úložišti hello `iot-hub-node-edison-getting-started`. Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:

   ```bash
   cd Lesson4
   code .
   ```

   soubor Hello v hello `app` podsložky je hello klíče zdrojový soubor, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello. Hello `blinkLED` funkce bliká DIODU hello.

   ![Struktura úložišti v ukázkové aplikaci hello][repo-structure]
2. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   npm install
   gulp init
   ```

   Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace hello, takže hello krok toohello úloh nasazení, můžete přeskočit a spuštění ukázkové aplikace hello. Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-edison.json` souboru. Hello `config-edison.json` soubor je v podsložce hello domovskou složku.

   ![Obsah souboru config edison.json hello](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Nahraďte **[zařízení hostitele nebo IP adresa]** s adresou IP zařízení hello označena dolů, při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.
   * Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name}` příkaz.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na Edison spuštěním hello následující příkazy:

```bash
gulp deploy && gulp run
```

příkaz gulp Hello nasadí hello ukázkové aplikace tooEdison. Pak je spuštěna aplikace hello na Edison a samostatná úloha na hostiteli počítače toosend 20 blikání zprávy tooEdison z služby IoT hub.

Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub. Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší tooEdison centra IoT. Pro každou blikání zprávu, která přijímá Edison, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.

Měli byste vidět blikání DIODU hello každé dvě sekundy jako hello gulp úloh zasílá 20 zprávy z vaší tooEdison centra IoT. Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][gulp-command-and-blink-messages]

## <a name="summary"></a>Souhrn
Z vaší IoT hub tooEdison tooblink hello DIODU úspěšně odeslat zprávy. Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.

## <a name="next-steps"></a>Další kroky
[Změnit hello zapnout a vypnout chování hello DIODU][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md