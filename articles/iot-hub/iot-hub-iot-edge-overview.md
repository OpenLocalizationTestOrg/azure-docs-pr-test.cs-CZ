---
title: aaaOverview Azure IoT Edge | Microsoft Docs
description: "Popisuje hello klíče architektury koncepty v Azure IoT Edge například brány, moduly a zprostředkovatelé."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.openlocfilehash: 32debc0d4f40cfd7f2cce7cf8c76b12ec18ee2dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Koncepty architektury služby Azure IoT Edge

Před zkontrolujte všechny ukázkový kód nebo vytvořit vlastní pole bránu pomocí IoT Edge, byste měli porozumět hello klíčové koncepty, které jsou základem hello architektura IoT okraj.

## <a name="iot-edge-modules"></a>Moduly IoT Edge

Vytvořit bránu s Azure IoT hranou vytvořením a ty se *IoT Edge moduly*. Použijte moduly *zprávy* tooexchange data mezi sebou. Modul přijme zprávu o, provádí některé akce, případně ho transformuje na novou zprávu a pak publikuje pro ostatní moduly tooprocess. Některé moduly pouze vytvářejí nové zprávy a nezpracovávají zprávy příchozí. Řetěz modulů vytvoří kanál zpracování dat se každý modul provádění transformace na text hello data na jednom místě v tomto kanálu.

![Řetěz modulů v bráně vytvořené s použitím služby Azure IoT Edge][1]

Okraj IoT obsahuje hello následující součásti:

* Předem napsané moduly, které provádějí běžné funkce brány.
* Hello rozhraní a vývojáře, můžete použít vlastní moduly toowrite.
* Hello toodeploy nezbytné infrastruktury a spustit sadu moduly.

Hello SDK poskytuje abstraktní vrstvu, která vám umožní toobuild brány toorun v různých operačních systémů a platformy.

![Vrstva abstrakce služby Azure IoT Edge][2]

## <a name="messages"></a>Zprávy

I když přemýšlíte o moduly předávání zpráv tooeach jiných je tooconceptualize pohodlný způsob, jak funkce brány, se nemusí odpovídat přesně co se stane. Moduly IoT Edge pomocí zprostředkovatele toocommunicate mezi sebou. Moduly publikování zpráv toohello zprostředkovatele (pomocí zasílání zpráv vzory, třeba sběrnice nebo publikování a přihlášení k odběru) a nechte hello zprostředkovatele trasy hello zpráva toohello moduly připojené tooit.

Modul používá hello **Broker_Publish** funkce toopublish toohello zprostředkovatele zpráv. Zprostředkovatel Hello přináší zprávy tooa modulu vyvoláním funkce zpětného volání. Zpráva se skládá ze sady vlastností klíč/hodnota a obsah se předává jako blok paměti.

![Hello role hello zprostředkovatele v Azure IoT Edge][3]

## <a name="message-routing-and-filtering"></a>Směrování a filtrování zpráv

Existují dva způsoby toodirect zprávy toohello správné IoT Edge moduly:

* Můžete předat sadu odkazy mohou být předány toohello zprostředkovatele tak hello zprostředkovatele zná hello zdroj a jímka pro každý modul.
* Modul můžete filtrovat podle vlastnosti hello hello zprávy.

Modul by měl jednat pouze na základě zprávu, pokud zpráva hello je určený pro ni. Odkazy a efektivně filtrování zpráv vytvoření kanálu zprávy.

## <a name="next-steps"></a>Další kroky

v tématu tyto koncepty použijí v ukázku, můžete spustit, toosee [architektura prozkoumat Edge Azure IoT][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md