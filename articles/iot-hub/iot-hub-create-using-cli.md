---
title: "aaaCreate služby IoT Hub pomocí rozhraní příkazového řádku Azure (az.py) | Microsoft Docs"
description: "Jak toocreate pomocí Azure IoT hub hello a platformy Azure CLI 2.0 (az.py)."
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
ms.openlocfilehash: 9c9639235c2ac343e6ceb9578291dafaea26ea24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a>Vytvoření služby IoT hub pomocí hello 2.0 rozhraní příkazového řádku Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Můžete použít toocreate 2.0 rozhraní příkazového řádku Azure (az.py) a spravovat Azure IoT hubs prostřednictvím kódu programu. Tento článek ukazuje, jak toouse hello 2.0 rozhraní příkazového řádku Azure (az.py) toocreate služby IoT hub.

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* [Rozhraní příkazového řádku Azure (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů.
* Azure CLI 2.0 (az.py) - hello nové generace rozhraní příkazového řádku pro hello správu model nasazení prostředků jak je popsáno v tomto článku.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].

## <a name="sign-in-and-set-your-azure-account"></a>Přihlaste se a nastavit váš účet Azure

Přihlaste se tooyour účet Azure a vybrat své předplatné.

1. Na příkazovém řádku hello spustit hello [přihlášení příkaz][lnk-login-command]:
    
    ```azurecli
    az login
    ```

    Postupujte podle tooauthenticate pokyny hello pomocí kódu hello a přihlaste se tooyour účet Azure prostřednictvím webového prohlížeče.

2. Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello Azure účty přidružené k přihlašovacích údajů. Použijte hello [toolist příkaz hello účty Azure] [ lnk-az-account-command] k dispozici pro toouse můžete:
    
    ```azurecli
    az account list 
    ```

    Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello. Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a>Vytvoření služby IoT Hub

Pomocí rozhraní příkazového řádku Azure toocreate hello skupinu prostředků a poté přidejte služby IoT hub.

1. Když vytvoříte Centrum IoT, musíte ji vytvořit ve skupině prostředků. Buď použijte existující skupinu prostředků nebo spusťte následující hello [příkaz toocreate skupinu prostředků][lnk-az-resource-command]:
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > předchozí příklad Hello vytvoří skupinu prostředků hello v hello umístění západní USA. Seznam dostupných umístění, můžete zobrazit spuštěním příkazu hello `az account list-locations -o table`.
    >
    >

2. Spusťte následující hello [příkaz toocreate služby IoT hub] [ lnk-az-iot-command] ve vaší skupině prostředků pomocí globálně jedinečného názvu pro službu IoT hub:
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> předchozí příkaz Hello vytvoří služby IoT hub v hello S1 cenovou úroveň, pro kterou se účtují. Další informace najdete v tématu [ceny služby Azure IoT Hub][lnk-iot-pricing].
>
>

## <a name="remove-an-iot-hub"></a>Odeberte služby IoT Hub

Hello rozhraní příkazového řádku Azure můžete použít příliš[odstranit prostředek jednotlivých][lnk-az-resource-command], například služby IoT hub, nebo odstranit skupinu prostředků a všechny její prostředky, včetně všech centra IoT.

toodelete služby IoT hub, spusťte následující příkaz hello:

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

toodelete skupinu prostředků a všechny její prostředky, spusťte hello následující příkaz:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Další kroky
toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:

* [Příručka vývojáře pro službu IoT Hub][lnk-devguide]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Pomocí hello portálu toomanage Azure IoT Hub][lnk-portal]

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
