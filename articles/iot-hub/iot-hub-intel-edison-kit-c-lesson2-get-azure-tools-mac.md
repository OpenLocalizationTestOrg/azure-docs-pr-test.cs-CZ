---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 2: nástroje Azure (macOS) | Microsoft Docs"
description: "Instalace v systému macOS Python a rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "rozhraní příkazového řádku Azure, Cloudová služba iot, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: d561680f-69cc-427a-820d-24f710ba05a8
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 0dfc9ff90e879d5fd03040016ac71a9fe4f4a744
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-macos-1010"></a>Získat nástroje Azure (systému macOS 10.10)
> [!div class="op_single_selector"]
> * [Windows 7 a novější][windows]
> * [Ubuntu 16.04][ubuntu]
> * [systému macOS 10.10][macos]

## <a name="what-you-will-do"></a>Co provedete
Nainstalujte hello rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak tooinstall rozhraní příkazového řádku Azure.
* Jak tooadd podskupině hello rozhraní příkazového řádku Azure IoT.

## <a name="what-you-need"></a>Co potřebujete
* Mac s připojením k Internetu.
* Aktivní předplatné Azure. Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-python"></a>Instalace jazyka Python
I když systému macOS dodává s Python 2.7 předinstalované hello, doporučujeme nainstalovat Python prostřednictvím Homebrew. V tématu [Python instalace v systému macOS](http://docs.python-guide.org/en/latest/starting/install/osx/).

Instalace jazyka Python a pip spuštěním hello následující příkaz:

```bash
brew install python
```

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Práce přímo z vašeho tooprovision příkazového řádku a spravovat prostředky. 

tooinstall hello nejnovější rozhraní příkazového řádku Azure, postupujte takto:

1. Spusťte následující příkazy v okno terminálu hello. Může trvat pět minut tooinstall hello rozhraní příkazového řádku Azure.

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
2. Hello instalaci ověřte spuštěním hello následující příkaz:

   ```bash
   az iot -h
   ```

Měli byste vidět, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.

![Výstup, který označuje úspěch](media/iot-hub-intel-edison-lessons/lesson2/az_iot_help_osx.png)

## <a name="summary"></a>Souhrn
Jste nainstalovali hello rozhraní příkazového řádku Azure. Svůj další úkol je toocreate hello svou identitu Azure IoT hub a zařízení pomocí rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte Intel Edison][create-your-iot-hub-and-register-intel-edison]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[create-your-iot-hub-and-register-intel-edison]: iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md
[windows]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[ubuntu]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-ubuntu.md
[macos]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-mac.md
