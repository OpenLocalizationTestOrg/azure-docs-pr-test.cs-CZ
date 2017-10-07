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
# <a name="create-an-iot-hub-using-hello-azure-cli"></a><span data-ttu-id="d3cf0-103">Vytvoření služby IoT hub pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="d3cf0-103">Create an IoT hub using hello Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="d3cf0-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d3cf0-104">Introduction</span></span>

<span data-ttu-id="d3cf0-105">Můžete použít rozhraní příkazového řádku Azure (azure.js) toocreate a spravovat Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-105">You can use Azure CLI (azure.js) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="d3cf0-106">Tento článek ukazuje, jak toouse hello rozhraní příkazového řádku Azure (azure.js) toocreate služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-106">This article shows you how toouse hello Azure CLI (azure.js) toocreate an IoT hub.</span></span>

<span data-ttu-id="d3cf0-107">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="d3cf0-108">Azure CLI (azure.js) – hello rozhraní příkazového řádku pro hello classic a modelů nasazení správy prostředků, jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-108">Azure CLI (azure.js) – hello CLI for hello classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="d3cf0-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) -hello nové generace rozhraní příkazového řádku pro model nasazení správy prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - hello next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="d3cf0-110">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d3cf0-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-111">An active Azure account.</span></span> <span data-ttu-id="d3cf0-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d3cf0-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d3cf0-113">[Rozhraní příkazového řádku Azure 0.10.4] [ lnk-CLI-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="d3cf0-114">Pokud již máte hello nainstalované rozhraní příkazového řádku Azure, můžete ověřit aktuální verze hello hello příkazového řádku s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-114">If you already have hello Azure CLI installed, you can validate hello current version at hello command prompt with hello following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="d3cf0-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d3cf0-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d3cf0-116">Hello rozhraní příkazového řádku Azure musí být v režimu Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-116">hello Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="d3cf0-117">Nastavení předplatného a účtu Azure</span><span class="sxs-lookup"><span data-stu-id="d3cf0-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="d3cf0-118">Na příkazovém řádku hello hello přihlášení tak, že zadáte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-118">At hello command prompt, login by typing hello following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="d3cf0-119">Použijte hello navrhované webový prohlížeč a tooauthenticate kódu.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-119">Use hello suggested web browser and code tooauthenticate.</span></span>
1. <span data-ttu-id="d3cf0-120">Pokud máte víc předplatných Azure, připojení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-120">If you have multiple Azure subscriptions, connecting tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="d3cf0-121">Můžete zobrazit hello předplatných Azure a zjistit, které z nich je výchozí hello, pomocí příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-121">You can view hello Azure subscriptions, and identify which one is hello default, using hello command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="d3cf0-122">kontext předplatného hello tooset, pod kterým chcete toorun hello zbytek hello příkazy použití:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-122">tooset hello subscription context under which you want toorun hello rest of hello commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="d3cf0-123">Pokud jste skupinu prostředků, můžete vytvořit jednu s názvem **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="d3cf0-124">článek Hello [pomocí rozhraní příkazového řádku Azure toomanage hello Azure skupiny prostředků a prostředky] [ lnk-CLI-arm] poskytuje další informace o tom, jak toouse hello rozhraní příkazového řádku Azure toomanage Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-124">hello article [Use hello Azure CLI toomanage Azure resources and resource groups][lnk-CLI-arm] provides more information about how toouse hello Azure CLI toomanage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="d3cf0-125">Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="d3cf0-125">Create an IoT Hub</span></span>

<span data-ttu-id="d3cf0-126">Požadované parametry:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="d3cf0-127">**Skupina prostředků**.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-127">**resource-group**.</span></span> <span data-ttu-id="d3cf0-128">Název skupiny prostředků Hello.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-128">hello resource group name.</span></span> <span data-ttu-id="d3cf0-129">Formát Hello je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délka 1-64.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-129">hello format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="d3cf0-130">**name**.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-130">**name**.</span></span> <span data-ttu-id="d3cf0-131">Název Hello hello IoT hub toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-131">hello name of hello IoT hub toobe created.</span></span> <span data-ttu-id="d3cf0-132">Formát Hello je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délku 3 až 50.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-132">hello format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="d3cf0-133">**umístění**.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-133">**location**.</span></span> <span data-ttu-id="d3cf0-134">Hello umístění (oblast/datové centrum azure) tooprovision hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-134">hello location (azure region/datacenter) tooprovision hello IoT hub.</span></span>
* <span data-ttu-id="d3cf0-135">**Název SKU**.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-135">**sku-name**.</span></span> <span data-ttu-id="d3cf0-136">Název Hello hello sku, jeden z: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="d3cf0-136">hello name of hello sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="d3cf0-137">Hello nejnovější úplný seznam najdete v části toohello stránce s cenami pro IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-137">For hello latest full list, refer toohello pricing page for IoT Hub.</span></span>
* <span data-ttu-id="d3cf0-138">**jednotky**.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-138">**units**.</span></span> <span data-ttu-id="d3cf0-139">počet Hello zřízené jednotek.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-139">hello number of provisioned units.</span></span> <span data-ttu-id="d3cf0-140">Rozsah: F1 [1-1]: S1, S2 [1 – 200]: [1 – 10] S3.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="d3cf0-141">Jednotky služby IoT Hub jsou založené na vaše číslo zprávy celkový počet a hello zařízení chcete tooconnect.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-141">IoT Hub units are based on your total message count and hello number of devices you want tooconnect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="d3cf0-142">toosee všechny hello parametry, které jsou k dispozici pro vytvoření, můžete použít příkaz hello help v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-142">toosee all hello parameters available for creation, you can use hello help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="d3cf0-143">Zběžný příklad: toocreate názvem služby IoT Hub **exampleIoTHubName** ve skupině prostředků hello **exampleResourceGroup**spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-143">Quick example: toocreate an IoT Hub called **exampleIoTHubName** in hello resource group **exampleResourceGroup**, run hello following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="d3cf0-144">Tento příkaz rozhraní příkazového řádku Azure vytvoří S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="d3cf0-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="d3cf0-145">Odstraněním hello IoT hub **exampleIoTHubName** pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-145">You can delete hello IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="d3cf0-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3cf0-146">Next steps</span></span>

<span data-ttu-id="d3cf0-147">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následujícího článku:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-147">toolearn more about developing for IoT Hub, see hello following article:</span></span>

* <span data-ttu-id="d3cf0-148">[Sady SDK služby IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d3cf0-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d3cf0-149">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d3cf0-149">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d3cf0-150">[Pomocí hello portálu toomanage Azure IoT Hub][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="d3cf0-150">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
