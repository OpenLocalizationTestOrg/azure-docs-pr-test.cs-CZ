---
title: "Připojení k Azure IoT - lekci 2 Intel Edison (uzel): Azure nástroje (macOS) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 8a2a8031-b1a6-4219-b17d-2825550c35e1
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 16705d54e90ee1482822986ca8c581a192e8702f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-macos-1010"></a>Získat nástroje Azure (systému macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 a novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete
Nainstalujte rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

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

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Souhrn
Instalaci rozhraní příkazového řádku Azure. Svůj další úkol je vytvoření Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
