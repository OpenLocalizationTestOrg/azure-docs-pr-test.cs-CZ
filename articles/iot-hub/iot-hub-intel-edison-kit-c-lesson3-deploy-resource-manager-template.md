---
title: "Connect Intel Edison (C) tooAzure IoT - Lekce 3: vytvoření aplikace pro funkce | Microsoft Docs"
description: "aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
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
ms.openlocfilehash: ef045ec2f44fe379a5e6c777d1bfb97de8b965a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
[Azure Functions](../../articles/azure-functions/functions-overview.md) je řešení umožňující snadno spouštět *funkce* (malé části kódu) v cloudu hello. Aplikace Azure funkce hostuje hello provádění funkcí v Azure.

## <a name="what-will-you-do"></a>Co se dělat
Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager. aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště. Hello účet úložiště se používá ke čtení hello trvalé kopie zpráv z tabulky Azure. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-will-you-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure prostředky.
* Jak toouse Azure funkce aplikace tooprocess zprávy centra IoT a jejich zápis tooa tabulky ve službě Azure Table storage.

## <a name="what-do-you-need"></a>Co je potřeba
Musíte úspěšně jste dokončili:
- [Začínáme s vaší Edison Intel][get-started-with-your-intel-edison]
- [Vytvoření služby Azure IoT hub][create-your-azure-iot-hub]

## <a name="open-hello-sample-app"></a>Ukázkové aplikace otevřete hello
Otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkazy:

```bash
cd Lesson3
code .
```

![Struktura úložiště][repo-structure]

* soubor Hello v hello `app` podsložky je hello klíče zdrojového souboru. Tento zdrojový soubor obsahuje kód toosend hello zprávu 20krát tooyour IoT hub a blikání hello DIODU pro každou zprávu, že odešle.
* Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
* Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.
* Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure
Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.

![Parametry šablony Azure Resource Manager][arm-template-parameters]

* Nahraďte **[název služby IoT Hub]** s **{Moje název centra}** kterou jste zadali při jste [vytvoření služby IoT hub a registraci Intel Edison][created-your-iot-hub-and-registered-intel-edison].
* Nahraďte **[předpony řetězec pro nové prostředky]** s jakoukoli předponu chcete. Předpona Hello zajistí, že tento název zdroje hello je globálně jedinečný tooavoid konflikt. Nepoužívejte dash nebo číslo počáteční v hello předponu.

Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Trvá asi 5 minut toocreate tyto prostředky. Při vytvoření prostředku hello probíhá, můžete přesunout na následující článek toohello.

## <a name="summary"></a>Souhrn
Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy. Teď můžete nasadit a spustit zpráv typu zařízení cloud hello ukázkové toosend na Edison.

## <a name="next-steps"></a>Další kroky
[Spuštění ukázkové aplikace toosend zpráv typu zařízení cloud na Intel Edison][send-device-to-cloud-messages].
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[get-started-with-your-intel-edison]: iot-hub-intel-edison-kit-c-get-started.md
[create-your-azure-iot-hub]: iot-hub-intel-edison-kit-c-get-started.md
[repo-structure]: media/iot-hub-intel-edison-lessons/lesson3/repo_structure_c.png
[arm-template-parameters]: /media/iot-hub-intel-edison-lessons/lesson3/arm_para_c.png
[created-your-iot-hub-and-registered-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md