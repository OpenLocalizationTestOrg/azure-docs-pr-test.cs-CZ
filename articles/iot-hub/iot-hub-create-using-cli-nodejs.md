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
# <a name="create-an-iot-hub-using-the-azure-cli"></a><span data-ttu-id="7208d-103">Vytvoření služby IoT hub pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="7208d-103">Create an IoT hub using the Azure CLI</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="7208d-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="7208d-104">Introduction</span></span>

<span data-ttu-id="7208d-105">Rozhraní příkazového řádku Azure (azure.js) můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="7208d-105">You can use Azure CLI (azure.js) to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="7208d-106">Tento článek ukazuje, jak používat rozhraní příkazového řádku Azure (azure.js) k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="7208d-106">This article shows you how to use the Azure CLI (azure.js) to create an IoT hub.</span></span>

<span data-ttu-id="7208d-107">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="7208d-107">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="7208d-108">Azure CLI (azure.js) – rozhraní příkazového řádku pro classic a resource správu modelech nasazení popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="7208d-108">Azure CLI (azure.js) – the CLI for the classic and resource management deployment models as described in this article.</span></span>
* <span data-ttu-id="7208d-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) – nové generace rozhraní příkazového řádku pro model nasazení prostředků správy.</span><span class="sxs-lookup"><span data-stu-id="7208d-109">[Azure CLI 2.0 (az.py)](iot-hub-create-using-cli.md) - the next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="7208d-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="7208d-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="7208d-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="7208d-111">An active Azure account.</span></span> <span data-ttu-id="7208d-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="7208d-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="7208d-113">[Rozhraní příkazového řádku Azure 0.10.4] [ lnk-CLI-install] nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7208d-113">[Azure CLI 0.10.4][lnk-CLI-install] or later.</span></span> <span data-ttu-id="7208d-114">Pokud již máte nainstalované rozhraní příkazového řádku Azure, můžete ověřit aktuální verze na příkazovém řádku pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="7208d-114">If you already have the Azure CLI installed, you can validate the current version at the command prompt with the following command:</span></span>

```azurecli
azure --version
```

