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
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-iot-hub-using-the-azure-cli-20"></a><span data-ttu-id="d2969-103">Vytvoření služby IoT hub pomocí Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="d2969-103">Create an IoT hub using the Azure CLI 2.0</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="d2969-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="d2969-104">Introduction</span></span>

<span data-ttu-id="d2969-105">Azure CLI 2.0 (az.py) můžete použít k vytváření a správě Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="d2969-105">You can use Azure CLI 2.0 (az.py) to create and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="d2969-106">Tento článek ukazuje, jak používat Azure CLI 2.0 (az.py) k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d2969-106">This article shows you how to use the Azure CLI 2.0 (az.py) to create an IoT hub.</span></span>

<span data-ttu-id="d2969-107">K dokončení úlohy můžete využít jednu z následujících verzí rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="d2969-107">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="d2969-108">[Rozhraní příkazového řádku Azure (azure.js)](iot-hub-create-using-cli-nodejs.md) – rozhraní příkazového řádku pro modelem nasazení classic a resource správy.</span><span class="sxs-lookup"><span data-stu-id="d2969-108">[Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – the CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="d2969-109">Azure CLI 2.0 (az.py) - nové generace rozhraní příkazového řádku pro model nasazení prostředků správy popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="d2969-109">Azure CLI 2.0 (az.py) - the next generation CLI for the resource management deployment model as described in this article.</span></span>

<span data-ttu-id="d2969-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d2969-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="d2969-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d2969-111">An active Azure account.</span></span> <span data-ttu-id="d2969-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d2969-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d2969-113">[Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="d2969-113">[Azure CLI 2.0][lnk-CLI-install].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="d2969-114">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="d2969-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="d2969-115">Přihlaste se k účtu Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="d2969-115">Sign in to your Azure account and select your subscription.</span></span>

1. <span data-ttu-id="d2969-116">Na příkazovém řádku, spusťte [přihlášení příkaz][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="d2969-116">At the command prompt, run the [login command][lnk-login-command]:</span></span>
    
    ```azurecli
    az login
    ```

    <span data-ttu-id="d2969-117">Postupujte podle pokynů k ověření pomocí kódu a přihlaste se k účtu Azure prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d2969-117">Follow the instructions to authenticate using the code and sign in to your Azure account through a web browser.</span></span>

2. <span data-ttu-id="d2969-118">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup k Azure účty přidružené přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="d2969-118">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure accounts associated with your credentials.</span></span> <span data-ttu-id="d2969-119">Použijte následující [seznam účtů Azure příkazu] [ lnk-az-account-command] k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="d2969-119">Use the following [command to list the Azure accounts][lnk-az-account-command] available for you to use:</span></span>
    
    ```azurecli
    az account list 
    ```

    <span data-ttu-id="d2969-120">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů pro vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d2969-120">Use the following command to select subscription that you want to use to run the commands to create your IoT hub.</span></span> <span data-ttu-id="d2969-121">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="d2969-121">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a><span data-ttu-id="d2969-122">Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="d2969-122">Create an IoT Hub</span></span>

<span data-ttu-id="d2969-123">Pomocí rozhraní příkazového řádku Azure k vytvoření skupiny prostředků a poté přidejte služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d2969-123">Use the Azure CLI to create a resource group and then add an IoT hub.</span></span>

1. <span data-ttu-id="d2969-124">Když vytvoříte Centrum IoT, musíte ji vytvořit ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="d2969-124">When you create an IoT hub, you must create it in a resource group.</span></span> <span data-ttu-id="d2969-125">Použijte existující skupinu prostředků nebo spusťte následující [příkazu vytvořte skupinu prostředků][lnk-az-resource-command]:</span><span class="sxs-lookup"><span data-stu-id="d2969-125">Either use an existing resource group, or run the following [command to create a resource group][lnk-az-resource-command]:</span></span>
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > <span data-ttu-id="d2969-126">Předchozí příklad vytvoří skupinu prostředků v umístění západní USA.</span><span class="sxs-lookup"><span data-stu-id="d2969-126">The previous example creates the resource group in the West US location.</span></span> <span data-ttu-id="d2969-127">Spuštěním příkazu můžete zobrazit seznam dostupných umístění `az account list-locations -o table`.</span><span class="sxs-lookup"><span data-stu-id="d2969-127">You can view a list of available locations by running the command `az account list-locations -o table`.</span></span>
    >
    >

2. <span data-ttu-id="d2969-128">Spusťte následující [příkaz pro vytvoření služby IoT hub] [ lnk-az-iot-command] ve vaší skupině prostředků pomocí globálně jedinečného názvu pro službu IoT hub:</span><span class="sxs-lookup"><span data-stu-id="d2969-128">Run the following [command to create an IoT hub][lnk-az-iot-command] in your resource group, using a globally unique name for your IoT hub:</span></span>
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> <span data-ttu-id="d2969-129">Předchozí příkaz vytvoří služby IoT hub S1 cenovou úroveň, pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="d2969-129">The previous command creates an IoT hub in the S1 pricing tier for which you are billed.</span></span> <span data-ttu-id="d2969-130">Další informace najdete v tématu [ceny služby Azure IoT Hub][lnk-iot-pricing].</span><span class="sxs-lookup"><span data-stu-id="d2969-130">For more information, see [Azure IoT Hub pricing][lnk-iot-pricing].</span></span>
>
>

## <a name="remove-an-iot-hub"></a><span data-ttu-id="d2969-131">Odeberte služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="d2969-131">Remove an IoT Hub</span></span>

<span data-ttu-id="d2969-132">Můžete použít rozhraní příkazového řádku Azure pro [odstranit prostředek jednotlivých][lnk-az-resource-command], například služby IoT hub, nebo odstranit skupinu prostředků a všechny její prostředky, včetně všech centra IoT.</span><span class="sxs-lookup"><span data-stu-id="d2969-132">You can use the Azure CLI to [delete an individual resource][lnk-az-resource-command], such as an IoT hub, or delete a resource group and all its resources, including any IoT hubs.</span></span>

<span data-ttu-id="d2969-133">Pokud chcete odstranit centrum IoT, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2969-133">To delete an IoT hub, run the following command:</span></span>

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

<span data-ttu-id="d2969-134">Pokud chcete odstranit skupinu prostředků a všechny její prostředky, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2969-134">To delete a resource group and all its resources, run the following command:</span></span>

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a><span data-ttu-id="d2969-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2969-135">Next steps</span></span>
<span data-ttu-id="d2969-136">Další informace o vývoji pro Centrum IoT, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="d2969-136">To learn more about developing for IoT Hub, see the following articles:</span></span>

* <span data-ttu-id="d2969-137">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d2969-137">[IoT Hub developer guide][lnk-devguide]</span></span>

<span data-ttu-id="d2969-138">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="d2969-138">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d2969-139">[Použití portálu Azure ke správě služby IoT Hub][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="d2969-139">[Using the Azure portal to manage IoT Hub][lnk-portal]</span></span>

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
