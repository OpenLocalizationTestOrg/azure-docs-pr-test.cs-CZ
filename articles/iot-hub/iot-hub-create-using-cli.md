---
title: "Vytvoření služby IoT Hub pomocí rozhraní příkazového řádku Azure (az.py) | Microsoft Docs"
description: "Postup vytvoření služby Azure IoT hub pomocí Azure CLI a platformy 2.0 (az.py)."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-hub
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 161089159999a4a63a39b059e69a08b7a9297445
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a>Vytvoření služby IoT hub pomocí Azure CLI 2.0

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Azure CLI 2.0 (az.py) můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu. Tento článek ukazuje, jak používat Azure CLI 2.0 (az.py) k vytvoření služby IoT hub.

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* [Rozhraní příkazového řádku Azure (azure.js)](iot-hub-create-using-cli-nodejs.md) – rozhraní příkazového řádku pro modelem nasazení classic a resource správy.
* Azure CLI 2.0 (az.py) - nové generace rozhraní příkazového řádku pro model nasazení prostředků správy popsané v tomto článku.

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Přihlaste se a nastavit váš účet Azure

Přihlaste se k účtu Azure a vybrat své předplatné.

1. Na příkazovém řádku, spusťte [přihlášení příkaz][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Postupujte podle pokynů k ověření pomocí kódu a přihlaste se k účtu Azure prostřednictvím webového prohlížeče.

2. Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup k Azure účty přidružené přihlašovací údaje. Použijte následující [seznam účtů Azure příkazu] [ lnk-az-account-command] k dispozici pro použití:
    
    ```azurecli
    az account list 
    ```

    Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů pro vytvoření služby IoT hub. Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Vytvoření služby IoT Hub

Pomocí rozhraní příkazového řádku Azure k vytvoření skupiny prostředků a poté přidejte služby IoT hub.

1. Když vytvoříte Centrum IoT, musíte ji vytvořit ve skupině prostředků. Použijte existující skupinu prostředků nebo spusťte následující [příkazu vytvořte skupinu prostředků][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > Předchozí příklad vytvoří skupinu prostředků v umístění západní USA. Spuštěním příkazu můžete zobrazit seznam dostupných umístění `az account list-locations -o table`.
    >
    >

2. Spusťte následující [příkaz pro vytvoření služby IoT hub] [ lnk-az-iot-command] ve vaší skupině prostředků pomocí globálně jedinečného názvu pro službu IoT hub:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> Předchozí příkaz vytvoří služby IoT hub S1 cenovou úroveň, pro kterou se účtují. Další informace najdete v tématu [ceny služby Azure IoT Hub][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Odeberte služby IoT Hub

Můžete použít rozhraní příkazového řádku Azure pro [odstranit prostředek jednotlivých][lnk-az-resource-command], například služby IoT hub, nebo odstranit skupinu prostředků a všechny její prostředky, včetně všech centra IoT.

Pokud chcete odstranit centrum IoT, spusťte následující příkaz:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

Pokud chcete odstranit skupinu prostředků a všechny její prostředky, spusťte následující příkaz:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Další kroky
Další informace o vývoji pro Centrum IoT, naleznete v následujících článcích:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Použití portálu Azure ke správě služby IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-devguide]: iot-hub-devguide.md
[lnk-portal]: iot-hub-create-through-portal.md 
