---
title: "Simulované zařízení & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojení brány do služby IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou internet věcí, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 916fa40d9ac857dfa72197b40c232834593d3891
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a>Začínáme s IoT brány Starter Kit s simulovaného zařízení

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simulované zařízení](iot-hub-gateway-kit-c-sim-get-started.md)

V tomto kurzu začnete podle učení základní informace o práci s [IoT brány Starter Kit](https://aka.ms/gateway-kit). Budete pracovat s NUC Intel, se systémem Linux větru oblasti. Se dozvíte, jak k seamleesly připojení zařízení do cloudu pomocí Azure IoT Hub.

***
**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit).
***

## <a name="lesson-1-configure-your-nuc"></a>Lekce 1: Konfigurace NUC
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

V této lekci nastavit NUC Intel (Další jednotky Computing) v sadě Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge na NUC a spusťte ukázkové aplikace, pokud chcete ověřit funkčnost brány.

*Odhadovaný čas dokončení: 15 minut*

Přejděte na [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekce 2: Vytvoření služby IoT Hub
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

V této lekci nainstalovat nástroje a software v hostitelském počítači. Pak vytvořte účet Azure zdarma, zřídit služby Azure IoT hub a vytvoření vaší první zařízení ve službě IoT hub.

Před zahájením této lekci, proveďte Lekce 1.

### <a name="get-the-tools"></a>Získat nástroje
Nainstaluje nástroje a software v hostitelském počítači.

*Odhadovaný čas dokončení: 20 minut*

Přejděte na [získat nástroje](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Vytvoření služby IoT hub a registrace zařízení
Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidat svoje první zařízení ke službě IoT hub pomocí rozhraní příkazového řádku Azure.

*Odhadovaný čas dokončení: 10 minut*

Přejděte na [vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-the-simulated-device-and-read-messages-from-your-iot-hub"></a>Lekce 3: Přijímat zprávy ze simulovaného zařízení a čtení zpráv z centra IoT
V této lekci použijete skripty pro automatizaci konfigurace a spuštění aplikace simulovaného zařízení ve vaší brány. Aplikaci simulovaného zařízení vygeneruje ukázková teploty data a odešle ji do modul služby IoT hub. Modul IoT hub balíčky získaná data a odešle ji do služby IoT hub prostřednictvím rozhraní brány v Azure IoT okraj.

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a>Nakonfigurujte a spusťte simulovaného zařízení
Připravte ukázkové kódy. Potom nakonfigurujte a spusťte ukázkovou aplikaci simulovaného zařízení.

*Odhadovaný čas dokončení: 15 minut*

Přejděte na [konfigurace a spuštění simulovaného zařízení](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT
Spustíte ukázkový kód v hostitelském počítači ke čtení zpráv ze služby IoT hub.

*Odhadovaný čas dokončení: 15 minut*

Přejděte na [čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>Lekce 4: Uložení zpráv do Azure Table Storage
Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapisuje je do Azure Table storage.

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
Vytvoření aplikace Azure funkce a účet úložiště Azure pomocí šablony Azure Resource Manager.

*Odhadovaný čas dokončení: 10 minut*

Přejděte na [vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Čtení zprávy uchovávané v úložišti Azure Table
Sledujte zprávy odesílané brány do cloudu, jako jsou zapsány do Azure Table storage.

*Odhadovaný čas dokončení: 5 minut*

Přejděte na [číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud máte potíže při poznatky, vyhledejte řešení v [Poradce při potížích s](iot-hub-gateway-kit-c-sim-troubleshooting.md) článku.

## <a name="explore-more"></a>Prozkoumat další
Přejděte [zóny developer Kit brány IoT Intel](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) Další informace.