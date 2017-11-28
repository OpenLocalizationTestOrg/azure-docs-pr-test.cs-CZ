---
title: "Připojení k Azure IoT Suite pomocí Node.js skutečné snímači malin platformy | Microsoft Docs"
description: "Pomocí Startovní sady Microsoft Azure IoT Malinová pí 3 a sady Azure IoT Suite. Použití Node.js pro vaše platformy malin připojení k řešení vzdáleného monitorování, odesílat telemetrická data ze senzorů do cloudu a reagovat na metody vyvolané z řídicího panelu řešení."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 91546157cc8eabf68706391ce706038d8dc5f82d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a><span data-ttu-id="f586e-104">Připojení k řešení vzdáleného monitorování vaší malin pí 3 a odesílat telemetrická data z reálného senzor pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="f586e-104">Connect your Raspberry Pi 3 to the remote monitoring solution and send telemetry from a real sensor using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="f586e-105">V tomto kurzu se dozvíte, jak používat Microsoft Azure IoT Starter Kit pro malin pí 3 k vývoji čtečku teploty a vlhkosti, který může komunikovat s cloudem.</span><span class="sxs-lookup"><span data-stu-id="f586e-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to develop a temperature and humidity reader that can communicate with the cloud.</span></span> <span data-ttu-id="f586e-106">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="f586e-106">The tutorial uses:</span></span>

- <span data-ttu-id="f586e-107">Raspbian operačního systému, programovací jazyk Node.js a Microsoft Azure IoT SDK pro Node.js implementovat ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="f586e-107">Raspbian OS, the Node.js programming language, and the Microsoft Azure IoT SDK for Node.js to implement a sample device.</span></span>
- <span data-ttu-id="f586e-108">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="f586e-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="f586e-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="f586e-109">Overview</span></span>

<span data-ttu-id="f586e-110">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f586e-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="f586e-111">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="f586e-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="f586e-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="f586e-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="f586e-113">Nastavte zařízením a senzory ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="f586e-113">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="f586e-114">Aktualizujte zařízení ukázkový kód pro připojení k řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="f586e-114">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="f586e-115">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="f586e-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="f586e-116">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="f586e-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="f586e-117">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="f586e-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="f586e-118">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="f586e-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="f586e-119">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="f586e-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="f586e-120">Stáhnout a nakonfigurovat vzorku</span><span class="sxs-lookup"><span data-stu-id="f586e-120">Download and configure the sample</span></span>

<span data-ttu-id="f586e-121">Teď můžete stáhnout a nakonfigurovat vzdálené monitorování klientskou aplikaci na vaše malin platformy.</span><span class="sxs-lookup"><span data-stu-id="f586e-121">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="f586e-122">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="f586e-122">Install Node.js</span></span>

<span data-ttu-id="f586e-123">Nainstalujte Node.js na vaše Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="f586e-123">Install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="f586e-124">Sada IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f586e-124">The IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="f586e-125">Následující kroky vám ukážou, jak nainstalovat Node.js v6.10.2 na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f586e-125">The following steps show you how to install Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="f586e-126">Použijte následující příkaz k aktualizaci vašeho malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f586e-126">Use the following command to update your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="f586e-127">Stažení Node.js binární soubory pro vaše platformy malin použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f586e-127">Use the following command to download the Node.js binaries to your Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="f586e-128">Použijte následující příkaz pro instalaci binárních souborů:</span><span class="sxs-lookup"><span data-stu-id="f586e-128">Use the following command to install the binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="f586e-129">Pomocí následujícího příkazu ověřte, že jste úspěšně nainstalovali Node.js v6.10.2:</span><span class="sxs-lookup"><span data-stu-id="f586e-129">Use the following command to verify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a><span data-ttu-id="f586e-130">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="f586e-130">Clone the repositories</span></span>

<span data-ttu-id="f586e-131">Pokud jste tak již neučinili, klonování vyžaduje úložiště spuštěním následujících příkazů na vašeho platformy:</span><span class="sxs-lookup"><span data-stu-id="f586e-131">If you haven't already done so, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="f586e-132">Aktualizovat připojovací řetězec zařízení</span><span class="sxs-lookup"><span data-stu-id="f586e-132">Update the device connection string</span></span>

<span data-ttu-id="f586e-133">Otevření zdrojového souboru ukázka v **nano** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f586e-133">Open the sample source file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="f586e-134">Vyhledejte řádek:</span><span class="sxs-lookup"><span data-stu-id="f586e-134">Find the line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="f586e-135">Zástupné hodnoty nahraďte zařízení a informace služby IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f586e-135">Replace the placeholder values with the device and IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="f586e-136">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="f586e-136">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="f586e-137">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="f586e-137">Run the sample</span></span>

<span data-ttu-id="f586e-138">Spusťte následující příkazy pro instalaci požadovaných balíčků pro ukázku:</span><span class="sxs-lookup"><span data-stu-id="f586e-138">Run the following commands to install the prerequisite packages for the sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

<span data-ttu-id="f586e-139">Teď můžete spustit ukázkový program na malin pí.</span><span class="sxs-lookup"><span data-stu-id="f586e-139">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="f586e-140">Zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="f586e-140">Enter the command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="f586e-141">Následující ukázkový výstup je příklad výstupu, které vidíte na příkazovém řádku na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f586e-141">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="f586e-143">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="f586e-143">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="f586e-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f586e-144">Next steps</span></span>

<span data-ttu-id="f586e-145">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="f586e-145">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
