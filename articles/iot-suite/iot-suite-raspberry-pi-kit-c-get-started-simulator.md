---
title: "Připojení k Azure IoT Suite pomocí jazyka C s simulovanou telemetrii malin platformy | Microsoft Docs"
description: "Pomocí Startovní sady Microsoft Azure IoT Malinová pí 3 a sady Azure IoT Suite. Použití jazyka C pro vaše platformy malin připojení k řešení vzdáleného monitorování, odeslání simulovanou telemetrii do cloudu a reagovat na metody vyvolané z řídicího panelu řešení."
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
ms.openlocfilehash: 43b82e5fb6a309283979f23d8c87af600595bc55
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a><span data-ttu-id="2b32d-104">Připojení k řešení vzdáleného monitorování vaší malin pí 3 a odeslat simulovanou telemetrii pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="2b32d-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send simulated telemetry using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="2b32d-105">V tomto kurzu se dozvíte, jak používat pí 3 malin k simulaci teploty a vlhkosti data k odeslání do cloudu.</span><span class="sxs-lookup"><span data-stu-id="2b32d-105">This tutorial shows you how to use the Raspberry Pi 3 to simulate temperature and humidity data to send to the cloud.</span></span> <span data-ttu-id="2b32d-106">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="2b32d-106">The tutorial uses:</span></span>

- <span data-ttu-id="2b32d-107">Raspbian operačního systému, programovací jazyk C a Microsoft Azure IoT SDK pro jazyk C implementovat ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="2b32d-107">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
- <span data-ttu-id="2b32d-108">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="2b32d-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="2b32d-109">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="2b32d-109">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="2b32d-110">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="2b32d-110">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="2b32d-111">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="2b32d-111">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="2b32d-112">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="2b32d-112">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="2b32d-113">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="2b32d-113">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="2b32d-114">Stáhnout a nakonfigurovat vzorku</span><span class="sxs-lookup"><span data-stu-id="2b32d-114">Download and configure the sample</span></span>

<span data-ttu-id="2b32d-115">Teď můžete stáhnout a nakonfigurovat vzdálené monitorování klientskou aplikaci na vaše malin platformy.</span><span class="sxs-lookup"><span data-stu-id="2b32d-115">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="2b32d-116">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="2b32d-116">Clone the repositories</span></span>

<span data-ttu-id="2b32d-117">Pokud jste tak již neučinili, klonování vyžaduje úložiště spuštěním následujících příkazů v terminálu na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="2b32d-117">If you haven't already done so, clone the required repositories by running the following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="2b32d-118">Aktualizovat připojovací řetězec zařízení</span><span class="sxs-lookup"><span data-stu-id="2b32d-118">Update the device connection string</span></span>

<span data-ttu-id="2b32d-119">Otevření zdrojového souboru ukázka v **nano** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2b32d-119">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="2b32d-120">Vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="2b32d-120">Locate the following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="2b32d-121">Zástupné hodnoty nahraďte zařízení a informace služby IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2b32d-121">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="2b32d-122">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="2b32d-122">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="2b32d-123">Sestavit ukázku</span><span class="sxs-lookup"><span data-stu-id="2b32d-123">Build the sample</span></span>

<span data-ttu-id="2b32d-124">Nainstalujte požadované balíčky pro Microsoft Azure IoT zařízení SDK pro jazyk C spuštěním následujících příkazů v terminálu na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2b32d-124">Install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="2b32d-125">Teď můžete sestavit řešení aktualizovaný ukázkový na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2b32d-125">You can now build the updated sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

<span data-ttu-id="2b32d-126">Teď můžete spustit ukázkový program na malin pí.</span><span class="sxs-lookup"><span data-stu-id="2b32d-126">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="2b32d-127">Zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="2b32d-127">Enter the command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="2b32d-128">Následující ukázkový výstup je příklad výstupu, které vidíte na příkazovém řádku na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2b32d-128">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="2b32d-130">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="2b32d-130">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="2b32d-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2b32d-131">Next steps</span></span>

<span data-ttu-id="2b32d-132">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="2b32d-132">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
