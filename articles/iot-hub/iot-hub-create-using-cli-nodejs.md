---
title: "aaaCreate služby IoT hub pomocí rozhraní příkazového řádku Azure (azure.js) | Microsoft Docs"
description: "Jak toocreate pomocí Azure IoT hub hello a platformy Azure CLI (azure.js)."
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
ms.openlocfilehash: c2a7ea98500b0a0e55a39f4cdfea4605c92add94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-iot-hub-using-hello-azure-cli"></a>Vytvoření služby IoT hub pomocí hello rozhraní příkazového řádku Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Úvod

Můžete použít rozhraní příkazového řádku Azure (azure.js) toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu. Tento článek ukazuje, jak toouse hello rozhraní příkazového řádku Azure (azure.js) toocreate služby IoT hub.

Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:

* Azure CLI (azure.js) – hello rozhraní příkazového řádku pro hello classic a modelů nasazení správy prostředků, jak je popsáno v tomto článku.
* [Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* [Rozhraní příkazového řádku Azure 0.10.4] [ lnk-CLI-install] nebo novější. Pokud již máte hello nainstalované rozhraní příkazového řádku Azure, můžete ověřit aktuální verze hello hello příkazového řádku s hello následující příkaz:

```azurecli
azure --version
```

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Hello rozhraní příkazového řádku Azure musí být v režimu Azure Resource Manager:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Nastavení předplatného a účtu Azure

1. Na příkazovém řádku hello hello přihlášení tak, že zadáte následující příkaz:

   ```azurecli
    azure login
   ```

   Použijte hello navrhované webový prohlížeč a tooauthenticate kódu.
1. Pokud máte víc předplatných Azure, připojení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů. Můžete zobrazit hello předplatných Azure a zjistit, které z nich je výchozí hello, pomocí příkazu hello:

   ```azurecli
    azure account list
   ```

   kontext předplatného hello tooset, pod kterým chcete toorun hello zbytek hello příkazy použití:

   ```azurecli
    azure account set <subscription name>
   ```

1. Pokud jste skupinu prostředků, můžete vytvořit jednu s názvem **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> článek Hello [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky] [ lnk-CLI-arm] poskytuje další informace o tom, jak toouse hello rozhraní příkazového řádku Azure toomanage Azure prostředky.

## <a name="create-an-iot-hub"></a>Vytvoření služby IoT Hub

Požadované parametry:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **Skupina prostředků**. Název skupiny prostředků Hello. Formát Hello je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délka 1-64.
* **name**. Název Hello hello IoT hub toobe vytvořili. Formát Hello je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délku 3 až 50.
* **umístění**. Hello umístění (oblast/datové centrum azure) tooprovision hello IoT hub.
* **Název SKU**. Název Hello hello sku, jeden z: [F1, S1, S2, S3]. Hello nejnovější úplný seznam najdete v části toohello stránce s cenami pro IoT Hub.
* **jednotky**. počet Hello zřízené jednotek. Rozsah: F1 [1-1]: S1, S2 [1 – 200]: [1 – 10] S3. Jednotky služby IoT Hub jsou založené na vaše číslo zprávy celkový počet a hello zařízení chcete tooconnect.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

toosee všechny hello parametry, které jsou k dispozici pro vytvoření, můžete použít příkaz hello help v příkazovém řádku:

```azurecli
azure iothub create -h
```

Zběžný příklad: toocreate názvem služby IoT Hub **exampleIoTHubName** ve skupině prostředků hello **exampleResourceGroup**spusťte hello následující příkaz:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Tento příkaz rozhraní příkazového řádku Azure vytvoří S1 Standard IoT Hub pro kterou se účtují. Odstraněním hello IoT hub **exampleIoTHubName** pomocí následující příkaz:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Další kroky

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následujícího článku:

* [Sady SDK služby IoT][lnk-sdks]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Pomocí hello portálu toomanage Azure IoT Hub][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
