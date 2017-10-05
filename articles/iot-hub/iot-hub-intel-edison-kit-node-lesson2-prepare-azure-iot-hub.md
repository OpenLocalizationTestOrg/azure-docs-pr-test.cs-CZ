---
title: "Připojení k Azure IoT - lekci 2 Intel Edison (uzel): registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat Edison ve službě Azure IoT hub pomocí rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 81584c1b7314c4331ac059b8e3ed782970588ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a><span data-ttu-id="0a74e-103">Vytvoření služby IoT hub a zaregistrujte Intel Edison</span><span class="sxs-lookup"><span data-stu-id="0a74e-103">Create your IoT hub and register Intel Edison</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="0a74e-104">Co provedete</span><span class="sxs-lookup"><span data-stu-id="0a74e-104">What you will do</span></span>
* <span data-ttu-id="0a74e-105">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="0a74e-105">Create a resource group.</span></span>
* <span data-ttu-id="0a74e-106">Vytvoření služby Azure IoT hub ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="0a74e-106">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="0a74e-107">Přidáte Intel Edison ke službě Azure IoT hub pomocí rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="0a74e-107">Add Intel Edison to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="0a74e-108">Při použití Azure CLI pro přidání Edison do služby IoT hub, služba generuje klíč pro Edison k ověření u služby.</span><span class="sxs-lookup"><span data-stu-id="0a74e-108">When you use the Azure CLI to add Edison to your IoT hub, the service generates a key for Edison to authenticate with the service.</span></span> <span data-ttu-id="0a74e-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshooting].</span><span class="sxs-lookup"><span data-stu-id="0a74e-109">If you have any problems, look for solutions on the [troubleshooting page][troubleshooting].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="0a74e-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="0a74e-110">What you will learn</span></span>
<span data-ttu-id="0a74e-111">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="0a74e-111">In this article, you will learn:</span></span>
* <span data-ttu-id="0a74e-112">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0a74e-112">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="0a74e-113">Postup vytvoření identity zařízení pro Edison ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0a74e-113">How to create a device identity for Edison in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="0a74e-114">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="0a74e-114">What you need</span></span>
* <span data-ttu-id="0a74e-115">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0a74e-115">An Azure account.</span></span> <span data-ttu-id="0a74e-116">Pokud nemáte účet Azure, vytvořte [Bezplatný zkušební účet Azure](http://azure.microsoft.com/pricing/free-trial/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="0a74e-116">If you don't have an Azure account, create a [free Azure trial account](http://azure.microsoft.com/pricing/free-trial/) in just a few minutes.</span></span>
* <span data-ttu-id="0a74e-117">Měli byste mít nainstalované rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="0a74e-117">You should have the Azure CLI installed.</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="0a74e-118">Vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="0a74e-118">Create your IoT hub</span></span>
<span data-ttu-id="0a74e-119">Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky.</span><span class="sxs-lookup"><span data-stu-id="0a74e-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="0a74e-120">Pokud chcete vytvořit službu IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="0a74e-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="0a74e-121">Přihlaste se k účtu Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0a74e-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="0a74e-122">Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="0a74e-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="0a74e-123">Nastavte výchozí předplatné, které chcete použít tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0a74e-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="0a74e-124">`subscription ID or name`můžete najít ve výstupu `az login` nebo `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="0a74e-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="0a74e-125">Zaregistrujte zprostředkovatele spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="0a74e-125">Register the provider by running the following command.</span></span> <span data-ttu-id="0a74e-126">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0a74e-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="0a74e-127">Před nasazením Azure prostředek, který nabízí poskytovatele, je nutné zaregistrovat zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="0a74e-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="0a74e-128">Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="0a74e-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="0a74e-129">`westus`je umístění, vytvořit skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="0a74e-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="0a74e-130">Pokud chcete použít jiné umístění, můžete spustit `az account list-locations -o table` zobrazíte všechna místa Azure podporuje.</span><span class="sxs-lookup"><span data-stu-id="0a74e-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="0a74e-131">Vytvoření služby IoT hub ve skupině prostředků iot-sample spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="0a74e-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="0a74e-132">Ve výchozím nastavení vytvoří nástroj služby IoT Hub v cenové úrovně Free.</span><span class="sxs-lookup"><span data-stu-id="0a74e-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="0a74e-133">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="0a74e-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE] 
> <span data-ttu-id="0a74e-134">Název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="0a74e-134">The name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="0a74e-135">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0a74e-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>


## <a name="register-edison-in-your-iot-hub"></a><span data-ttu-id="0a74e-136">Edison zaregistrovat ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="0a74e-136">Register Edison in your IoT hub</span></span>
<span data-ttu-id="0a74e-137">Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="0a74e-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="0a74e-138">Registrovat Edison ve službě IoT hub a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="0a74e-138">Register Edison in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="0a74e-139">Souhrn</span><span class="sxs-lookup"><span data-stu-id="0a74e-139">Summary</span></span>
<span data-ttu-id="0a74e-140">Po vytvoření služby IoT hub a zaregistrována Edison identitu zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0a74e-140">You've created an IoT hub and registered Edison with a device identity in your IoT hub.</span></span> <span data-ttu-id="0a74e-141">Jste připraveni se dozvíte, jak k odesílání zpráv z Edison do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0a74e-141">You're ready to learn how to send messages from Edison to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a74e-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0a74e-142">Next steps</span></span>
<span data-ttu-id="0a74e-143">[Vytvoření aplikace Azure funkce a účet úložiště Azure pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="0a74e-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md