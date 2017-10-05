---
title: "Připojení k Azure IoT - lekci 4 Intel Edison (uzel): přijímat zprávy | Microsoft Docs"
description: "Ukázková aplikace běží na Edison a sleduje příchozí zprávy ze služby IoT hub. Nová úloha gulp odesílá zprávy Edison ze služby IoT hub blikání Indikátor."
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
ms.openlocfilehash: 76ea59acd848f60663a0c821bff42166aac5823a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="run-a-sample-application-to-receive-cloud-to-device-messages"></a>Spuštění ukázkové aplikace pro příjem zpráv typu cloud zařízení
V tomto článku nasadit ukázkovou aplikaci na Intel Edison. Ukázkovou aplikaci monitoruje příchozích zpráv ze služby IoT hub. Můžete taky spustit úlohu gulp ve vašem počítači k odesílání zpráv do Edison ze služby IoT hub. Zprávy přijetí ukázkovou aplikaci bliká Indikátor. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-do"></a>Co provedete
* Připojte ukázkovou aplikaci do služby IoT hub.
* Nasazení a spuštění ukázkové aplikace.
* Odesílat zprávy ze služby IoT hub Edison blikání Indikátor.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Postup sledování příchozích zpráv ze služby IoT hub.
* Postupy pro odesílání zpráv typu cloud zařízení ze služby IoT hub pro Edison.

## <a name="what-you-need"></a>Co potřebujete
* Intel Edison, nastavte pro použití. Další informace o nastavení Edison naleznete v tématu [konfigurace zařízení][configure-your-device].
* Služby IoT hub, který se vytvoří ve vašem předplatném Azure. Informace o vytvoření služby IoT hub, najdete v tématu [vytvoření služby Azure IoT Hub][create-your-azure-iot-hub].

## <a name="connect-the-sample-application-to-your-iot-hub"></a>Připojit ukázkovou aplikaci do služby IoT hub
1. Ujistěte se, že jste ve složce úložišti `iot-hub-node-edison-getting-started`. Otevřete ukázkovou aplikaci v aplikaci Visual Studio Code spuštěním následujících příkazů:

   ```bash
   cd Lesson4
   code .
   ```

   Soubor v `app` podsložky je klíče zdrojový soubor, který obsahuje kód ke sledování příchozích zpráv ze služby IoT hub. `blinkLED` Funkce bliká Indikátor.

   ![Struktura úložišti v ukázkové aplikace][repo-structure]
2. Inicializace konfiguračního souboru spuštěním následujících příkazů:

   ```bash
   npm install
   gulp init
   ```

   Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] v tomto počítači se dědí všechny konfigurace, takže můžete přeskočit na krok úlohy nasazení a spuštění ukázkové aplikace. Pokud jste dokončili kroky v [vytvoření účtu Azure funkce aplikace a úložiště] [ create-an-azure-function-app-and-storage-account] na jiném počítači, budete muset nahraďte zástupné symboly v `config-edison.json` souboru. `config-edison.json` Soubor je v podsložce domovskou složku.

   ![Obsah souboru config edison.json](media/iot-hub-intel-edison-lessons/lesson4/config-edison.png)

   * Nahraďte **[zařízení hostitele nebo IP adresa]** s IP adresou zařízení označit dolů, při konfiguraci zařízení.
   * Nahraďte **[řetězec připojení zařízení IoT]** připojovacím řetězcem zařízení, kterou můžete získat spuštěním `az iot device show-connection-string --hub-name {my hub name} --device-id {device id}` příkaz.
   * Nahraďte **[IoT hub, připojovací řetězec]** s IoT hub připojovací řetězec, který získáte spuštěním `az iot hub show-connection-string --name {my hub name}` příkaz.

## <a name="deploy-and-run-the-sample-application"></a>Nasazení a spuštění ukázkové aplikace
Nasazení a spuštění ukázkové aplikace na Edison spuštěním následujících příkazů:

```bash
gulp deploy && gulp run
```

Příkaz gulp nasadí ukázkovou aplikaci pro Edison. Pak spustí aplikaci na Edison a samostatná úloha v hostitelském počítači odesílat zprávy 20 blikání Edison ze služby IoT hub.

Po spuštění ukázkové aplikace, začne naslouchání zpráv ze služby IoT hub. Úloha gulp mezitím, odešle do Edison několik "blink" zpráv ze služby IoT hub. Pro každou blikání zprávu, která přijímá Edison, volá ukázkovou aplikaci `blinkLED` funkce blikání Indikátor.

Jako úlohu gulp odešle 20 zpráv ze služby IoT hub Edison, měli byste vidět blikání DIODU každé dvě sekundy. Poslední je "stop" zprávy, která zabrání spuštění aplikace.

![Ukázkovou aplikaci s gulp příkazu a blink – zprávy][gulp-command-and-blink-messages]

## <a name="summary"></a>Souhrn
Úspěšně jste odeslali zpráv ze služby IoT hub do Edison blikání Indikátor. Dalším krokem je volitelné: Změna zapnuto a vypnuto chování Indikátor.

## <a name="next-steps"></a>Další kroky
[Změnit zapnuto a vypnuto chování Indikátor][change-the-on-and-off-behavior-of-the-led]

<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[configure-your-device]: iot-hub-intel-edison-kit-node-lesson1-configure-your-device.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson4/repo_structure.png
[create-an-azure-function-app-and-storage-account]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[gulp-command-and-blink-messages]: media/iot-hub-intel-edison-lessons/lesson4/gulp_blink.png
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-node-lesson4-change-led-behavior.md