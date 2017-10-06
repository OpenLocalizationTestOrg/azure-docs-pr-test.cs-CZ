---
title: "aaaConnect tooAzure malin platformy IoT Suite simulovanou telemetrii pomocí Node.js | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použít Node.js tooconnect vaše řešení vzdáleného sledování toohello malin Pi, odesílání simulovanou telemetrii toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: f65eeaa6e83fd89cdedae8fa8386a3e9ef8417d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a><span data-ttu-id="58b85-104">Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odeslat simulovanou telemetrii pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="58b85-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send simulated telemetry using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="58b85-105">Tento kurz ukazuje, jak cloudové toouse hello malin pí 3 toosimulate teploty a vlhkosti data toosend toohello.</span><span class="sxs-lookup"><span data-stu-id="58b85-105">This tutorial shows you how toouse hello Raspberry Pi 3 toosimulate temperature and humidity data toosend toohello cloud.</span></span> <span data-ttu-id="58b85-106">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="58b85-106">hello tutorial uses:</span></span>

- <span data-ttu-id="58b85-107">Raspbian OS hello Node.js programovací jazyk a hello Microsoft Azure IoT SDK pro Node.js tooimplement ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="58b85-107">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="58b85-108">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="58b85-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="58b85-109">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="58b85-109">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="58b85-110">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="58b85-110">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="58b85-111">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="58b85-111">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="58b85-112">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="58b85-112">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="58b85-113">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="58b85-113">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="58b85-114">Stáhnout a nakonfigurovat ukázka hello</span><span class="sxs-lookup"><span data-stu-id="58b85-114">Download and configure hello sample</span></span>

<span data-ttu-id="58b85-115">Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="58b85-115">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="58b85-116">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="58b85-116">Install Node.js</span></span>

<span data-ttu-id="58b85-117">Pokud jste tak ještě neučinili, nainstalujte na vaše platformy malin Node.js.</span><span class="sxs-lookup"><span data-stu-id="58b85-117">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="58b85-118">Hello IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější.</span><span class="sxs-lookup"><span data-stu-id="58b85-118">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="58b85-119">Hello následující kroky ukazují, jak tooinstall Node.js v6.10.2 na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="58b85-119">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="58b85-120">Použijte následující příkaz tooupdate hello vaší malin platformy:</span><span class="sxs-lookup"><span data-stu-id="58b85-120">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="58b85-121">Použijte následující příkaz toodownload hello Node.js binární soubory tooyour malin pí hello:</span><span class="sxs-lookup"><span data-stu-id="58b85-121">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="58b85-122">Použijte následující příkaz tooinstall hello binární soubory hello:</span><span class="sxs-lookup"><span data-stu-id="58b85-122">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="58b85-123">Použijte následující příkaz tooverify jste úspěšně nainstalovali Node.js v6.10.2 hello:</span><span class="sxs-lookup"><span data-stu-id="58b85-123">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="58b85-124">Klonování úložiště hello</span><span class="sxs-lookup"><span data-stu-id="58b85-124">Clone hello repositories</span></span>

<span data-ttu-id="58b85-125">Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy v terminálu na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="58b85-125">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="58b85-126">Aktualizovat hello zařízení připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="58b85-126">Update hello device connection string</span></span>

<span data-ttu-id="58b85-127">Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="58b85-127">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="58b85-128">Najděte hello řádek:</span><span class="sxs-lookup"><span data-stu-id="58b85-128">Find hello line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="58b85-129">Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="58b85-129">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="58b85-130">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="58b85-130">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="58b85-131">Spuštění ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="58b85-131">Run hello sample</span></span>

<span data-ttu-id="58b85-132">Hello spusťte následující příkazy tooinstall hello požadované balíčky pro hello ukázku:</span><span class="sxs-lookup"><span data-stu-id="58b85-132">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

<span data-ttu-id="58b85-133">Ukázka programu hello teď můžete spustit na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="58b85-133">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="58b85-134">Zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="58b85-134">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

<span data-ttu-id="58b85-135">Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="58b85-135">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="58b85-137">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="58b85-137">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="58b85-138">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58b85-138">Next steps</span></span>

<span data-ttu-id="58b85-139">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="58b85-139">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
