---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 4: vytvoření aplikace pro funkce | Microsoft Docs"
description: "Ukládat zprávy z Intel NUC do služby IoT hub, zapisuje je do úložiště Azure Table a číst je z cloudu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "ukládání dat v cloudu, data uložená v cloudu, iot cloudové služby"
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
ms.openlocfilehash: 717c91e8332660f19d596c05a8a23afd8df1d51c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Vytvoření Azure Function App a účtu úložiště

Azure Functions je řešení umožňující snadno spouštět _funkce_ (malé části kódu) v cloudu. Aplikace Azure funkce hostuje provádění funkcí v Azure. 

## <a name="what-you-will-do"></a>Co provedete

- Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager. Aplikace Azure funkce naslouchá událostem, Azure IoT hub, zpracuje příchozí zprávy a zapisuje je do Azure Table storage.

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak používat Azure Resource Manager k nasazení prostředků Azure.
- Jak používat aplikaci Azure funkce zpracování zpráv služby IoT Hub a jejich zápis do tabulky v Azure Table storage.

## <a name="what-you-need"></a>Co potřebujete

Musíte úspěšně jste dokončili předchozí lekce:

- [Lekce 1: Nastavení vaší Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [Lekce 2: Příprava hostitelský počítač a Azure IoT hub](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [Lekce 3: Přijetí zpráv z SensorTag a čtení zpráv ze služby IoT hub](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Otevřete ukázkové aplikace

Přejděte na vaše `iot-hub-c-intel-nuc-gateway-getting-started` úložišti složky inicializovat konfigurační soubory a pak otevřete ukázkový projekt ve Visual Studio Code spuštěním následujícího příkazu:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Struktura úložiště](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- `arm-template.json` Soubor je šablony Azure Resource Manager, která obsahuje aplikaci Azure funkce a účet úložiště Azure.
- `arm-template-param.json` Soubor je soubor konfigurace používané šablony Azure Resource Manageru.
- `ReceiveDeviceMessages` Podsložky obsahuje kód Node.js pro funkci Azure.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurace šablon Azure Resource Manager a vytváření prostředků v Azure

Aktualizace `arm-template-param.json` soubor ve Visual Studio Code.

![json šablony arm](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Nahraďte `[your IoT Hub name]` s `{my hub name}` který jste zadali v lekci 2.

Po provedení aktualizace `arm-template-param.json` souboru, nasadit tyto prostředky do Azure tak, že spustíte následující příkaz:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Použití `iot-gateway` jako hodnotu `{resource group name}` Pokud nebylo změňte hodnotu v lekci 2.

## <a name="summary"></a>Souhrn

Vytvoření aplikace Azure funkce pro zpracování zprávy IoT hub a účet úložiště Azure slouží k uložení těchto zpráv. Teď si můžete přečíst zprávy, které odesílá bránu do služby IoT hub.

## <a name="next-steps"></a>Další kroky
[Čtení zprávy uchovávané v úložišti Azure](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).
