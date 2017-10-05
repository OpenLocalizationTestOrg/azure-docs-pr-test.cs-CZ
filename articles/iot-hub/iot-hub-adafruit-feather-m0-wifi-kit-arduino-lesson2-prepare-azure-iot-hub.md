---
title: "Připojení k Azure IoT - lekci 2 Arduino: registrace zařízení | Microsoft Docs"
description: "Vytvořte skupinu prostředků, vytvoření služby Azure IoT hub a zaregistrovat Adafruit prolnutí M0 Wi-Fi ve službě Azure IoT hub pomocí rozhraní příkazového řádku Azure."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "připojení ke cloudu, služby azure iot hub, internet věcí cloudu azure iot hub arduino vytvoření zařízení, arduino cloudu"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c5ad5e900671c7cedd3cdad2c2aa345315de223b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a><span data-ttu-id="ace01-104">Vytvoření služby IoT hub a zaregistrujte Adafruit prolnutí M0 Wi-Fi Arduino panel</span><span class="sxs-lookup"><span data-stu-id="ace01-104">Create your IoT hub and register your Adafruit Feather M0 WiFi Arduino board</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="ace01-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="ace01-105">What you will do</span></span>
* <span data-ttu-id="ace01-106">Vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="ace01-106">Create a resource group.</span></span>
* <span data-ttu-id="ace01-107">Vytvoření služby Azure IoT hub ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="ace01-107">Create your Azure IoT hub in the resource group.</span></span>
* <span data-ttu-id="ace01-108">Přidáte panel Arduino ke službě Azure IoT hub pomocí rozhraní příkazového řádku Azure (Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="ace01-108">Add your Arduino board to the Azure IoT hub by using the Azure command-line interface (Azure CLI).</span></span>

<span data-ttu-id="ace01-109">Pokud používáte Azure CLI přidat panel Arduino do služby IoT hub, služba generuje klíč pro panel Arduino k ověření u služby.</span><span class="sxs-lookup"><span data-stu-id="ace01-109">When you use the Azure CLI to add your Arduino board to your IoT hub, the service generates a key for your Arduino board to authenticate with the service.</span></span> <span data-ttu-id="ace01-110">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky][troubleshoot].</span><span class="sxs-lookup"><span data-stu-id="ace01-110">If you have any problems, look for solutions on the [troubleshooting page][troubleshoot].</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="ace01-111">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="ace01-111">What you will learn</span></span>
<span data-ttu-id="ace01-112">V tomto článku se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="ace01-112">In this article, you will learn:</span></span>
* <span data-ttu-id="ace01-113">Jak používat rozhraní příkazového řádku Azure k vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ace01-113">How to use the Azure CLI to create an IoT hub.</span></span>
* <span data-ttu-id="ace01-114">Postup vytvoření identity zařízení pro panel Arduino ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ace01-114">How to create a device identity for your Arduino board in your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="ace01-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="ace01-115">What you need</span></span>
* <span data-ttu-id="ace01-116">Účet Azure</span><span class="sxs-lookup"><span data-stu-id="ace01-116">An Azure account</span></span>
* <span data-ttu-id="ace01-117">Počítač s nainstalované rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="ace01-117">A computer with the Azure CLI installed</span></span>

## <a name="create-your-iot-hub"></a><span data-ttu-id="ace01-118">Vytvoření služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="ace01-118">Create your IoT hub</span></span>
<span data-ttu-id="ace01-119">Azure IoT Hub umožňuje připojit, sledovat a spravovat miliony IoT prostředky.</span><span class="sxs-lookup"><span data-stu-id="ace01-119">Azure IoT Hub helps you connect, monitor, and manage millions of IoT assets.</span></span> <span data-ttu-id="ace01-120">Pokud chcete vytvořit službu IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ace01-120">To create your IoT hub, follow these steps:</span></span>

1. <span data-ttu-id="ace01-121">Přihlaste se k účtu Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ace01-121">Sign in to your Azure account by running the following command:</span></span>

   ```bash
   az login
   ```

   <span data-ttu-id="ace01-122">Po úspěšném přihlášení jsou uvedeny všech dostupných předplatných.</span><span class="sxs-lookup"><span data-stu-id="ace01-122">All your available subscriptions are listed after a successful sign-in.</span></span>

