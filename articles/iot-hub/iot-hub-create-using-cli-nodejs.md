---
title: "Vytvoření služby IoT hub pomocí rozhraní příkazového řádku Azure (azure.js) | Microsoft Docs"
description: "Postup vytvoření služby Azure IoT hub pomocí Azure CLI (azure.js) napříč platformami."
services: iot-hub
documentationcenter: .net
author: BeatriceOltean
manager: timlt
editor: 
ms.assetid: 46a17831-650c-41d9-b228-445c5bb423d3
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/04/2017
ms.author: boltean
ms.openlocfilehash: 5e37c6c5e8625ce446ab203f19f9a8b2f1cd5a46
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Vytvoření služby IoT hub pomocí rozhraní příkazového řádku Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Rozhraní příkazového řádku Azure (azure.js) můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu. Tento článek ukazuje, jak používat rozhraní příkazového řádku Azure (azure.js) k vytvoření služby IoT hub.

K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:

* Azure CLI (azure.js) – rozhraní příkazového řádku pro classic a resource správu modelech nasazení popsané v tomto článku.
* [Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) – nové generace rozhraní příkazového řádku pro model nasazení prostředků správy.

Pro absolvování tohoto kurzu potřebujete:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rozhraní příkazového řádku Azure 0.10.4] [ lnk-CLI-install] nebo novější. Pokud již máte nainstalované rozhraní příkazového řádku Azure, můžete ověřit aktuální verze na příkazovém řádku pomocí následujícího příkazu:

```azurecli
azure --version
```

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Rozhraní příkazového řádku Azure musí být v režimu Azure Resource Manager:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Nastavení předplatného a účtu Azure

1. Na příkazovém řádku, přihlášení zadáním následujícího příkazu:

   ```azurecli
    azure login
   ```

   Navrhované webový prohlížeč a kódu použijte k ověření.
1. Pokud máte víc předplatných Azure, připojení k Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů. Můžete zobrazit předplatná Azure a určit, která je výchozí, pomocí příkazu:

   ```azurecli
    azure account list
   ```

   Chcete-li nastavit kontext předplatného, ve kterém chcete spustit zbytek použití příkazů:

   ```azurecli
    azure account set <subscription name>
   ```

1. Pokud jste skupinu prostředků, můžete vytvořit jednu s názvem **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> Článek [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků] [ lnk-CLI-arm] poskytuje další informace o tom, jak používat rozhraní příkazového řádku Azure ke správě prostředků Azure.

## <a name="create-an-iot-hub"></a>Vytvoření služby IoT Hub

Požadované parametry:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **Skupina prostředků**. Název skupiny prostředků. Formát je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délka 1-64.
* **name**. Název služby IoT hub, který se má vytvořit. Formát je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délku 3 až 50.
* **umístění**. Umístění (oblast/datové centrum azure) ke zřízení služby IoT hub.
* **Název SKU**. Název sku, jeden z: [F1, S1, S2, S3]. Nejnovější úplný seznam naleznete na stránce s cenami pro IoT Hub.
* **jednotky**. Počet jednotek zřízené. Rozsah: F1 [1-1]: S1, S2 [1 – 200]: [1 – 10] S3. Jednotky služby IoT Hub jsou založené na celkovém počtu zpráv a počet zařízení, které se chcete připojit.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Pokud chcete zobrazit všechny parametry, které jsou k dispozici pro vytvoření, můžete příkaz help v příkazovém řádku:

```azurecli
azure iothub create -h
```

Zběžný příklad: vytvoření služby IoT Hub s názvem **exampleIoTHubName** ve skupině prostředků **exampleResourceGroup**, spusťte následující příkaz:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Tento příkaz rozhraní příkazového řádku Azure vytvoří S1 Standard IoT Hub pro kterou se účtují. Odstraněním služby IoT hub **exampleIoTHubName** pomocí následující příkaz:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Další kroky

Další informace o vývoji pro IoT Hub, najdete v následujícím článku:

* [Sady SDK služby IoT][lnk-sdks]

Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:

* [Použití portálu Azure ke správě služby IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
