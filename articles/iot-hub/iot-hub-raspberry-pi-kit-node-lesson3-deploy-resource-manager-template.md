---
title: "Připojení k Azure IoT - lekci 3 malin platformy (uzel): nasazení šablony | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 44901faea37a847a418e6d2b4097302cdb610495
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
[Azure Functions](../azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu. Aplikace Azure funkce hostuje provádění funkcí v Azure.

## <a name="what-you-will-do"></a>Co provedete
Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager. Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage. Pokud máte potíže, hledají řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:

* Jak používat [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.
* Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete
Musíte úspěšně jste dokončili:
* [Začínáme s malin pí 3](iot-hub-raspberry-pi-kit-node-get-started.md)
* [Vytvoření služby Azure IoT hub](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-the-sample-app"></a>Otevřete ukázková aplikace
Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:

```bash
cd Lesson3
code .
```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* `app.js` v soubor `app` podsložky je klíče zdrojového souboru. Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.
* `arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
* `arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.
* `ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure
Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.

![Parametry šablony Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
* Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete. Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu. Nepoužívejte pomlčku nebo číslo počáteční v předponu.

Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Vytvořte tyto prostředky trvá asi 5 minut. Při vytvoření prostředku probíhá, můžete přesunout další článek.

## <a name="summary"></a>Souhrn
Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv. Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na pí.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na malin pí 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

