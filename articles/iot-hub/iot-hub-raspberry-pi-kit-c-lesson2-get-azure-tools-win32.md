---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 2: nástroje Azure (Windows) | Microsoft Docs"
description: "Ve Windows 7 a novějších verzích nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI)."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT cloudové služby, azure cli"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: a3c083b5-0d76-46af-bc77-2ad7d8aadc1e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1819d61fafbee6ac42a1bea5c16437cd8bf43af9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-azure-tools-windows-7-and-later"></a>Získat nástroje Azure (Windows 7 a novější)
> [!div class="op_single_selector"]
> * [Windows 7 a novější](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md)
> * [Ubuntu 16.04](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-ubuntu.md)
> * [systému macOS 10.10](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-mac.md)

## <a name="what-you-will-do"></a>Co provedete
Nainstalujte Python a hello rozhraní příkazového řádku Azure (Azure CLI). Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak tooinstall Python.
* Jak tooinstall hello rozhraní příkazového řádku Azure.

## <a name="what-you-need"></a>Co potřebujete
* Počítač Windows s připojením k Internetu.
* Aktivní předplatné Azure. Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.

## <a name="install-python"></a>Instalace jazyka Python
[Instalaci Pythonu](https://www.python.org/downloads/) na počítači s Windows. Můžete nainstalovat Python 2.7, 3.4 nebo 3.5. V tomto kurzu vychází z Python 2.7. Pokud jste již nainstalovali Python, přejděte toohello další části a nainstalujte hello rozhraní příkazového řádku Azure.

Musíte taky tooadd hello cesta hello složek, kdy jsou python.exe a pip.exe systému nainstalovaná toohello `PATH` proměnné prostředí. Ve výchozím nastavení je python.exe nainstalován v `C:\Python27` a pip.exe je nainstalován v `C:\Python27\Scripts`.

## <a name="install-hello-azure-cli"></a>Nainstalujte hello rozhraní příkazového řádku Azure
Hello rozhraní příkazového řádku Azure nabízí prostředí s více platformami příkazového řádku Azure. Práce přímo z vašeho tooprovision příkazového řádku a spravovat prostředky.

tooinstall hello rozhraní příkazového řádku Azure, postupujte takto:

1. Otevřete okno příkazového řádku jako správce.
2. Spusťte následující příkazy hello:

   ```bash
   pip install --upgrade azure-cli
   pip install --upgrade azure-cli-iot
   ```
3. Hello instalaci ověřte spuštěním hello následující příkaz:

   ```bash
   az iot -h
   ```

Uvidíte, že hello následující výstup, pokud hello instalace byla úspěšně dokončena.

![Výstup, který označuje úspěch](media/iot-hub-raspberry-pi-lessons/lesson2/az_iot_help_win.png)

## <a name="summary"></a>Souhrn
Jste nainstalovali hello rozhraní příkazového řádku Azure. Vaše další úloh toocreate Azure IoT hub a zařízení identity pomocí hello rozhraní příkazového řádku Azure.

## <a name="next-steps"></a>Další kroky
[Vytvoření služby IoT hub a zaregistrujte malin pí 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md)

