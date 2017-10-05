---
title: "Simulované zařízení & brány Azure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub pro internet věcí cloudu azure iot hub vytvořit zařízení ti sensortag, ti zakázat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 23cfbe21-22c6-4fe1-ae41-63714a897f12
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5557989453eb47e4c3a287b26603eff040eb1d96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Vytvoření služby Azure IoT hub a registrace zařízení

## <a name="what-you-will-do"></a>Co provedete

- Vytvoření skupiny prostředků
- Vytvoření první centra IoT
- Registrace zařízení ve službě IoT hub pomocí rozhraní příkazového řádku Azure. 

Při registraci zařízení ve službě IoT hub, službu Azure IoT Hub generuje klíč pro vaše zařízení sloužící k ověření u služby. 

Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte

V této lekci se dozvíte:

- Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.
- Postup registrace zařízení do služby IoT hub.

## <a name="what-you-need"></a>Co potřebujete

- Aktivní předplatné Azure. Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.
- Měli byste mít nainstalované rozhraní příkazového řádku Azure.

## <a name="create-an-iot-hub"></a>Vytvoření centra IoT

K vytvoření služby IoT hub, postupujte takto:

1. Přihlaste se k účtu Azure tak, že spustíte následující příkaz:

   ```bash
   az login
   ```

   Po úspěšném přihlášení se objeví všech dostupných předplatných.

2. Nastavte výchozí předplatné Azure, který chcete použít tak, že spustíte následující příkaz:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`můžete najít ve výstupu `az login` nebo `az account list` příkaz.

3. Zaregistrujte zprostředkovatele spuštěním následujícího příkazu. Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci. Před nasazením Azure prostředek, který nabízí poskytovatele, je nutné zaregistrovat zprostředkovatele.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Vytvořte skupinu prostředků s názvem `iot-gateway` v oblasti západní USA spuštěním následujícího příkazu:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus`je umístění, vytvořit skupiny prostředků. Pokud chcete použít jiné umístění, můžete spustit `az account list-locations -o table` zobrazíte všechna místa Azure podporuje.

5. Vytvoření služby IoT hub v `iot-gateway` skupinu prostředků spuštěním následujícího příkazu:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Ve výchozím nastavení vytvoří nástroj služby IoT Hub v cenové úrovně Free. Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Název služby IoT hub musí být globálně jedinečný. V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure Iot Hub.

## <a name="register-your-device-in-your-iot-hub"></a>Registrace zařízení ve službě IoT hub

Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.
Registrovat zařízení ve službě IoT hub, a spuštěné následující příkaz:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Souhrn

Po vytvoření služby IoT hub a zaregistrována identitu zařízení logického zařízení ve službě IoT hub. Jste připraveni se dozvíte, jak pro konfiguraci a spuštění ukázkové aplikace brána k odesílání dat z fyzického zařízení do služby IoT hub v cloudu.

## <a name="next-steps"></a>Další kroky
[Nakonfigurujte a spusťte ukázkové aplikace simulovaného zařízení cloudu nahrávání](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)