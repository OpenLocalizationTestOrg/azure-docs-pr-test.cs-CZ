---
title: "Azure IoT Hub - Začínáme připojení cloudu toohello zařízení IoT | Microsoft Docs"
description: "Zjistěte, jak tooconnect panely IoT a starter Kit tooAzure IoT Hub. Zařízení může odesílat telemetrii tooIoT rozbočovače a IoT Hub můžete sledovat a spravovat vaše zařízení."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Azure iot hub kurzu
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a>Azure IoT Hub Začínáme kurzy

Můžete použít Azure IoT Hub a řešení Internetu věcí (IoT) toobuild zařízení sady SDK aplikace hello Azure IoT:

* Azure IoT Hub je plně spravovaná služba ve hello cloudu, který bezpečně připojit, monitoruje a spravuje zařízení IoT. Použijte hello SDK pro zařízení Azure IoT tooimplement zařízení IoT.
* Používejte bránu IoT v složitější scénáře IoT. Například, kde musíte tooconsider faktorům, jako např. zařízení se starší verzí, náklady na šířku pásma, zásady zabezpečení a ochrana osobních údajů nebo zpracování dat okraj. V těchto scénářích použijete tooimplement Azure IoT hraniční bránu, která se připojuje zařízení tooyour IoT hub.

## <a name="what-hello-tutorials-cover"></a>Co zahrnují kurzy hello

Tyto kurzy vzniku tooAzure IoT Hub a sady SDK pro zařízení hello. kurzy Hello zahrnují běžné IoT scénáře toodemonstrate hello funkce služby IoT Hub. Hello kurzy také ilustrují jak toocombine IoT Hub s další Azure služby a další nástroje toobuild výkonné řešení IoT. V kurzech hello můžete toouse simulované nebo skutečné zařízení IoT. Kromě toho můžete získat informace jak toouse brány tooenable zařízení tooconnect tooyour IoT hub.

## <a name="set-up-your-device"></a>Nastavení zařízení

Připojte IoT zařízení nebo brány tooAzure IoT Hub. Můžete vybrat fyzické nebo simulované zařízení tooget, spustit:

| Zařízení IoT                       | Programovací jazyk |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| IoT DevKit                       | [Arduino v VSCode][DevKit]     |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Prolnutí Adafruit HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 věc vývojářů       | [Arduino][Th_Ard]              |
| Prolnutí Adafruit M0              | [Arduino][M0_Ard]              |
| Simulované zařízení na počítačích           | [Rozhraní .NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Simulátor online zařízení         | [Malinová platformy (Node.js)][Ol_Sim] |

Kromě toho můžete Centrum IoT hraniční brány tooenable zařízení tooconnect tooyour IoT:

| Zařízení brány               | Programovací jazyk | Platforma         |
|------------------------------|----------------------|------------------|
| Intel NUC (model DE3815TYKE) | C                    | [Linux větru oblasti][NUC_Lnx] |
| Simulované brány            | C                    | [Linux][Sim_Lnx], [Windows][Sim_Win] |

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
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
