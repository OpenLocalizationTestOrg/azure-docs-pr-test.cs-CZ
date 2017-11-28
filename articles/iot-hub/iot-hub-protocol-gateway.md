---
title: "brány protokolu IoT aaaAzure | Microsoft Docs"
description: "Jak toouse Azure IoT protokolu brány tooextend IoT Hub možnosti a protokol podporu tooenable zařízení tooconnect tooyour centra pomocí protokolů nejsou podporovány službou IoT Hub nativně."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.openlocfilehash: 9cfed30149672d8f7e021a9899192105bbcdd400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# Podpora dalších protokoly pro IoT Hub
Azure IoT Hub nativně podporuje komunikaci přes protokoly MQTT, AMQP a HTTP hello. V některých případech zařízení nebo pole brány nemusí být možné toouse jednu z těchto standardních protokolů a bude vyžadovat protokol přizpůsobení. V takových případech můžete použít vlastní bránu. Vlastní bránu můžete povolit protokol přizpůsobení pro koncové body centra IoT tak přemostění hello provoz tooand ze služby IoT Hub. Můžete použít hello [brány protokolu Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) jako vlastní bránu tooenable protokolu přizpůsobení pro IoT Hub.

## Brány protokolu Azure IoT
brány protokolu Azure IoT Hello je rozhraní pro protokol přizpůsobení, která je určená pro špičkové obousměrnou komunikaci zařízení službou IoT Hub. Brána protokolu Hello je průchozí komponenty, která přijímá připojení zařízení přes určitý protokol. Ho přemosťuje hello provoz tooIoT centra prostřednictvím protokolu AMQP 1.0. brány protokolu Azure IoT Hello je k dispozici jako softwaru open source projektu tooprovide flexibilitu pro přidání podpory pro různé protokoly a verze protokolu.

Vysoce škálovatelné způsobem můžete nasadit brány protokolu hello v Azure pomocí Azure Service Fabric, rolí pracovního procesu Azure Cloud Services nebo virtuální počítače s Windows. Kromě toho brány protokolu hello se dá nasadit v prostředích místní, například pole brány.

brány protokolu Azure IoT Hello zahrnuje adaptér MQTT protokol, který umožňuje vám toocustomize hello MQTT chování protokolu v případě potřeby. Vzhledem k tomu, že služba IoT Hub zajišťuje integrovanou podporu pro hello MQTT v3.1.1 protokol, měli byste pouze zvážit použití adaptér protokolu MQTT hello Pokud vlastní nastavení protokolu nebo specifické požadavky pro další funkce, je potřeba.

adaptér MQTT Hello také ukazuje hello programovací model pro vytváření adaptéry protokolu pro jiné protokoly. Kromě toho hello Azure IoT protokolu brány programovací model umožňuje specializuje tooplug ve vlastních součástech pro zpracování, jako jsou vlastní ověřování, zpráva transformace, kompresi nebo dekompresi nebo šifrování a dešifrování přenosů mezi hello zařízení a služby IoT Hub.

Pružnosti brány protokolu hello a MQTT implementace jsou uvedeny v projektu open-source softwaru. To vám umožní toocustomize hello implementace podle potřeby.

## Další kroky
Další informace o brány protokolu Azure IoT hello toolearn a jak toouse a nasadit jako součást řešení IoT, naleznete v tématu:

* [Úložiště brány protokolu Azure IoT na Githubu](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Příručka vývojáře brány protokolu sady Azure IoT](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

toolearn Další informace o plánování nasazení služby IoT Hub, najdete v části:

* [Porovnání s služby Event Hubs][lnk-compare]
* [Změna velikosti, HA a zotavení po Havárii][lnk-scaling]
* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
