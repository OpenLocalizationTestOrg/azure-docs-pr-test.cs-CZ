---
title: "Simulované zařízení & brány Azure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub pro internet věcí cloudu azure iot hub vytvořit zařízení ti sensortag, ti zakázat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 23cfbe21-22c6-4fe1-ae41-63714a897f12
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d1052ed2fa9e022966e6e71fa2c7d4f18c333b2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a><span data-ttu-id="e07d6-103">Vytvoření služby Azure IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="e07d6-103">Create your Azure IoT hub and register your device</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e07d6-104">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e07d6-104">What you will do</span></span>

- <span data-ttu-id="e07d6-105">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e07d6-105">Create a resource group</span></span>
- <span data-ttu-id="e07d6-106">Vytvoření první centra IoT</span><span class="sxs-lookup"><span data-stu-id="e07d6-106">Create your first IoT hub</span></span>
- <span data-ttu-id="e07d6-107">Registrace zařízení ve službě IoT hub pomocí hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e07d6-107">Register your device in your IoT hub by using hello Azure CLI.</span></span> 

<span data-ttu-id="e07d6-108">Při registraci zařízení ve službě IoT hub, hello služby Azure IoT Hub generuje klíč pro vaše zařízení toouse tooauthenticate službou hello.</span><span class="sxs-lookup"><span data-stu-id="e07d6-108">When you register your device in your IoT hub, hello Azure IoT Hub service generates a key for your device toouse tooauthenticate with hello service.</span></span> 

<span data-ttu-id="e07d6-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e07d6-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e07d6-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e07d6-110">What you will learn</span></span>

<span data-ttu-id="e07d6-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e07d6-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="e07d6-112">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e07d6-112">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
- <span data-ttu-id="e07d6-113">Jak tooregister zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e07d6-113">How tooregister a device in an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e07d6-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e07d6-114">What you need</span></span>

- <span data-ttu-id="e07d6-115">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="e07d6-115">An active Azure subscription.</span></span> <span data-ttu-id="e07d6-116">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="e07d6-116">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
- <span data-ttu-id="e07d6-117">Měli byste mít hello nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="e07d6-117">You should have hello Azure CLI installed.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="e07d6-118">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="e07d6-118">Create an IoT hub</span></span>

<span data-ttu-id="e07d6-119">toocreate služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e07d6-119">toocreate an IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="e07d6-120">Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="e07d6-120">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="e07d6-121">Po úspěšném přihlášení se objeví všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="e07d6-121">All your available subscriptions will be listed after a successful sign-in.</span></span>

2. <span data-ttu-id="e07d6-122">Nastavit výchozí hello předplatné Azure, že chcete toouse spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e07d6-122">Set hello default Azure subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="e07d6-123">`subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e07d6-123">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="e07d6-124">Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="e07d6-124">Register hello provider by running hello following command.</span></span> <span data-ttu-id="e07d6-125">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e07d6-125">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="e07d6-126">Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="e07d6-126">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. <span data-ttu-id="e07d6-127">Vytvořte skupinu prostředků s názvem `iot-gateway` v oblasti západní USA hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e07d6-127">Create a resource group named `iot-gateway` in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   <span data-ttu-id="e07d6-128">`westus`je umístění hello vytvoříte vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="e07d6-128">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="e07d6-129">Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.</span><span class="sxs-lookup"><span data-stu-id="e07d6-129">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>

5. <span data-ttu-id="e07d6-130">Vytvoření služby IoT hub v hello `iot-gateway` skupinu prostředků spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e07d6-130">Create an IoT hub in hello `iot-gateway` resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

<span data-ttu-id="e07d6-131">Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="e07d6-131">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="e07d6-132">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="e07d6-132">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="e07d6-133">Hello název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="e07d6-133">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="e07d6-134">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure Iot Hub.</span><span class="sxs-lookup"><span data-stu-id="e07d6-134">You can create only one F1 edition of Azure Iot Hub under your Azure subscription.</span></span>

## <a name="register-your-device-in-your-iot-hub"></a><span data-ttu-id="e07d6-135">Registrace zařízení ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="e07d6-135">Register your device in your IoT hub</span></span>

<span data-ttu-id="e07d6-136">Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="e07d6-136">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>
<span data-ttu-id="e07d6-137">Registrovat zařízení ve službě IoT hub, a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e07d6-137">Register your device in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a><span data-ttu-id="e07d6-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e07d6-138">Summary</span></span>

<span data-ttu-id="e07d6-139">Po vytvoření služby IoT hub a zaregistrována identitu zařízení logického zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e07d6-139">You've created an IoT hub and registered your logical device with a device identity in your IoT hub.</span></span> <span data-ttu-id="e07d6-140">Jste připravené toolearn jak cloudové tooconfigure a spustit bránu ukázková aplikace toosend data z centra IoT tooyour fyzického zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="e07d6-140">You're ready toolearn how tooconfigure and run a gateway sample application toosend data from your physical device tooyour IoT hub in hello cloud.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e07d6-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e07d6-141">Next steps</span></span>
[<span data-ttu-id="e07d6-142">Nakonfigurujte a spusťte ukázkové aplikace simulovaného zařízení cloudu nahrávání</span><span class="sxs-lookup"><span data-stu-id="e07d6-142">Configure and run a simulated device cloud upload sample application</span></span>](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)