2. <span data-ttu-id="ace01-123">Nastavte výchozí předplatné, které chcete použít tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ace01-123">Set the default subscription that you want to use by running the following command:</span></span>

   ```bash
   az account set --subscription {subscription id or name}
   ```

   <span data-ttu-id="ace01-124">`subscription ID or name`můžete najít ve výstupu `az login` nebo `az account list` příkaz.</span><span class="sxs-lookup"><span data-stu-id="ace01-124">`subscription ID or name` can be found in the output of the `az login` or the `az account list` command.</span></span>

3. <span data-ttu-id="ace01-125">Zaregistrujte zprostředkovatele spuštěním následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="ace01-125">Register the provider by running the following command.</span></span> <span data-ttu-id="ace01-126">Zprostředkovatelé prostředků jsou služby, které poskytují prostředky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ace01-126">Resource providers are services that provide resources for your application.</span></span> <span data-ttu-id="ace01-127">Před nasazením Azure prostředek, který nabízí poskytovatele, je nutné zaregistrovat zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ace01-127">You must register the provider before you can deploy the Azure resource that the provider offers.</span></span>

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. <span data-ttu-id="ace01-128">Vytvořte skupinu prostředků s názvem iot-sample v oblasti západní USA spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ace01-128">Create a resource group named iot-sample in the West US region by running the following command:</span></span>

   ```bash
   az group create --name iot-sample --location westus
   ```

   <span data-ttu-id="ace01-129">`westus`je umístění, vytvořit skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="ace01-129">`westus` is the location you create your resource group.</span></span> <span data-ttu-id="ace01-130">Pokud chcete použít jiné umístění, můžete spustit `az account list-locations -o table` zobrazíte všechna místa Azure podporuje.</span><span class="sxs-lookup"><span data-stu-id="ace01-130">If you want to use another location, you can run `az account list-locations -o table` to see all the locations Azure supports.</span></span>

5. <span data-ttu-id="ace01-131">Vytvoření služby IoT hub ve skupině prostředků iot-sample spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="ace01-131">Create an IoT hub in the iot-sample resource group by running the following command:</span></span>

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

<span data-ttu-id="ace01-132">Ve výchozím nastavení vytvoří nástroj služby IoT Hub v cenové úrovně Free.</span><span class="sxs-lookup"><span data-stu-id="ace01-132">By default, the tool creates an IoT Hub in the Free pricing tier.</span></span> <span data-ttu-id="ace01-133">Další informace najdete v tématu [ceny služby Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="ace01-133">For more infomation, see [Azure IoT Hub pricing](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

> [!NOTE]
> <span data-ttu-id="ace01-134">Název služby IoT hub musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ace01-134">The name of your IoT hub must be globally unique.</span></span>
> <span data-ttu-id="ace01-135">V rámci vašeho předplatného Azure můžete vytvářet jenom jeden F1 edice služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ace01-135">You can create only one F1 edition of Azure IoT Hub under your Azure subscription.</span></span>

## <a name="register-your-arduino-board-in-your-iot-hub"></a><span data-ttu-id="ace01-136">Panel Arduino zaregistrovat ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="ace01-136">Register your Arduino board in your IoT hub</span></span>
<span data-ttu-id="ace01-137">Každé zařízení, která odesílá zprávy do služby IoT hub a přijímá zprávy od služby IoT hub musí být zaregistrovaný u jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="ace01-137">Each device that sends messages to your IoT hub and receives messages from your IoT hub must be registered with a unique ID.</span></span>

<span data-ttu-id="ace01-138">Panel Arduino registrovat ve službě IoT hub a spuštěné následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ace01-138">Register your Arduino board in your IoT hub by running following command:</span></span>

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a><span data-ttu-id="ace01-139">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ace01-139">Summary</span></span>
<span data-ttu-id="ace01-140">Po vytvoření služby IoT hub a zaregistrována panel Arduino identitu zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ace01-140">You've created an IoT hub and registered your Arduino board with a device identity in your IoT hub.</span></span> <span data-ttu-id="ace01-141">Jste připraveni se dozvíte, jak k odesílání zpráv z vaší karty Arduino do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ace01-141">You're ready to learn how to send messages from your Arduino board to your IoT hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ace01-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ace01-142">Next steps</span></span>
<span data-ttu-id="ace01-143">[Vytvoření aplikace Azure funkce a účet úložiště Azure pro zpracování a ukládání zpráv centra IoT][process-and-store-iot-hub-messages].</span><span class="sxs-lookup"><span data-stu-id="ace01-143">[Create an Azure function app and an Azure Storage account to process and store IoT hub messages][process-and-store-iot-hub-messages].</span></span>


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md