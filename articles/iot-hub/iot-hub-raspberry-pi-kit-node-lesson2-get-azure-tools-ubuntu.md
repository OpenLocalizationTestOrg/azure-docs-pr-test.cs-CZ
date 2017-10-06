---
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 2: získání nástroje (Ubuntu) | Microsoft Docs"
description: "Nainstalujte Python a na Ubuntu hello rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 2f98923a-5274-4220-87d4-77ac8beb4d0f
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0adf91bef41f502e1333fdcc75cfb2fe912364c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-ubuntu-1604"></a>Získat nástroje Azure (Ubuntu 16.04)
> [!div class="op_single_selector"]
> * [Windows 7 a novější](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak tooinstall hello rozhraní příkazového řádku Azure.
* Jak tooadd podskupině hello rozhraní příkazového řádku Azure IoT.

## <a name="what-you-need"></a>Co potřebujete
* Počítač s Ubuntu s připojením k Internetu.
* Aktivní předplatné Azure. Pokud účet nemáte, můžete vytvořit [Bezplatný zkušební účet](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure, takže se budete toowork přímo z vašeho příkazového řádku tooprovision a spravovat prostředky.

tooinstall hello nejnovější rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v okno terminálu hello. Může trvat pět minut tooinstall hello rozhraní příkazového řádku Azure.

   ```bash
   sudo apt-get update
   sudo apt-get install -y libssl-dev libffi-dev
   sudo apt-get install -y python-dev
   sudo apt-get install -y build-essential
   sudo apt-get install -y python-pip
   sudo pip install --upgrade azure-cli
   sudo pip install --upgrade azure-cli-iot
   ```
2. Hello instalaci ověřte spuštěním hello následující příkaz:

   ```bash
   az iot -h
   ```

Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_ubuntu.png)

## <a name="summary"></a>Souhrn
Jste nainstalovali hello rozhraní příkazového řádku Azure. Svůj další úkol je toocreate Azure IoT hub a pomocí identity zařízení hello rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)

