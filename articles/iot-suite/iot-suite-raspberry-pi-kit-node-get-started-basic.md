---
title: "aaaConnect tooAzure malin platformy IoT Suite pomocí Node.js skutečné snímači | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Pomocí Node.js tooconnect vaše řešení vzdáleného sledování toohello malin platformy, odesílat telemetrická data ze senzorů toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 7ffb4a7a8c04b424a1f29170f4739d89f39a2429
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a><span data-ttu-id="f708b-104">Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odesílat telemetrická data z reálného senzor pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="f708b-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send telemetry from a real sensor using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="f708b-105">Tento kurz ukazuje, jak toouse hello Microsoft Azure IoT Starter Kit pro malin pí 3 toodevelop čtečku teploty a vlhkosti, který může komunikovat s hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="f708b-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 toodevelop a temperature and humidity reader that can communicate with hello cloud.</span></span> <span data-ttu-id="f708b-106">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="f708b-106">hello tutorial uses:</span></span>

- <span data-ttu-id="f708b-107">Raspbian OS hello Node.js programovací jazyk a hello Microsoft Azure IoT SDK pro Node.js tooimplement ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="f708b-107">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="f708b-108">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="f708b-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="f708b-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="f708b-109">Overview</span></span>

<span data-ttu-id="f708b-110">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f708b-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="f708b-111">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f708b-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="f708b-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="f708b-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="f708b-113">Nastavte vaše zařízení a senzory toocommunicate s počítačem a hello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="f708b-113">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="f708b-114">Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="f708b-114">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="f708b-115">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="f708b-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="f708b-116">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="f708b-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="f708b-117">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="f708b-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="f708b-118">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="f708b-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="f708b-119">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="f708b-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="f708b-120">Stáhnout a nakonfigurovat ukázka hello</span><span class="sxs-lookup"><span data-stu-id="f708b-120">Download and configure hello sample</span></span>

<span data-ttu-id="f708b-121">Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="f708b-121">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="f708b-122">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="f708b-122">Install Node.js</span></span>

<span data-ttu-id="f708b-123">Nainstalujte Node.js na vaše Malinová pí.</span><span class="sxs-lookup"><span data-stu-id="f708b-123">Install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="f708b-124">Hello IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f708b-124">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="f708b-125">Hello následující kroky ukazují, jak tooinstall Node.js v6.10.2 na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f708b-125">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="f708b-126">Použijte následující příkaz tooupdate hello vaší malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f708b-126">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="f708b-127">Použijte následující příkaz toodownload hello Node.js binární soubory tooyour malin pí hello:</span><span class="sxs-lookup"><span data-stu-id="f708b-127">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="f708b-128">Použijte následující příkaz tooinstall hello binární soubory hello:</span><span class="sxs-lookup"><span data-stu-id="f708b-128">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="f708b-129">Použijte následující příkaz tooverify jste úspěšně nainstalovali Node.js v6.10.2 hello:</span><span class="sxs-lookup"><span data-stu-id="f708b-129">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="f708b-130">Klonování úložiště hello</span><span class="sxs-lookup"><span data-stu-id="f708b-130">Clone hello repositories</span></span>

<span data-ttu-id="f708b-131">Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="f708b-131">If you haven't already done so, clone hello required repositories by running hello following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="f708b-132">Aktualizovat hello zařízení připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="f708b-132">Update hello device connection string</span></span>

<span data-ttu-id="f708b-133">Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f708b-133">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="f708b-134">Najděte hello řádek:</span><span class="sxs-lookup"><span data-stu-id="f708b-134">Find hello line:</span></span>

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

<span data-ttu-id="f708b-135">Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f708b-135">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="f708b-136">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="f708b-136">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="f708b-137">Spuštění ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="f708b-137">Run hello sample</span></span>

<span data-ttu-id="f708b-138">Hello spusťte následující příkazy tooinstall hello požadované balíčky pro hello ukázku:</span><span class="sxs-lookup"><span data-stu-id="f708b-138">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

<span data-ttu-id="f708b-139">Ukázka programu hello teď můžete spustit na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="f708b-139">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="f708b-140">Zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f708b-140">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

<span data-ttu-id="f708b-141">Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="f708b-141">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="f708b-143">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="f708b-143">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="f708b-144">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f708b-144">Next steps</span></span>

<span data-ttu-id="f708b-145">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="f708b-145">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
