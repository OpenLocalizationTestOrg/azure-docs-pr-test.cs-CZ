---
title: "Zařízení SensorTag & brány Azure IoT - Začínáme | Microsoft Docs"
description: "Začínáme s IoT brány Starter Kit, vytvoření služby Azure IoT hub a připojit SensorTag a brány toohello IoT hub"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub iot brány, Začínáme se službou hello internet věcí, iot toolkit"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Začínáme s IoT brány Starter Kit s SensorTag

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Simulované zařízení](iot-hub-gateway-kit-c-sim-get-started.md)

V tomto kurzu začnete podle učení hello základy práce s [IoT brány Starter Kit](https://aka.ms/gateway-kit). Budete pracovat s NUC Intel, se systémem Linux větru oblasti a hello [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Se dozvíte, jak tooseamleesly propojit své cloudové toohello zařízení pomocí služby Azure IoT Hub.

***
**Ještě není hotová kit?:** klikněte na tlačítko [zde](https://aka.ms/gateway-kit). **Není k dispozici SensorTag?:** [začínat simulované zařízení](iot-hub-gateway-kit-c-sim-get-started.md) nebo [koupit SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)
***

## <a name="lesson-1-configure-your-nuc"></a>Lekce 1: Konfigurace NUC
![Diagram začátku do konce Lesson1](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

V této lekci nastavit Intel NUC (Další jednotky Computing) v hello Kit jako bránu Azure IoT, nainstalovat balíček Azure IoT Edge hello na NUC a spusťte funkci brány tooverify hello ukázkové aplikace.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>Lekce 2: Vytvoření služby IoT Hub
![Diagram začátku do konce Lesson2](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

V této lekci nainstalovat nástroje hello a software na hostitelském počítači. Pak vytvoříte účet Azure zdarma, zřídit služby Azure IoT hub a vytvořit první zařízení IoT hub hello.

Před zahájením této lekci, proveďte Lekce 1.

### <a name="get-hello-tools"></a>Získat nástroje hello
Nainstalujte nástroje hello a software v hostitelském počítači.

*Odhadovaný čas toocomplete: 20 minut*

Přejděte příliš[získat nástroje hello](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Vytvoření služby IoT hub a registrace zařízení
Vytvoření skupiny prostředků, zřídit první Azure IoT hub a přidejte první zařízení toohello IoT hub pomocí hello rozhraní příkazového řádku Azure.

*Odhadovaný čas toocomplete: 10 minut*

Přejděte příliš[vytvoření služby IoT hub a registrace zařízení](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>Lekce 3: Přijetí zpráv z SensorTag a čtení zpráv z centra IoT
V této lekci použijete skripty tooautomate hello konfiguraci a spuštění ukázkové aplikace zakázat v bránu. Tyto aplikace použít kolekci s moduly tooaggregate a transformace dat, zpracování příkazů nebo proveďte libovolný počet souvisejících úloh. Moduly vzájemnou komunikaci přes zprostředkovatele zpráv. Ukázková aplikace Hello má modul zakázat a modul služby IoT hub. Hello zakázat modul přijímá data z SensorTag zakázat. Hello IoT hub modulu balíčky hello dat přijatých a odešle ji tooyour IoT hub prostřednictvím hello brány rámec poskytovaný v Azure IoT Edge.

![Diagram začátku do konce Lekce 3](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a>Nakonfigurujte a spusťte hello zakázat ukázkové aplikace
Nastavte hello propojení mezi SensorTag a bránu. Pak dokončit konfiguraci hello a spusťte hello zakázat ukázkovou aplikaci.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[konfigurace a spuštění hello zakázat ukázková aplikace](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Čtení zpráv z centra IoT
Spusťte ukázkový kód na hostiteli zprávy tooread počítače ze služby IoT hub.

*Odhadovaný čas toocomplete: 15 minut*

Přejděte příliš[čtení zpráv z centra IoT](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-tooazure-table-storage"></a>Lekce 4: Uložení zpráv tooAzure Table storage
Vytvořte aplikaci Azure funkce, která se získá příchozích zpráv ze služby IoT hub a zapíše tooAzure tabulka úložiště.

![Diagram začátku do konce Lekce 4](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Vytvořte aplikaci Azure funkce a účet úložiště Azure
Aplikace Azure funkce a účet úložiště Azure, použijte toocreate šablony Azure Resource Manager.

*Odhadovaný čas toocomplete: 10 minut*

Přejděte příliš[vytvořte aplikaci Azure funkce a účet úložiště Azure](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Čtení zprávy uchovávané v úložišti Azure Table
Zprávy brány cloud hello sledovat, jak je, jak jsou napsány tooAzure Table storage.

*Odhadovaný čas toocomplete: 5 minut*

Přejděte příliš[číst zprávy uchovávané v úložišti Azure Table](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Řešení potíží
Pokud máte potíže při hello lekce, vyhledejte řešení v hello [Poradce při potížích s](iot-hub-gateway-kit-c-troubleshooting.md) článku.

## <a name="explore-more"></a>Prozkoumat další
Navštivte hello [zóny developer Kit brány IoT Intel](http://software.intel.com/iot/microsoft-azure) toolearn Další.
