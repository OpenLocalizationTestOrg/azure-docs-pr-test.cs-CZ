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
# <a name="create-an-iot-hub-using-hello-azure-cli-20"></a><span data-ttu-id="0e1dd-103">Vytvoření služby IoT hub pomocí hello 2.0 rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="0e1dd-103">Create an IoT hub using hello Azure CLI 2.0</span></span>

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a><span data-ttu-id="0e1dd-104">Úvod</span><span class="sxs-lookup"><span data-stu-id="0e1dd-104">Introduction</span></span>

<span data-ttu-id="0e1dd-105">Můžete použít toocreate 2.0 rozhraní příkazového řádku Azure (az.py) a spravovat Azure IoT hubs prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-105">You can use Azure CLI 2.0 (az.py) toocreate and manage Azure IoT hubs programmatically.</span></span> <span data-ttu-id="0e1dd-106">Tento článek ukazuje, jak toouse hello 2.0 rozhraní příkazového řádku Azure (az.py) toocreate služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-106">This article shows you how toouse hello Azure CLI 2.0 (az.py) toocreate an IoT hub.</span></span>

<span data-ttu-id="0e1dd-107">Můžete dokončit hello úloh pomocí jedné z hello následující verze rozhraní příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-107">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="0e1dd-108">[Rozhraní příkazového řádku Azure (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello rozhraní příkazového řádku pro hello classic a resource správy nasazení modelů.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-108">[Azure CLI (azure.js)](iot-hub-create-using-cli-nodejs.md) – hello CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="0e1dd-109">Azure CLI 2.0 (az.py) - hello nové generace rozhraní příkazového řádku pro hello správu model nasazení prostředků jak je popsáno v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-109">Azure CLI 2.0 (az.py) - hello next generation CLI for hello resource management deployment model as described in this article.</span></span>

<span data-ttu-id="0e1dd-110">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="0e1dd-111">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-111">An active Azure account.</span></span> <span data-ttu-id="0e1dd-112">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="0e1dd-112">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="0e1dd-113">[Rozhraní příkazového řádku Azure 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="0e1dd-113">[Azure CLI 2.0][lnk-CLI-install].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="0e1dd-114">Přihlaste se a nastavit váš účet Azure</span><span class="sxs-lookup"><span data-stu-id="0e1dd-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="0e1dd-115">Přihlaste se tooyour účet Azure a vybrat své předplatné.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="0e1dd-116">Na příkazovém řádku hello spustit hello [přihlášení příkaz][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>
    
    ```azurecli
    az login
    ```

    <span data-ttu-id="0e1dd-117">Postupujte podle tooauthenticate pokyny hello pomocí kódu hello a přihlaste se tooyour účet Azure prostřednictvím webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

2. <span data-ttu-id="0e1dd-118">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello Azure účty přidružené k přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="0e1dd-119">Použijte hello [toolist příkaz hello účty Azure] [ lnk-az-account-command] k dispozici pro toouse můžete:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>
    
    ```azurecli
    az account list 
    ```

    <span data-ttu-id="0e1dd-120">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toocreate služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="0e1dd-121">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="create-an-iot-hub"></a><span data-ttu-id="0e1dd-122">Vytvoření služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="0e1dd-122">Create an IoT Hub</span></span>

<span data-ttu-id="0e1dd-123">Pomocí rozhraní příkazového řádku Azure toocreate hello skupinu prostředků a poté přidejte služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-123">Use hello Azure CLI toocreate a resource group and then add an IoT hub.</span></span>

1. <span data-ttu-id="0e1dd-124">Když vytvoříte Centrum IoT, musíte ji vytvořit ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-124">When you create an IoT hub, you must create it in a resource group.</span></span> <span data-ttu-id="0e1dd-125">Buď použijte existující skupinu prostředků nebo spusťte následující hello [příkaz toocreate skupinu prostředků][lnk-az-resource-command]:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-125">Either use an existing resource group, or run hello following [command toocreate a resource group][lnk-az-resource-command]:</span></span>
    
    ```azurecli
     az group create --name {your resource group name} --location westus
    ```

    > [!TIP]
    > <span data-ttu-id="0e1dd-126">předchozí příklad Hello vytvoří skupinu prostředků hello v hello umístění západní USA.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-126">hello previous example creates hello resource group in hello West US location.</span></span> <span data-ttu-id="0e1dd-127">Seznam dostupných umístění, můžete zobrazit spuštěním příkazu hello `az account list-locations -o table`.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-127">You can view a list of available locations by running hello command `az account list-locations -o table`.</span></span>
    >
    >

2. <span data-ttu-id="0e1dd-128">Spusťte následující hello [příkaz toocreate služby IoT hub] [ lnk-az-iot-command] ve vaší skupině prostředků pomocí globálně jedinečného názvu pro službu IoT hub:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-128">Run hello following [command toocreate an IoT hub][lnk-az-iot-command] in your resource group, using a globally unique name for your IoT hub:</span></span>
    
    ```azurecli
    az iot hub create --name {your iot hub name} --resource-group {your resource group name} --sku S1
    ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


> [!NOTE]
> <span data-ttu-id="0e1dd-129">předchozí příkaz Hello vytvoří služby IoT hub v hello S1 cenovou úroveň, pro kterou se účtují.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-129">hello previous command creates an IoT hub in hello S1 pricing tier for which you are billed.</span></span> <span data-ttu-id="0e1dd-130">Další informace najdete v tématu [ceny služby Azure IoT Hub][lnk-iot-pricing].</span><span class="sxs-lookup"><span data-stu-id="0e1dd-130">For more information, see [Azure IoT Hub pricing][lnk-iot-pricing].</span></span>
>
>

## <a name="remove-an-iot-hub"></a><span data-ttu-id="0e1dd-131">Odeberte služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="0e1dd-131">Remove an IoT Hub</span></span>

<span data-ttu-id="0e1dd-132">Hello rozhraní příkazového řádku Azure můžete použít příliš[odstranit prostředek jednotlivých][lnk-az-resource-command], například služby IoT hub, nebo odstranit skupinu prostředků a všechny její prostředky, včetně všech centra IoT.</span><span class="sxs-lookup"><span data-stu-id="0e1dd-132">You can use hello Azure CLI too[delete an individual resource][lnk-az-resource-command], such as an IoT hub, or delete a resource group and all its resources, including any IoT hubs.</span></span>

<span data-ttu-id="0e1dd-133">toodelete služby IoT hub, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-133">toodelete an IoT hub, run hello following command:</span></span>

```azurecli
az iot hub delete --name {your iot hub name} --resource-group {your resource group name}
```

<span data-ttu-id="0e1dd-134">toodelete skupinu prostředků a všechny její prostředky, spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-134">toodelete a resource group and all its resources, run hello following command:</span></span>

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a><span data-ttu-id="0e1dd-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e1dd-135">Next steps</span></span>
<span data-ttu-id="0e1dd-136">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-136">toolearn more about developing for IoT Hub, see hello following articles:</span></span>

* <span data-ttu-id="0e1dd-137">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="0e1dd-137">[IoT Hub developer guide][lnk-devguide]</span></span>

<span data-ttu-id="0e1dd-138">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="0e1dd-138">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="0e1dd-139">[Pomocí hello portálu toomanage Azure IoT Hub][lnk-portal]</span><span class="sxs-lookup"><span data-stu-id="0e1dd-139">[Using hello Azure portal toomanage IoT Hub][lnk-portal]</span></span>

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