> [!NOTE]
> <span data-ttu-id="7208d-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Azure Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7208d-115">Azure has two different deployment models for creating and working with resources:  [Azure Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7208d-116">Rozhraní příkazového řádku Azure musí být v režimu Azure Resource Manager:</span><span class="sxs-lookup"><span data-stu-id="7208d-116">The Azure CLI must be in Azure Resource Manager mode:</span></span>
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a><span data-ttu-id="7208d-117">Nastavení předplatného a účtu Azure</span><span class="sxs-lookup"><span data-stu-id="7208d-117">Set your Azure account and subscription</span></span>

1. <span data-ttu-id="7208d-118">Na příkazovém řádku, přihlášení zadáním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="7208d-118">At the command prompt, login by typing the following command:</span></span>

   ```azurecli
    azure login
   ```

   <span data-ttu-id="7208d-119">Navrhované webový prohlížeč a kódu použijte k ověření.</span><span class="sxs-lookup"><span data-stu-id="7208d-119">Use the suggested web browser and code to authenticate.</span></span>
1. <span data-ttu-id="7208d-120">Pokud máte víc předplatných Azure, připojení k Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="7208d-120">If you have multiple Azure subscriptions, connecting to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="7208d-121">Můžete zobrazit předplatná Azure a určit, která je výchozí, pomocí příkazu:</span><span class="sxs-lookup"><span data-stu-id="7208d-121">You can view the Azure subscriptions, and identify which one is the default, using the command:</span></span>

   ```azurecli
    azure account list
   ```

   <span data-ttu-id="7208d-122">Chcete-li nastavit kontext předplatného, ve kterém chcete spustit zbytek použití příkazů:</span><span class="sxs-lookup"><span data-stu-id="7208d-122">To set the subscription context under which you want to run the rest of the commands use:</span></span>

   ```azurecli
    azure account set <subscription name>
   ```

1. <span data-ttu-id="7208d-123">Pokud jste skupinu prostředků, můžete vytvořit jednu s názvem **exampleResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="7208d-123">If you do not have a resource group, you can create one named **exampleResourceGroup**:</span></span>

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> <span data-ttu-id="7208d-124">Článek [používat rozhraní příkazového řádku Azure ke správě prostředků Azure a skupiny prostředků] [ lnk-CLI-arm] poskytuje další informace o tom, jak používat rozhraní příkazového řádku Azure ke správě prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="7208d-124">The article [Use the Azure CLI to manage Azure resources and resource groups][lnk-CLI-arm] provides more information about how to use the Azure CLI to manage Azure resources.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="7208d-125">Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="7208d-125">Create an IoT Hub</span></span>

<span data-ttu-id="7208d-126">Požadované parametry:</span><span class="sxs-lookup"><span data-stu-id="7208d-126">Required parameters:</span></span>

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* <span data-ttu-id="7208d-127">**Skupina prostředků**.</span><span class="sxs-lookup"><span data-stu-id="7208d-127">**resource-group**.</span></span> <span data-ttu-id="7208d-128">Název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="7208d-128">The resource group name.</span></span> <span data-ttu-id="7208d-129">Formát je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délka 1-64.</span><span class="sxs-lookup"><span data-stu-id="7208d-129">The format is case insensitive alphanumeric, underscore, and hyphen, 1-64 length.</span></span>
* <span data-ttu-id="7208d-130">**name**.</span><span class="sxs-lookup"><span data-stu-id="7208d-130">**name**.</span></span> <span data-ttu-id="7208d-131">Název služby IoT hub, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="7208d-131">The name of the IoT hub to be created.</span></span> <span data-ttu-id="7208d-132">Formát je malá a velká písmena alfanumerické znaky, podtržítka a pomlčky, délku 3 až 50.</span><span class="sxs-lookup"><span data-stu-id="7208d-132">The format is case insensitive alphanumeric, underscore, and hyphen, 3-50 length.</span></span>
* <span data-ttu-id="7208d-133">**umístění**.</span><span class="sxs-lookup"><span data-stu-id="7208d-133">**location**.</span></span> <span data-ttu-id="7208d-134">Umístění (oblast/datové centrum azure) ke zřízení služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="7208d-134">The location (azure region/datacenter) to provision the IoT hub.</span></span>
* <span data-ttu-id="7208d-135">**Název SKU**.</span><span class="sxs-lookup"><span data-stu-id="7208d-135">**sku-name**.</span></span> <span data-ttu-id="7208d-136">Název sku, jeden z: [F1, S1, S2, S3].</span><span class="sxs-lookup"><span data-stu-id="7208d-136">The name of the sku, one of: [F1, S1, S2, S3].</span></span> <span data-ttu-id="7208d-137">Nejnovější úplný seznam naleznete na stránce s cenami pro IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="7208d-137">For the latest full list, refer to the pricing page for IoT Hub.</span></span>
* <span data-ttu-id="7208d-138">**jednotky**.</span><span class="sxs-lookup"><span data-stu-id="7208d-138">**units**.</span></span> <span data-ttu-id="7208d-139">Počet jednotek zřízené.</span><span class="sxs-lookup"><span data-stu-id="7208d-139">The number of provisioned units.</span></span> <span data-ttu-id="7208d-140">Rozsah: F1 [1-1]: S1, S2 [1 – 200]: [1 – 10] S3.</span><span class="sxs-lookup"><span data-stu-id="7208d-140">Range: F1 [1-1] : S1, S2 [1-200] : S3 [1-10].</span></span> <span data-ttu-id="7208d-141">Jednotky služby IoT Hub jsou založené na celkovém počtu zpráv a počet zařízení, které se chcete připojit.</span><span class="sxs-lookup"><span data-stu-id="7208d-141">IoT Hub units are based on your total message count and the number of devices you want to connect.</span></span>

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

<span data-ttu-id="7208d-142">Pokud chcete zobrazit všechny parametry, které jsou k dispozici pro vytvoření, můžete příkaz help v příkazovém řádku:</span><span class="sxs-lookup"><span data-stu-id="7208d-142">To see all the parameters available for creation, you can use the help command in command prompt:</span></span>

```azurecli
azure iothub create -h
```

<span data-ttu-id="7208d-143">Zběžný příklad: vytvoření služby IoT Hub s názvem **exampleIoTHubName** ve skupině prostředků **exampleResourceGroup**, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7208d-143">Quick example: To create an IoT Hub called **exampleIoTHubName** in the resource group **exampleResourceGroup**, run the following command:</span></span>

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> <span data-ttu-id="7208d-144">Tento příkaz rozhraní příkazového řádku Azure vytvoří S1 Standard IoT Hub pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="7208d-144">This Azure CLI command creates an S1 Standard IoT Hub for which you are billed.</span></span> <span data-ttu-id="7208d-145">Odstraněním služby IoT hub **exampleIoTHubName** pomocí následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="7208d-145">You can delete the IoT hub **exampleIoTHubName** using following command:</span></span>
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a><span data-ttu-id="7208d-146">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7208d-146">Next steps</span></span>

<span data-ttu-id="7208d-147">Další informace o vývoji pro IoT Hub, najdete v následujícím článku:</span><span class="sxs-lookup"><span data-stu-id="7208d-147">To learn more about developing for IoT Hub, see the following article:</span></span>

* <span data-ttu-id="7208d-148">[Sady SDK služby IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="7208d-148">[IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="7208d-149">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="7208d-149">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="7208d-150">[Použití portálu Azure ke správě služby IoT Hub][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="7208d-150">[Using the Azure portal to manage IoT Hub][lnk-portal]</span></span>

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
