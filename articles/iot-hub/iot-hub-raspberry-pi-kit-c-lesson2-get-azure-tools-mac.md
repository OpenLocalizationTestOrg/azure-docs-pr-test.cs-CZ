---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 2: nástroje Azure (macOS) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: f2d7d584-7734-401c-976c-81788a7282a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 3810990f4a27270fa45709f4d9dbb36a8f4369a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a>Získat nástroje Azure (systému macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 a novější](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Nainstalujte rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Postup instalace rozhraní příkazového řádku Azure.
* Postup přidání podskupině IoT rozhraní příkazového řádku Azure.

## <a name="what-you-need"></a>Co potřebujete
* Mac s připojením k Internetu.
* Aktivní předplatné Azure. Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-python"></a>Instalace jazyka Python
I když systému macOS dodává s Python 2.7 z pole, doporučujeme nainstalovat Python prostřednictvím Homebrew. V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Instalace Python a pip spuštěním následujícího příkazu:

```bash
brew install python
```

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI
Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Pracovat přímo z příkazového řádku pro zřizování a správu prostředků. 

Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v okno terminálu. Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Ověřte instalaci tak, že spustíte následující příkaz:

   ```bash
   az iot -h
   ```

Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Souhrn
Instalaci rozhraní příkazového řádku Azure. Svůj další úkol je vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

