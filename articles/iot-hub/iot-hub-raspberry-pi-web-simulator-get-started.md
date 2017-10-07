---
title: "aaaSimulated malin pí toocloud (Node.js) – tooAzure simulátoru webové připojení malin platformy IoT Hub | Microsoft Docs"
description: "Připojte malin pí webové simulátoru tooAzure IoT Hub pro platformy malin toosend data toohello cloudu Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Malinová pí simulátoru, azure Malinová pi, malinová platformy iot Centrum iot, malinová pí odesílat data toocloud Malinová pí toocloud"
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/28/2017
ms.author: xshi
ms.openlocfilehash: 83736caf6ce723a49001058495a780f7f51946a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a>Připojit tooAzure online simulátoru malin platformy IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

V tomto kurzu zahájíte učení hello základy práce s online simulátoru malin pí. Pak zjistíte, jak připojit tooseamlessly hello pí simulátoru toohello cloudu pomocí [Azure IoT Hub](iot-hub-what-is-iot-hub.md). 

Pokud máte fyzické zařízení, navštivte [tooAzure připojit malin platformy IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) tooget spuštěna. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Co dělat

* Přečtěte si základy hello online simulátoru malin pí.
* Vytvoření služby IoT hub.
* Registrovat zařízení pro platformy ve službě IoT hub.
* Spuštění ukázkové aplikace na platformy toosend simulated senzor data tooyour IoT hub.

Připojte simulované malin pí tooan IoT hub, který vytvoříte. Pak spusťte ukázkovou aplikaci s dat snímačů toogenerate simulátoru hello. Nakonec odeslat hello senzor data tooyour IoT hub.

## <a name="what-you-learn"></a>Co se naučíte

* Jak toocreate služby Azure IoT hub a získat nový připojovací řetězec zařízení. Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.
* Jak toowork s online simulátoru malin pí.
* Jak toosend senzor data tooyour IoT hub.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Přehled simulátoru webové platformy malin

Klikněte na tlačítko hello tlačítko toolaunch malin pí online simulátoru.

> [!div class="button"]
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Spustit simulátoru Malinová platformy</a>

Existují tři oblasti v simulátoru webové hello.
1. Sestavení oblast – hello výchozí okruh je, že na Pi připojí senzor BME280 a Indikátor. Hello oblasti uzamčeno ve verzi preview proto nyní nemůže provádět přizpůsobení.
2. Kódování oblast – editor online kód pro toocode s malin pí. Hello výchozí ukázkovou aplikaci pomáhá toocollect data snímačů z BME280 senzor a odešle tooyour Azure IoT Hub. aplikace Hello je plně kompatibilní se skutečné platformy zařízení. 
3. Okno integrovaného konzoly - zobrazuje výstup hello kódu. Hello horní části tohoto okna existují tři tlačítka.
   * **Spustit** -hello aplikaci spustit v hello kódování oblasti.
   * **Resetování** -resetování hello kódování oblasti toohello výchozí ukázkovou aplikaci.
   * **Násobek/rozšířit** – na pravé straně existuje je tlačítko pro můžete rozšířit nebo toofold hello okna konzoly hello.

> [!NOTE] 
Simulátor webové platformy malin Hello je teď dostupná ve verzi preview. Rádi bychom znali toohear váš hlas v hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). Hello zdrojový kód je veřejný na [Githubu](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Přehled platformy online simulátoru](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Spouštět ukázkovou aplikaci v simulátoru webové platformy

1. V oblasti kódování, zkontrolujte, zda že pracujete na hello výchozí ukázkovou aplikaci. Nahraďte zástupný symbol hello v řádku 15 hello Azure IoT hub zařízení připojovací řetězec.
   ![Nahraďte řetězec připojení zařízení hello](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Klikněte na tlačítko **spustit** nebo typ `npm start` toorun hello aplikace.


Měli byste vidět hello následující výstup, který zobrazuje data snímačů hello a hello zpráv, které jsou odeslány tooyour IoT hub ![výstup - senzor dat odesílaných ze služby IoT hub malin pí tooyour](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Další kroky

Spustil jsem ukázková data snímačů toocollect aplikace a odešlete ji tooyour IoT hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
