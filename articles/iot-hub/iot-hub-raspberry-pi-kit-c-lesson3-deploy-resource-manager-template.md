---
title: "Connect Raspberry PI (C) tooAzure IoT - Lekce 3: nasazení šablony | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 11aaad4935681d8b3d338779eec1b19d77cb11e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello. Aplikace Azure funkce hostuje hello provádění funkcí v Azure.

## <a name="what-will-you-do"></a>Co se dělat
Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager. aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-will-you-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.
* Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.

## <a name="what-do-you-need"></a>Co je potřeba
* Musíte úspěšně jste dokončili:
- [Začínáme s vaší malin pí 3](iot-hub-raspberry-pi-kit-c-get-started.md)
- [Vytvoření služby Azure IoT hub](iot-hub-raspberry-pi-kit-c-get-started.md)

## <a name="open-hello-sample-app"></a>Ukázkové aplikace otevřete hello
Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:

```bash
cd Lesson3
code .
```

![Struktura úložiště](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure_c.png)

* Hello `main.c` souboru v hello `app` podsložky je hello klíče zdrojového souboru. Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.
* Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
* Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.
* Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure
Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.

![Parametry šablony Azure Resource Manager](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para_c.png)

* Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md).
* Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete. Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt. Nepoužívejte dash nebo číslo počáteční v hello předponu.

Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Trvá asi 5 minut toocreate tyto prostředky. Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.

## <a name="summary"></a>Souhrn
Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy. Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend na pí.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md)
