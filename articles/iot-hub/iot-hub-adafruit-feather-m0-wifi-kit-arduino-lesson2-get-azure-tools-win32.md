---
title: "Připojení k Azure IoT - lekci 2 Arduino: nástroje Azure (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 70dfff14-4be1-468d-9919-e40f4bead308
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1f121d9f22f8a7c8582df4d2e62191cec364da46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Získat nástroje Azure (Windows 7 a novější)

> [!div class="op_single_selector"]
> * [Windows 7 nebo novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete

Instalace jazyka Python a rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md) pro panel Adafruit prolnutí M0 Wi-Fi Arduino.

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Postup instalace Python.
* Postup instalace rozhraní příkazového řádku Azure.

## <a name="what-you-need"></a>Co potřebujete
* Počítač Windows s připojením k Internetu.
* Aktivní předplatné Azure. Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-python"></a>Instalace jazyka Python
[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows. Můžete nainstalovat Python 2.7, 3.4 nebo 3.5. V tomto kurzu vychází z Python 2.7. Pokud jste již nainstalovali Python, přejděte k další části a nainstalujte rozhraní příkazového řádku Azure.

Musíte taky přidat cestu složky, kde jsou v systému nainstalovány python.exe a pip.exe `PATH` proměnné prostředí. Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI
Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Pracovat přímo z příkazového řádku pro zřizování a správu prostředků.

Pokud chcete nainstalovat rozhraní příkazového řádku Azure, postupujte takto:

1. Otevřete okno příkazového řádku jako správce.
2. Spusťte následující příkazy:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Ověřte instalaci tak, že spustíte následující příkaz:

   ```bash
   az iot -h
   ```

Pokud je instalace úspěšná, zobrazí se následující výstup.

![Výstup, který označuje úspěch][output]

## <a name="summary"></a>Souhrn
Instalaci rozhraní příkazového řádku Azure. Svůj další úkol k vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte Arduino panel][create-your-iot-hub-and-register-your-arduino-board]


<!-- Images and links -->

[windows]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-mac.md
[output]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson2/az_iot_help_win.png
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md