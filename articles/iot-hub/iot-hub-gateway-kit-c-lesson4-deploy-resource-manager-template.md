---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 4: vytvoření aplikace pro funkce | Microsoft Docs"
description: "Uložení zpráv ze služby IoT hub Intel NUC tooyour, zapisuje je úložiště Table tooAzure a číst je z cloudu hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu hello, data uložená v cloudu, iot cloudové služby"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: efee3bdc15ced104651f4a500311a5fe614267c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Vytvoření Azure Function App a účtu úložiště

Azure Functions je řešení umožňující snadno spouštět _funkce_ (malé části kódu) v cloudu hello. Aplikace Azure funkce hostuje hello provádění funkcí v Azure. 

## <a name="what-you-will-do"></a>Co provedete

- Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager. aplikaci Azure Funkce Hello naslouchá tooAzure IoT Centrum událostí, zpracuje příchozí zprávy a zapíše tooAzure tabulka úložiště.

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak toouse Azure Resource Manager toodeploy prostředků Azure.
- Jak toouse Azure funkce aplikace tooprocess zprávy služby IoT Hub a jejich zápis tooa tabulky ve službě Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete

Musíte úspěšně jste dokončili předchozí lekce hello:

- [Lekce 1: Nastavení vaší Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [Lekce 2: Příprava hostitelský počítač a Azure IoT hub](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [Lekce 3: Přijetí zpráv z SensorTag a čtení zpráv ze služby IoT hub](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Otevřete ukázkové aplikace

Přejděte tooyour `iot-hub-c-intel-nuc-gateway-getting-started` složky úložišti, inicializovat hello konfigurační soubory a pak otevřete hello ukázkový projekt ve Visual Studio Code spuštěním hello následující příkaz:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Struktura úložiště](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- Hello `arm-template.json` soubor je hello šablony Azure Resource Manageru, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
- Hello `arm-template-param.json` soubor je soubor konfigurace hello používá šablony Azure Resource Manageru hello.
- Hello `ReceiveDeviceMessages` podsložky obsahuje kód Node.js hello pro hello Azure funkce.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure

Aktualizace hello `arm-template-param.json` soubor ve Visual Studio Code.

![json šablony arm](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Nahraďte `[your IoT Hub name]` s `{my hub name}` který jste zadali v lekci 2.

Po dokončení aktualizace hello `arm-template-param.json` souboru, nasazení hello prostředky tooAzure spuštěním hello následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Použití `iot-gateway` jako hodnota hello `{resource group name}` Pokud hodnota hello v lekci 2 nebyla změněna.

## <a name="summary"></a>Souhrn

Jste vytvořili vaší aplikaci tooprocess funkce Azure IoT hub zprávy a Azure storage účet toostore tyto zprávy. Teď si můžete přečíst zprávy odesílané ze služby IoT hub tooyour brány.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).
