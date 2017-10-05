---
title: "Připojení k Azure IoT - lekci 2 Intel Edison (uzel): Azure nástroje (Ubuntu) | Microsoft Docs"
description: "Nainstalujte na Ubuntu Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 6dcb34bf-54a3-4af0-ba89-95d5cfafceff
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 65248e14d0ea05a44efe76e66e1ef519285982b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a>Získat nástroje Azure (Ubuntu 16.04)
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
* Počítač s Ubuntu s připojením k Internetu.
* Aktivní předplatné Azure. Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-the-azure-cli"></a>Instalace rozhraní příkazového řádku Azure CLI
Rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, umožňuje pracovat přímo z příkazového řádku pro zřizování a správu prostředků.

Pokud chcete nainstalovat nejnovější rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v okno terminálu. Může trvat pět minut, chcete-li nainstalovat rozhraní příkazového řádku Azure.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Ověřte instalaci tak, že spustíte následující příkaz:

   ```bash
   az iot -h
   ```

Pokud k úspěšnému dokončení instalace, měli byste vidět následující výstup.

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Souhrn
Instalaci rozhraní příkazového řádku Azure. Svůj další úkol je vytvoření vaší Azure IoT hub a zařízení identity pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-node-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-node-lesson2-get-azure-tools-mac.md
