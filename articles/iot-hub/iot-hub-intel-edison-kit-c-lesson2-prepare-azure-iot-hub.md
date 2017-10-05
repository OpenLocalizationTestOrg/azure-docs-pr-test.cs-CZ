---
title: "Connect Intel Edison (C) k Azure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat Edison ve službě Azure IoT hub pomocí rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 80bfc3cd-a1fc-4775-8994-d8033381dd3d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 52e3e4734dfd2b89f79b0c66683163e69b8e5f25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a>Vytvoření služby IoT hub a zaregistrujte Intel Edison
## <a name="what-you-will-do"></a>Co provedete
* Vytvořte skupinu prostředků.
* Vytvoření služby Azure IoT hub ve skupině prostředků.
* Přidáte Intel Edison ke službě Azure IoT hub pomocí rozhraní příkazového řádku Azure (Azure CLI).

Při použití Azure CLI pro přidání Edison do služby IoT hub, služba generuje klíč pro Edison k ověření u služby. Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].

## <a name="what-you-will-learn"></a>Co se dozvíte
V tomto článku se dozvíte:
* Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.
* Postup vytvoření identity zařízení pro Edison ve službě IoT hub.

## <a name="what-you-need"></a>Co potřebujete
* Účet Azure. Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.
* Měli byste mít nainstalované rozhraní příkazového řádku Azure.

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
> Název služby IoT hub musí být globálně jedinečný.
> V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.


## <a name="register-edison-in-your-iot-hub"></a>Edison zaregistrovat ve službě IoT hub
Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.

Registrovat Edison ve službě IoT hub a spuštěné následující příkaz:

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a>Souhrn
Po vytvoření služby IoT hub a zaregistrována Edison identitu zařízení ve službě IoT hub. Jste připraveni se dozvíte, jak k odesílání zpráv z Edison do služby IoT hub.

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace Azure funkce a účet úložiště Azure pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md