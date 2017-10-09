---
title: "Connect Raspberry PI (C) tooAzure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat pí hello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "připojit cloudové platformy Malinová pí cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 473658c5a8e1e0d4cfced0efafbad2640a1e0696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="fb48c-104">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="fb48c-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="fb48c-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="fb48c-105">What you will do</span></span>
* <span data-ttu-id="fb48c-106">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="fb48c-106">Create a resource group.</span></span>
* <span data-ttu-id="fb48c-107">Vytvoření služby Azure IoT hub ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fb48c-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="fb48c-108">Přidáte malin pí 3 toohello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="fb48c-108">Add Raspberry Pi 3 toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="fb48c-109">Při použití hello rozhraní příkazového řádku Azure tooadd pí tooyour IoT hub služba hello generuje klíč pro platformy tooauthenticate službou hello.</span><span class="sxs-lookup"><span data-stu-id="fb48c-109">When you use hello Azure CLI tooadd Pi tooyour IoT hub, hello service generates a key for Pi tooauthenticate with hello service.</span></span> <span data-ttu-id="fb48c-110">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="fb48c-110">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="fb48c-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="fb48c-111">What you will learn</span></span>
<span data-ttu-id="fb48c-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="fb48c-112">In this article, you will learn:</span></span>
* <span data-ttu-id="fb48c-113">Jak toouse hello toocreate rozhraní příkazového řádku Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb48c-113">How toouse hello Azure CLI toocreate an IoT hub.</span></span>
* <span data-ttu-id="fb48c-114">Jak toocreate identitu zařízení pí ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb48c-114">How toocreate a device identity for Pi in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fb48c-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="fb48c-115">What you need</span></span>
* <span data-ttu-id="fb48c-116">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="fb48c-116">An Azure account</span></span>
* <span data-ttu-id="fb48c-117">Mac nebo počítači se systémem Windows s hello nainstalované rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="fb48c-117">A Mac or a Windows computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="fb48c-118">Vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="fb48c-118">Create your IoT hub</span></span>
<span data-ttu-id="fb48c-119">Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky.</span><span class="sxs-lookup"><span data-stu-id="fb48c-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="fb48c-120">toocreate služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="fb48c-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="fb48c-121">Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fb48c-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="fb48c-122">Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="fb48c-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="fb48c-123">Nastavit výchozí předplatné hello chcete toouse spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb48c-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="fb48c-124">`subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="fb48c-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="fb48c-125">Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fb48c-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="fb48c-126">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fb48c-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="fb48c-127">Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="fb48c-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="fb48c-128">Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA hello, spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb48c-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="fb48c-129">`westus`je umístění hello vytvoříte vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="fb48c-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="fb48c-130">Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.</span><span class="sxs-lookup"><span data-stu-id="fb48c-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>
 
5. <span data-ttu-id="fb48c-131">Vytvoření služby IoT hub ve skupině prostředků iot-sample hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb48c-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="fb48c-132">Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="fb48c-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="fb48c-133">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="fb48c-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="fb48c-134">Hello název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fb48c-134">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="fb48c-135">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fb48c-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="fb48c-136">Pi zaregistrovat ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="fb48c-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="fb48c-137">Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="fb48c-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="fb48c-138">Registrovat platformy v centru a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb48c-138">Register Pi in your hub by running following command:</span></span>

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a><span data-ttu-id="fb48c-139">Souhrn</span><span class="sxs-lookup"><span data-stu-id="fb48c-139">Summary</span></span>
<span data-ttu-id="fb48c-140">Po vytvoření služby IoT hub a zaregistrována pí identitu zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb48c-140">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="fb48c-141">Jste připravené toolearn jak toosend zprávy z centra IoT tooyour pí.</span><span class="sxs-lookup"><span data-stu-id="fb48c-141">You're ready toolearn how toosend messages from Pi tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb48c-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb48c-142">Next steps</span></span>
<span data-ttu-id="fb48c-143">[Vytvoření aplikace Azure funkce a služby úložiště Azure účtu úložiště a tooprocess IoT hub zpráv](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="fb48c-143">[Create an Azure function app and an Azure Storage account tooprocess and store IoT hub messages](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).</span></span>

