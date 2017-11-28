---
title: "aaaConnect tooAzure malin platformy IoT Suite pomocí jazyka C s simulovanou telemetrii | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použít C tooconnect vaše řešení vzdáleného sledování toohello malin Pi, odesílání simulovanou telemetrii toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 3c4fa43b381385d1a7896cada34aa3aa0b8e5fb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-simulated-telemetry-using-c"></a><span data-ttu-id="bab18-104">Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odeslat simulovanou telemetrii pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="bab18-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send simulated telemetry using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="bab18-105">Tento kurz ukazuje, jak cloudové toouse hello malin pí 3 toosimulate teploty a vlhkosti data toosend toohello.</span><span class="sxs-lookup"><span data-stu-id="bab18-105">This tutorial shows you how toouse hello Raspberry Pi 3 toosimulate temperature and humidity data toosend toohello cloud.</span></span> <span data-ttu-id="bab18-106">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="bab18-106">hello tutorial uses:</span></span>

- <span data-ttu-id="bab18-107">Raspbian operačního systému, programovací jazyk hello C a hello sady SDK pro aplikaci Microsoft Azure IoT pro C tooimplement ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="bab18-107">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
- <span data-ttu-id="bab18-108">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="bab18-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="bab18-109">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="bab18-109">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="bab18-110">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="bab18-110">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="bab18-111">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="bab18-111">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="bab18-112">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="bab18-112">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="bab18-113">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="bab18-113">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="bab18-114">Stáhnout a nakonfigurovat ukázka hello</span><span class="sxs-lookup"><span data-stu-id="bab18-114">Download and configure hello sample</span></span>

<span data-ttu-id="bab18-115">Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="bab18-115">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="bab18-116">Klonování úložiště hello</span><span class="sxs-lookup"><span data-stu-id="bab18-116">Clone hello repositories</span></span>

<span data-ttu-id="bab18-117">Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy v terminálu na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="bab18-117">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="bab18-118">Aktualizovat hello zařízení připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="bab18-118">Update hello device connection string</span></span>

<span data-ttu-id="bab18-119">Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bab18-119">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="bab18-120">Vyhledejte hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="bab18-120">Locate hello following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="bab18-121">Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="bab18-121">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="bab18-122">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="bab18-122">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="bab18-123">Vytvoření ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="bab18-123">Build hello sample</span></span>

<span data-ttu-id="bab18-124">Nainstalujte hello požadovaných balíčků hello Microsoft Azure IoT zařízení SDK pro jazyk C tak, že spustíte následující příkazy v terminálu na hello malin pí hello:</span><span class="sxs-lookup"><span data-stu-id="bab18-124">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="bab18-125">Teď můžete sestavit hello aktualizované ukázkové řešení na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="bab18-125">You can now build hello updated sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/simulator/build.sh
```

<span data-ttu-id="bab18-126">Ukázka programu hello teď můžete spustit na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="bab18-126">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="bab18-127">Zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="bab18-127">Enter hello command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="bab18-128">Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="bab18-128">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="bab18-130">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="bab18-130">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a><span data-ttu-id="bab18-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bab18-131">Next steps</span></span>

<span data-ttu-id="bab18-132">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="bab18-132">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-simulator/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
