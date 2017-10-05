---
title: "Connect Intel Edison (C) k Azure IoT - Lekce 3: vytvoření aplikace pro funkce | Microsoft Docs"
description: "Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 739b82e9-5d4e-4485-8971-f57cbb682faf
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30018cc2d03fc19c13625ef066989a89f21800e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu. Aplikace Azure funkce hostuje provádění funkcí v Azure.

## <a name="what-will-you-do"></a>Co se dělat
Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager. Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage. Účet úložiště se používá k přečtení trvalou kopie zprávy z tabulky Azure. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-will-you-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak používat [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) nasazení prostředků Azure.
* Jak používat aplikaci Azure funkce zpracování zpráv centra IoT a jejich zápis do tabulky v Azure Table storage.

## <a name="what-do-you-need"></a>Co je potřeba
Musíte úspěšně jste dokončili:
- [Začínáme s vaší Edison Intel][get-started-with-your-intel-edison]
- [Vytvoření služby Azure IoT hub][create-your-azure-iot-hub]

## <a name="open-the-sample-app"></a>Otevřete ukázková aplikace
Otevřete ukázkový projekt ve Visual Studio Code spuštěním následujících příkazů:

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* Soubor v `app` podsložky je klíče zdrojového souboru. Tento zdrojový soubor obsahuje kód, který odešle zprávu 20krát do služby IoT hub a blink DIODU pro každou zprávu, kterou odešle.
* `arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
* `arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.
* `ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure
Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.

![Parametry šablony Azure Resource Manager][arm-template-parameters]

* Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci Intel Edison][created-your-iot-hub-and-registered-intel-edison].
* Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete. Předpona, která zajišťuje, že název prostředku globálně jedinečný, aby nedošlo ke konfliktu. Nepoužívejte pomlčku nebo číslo počáteční v předponu.

Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Vytvořte tyto prostředky trvá asi 5 minut. Při vytvoření prostředku probíhá, můžete přesunout další článek.

## <a name="summary"></a>Souhrn
Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv. Teď můžete nasadit a spustit ukázkový k odesílání zpráv typu zařízení cloud na Edison.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace k odesílání zpráv typu zařízení cloud na Intel Edison][send-device-to-cloud-messages].
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-c-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure_c.png
[arm-template-parameters]: /media/iot-hub-intel-edison-lessons/lesson3/arm_para_c.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md