---
title: "Connect Intel Edison (C) tooAzure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat Edison hello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 9635e916425883d65793d0ed46843ab49b3f35ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a><span data-ttu-id="2b20d-103">Vytvoření služby IoT hub a zaregistrujte Intel Edison</span><span class="sxs-lookup"><span data-stu-id="2b20d-103">Create your IoT hub and register Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="2b20d-104">Co provedete</span><span class="sxs-lookup"><span data-stu-id="2b20d-104">What you will do</span></span>
* <span data-ttu-id="2b20d-105">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="2b20d-105">Create a resource group.</span></span>
* <span data-ttu-id="2b20d-106">Vytvoření služby Azure IoT hub ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="2b20d-106">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="2b20d-107">Přidáte Intel Edison toohello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="2b20d-107">Add Intel Edison toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="2b20d-108">Pokud používáte Azure CLI tooadd hello Edison tooyour IoT hub, služba hello generuje klíč pro Edison tooauthenticate službou hello.</span><span class="sxs-lookup"><span data-stu-id="2b20d-108">When you use hello Azure CLI tooadd Edison tooyour IoT hub, hello service generates a key for Edison tooauthenticate with hello service.</span></span> <span data-ttu-id="2b20d-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="2b20d-109">If you have any problems, look for solutions on hello [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2b20d-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="2b20d-110">What you will learn</span></span>
<span data-ttu-id="2b20d-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="2b20d-111">In this article, you will learn:</span></span>
* <span data-ttu-id="2b20d-112">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b20d-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="2b20d-113">Jak toocreate identitu zařízení pro Edison ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b20d-113">How toocreate a device identity for Edison in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2b20d-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2b20d-114">What you need</span></span>
* <span data-ttu-id="2b20d-115">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="2b20d-115">An Azure account.</span></span> <span data-ttu-id="2b20d-116">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="2b20d-116">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
* <span data-ttu-id="2b20d-117">Měli byste mít hello nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="2b20d-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="2b20d-118">Vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="2b20d-118">Create your IoT hub</span></span>
<span data-ttu-id="2b20d-119">Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky.</span><span class="sxs-lookup"><span data-stu-id="2b20d-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="2b20d-120">toocreate služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2b20d-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="2b20d-121">Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2b20d-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="2b20d-122">Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="2b20d-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="2b20d-123">Nastavit výchozí předplatné hello chcete toouse spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b20d-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="2b20d-124">`subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="2b20d-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="2b20d-125">Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="2b20d-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="2b20d-126">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2b20d-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="2b20d-127">Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="2b20d-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="2b20d-128">Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA hello, spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b20d-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="2b20d-129">`westus`je umístění hello vytvoříte vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="2b20d-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="2b20d-130">Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.</span><span class="sxs-lookup"><span data-stu-id="2b20d-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="2b20d-131">Vytvoření služby IoT hub ve skupině prostředků iot-sample hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b20d-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="2b20d-132">Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="2b20d-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="2b20d-133">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="2b20d-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="2b20d-134">Hello název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2b20d-134">hello name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="2b20d-135">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2b20d-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>


## <a name="register-edison-in-your-iot-hub"></a><span data-ttu-id="2b20d-136">Edison zaregistrovat ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="2b20d-136">Register Edison in your IoT hub</span></span>
<span data-ttu-id="2b20d-137">Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="2b20d-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="2b20d-138">Registrovat Edison ve službě IoT hub a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b20d-138">Register Edison in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="2b20d-139">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2b20d-139">Summary</span></span>
<span data-ttu-id="2b20d-140">Po vytvoření služby IoT hub a zaregistrována Edison identitu zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b20d-140">You've created an IoT hub and registered Edison with a device identity in your IoT hub.</span></span> <span data-ttu-id="2b20d-141">Jste připravené toolearn jak toosend zpráv ze služby IoT hub Edison tooyour.</span><span class="sxs-lookup"><span data-stu-id="2b20d-141">You're ready toolearn how toosend messages from Edison tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2b20d-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b20d-142">Next steps</span></span>
<span data-ttu-id="2b20d-143">[Vytvoření aplikace Azure funkce a služby úložiště Azure účtu úložiště a tooprocess IoT hub zpráv][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="2b20d-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md