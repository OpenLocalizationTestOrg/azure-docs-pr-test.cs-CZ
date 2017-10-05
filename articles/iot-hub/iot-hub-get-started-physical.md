---
title: "Začínáme fyzického zařízení připojovat k službě Azure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak se připojit ke službě Azure IoT Hub fyzických zařízení a na panely. Zařízení může odesílat telemetrická data do služby IoT Hub a IoT Hub můžete sledovat a spravovat vaše zařízení."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub kurzu
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: f4128b6b049aa876e170c56dcf2e40720644dc3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Azure IoT Hub začít pracovat s kurzy fyzických zařízení

Tyto kurzy vám představí Azure IoT Hub a sady SDK pro zařízení. Kurzů k zahrnují běžné scénáře IoT k předvedení funkcí služby IoT Hub. Kurzů k také znázorňují, jak kombinovat s jinými službami Azure a nástroje pro vytváření výkonnější IoT řešení IoT Hub. Kurzy uvedené v následující tabulce ukazují, jak vytvořit fyzického zařízení IoT.

| Zařízení IoT                       | Programovací jazyk |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                      | [Arduino v VSCode][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Prolnutí Adafruit HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 věc vývojářů      | [Arduino][Th_Ard]              |
| Prolnutí Adafruit M0             | [Arduino][M0_Ard]              |

Kromě toho můžete IoT vstupní brána k zařízením povolit, aby připojení do služby IoT hub.

| Zařízení brány               | Programovací jazyk | Platforma         |
|------------------------------|----------------------|------------------|
| Intel NUC (model DE3815TYKE) | C                    | [Linux větru oblasti][NUC_Lnx] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]


[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
