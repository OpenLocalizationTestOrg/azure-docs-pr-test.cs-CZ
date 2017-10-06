---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: registrace zařízení | Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub pro internet věcí cloudu azure iot hub vytvořit zařízení ti sensortag, ti zakázat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d2322268aa18f52f60c2833778323773ac4eec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Vytvoření služby Azure IoT hub a registrace zařízení

## <a name="what-you-will-do"></a>Co provedete

- Vytvoření skupiny prostředků
- Vytvoření první centra IoT
- Registrace zařízení ve službě IoT hub pomocí hello rozhraní příkazového řádku Azure. 

Při registraci zařízení ve službě IoT hub, hello služby Azure IoT Hub generuje klíč pro vaše zařízení toouse tooauthenticate službou hello. 

Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.
- Jak tooregister zařízení do služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Aktivní předplatné Azure. Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.
- Měli byste mít hello nainstalované rozhraní příkazového řádku Azure.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

toocreate služby IoT hub, postupujte takto:

1. Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:

   ```bash
   az login
   ```

   Po úspěšném přihlášení se objeví všech dostupných předplatných.

2. Nastavit výchozí hello předplatné Azure, že chcete toouse spuštěním hello následující příkaz:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.

3. Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello. Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci. Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Vytvořte skupinu prostředků s názvem `iot-gateway` v oblasti západní USA hello spuštěním hello následující příkaz:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus`je umístění hello vytvoříte vaší skupiny prostředků. Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.

5. Vytvoření služby IoT hub v hello `iot-gateway` skupinu prostředků spuštěním hello následující příkaz:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free. Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Hello název služby IoT hub musí být globálně jedinečný. V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure Iot Hub.

## <a name="register-your-device-in-your-iot-hub"></a>Registrace zařízení ve službě IoT hub

Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.
Registrovat zařízení ve službě IoT hub, a spuštěné následující příkaz:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Souhrn

Po vytvoření služby IoT hub a zaregistrována identitu zařízení logického zařízení ve službě IoT hub. Jste připravené toolearn jak cloudové tooconfigure a spustit bránu ukázková aplikace toosend data z centra IoT tooyour fyzického zařízení hello.

## <a name="next-steps"></a>Další kroky
[Nakonfigurujte a spusťte ukázkovou aplikaci zakázat](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)