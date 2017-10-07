---
title: "Simulované zařízení & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojení brány toohello IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou hello internet věcí, iot toolkit"
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
ms.openlocfilehash: 1a54a5e5f1c1d9b2e657c9e4448274256e2533f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a>Začínáme s IoT brány Starter Kit s simulovaného zařízení

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simulované zařízení](iot-hub-gateway-kit-c-sim-get-started.md)

V tomto kurzu začnete podle učení hello základy práce s [IoT brány Starter Kit](https://aka.ms/gateway-kit). Budete pracovat s NUC Intel, se systémem Linux větru oblasti. Se dozvíte, jak tooseamleesly propojit své cloudové toohello zařízení pomocí služby Azure IoT Hub.

***
**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit).
***

## <a name="lesson-1-configure-your-nuc"></a>Lekce 1: Konfigurace NUC
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

V této lekci nastavit Intel NUC (Další jednotky Computing) v hello Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge hello na NUC a spusťte funkci brány tooverify hello ukázkové aplikace.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekce 2: Vytvoření služby IoT Hub
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

V této lekci nainstalovat nástroje hello a software na hostitelském počítači. Pak vytvoříte účet Azure zdarma, zřídit služby Azure IoT hub a vytvořit první zařízení IoT hub hello.

Před zahájením této lekci, proveďte Lekce 1.

### <a name="get-hello-tools"></a>Získat nástroje hello
Nainstalujte nástroje hello a software v hostitelském počítači.

*Odhadovaný čas toocomplete: 20 minut*

Přejděte příliš[získat nástroje hello](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Vytvoření služby IoT hub a registrace zařízení
Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidejte první zařízení toohello IoT hub pomocí hello rozhraní příkazového řádku Azure.

*Odhadovaný čas toocomplete: 10 minut*

Přejděte příliš[vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-hello-simulated-device-and-read-messages-from-your-iot-hub"></a>Lekce 3: Přijetí zpráv z hello simulované zařízení a čtení zpráv z centra IoT
V této lekci použijete skripty tooautomate hello konfiguraci a spuštění aplikace simulovaného zařízení ve vaší brány. aplikaci simulovaného zařízení Hello vygeneruje ukázková teploty data a odešle ji tooan IoT hub modulu. Hello IoT hub modulu balíčky hello dat přijatých a odešle ji tooyour IoT hub prostřednictvím hello brány rámec poskytovaný v Azure IoT Edge.

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a>Nakonfigurujte a spusťte simulovaného zařízení
Připravte hello ukázkové kódy. Potom nakonfigurujte a spusťte ukázkové aplikace hello simulované zařízení.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[konfigurace a spuštění simulovaného zařízení](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT
Spusťte ukázkový kód na hostitele počítače tooread hello zpráv ze služby IoT hub.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[čtení zpráv z centra IoT](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-tooazure-table-storage"></a>Lekce 4: Uložení zpráv tooAzure Table storage
Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapíše tooAzure tabulka úložiště.

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.

*Odhadovaný čas toocomplete: 10 minut*

Přejděte příliš[vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Čtení zprávy uchovávané v úložišti Azure Table
Zprávy brány cloud hello sledovat, jak je, jak jsou napsány tooAzure Table storage.

*Odhadovaný čas toocomplete: 5 minut*

Přejděte příliš[číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud máte potíže při hello lekce, vyhledejte řešení v hello [Poradce při potížích s](iot-hub-gateway-kit-c-sim-troubleshooting.md) článku.

## <a name="explore-more"></a>Prozkoumat další
Navštivte hello [zóny developer Kit brány IoT Intel](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) toolearn Další.
