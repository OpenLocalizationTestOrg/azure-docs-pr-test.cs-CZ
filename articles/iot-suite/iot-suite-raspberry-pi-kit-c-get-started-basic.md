---
title: "Připojení k Azure IoT Suite pomocí jazyka C skutečné snímači malin platformy | Microsoft Docs"
description: "Pomocí Startovní sady Microsoft Azure IoT Malinová pí 3 a sady Azure IoT Suite. Použití jazyka C pro vaše platformy malin připojení k řešení vzdáleného monitorování, odesílat telemetrická data ze senzorů do cloudu a reagovat na metody vyvolané z řídicího panelu řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 418108e8236518d2671abca0f06f1ae8159d6442
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a><span data-ttu-id="65b7b-104">Připojení k řešení vzdáleného monitorování vaší malin pí 3 a odesílat telemetrická data z reálného senzor pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="65b7b-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send telemetry from a real sensor using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="65b7b-105">V tomto kurzu se dozvíte, jak používat Microsoft Azure IoT Starter Kit pro malin pí 3 k vývoji čtečku teploty a vlhkosti, který může komunikovat s cloudem.</span><span class="sxs-lookup"><span data-stu-id="65b7b-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to develop a temperature and humidity reader that can communicate with the cloud.</span></span> <span data-ttu-id="65b7b-106">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="65b7b-106">The tutorial uses:</span></span>

- <span data-ttu-id="65b7b-107">Raspbian operačního systému, programovací jazyk C a Microsoft Azure IoT SDK pro jazyk C implementovat ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="65b7b-107">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
- <span data-ttu-id="65b7b-108">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="65b7b-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="65b7b-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="65b7b-109">Overview</span></span>

<span data-ttu-id="65b7b-110">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="65b7b-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="65b7b-111">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="65b7b-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="65b7b-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="65b7b-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="65b7b-113">Nastavte zařízením a senzory ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="65b7b-113">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="65b7b-114">Aktualizujte zařízení ukázkový kód pro připojení k řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="65b7b-114">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="65b7b-115">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="65b7b-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="65b7b-116">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="65b7b-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="65b7b-117">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="65b7b-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="65b7b-118">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="65b7b-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="65b7b-119">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="65b7b-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="65b7b-120">Stáhnout a nakonfigurovat vzorku</span><span class="sxs-lookup"><span data-stu-id="65b7b-120">Download and configure the sample</span></span>

<span data-ttu-id="65b7b-121">Teď můžete stáhnout a nakonfigurovat vzdálené monitorování klientskou aplikaci na vaše malin platformy.</span><span class="sxs-lookup"><span data-stu-id="65b7b-121">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="65b7b-122">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="65b7b-122">Clone the repositories</span></span>

<span data-ttu-id="65b7b-123">Pokud jste tak již neučinili, klonování vyžaduje úložiště spuštěním následujících příkazů v terminálu na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="65b7b-123">If you haven't already done so, clone the required repositories by running the following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="65b7b-124">Aktualizovat připojovací řetězec zařízení</span><span class="sxs-lookup"><span data-stu-id="65b7b-124">Update the device connection string</span></span>

<span data-ttu-id="65b7b-125">Otevření zdrojového souboru ukázka v **nano** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="65b7b-125">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="65b7b-126">Vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="65b7b-126">Locate the following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="65b7b-127">Zástupné hodnoty nahraďte zařízení a informace služby IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="65b7b-127">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="65b7b-128">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="65b7b-128">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="65b7b-129">Sestavit ukázku</span><span class="sxs-lookup"><span data-stu-id="65b7b-129">Build the sample</span></span>

<span data-ttu-id="65b7b-130">Nainstalujte požadované balíčky pro Microsoft Azure IoT zařízení SDK pro jazyk C spuštěním následujících příkazů v terminálu na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="65b7b-130">Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="65b7b-131">Teď můžete sestavit řešení aktualizovaný ukázkový na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="65b7b-131">You can now build the updated sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

<span data-ttu-id="65b7b-132">Teď můžete spustit ukázkový program na malin pí.</span><span class="sxs-lookup"><span data-stu-id="65b7b-132">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="65b7b-133">Zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="65b7b-133">Enter the command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="65b7b-134">Následující ukázkový výstup je příklad výstupu, které vidíte na příkazovém řádku na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="65b7b-134">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="65b7b-136">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="65b7b-136">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="65b7b-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="65b7b-137">Next steps</span></span>

<span data-ttu-id="65b7b-138">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="65b7b-138">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
