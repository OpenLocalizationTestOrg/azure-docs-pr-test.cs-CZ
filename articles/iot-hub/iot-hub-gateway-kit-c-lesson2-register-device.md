---
title: "Zařízení SensorTag & brány Azure IoT - Lekce 2: registrace zařízení | Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Centrum Azure iot hub pro internet věcí cloudu azure iot hub vytvořit zařízení ti sensortag, ti zakázat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 685a479583f5f11f57bef22dc5881285bb1f70d0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a><span data-ttu-id="45555-103">Vytvoření služby Azure IoT hub a registrace zařízení</span><span class="sxs-lookup"><span data-stu-id="45555-103">Create your Azure IoT hub and register your device</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="45555-104">Co provedete</span><span class="sxs-lookup"><span data-stu-id="45555-104">What you will do</span></span>

- <span data-ttu-id="45555-105">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="45555-105">Create a resource group</span></span>
- <span data-ttu-id="45555-106">Vytvoření první centra IoT</span><span class="sxs-lookup"><span data-stu-id="45555-106">Create your first IoT hub</span></span>
- <span data-ttu-id="45555-107">Registrace zařízení ve službě IoT hub pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="45555-107">Register your device in your IoT hub by using the Azure CLI.</span></span> 

<span data-ttu-id="45555-108">Při registraci zařízení ve službě IoT hub, službu Azure IoT Hub generuje klíč pro vaše zařízení sloužící k ověření u služby.</span><span class="sxs-lookup"><span data-stu-id="45555-108">When you register your device in your IoT hub, the Azure IoT Hub service generates a key for your device to use to authenticate with the service.</span></span> 

<span data-ttu-id="45555-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="45555-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="45555-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="45555-110">What you will learn</span></span>

<span data-ttu-id="45555-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="45555-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="45555-112">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="45555-112">How to use the Azure CLI to create an IoT hub.</span></span>
- <span data-ttu-id="45555-113">Postup registrace zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="45555-113">How to register a device in an IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="45555-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="45555-114">What you need</span></span>

- <span data-ttu-id="45555-115">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="45555-115">An active Azure subscription.</span></span> <span data-ttu-id="45555-116">Pokud nemáte účet Azure, můžete vytvořit [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="45555-116">If you don't have an Azure account, you can create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
- <span data-ttu-id="45555-117">Měli byste mít nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="45555-117">You should have the Azure CLI installed.</span></span>

## <a name="create-an-iot-hub"></a><span data-ttu-id="45555-118">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="45555-118">Create an IoT hub</span></span>

<span data-ttu-id="45555-119">K vytvoření služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="45555-119">To create an IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="45555-120">Přihlaste se k účtu Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45555-120">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="45555-121">Po úspěšném přihlášení se objeví všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="45555-121">All your available subscriptions will be listed after a successful sign-in.</span></span>

2. <span data-ttu-id="45555-122">Nastavte výchozí předplatné Azure, který chcete použít tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45555-122">Set the default Azure subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="45555-123">`subscription ID or name`můžete najít ve výstupu `az login` nebo `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="45555-123">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="45555-124">Zaregistrujte zprostředkovatele spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="45555-124">Register the provider by running the following command.</span></span> <span data-ttu-id="45555-125">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="45555-125">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="45555-126">Před nasazením Azure prostředek, který nabízí poskytovatele, je nutné zaregistrovat zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="45555-126">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. <span data-ttu-id="45555-127">Vytvořte skupinu prostředků s názvem `iot-gateway` v oblasti západní USA spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="45555-127">Create a resource group named `iot-gateway` in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   <span data-ttu-id="45555-128">`westus`je umístění, vytvořit skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="45555-128">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="45555-129">Pokud chcete použít jiné umístění, můžete spustit `az account list-locations -o table` zobrazíte všechna místa Azure podporuje.</span><span class="sxs-lookup"><span data-stu-id="45555-129">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="45555-130">Vytvoření služby IoT hub v `iot-gateway` skupinu prostředků spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="45555-130">Create an IoT hub in the `iot-gateway` resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

<span data-ttu-id="45555-131">Ve výchozím nastavení vytvoří nástroj služby IoT Hub v cenové úrovně Free.</span><span class="sxs-lookup"><span data-stu-id="45555-131">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="45555-132">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="45555-132">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="45555-133">Název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="45555-133">The name of your IoT hub must be globally unique.</span></span> <span data-ttu-id="45555-134">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure Iot Hub.</span><span class="sxs-lookup"><span data-stu-id="45555-134">You can create only one F1 edition of Azure Iot Hub under your Azure subscription.</span></span>

## <a name="register-your-device-in-your-iot-hub"></a><span data-ttu-id="45555-135">Registrace zařízení ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="45555-135">Register your device in your IoT hub</span></span>

<span data-ttu-id="45555-136">Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="45555-136">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>
<span data-ttu-id="45555-137">Registrovat zařízení ve službě IoT hub, a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="45555-137">Register your device in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a><span data-ttu-id="45555-138">Souhrn</span><span class="sxs-lookup"><span data-stu-id="45555-138">Summary</span></span>

<span data-ttu-id="45555-139">Po vytvoření služby IoT hub a zaregistrována identitu zařízení logického zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="45555-139">You've created an IoT hub and registered your logical device with a device identity in your IoT hub.</span></span> <span data-ttu-id="45555-140">Jste připraveni se dozvíte, jak pro konfiguraci a spuštění ukázkové aplikace brána k odesílání dat z fyzického zařízení do služby IoT hub v cloudu.</span><span class="sxs-lookup"><span data-stu-id="45555-140">You're ready to learn how to configure and run a gateway sample application to send data from your physical device to your IoT hub in the cloud.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45555-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="45555-141">Next steps</span></span>
[<span data-ttu-id="45555-142">Nakonfigurujte a spusťte ukázkovou aplikaci zakázat</span><span class="sxs-lookup"><span data-stu-id="45555-142">Configure and run a BLE sample app</span></span>](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)