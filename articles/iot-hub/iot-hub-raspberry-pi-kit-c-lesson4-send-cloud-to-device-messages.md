---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 4: Cloud zařízení | Microsoft Docs"
description: "Ukázková aplikace běží na platformy a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odešle tooPi zprávy z vaší IoT hub tooblink hello Indikátor."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "cloud toodevice, zpráv z cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: fcbc0dd0-cae3-47b0-8e58-240e4f406f75
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5596bf3a83c21f2bd54b2f83e2a8fdad7a608b94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-tooreceive-cloud-to-device-messages"></a>Spuštění ukázkové aplikace tooreceive zprávy typu cloud zařízení
V tomto článku nasadit ukázkovou aplikaci na malin pí 3. Ukázková aplikace Hello monitoruje příchozích zpráv ze služby IoT hub. Také spustit úlohu gulp na váš počítač toosend zprávy tooPi ze služby IoT hub. Zprávy hello přijetí hello ukázkovou aplikaci bliká DIODU hello. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-do"></a>Co provedete
* Připojte hello ukázkové aplikace tooyour IoT hub.
* Nasazení a spuštění hello ukázkovou aplikaci.
* Odesílat zprávy z vaší IoT hub tooPi tooblink hello Indikátor.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toomonitor příchozích zpráv ze služby IoT hub.
* Jak toosend cloud zařízení zprávy z vaší tooPi centra IoT.

## <a name="what-you-need"></a>Co potřebujete
* Malinová 3 Pi, nastavte pro použití. jak zjistit, tooset až pí, toolearn [konfigurace zařízení](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md).
* Služby IoT hub, který se vytvoří ve vašem předplatném Azure. toolearn jak toocreate služby IoT hub, najdete v části [vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="connect-hello-sample-application-tooyour-iot-hub"></a>Připojit hello ukázkové aplikace tooyour IoT hub
1. Ujistěte se, že jste ve složce úložišti hello `iot-hub-c-raspberrypi-getting-started`. Otevřete hello ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním hello následující příkazy:

   ```bash
   cd Lesson4
   code .
   ```

   Všimněte si hello `app.c` souboru v hello `app` podsložky. Hello `app.c` soubor je soubor hello zdroj klíče, který obsahuje hello kód toomonitor příchozí zprávy ze služby IoT hub hello. Hello `blinkLED` funkce bliká DIODU hello.

   ![Struktura úložišti v ukázkové aplikaci hello](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure_c.png)
2. Inicializovat konfigurační soubor hello spuštěním hello následující příkazy:

   ```bash
   npm install
   gulp init
   ```

   Pokud jste dokončili postup hello v [vytvoření účtu úložiště a aplikace Azure funkce](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) v tomto počítači jsou děděné všechny konfigurace hello, takže toostep toohello úloh nasazení a spuštění hello ukázkovou aplikaci, můžete přeskočit. Pokud jste dokončili postup hello v [vytvoření účtu Azure funkce aplikace a úložiště](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md) na jiném počítači, musíte tooreplace hello zástupné symboly v hello `config-raspberrypi.json` souboru. Hello `config-raspberrypi.json` soubor je v podsložce hello domovskou složku.

   ![Obsah souboru config raspberrypi.json hello](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

* Nahraďte **[zařízení hostitele nebo IP adresa]** s platformy na IP adresu nebo název hostitele, můžete získat spuštěním hello `devdisco list --eth` příkaz.
* Nahraďte **[řetězec připojení zařízení IoT]** s hello zařízení připojovací řetězec, který můžete získat spuštěním hello `az iot device show-connection-string --hub-name {my hub name} --device-id {device id} -g iot-sample {resource group name}` příkaz.
* Nahraďte **[IoT hub, připojovací řetězec]** s hello IoT hub připojovací řetězec, který můžete získat spuštěním hello `az iot hub show-connection-string --name {my hub name} -g iot-sample {resource group name}` příkaz.

> [!NOTE]
> Spustit **gulp instalace nástroje** i, pokud jste neučinili v lekci 1.

## <a name="deploy-and-run-hello-sample-application"></a>Nasazení a spuštění ukázkové aplikace hello
Nasazení a spuštění ukázkové aplikace hello na platformy spuštěním hello následující příkazy:

```
gulp deploy && gulp run
```

Spustí příkaz gulp Hello nejprve hello úloh instalace nástroje. Potom nasadí hello ukázkové aplikace tooPi. Nakonec je spuštěna aplikace hello na platformy a samostatná úloha na hostiteli počítače toosend 20 blikání zprávy tooPi z služby IoT hub.

Po spuštění ukázkové aplikace hello začne naslouchat toomessages ze služby IoT hub. Mezitím hello gulp úloh odešle několik "blink" zprávy z vaší tooPi centra IoT. Pro každou blikání zprávu, která přijímá Pi, hello ukázkové aplikace volá hello `blinkLED` funkce tooblink hello Indikátor.

Měli byste vidět blikání DIODU hello každé dvě sekundy jako hello gulp úloh zasílá 20 zprávy z vaší tooPi centra IoT. Hello poslední jedním je zpráva "stop", která ukončí hello spuštění aplikace.

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink_c.png)

## <a name="summary"></a>Souhrn
Z vaší IoT hub tooPi tooblink hello DIODU úspěšně odeslat zprávy. Další úlohou Hello je volitelné: Změna hello zapnout a vypnout chování hello Indikátor.

## <a name="next-steps"></a>Další kroky
[Změnit hello zapnout a vypnout chování hello DIODU](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md)
