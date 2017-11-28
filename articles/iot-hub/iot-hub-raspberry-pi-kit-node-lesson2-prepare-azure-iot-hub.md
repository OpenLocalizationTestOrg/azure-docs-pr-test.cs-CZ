---
featureFlags: usabilla
title: "Připojit malin platformy (uzel) tooAzure IoT - lekci 2: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a registrace platformy v hello registru identit služby IoT Hub pomocí rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "připojit cloudové platformy Malinová pí cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97533298d52d1187c49a4c35ddda922d6e45c87d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a><span data-ttu-id="5be38-104">Vytvoření služby IoT hub a zaregistrujte malin pí 3</span><span class="sxs-lookup"><span data-stu-id="5be38-104">Create your IoT hub and register Raspberry Pi 3</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="5be38-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="5be38-105">What you will do</span></span>
* <span data-ttu-id="5be38-106">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5be38-106">Create a resource group.</span></span>
* <span data-ttu-id="5be38-107">Vytvoření služby Azure IoT hub ve skupině prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="5be38-107">Create your Azure IoT hub in hello resource group.</span></span>
* <span data-ttu-id="5be38-108">Přidáte malin pí 3 toohello Azure IoT hub pomocí hello rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="5be38-108">Add Raspberry Pi 3 toohello Azure IoT hub by using hello Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="5be38-109">Pokud používáte Azure CLI tooadd pí tooyour IoT hub, služba hello generuje klíč pro platformy tooauthenticate službou hello.</span><span class="sxs-lookup"><span data-stu-id="5be38-109">When you use Azure CLI tooadd Pi tooyour IoT hub, hello service generates a key for Pi tooauthenticate with hello service.</span></span> <span data-ttu-id="5be38-110">Pokud máte potíže, hledají řešení na hello [řešení potíží s stránky](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="5be38-110">If you have any problems, seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="5be38-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="5be38-111">What you will learn</span></span>
<span data-ttu-id="5be38-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="5be38-112">In this article, you will learn:</span></span>
* <span data-ttu-id="5be38-113">Jak toocreate toouse rozhraní příkazového řádku Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="5be38-113">How toouse Azure CLI toocreate an IoT hub</span></span>
* <span data-ttu-id="5be38-114">Jak toocreate identitu zařízení pí ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="5be38-114">How toocreate a device identity for Pi in your IoT hub</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5be38-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5be38-115">What you need</span></span>
* <span data-ttu-id="5be38-116">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="5be38-116">An Azure account</span></span>
* <span data-ttu-id="5be38-117">Mac nebo počítači se systémem Windows s hello nainstalované rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5be38-117">A Mac or a Windows computer with hello Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="5be38-118">Vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="5be38-118">Create your IoT hub</span></span>
<span data-ttu-id="5be38-119">Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky.</span><span class="sxs-lookup"><span data-stu-id="5be38-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="5be38-120">toocreate služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5be38-120">toocreate your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="5be38-121">Přihlaste se tooyour účet Azure tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5be38-121">Sign in tooyour Azure account by running hello following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="5be38-122">Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="5be38-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="5be38-123">Nastavit výchozí předplatné hello chcete toouse spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5be38-123">Set hello default subscription that you want toouse by running hello following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="5be38-124">`subscription ID or name`lze nalézt v hello výstup hello `az login` nebo hello `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5be38-124">`subscription ID or name` can be found in hello output of hello `az login` or hello `az account list` command.</span></span>

3. <span data-ttu-id="5be38-125">Registrace zprostředkovatele hello tak, že spustíte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="5be38-125">Register hello provider by running hello following command.</span></span> <span data-ttu-id="5be38-126">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5be38-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="5be38-127">Před nasazením hello prostředků Azure, který hello zprostředkovatele nabídky, je nutné zaregistrovat hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="5be38-127">You must register hello provider before you can deploy hello Azure resource that hello provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="5be38-128">Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA hello, spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5be38-128">Create a resource group named iot-sample in hello West US region by running hello following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="5be38-129">`westus`je umístění hello vytvoříte vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5be38-129">`westus` is hello location you create your resource group.</span></span> <span data-ttu-id="5be38-130">Pokud chcete toouse jiného umístění, můžete spustit `az account list-locations -o table` toosee všechny hello umístění, které podporuje Azure.</span><span class="sxs-lookup"><span data-stu-id="5be38-130">If you want toouse another location, you can run `az account list-locations -o table` toosee all hello locations Azure supports.</span></span>
 
5. <span data-ttu-id="5be38-131">Vytvoření služby IoT hub ve skupině prostředků iot-sample hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5be38-131">Create an IoT hub in hello iot-sample resource group by running hello following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   <span data-ttu-id="5be38-132">Ve výchozím nastavení vytvoří nástroj hello služby IoT Hub v hello cenová úroveň Free.</span><span class="sxs-lookup"><span data-stu-id="5be38-132">By default, hello tool creates an IoT Hub in hello Free pricing tier.</span></span> <span data-ttu-id="5be38-133">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="5be38-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="5be38-134">Hello název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5be38-134">hello name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="5be38-135">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="5be38-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-pi-in-your-iot-hub"></a><span data-ttu-id="5be38-136">Pi zaregistrovat ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="5be38-136">Register Pi in your IoT hub</span></span>
<span data-ttu-id="5be38-137">Každé zařízení, která odesílá zprávy tooyour IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="5be38-137">Each device that sends messages tooyour IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span> <span data-ttu-id="5be38-138">Budete používat rozhraní příkazového řádku Azure tooregister vaše platformy a vytvořit certifikát podepsaný svým držitelem pro ověřování zařízení X.509.</span><span class="sxs-lookup"><span data-stu-id="5be38-138">You will use Azure CLI tooregister your Pi and create a self-signed X.509 certificate for device authentication.</span></span>

<span data-ttu-id="5be38-139">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5be38-139">Run hello following command:</span></span>

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a><span data-ttu-id="5be38-140">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5be38-140">Summary</span></span>
<span data-ttu-id="5be38-141">Po vytvoření služby IoT hub a zaregistrována pí identitu zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5be38-141">You've created an IoT hub and registered Pi with a device identity in your IoT hub.</span></span> <span data-ttu-id="5be38-142">Jste připravené toolearn jak toosend zprávy z centra IoT tooyour pí.</span><span class="sxs-lookup"><span data-stu-id="5be38-142">You're ready toolearn how toosend messages from Pi tooyour IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5be38-143">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5be38-143">Next steps</span></span>
[<span data-ttu-id="5be38-144">Vytvoření aplikace Azure funkce a tooprocess účet úložiště Azure a ukládat zprávy centra IoT</span><span class="sxs-lookup"><span data-stu-id="5be38-144">Create an Azure function app and an Azure storage account tooprocess and store IoT hub messages</span></span>](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

