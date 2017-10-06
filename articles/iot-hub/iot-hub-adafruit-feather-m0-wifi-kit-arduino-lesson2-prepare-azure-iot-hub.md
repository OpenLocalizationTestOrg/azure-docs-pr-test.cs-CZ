---
title: "Připojit Arduino tooAzure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat Adafruit prolnutí M0 Wi-Fi ve hello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "připojení arduino toocloud, azure iot hub, internet věcí cloudové služby azure iot hub vytvoření zařízení, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ca362f9c143dd3a98bf47a66b63a9725a0ffc2d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a>Vytvoření služby IoT hub a zaregistrujte Adafruit prolnutí M0 Wi-Fi Arduino panel

## <a name="what-you-will-do"></a>Co provedete
* Vytvořte skupinu prostředků.
* Vytvoření služby Azure IoT hub ve skupině prostředků hello.
* Přidáte Arduino Tabule toohello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure (Azure CLI).

Při použití Azure CLI tooadd hello Arduino Tabule tooyour IoT hub služba hello generuje klíč pro vaše Arduino Tabule tooauthenticate službou hello. Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshoot].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.
* Jak toocreate identitu zařízení pro vaše Arduino board ve službě IoT hub.

## <a name="what-you-need"></a>Co potřebujete
* Účet Azure
* Počítač se hello nainstalované rozhraní příkazového řádku Azure

## <a name="create-your-iot-hub"></a>Vytvoření služby IoT hub
Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky. toocreate služby IoT hub, postupujte takto:

1. Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:

   ```bash
   az login
   ```

   Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.

2. Nastavit výchozí předplatné hello chcete toouse spuštěním hello následující příkaz:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.

3. Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello. Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci. Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA hello, spuštěním hello následující příkaz:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`je umístění hello vytvoříte vaší skupiny prostředků. Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.

5. Vytvoření služby IoT hub ve skupině prostředků iot-sample hello spuštěním hello následující příkaz:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free. Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Hello název služby IoT hub musí být globálně jedinečný.
> V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.

## <a name="register-your-arduino-board-in-your-iot-hub"></a>Panel Arduino zaregistrovat ve službě IoT hub
Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.

Panel Arduino registrovat ve službě IoT hub a spuštěné následující příkaz:

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a>Souhrn
Po vytvoření služby IoT hub a zaregistrována panel Arduino identitu zařízení ve službě IoT hub. Jste připravené toolearn jak toosend zprávy z Arduino Tabule tooyour IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace Azure funkce a služby úložiště Azure účtu úložiště a tooprocess IoT hub zpráv][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md