---
title: "Connect Arduino (C) k Azure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: be6105927645ae2ec56f6885c61dbcb6faf5b11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu. Aplikace Azure funkce hostuje provádění funkcí v Azure.

## <a name="what-will-you-do"></a>Co se dělat
Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager. Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky pro panel Adafruit prolnutí M0 Wi-Fi Arduino](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-will-you-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak používat [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.
* Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.

## <a name="what-do-you-need"></a>Co je potřeba
Musíte úspěšně jste dokončili:
- [Začínáme s Arduino panel][get-started]
- [Vytvoření služby Azure IoT hub][create-iot-hub]

## <a name="open-the-sample-app"></a>Otevřete ukázková aplikace
Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* `app.ino` v soubor `app` podsložky je klíče zdrojového souboru. Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.
* `config.json` Obsahuje požadovaná nastavení konfigurace.
* `arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
* `arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.
* `ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure
Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.

![Parametry šablony Azure Resource Manager][arm-template-params]

* Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci panel Arduino][created-iot-hub-and-registered-arduino-board].
* Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete. Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu. Nepoužívejte pomlčku nebo číslo počáteční v předponu.

Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Vytvořte tyto prostředky trvá asi 5 minut. Při vytvoření prostředku probíhá, můžete přesunout další článek.

## <a name="summary"></a>Souhrn
Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv. Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na vaší kartě Arduino.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na vaší kartě Arduino][send-device-to-cloud-messages]

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md