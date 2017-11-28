---
title: "aaaConnect tooAzure malin platformy IoT Suite pomocí jazyka C skutečné snímači | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Použijte C tooconnect vaše řešení vzdáleného sledování toohello malin platformy, odesílat telemetrická data ze senzorů toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 7dac55ae5fde4c6f8e3ea6a7debf9a6822dc07ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a><span data-ttu-id="8cca1-104">Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a odesílat telemetrická data z reálného senzor pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="8cca1-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and send telemetry from a real sensor using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="8cca1-105">Tento kurz ukazuje, jak toouse hello Microsoft Azure IoT Starter Kit pro malin pí 3 toodevelop čtečku teploty a vlhkosti, který může komunikovat s hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="8cca1-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 toodevelop a temperature and humidity reader that can communicate with hello cloud.</span></span> <span data-ttu-id="8cca1-106">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="8cca1-106">hello tutorial uses:</span></span>

- <span data-ttu-id="8cca1-107">Raspbian operačního systému, programovací jazyk hello C a hello sady SDK pro aplikaci Microsoft Azure IoT pro C tooimplement ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="8cca1-107">Raspbian OS, hello C programming language, and hello Microsoft Azure IoT SDK for C tooimplement a sample device.</span></span>
- <span data-ttu-id="8cca1-108">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="8cca1-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="8cca1-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="8cca1-109">Overview</span></span>

<span data-ttu-id="8cca1-110">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8cca1-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="8cca1-111">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8cca1-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="8cca1-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="8cca1-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="8cca1-113">Nastavte vaše zařízení a senzory toocommunicate s počítačem a hello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="8cca1-113">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="8cca1-114">Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="8cca1-114">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="8cca1-115">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="8cca1-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="8cca1-116">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="8cca1-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="8cca1-117">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="8cca1-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="8cca1-118">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="8cca1-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="8cca1-119">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="8cca1-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="8cca1-120">Stáhnout a nakonfigurovat ukázka hello</span><span class="sxs-lookup"><span data-stu-id="8cca1-120">Download and configure hello sample</span></span>

<span data-ttu-id="8cca1-121">Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="8cca1-121">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-hello-repositories"></a><span data-ttu-id="8cca1-122">Klonování úložiště hello</span><span class="sxs-lookup"><span data-stu-id="8cca1-122">Clone hello repositories</span></span>

<span data-ttu-id="8cca1-123">Pokud jste tak již neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy v terminálu na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="8cca1-123">If you haven't already done so, clone hello required repositories by running hello following commands in a terminal on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="8cca1-124">Aktualizovat hello zařízení připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="8cca1-124">Update hello device connection string</span></span>

<span data-ttu-id="8cca1-125">Otevřete hello ukázka zdrojový soubor v hello **nano** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8cca1-125">Open hello sample source file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

<span data-ttu-id="8cca1-126">Vyhledejte hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="8cca1-126">Locate hello following lines:</span></span>

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

<span data-ttu-id="8cca1-127">Hello zástupné hodnoty nahraďte hello zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8cca1-127">Replace hello placeholder values with hello device and IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="8cca1-128">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="8cca1-128">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="build-hello-sample"></a><span data-ttu-id="8cca1-129">Vytvoření ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="8cca1-129">Build hello sample</span></span>

<span data-ttu-id="8cca1-130">Nainstalujte hello požadovaných balíčků hello Microsoft Azure IoT zařízení SDK pro jazyk C tak, že spustíte následující příkazy v terminálu na hello malin pí hello:</span><span class="sxs-lookup"><span data-stu-id="8cca1-130">Install hello prerequisite packages for hello Microsoft Azure IoT Device SDK for C by running hello following commands in a terminal on hello Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="8cca1-131">Teď můžete sestavit hello aktualizované ukázkové řešení na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="8cca1-131">You can now build hello updated sample solution on hello Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

<span data-ttu-id="8cca1-132">Ukázka programu hello teď můžete spustit na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="8cca1-132">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="8cca1-133">Zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="8cca1-133">Enter hello command:</span></span>

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

<span data-ttu-id="8cca1-134">Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="8cca1-134">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="8cca1-136">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="8cca1-136">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a><span data-ttu-id="8cca1-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8cca1-137">Next steps</span></span>

<span data-ttu-id="8cca1-138">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="8cca1-138">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
