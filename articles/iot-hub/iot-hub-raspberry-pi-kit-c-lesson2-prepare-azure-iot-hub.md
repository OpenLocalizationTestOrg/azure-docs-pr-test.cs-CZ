---
title: "Connect Raspberry PI (C) k Azure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat platformy ve službě Azure IoT hub pomocí rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "připojit cloudové platformy Malinová pí cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d7bfd8f6ae8d15dfe09f06a40a4ab415ff2e0a7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Vytvoření služby IoT hub a zaregistrujte malin pí 3
## <a name="what-you-will-do"></a>Co provedete
* Vytvořte skupinu prostředků.
* Vytvoření služby Azure IoT hub ve skupině prostředků.
* Přidáte malin pí 3 ke službě Azure IoT hub pomocí rozhraní příkazového řádku Azure (Azure CLI).

Pokud používáte Azure CLI přidat pí do služby IoT hub, službu vygeneruje klíč pro platformy k ověření u služby. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.
* Postup vytvoření identity zařízení pí ve službě IoT hub.

## <a name="what-you-need"></a>Co potřebujete
* Účet Azure
* Mac nebo počítači se systémem Windows pomocí rozhraní příkazového řádku Azure, který je nainstalován

## <a name="create-your-iot-hub"></a>Vytvoření služby IoT hub
Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky. Pokud chcete vytvořit službu IoT hub, postupujte takto:

1. Přihlaste se k účtu Azure tak, že spustíte následující příkaz:

   ```bash
   az login
   ```

   Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.

2. Nastavte výchozí předplatné, které chcete použít tak, že spustíte následující příkaz:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`můžete najít ve výstupu `az login` nebo `az account list` příkaz.

3. Zaregistrujte zprostředkovatele spuštěním následujícího příkazu. Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci. Před nasazením Azure prostředek, který nabízí poskytovatele, je nutné zaregistrovat zprostředkovatele.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA spuštěním následujícího příkazu:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`je umístění, vytvořit skupiny prostředků. Pokud chcete použít jiné umístění, můžete spustit `az account list-locations -o table` zobrazíte všechna místa Azure podporuje.
 
5. Vytvoření služby IoT hub ve skupině prostředků iot-sample spuštěním následujícího příkazu:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   Ve výchozím nastavení vytvoří nástroj služby IoT Hub v cenové úrovně Free. Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Název služby IoT hub musí být globálně jedinečný. V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.

## <a name="register-pi-in-your-iot-hub"></a>Pi zaregistrovat ve službě IoT hub
Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.

Registrovat platformy v centru a spuštěné následující příkaz:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a>Souhrn
Po vytvoření služby IoT hub a zaregistrována pí identitu zařízení ve službě IoT hub. Jste připraveni se dozvíte, jak k odesílání zpráv z platformy do služby IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace Azure funkce a účet úložiště Azure pro zpracování a ukládání IoT Centrum zpráv](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

