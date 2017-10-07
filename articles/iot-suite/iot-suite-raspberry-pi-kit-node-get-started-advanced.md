---
title: "aktualizace aaaConnect tooAzure malin platformy IoT Suite pomocí firmwaru toosupport Node.js | Microsoft Docs"
description: "Použijte hello Microsoft Azure IoT Starter Kit pro hello malin pí 3 a Azure IoT Suite. Pomocí Node.js tooconnect vaše řešení vzdáleného sledování toohello malin Pi, odesílat telemetrická data ze senzorů toohello cloudu a provést aktualizaci firmwaru vzdálené."
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
ms.openlocfilehash: 43bd3f16ee3d292cd9cffa8bfe7d4ca721e5c39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-raspberry-pi-3-toohello-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a><span data-ttu-id="2272c-104">Připojit vaše řešení vzdáleného sledování malin pí 3 toohello a povolit vzdálenou firmware aktualizace pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="2272c-104">Connect your Raspberry Pi 3 toohello remote monitoring solution and enable remote firmware updates using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="2272c-105">Tento kurz ukazuje, jak toouse hello Microsoft Azure IoT Starter Kit pro malin pí 3 na:</span><span class="sxs-lookup"><span data-stu-id="2272c-105">This tutorial shows you how toouse hello Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="2272c-106">Vývoj čtečku teploty a vlhkosti, který může komunikovat s hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="2272c-106">Develop a temperature and humidity reader that can communicate with hello cloud.</span></span>
* <span data-ttu-id="2272c-107">Povolit a provádět vzdálenou firmware aktualizace tooupdate hello klientskou aplikaci na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="2272c-107">Enable and perform a remote firmware update tooupdate hello client application on hello Raspberry Pi.</span></span>

<span data-ttu-id="2272c-108">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="2272c-108">hello tutorial uses:</span></span>

- <span data-ttu-id="2272c-109">Raspbian OS hello Node.js programovací jazyk a hello Microsoft Azure IoT SDK pro Node.js tooimplement ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="2272c-109">Raspbian OS, hello Node.js programming language, and hello Microsoft Azure IoT SDK for Node.js tooimplement a sample device.</span></span>
- <span data-ttu-id="2272c-110">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="2272c-110">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="2272c-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="2272c-111">Overview</span></span>

<span data-ttu-id="2272c-112">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2272c-112">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="2272c-113">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="2272c-113">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="2272c-114">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="2272c-114">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="2272c-115">Nastavte vaše zařízení a senzory toocommunicate s počítačem a hello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="2272c-115">Set up your device and sensors toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="2272c-116">Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-116">Update hello sample device code tooconnect toohello remote monitoring solution, and send telemetry that you can view on hello solution dashboard.</span></span>
- <span data-ttu-id="2272c-117">Pomocí klienta aplikace hello kód tooupdate hello ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="2272c-117">Use hello sample device code tooupdate hello client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="2272c-118">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="2272c-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="2272c-119">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="2272c-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="2272c-120">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="2272c-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="2272c-121">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="2272c-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="2272c-122">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="2272c-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-hello-sample"></a><span data-ttu-id="2272c-123">Stáhnout a nakonfigurovat ukázka hello</span><span class="sxs-lookup"><span data-stu-id="2272c-123">Download and configure hello sample</span></span>

<span data-ttu-id="2272c-124">Nyní můžete stáhnout a nakonfigurovat hello vzdálené monitorování klientské aplikace na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="2272c-124">You can now download and configure hello remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="2272c-125">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="2272c-125">Install Node.js</span></span>

<span data-ttu-id="2272c-126">Pokud jste tak ještě neučinili, nainstalujte na vaše platformy malin Node.js.</span><span class="sxs-lookup"><span data-stu-id="2272c-126">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="2272c-127">Hello IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2272c-127">hello IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="2272c-128">Hello následující kroky ukazují, jak tooinstall Node.js v6.10.2 na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2272c-128">hello following steps show you how tooinstall Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="2272c-129">Použijte následující příkaz tooupdate hello vaší malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2272c-129">Use hello following command tooupdate your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="2272c-130">Použijte následující příkaz toodownload hello Node.js binární soubory tooyour malin pí hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-130">Use hello following command toodownload hello Node.js binaries tooyour Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="2272c-131">Použijte následující příkaz tooinstall hello binární soubory hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-131">Use hello following command tooinstall hello binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="2272c-132">Použijte následující příkaz tooverify jste úspěšně nainstalovali Node.js v6.10.2 hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-132">Use hello following command tooverify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-hello-repositories"></a><span data-ttu-id="2272c-133">Klonování úložiště hello</span><span class="sxs-lookup"><span data-stu-id="2272c-133">Clone hello repositories</span></span>

<span data-ttu-id="2272c-134">Pokud jste tak ještě neučinili, požadované klon hello hello úložiště tak, že spustíte následující příkazy na vaše platformy:</span><span class="sxs-lookup"><span data-stu-id="2272c-134">If you haven't done so already, clone hello required repositories by running hello following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-hello-device-connection-string"></a><span data-ttu-id="2272c-135">Aktualizovat hello zařízení připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="2272c-135">Update hello device connection string</span></span>

<span data-ttu-id="2272c-136">Otevřete hello vzorový konfigurační soubor v hello **nano** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2272c-136">Open hello sample configuration file in hello **nano** editor using hello following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="2272c-137">Hello zástupné hodnoty nahraďte hello id zařízení a informace služby IoT Hub vytvořili a uložili při spuštění hello tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="2272c-137">Replace hello placeholder values with hello device id and IoT Hub information you created and saved at hello start of this tutorial.</span></span>

<span data-ttu-id="2272c-138">Až skončíte, hello obsah souboru deviceinfo hello by měl vypadat podobně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="2272c-138">When you are done, hello contents of hello deviceinfo file should look like hello following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="2272c-139">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="2272c-139">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>

## <a name="run-hello-sample"></a><span data-ttu-id="2272c-140">Spuštění ukázkové hello</span><span class="sxs-lookup"><span data-stu-id="2272c-140">Run hello sample</span></span>

<span data-ttu-id="2272c-141">Hello spusťte následující příkazy tooinstall hello požadované balíčky pro hello ukázku:</span><span class="sxs-lookup"><span data-stu-id="2272c-141">Run hello following commands tooinstall hello prerequisite packages for hello sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

<span data-ttu-id="2272c-142">Ukázka programu hello teď můžete spustit na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="2272c-142">You can now run hello sample program on hello Raspberry Pi.</span></span> <span data-ttu-id="2272c-143">Zadejte příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-143">Enter hello command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

<span data-ttu-id="2272c-144">Hello následující ukázkový výstup je příklad výstupu hello, uvidíte hello příkazového řádku na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2272c-144">hello following sample output is an example of hello output you see at hello command prompt on hello Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="2272c-146">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="2272c-146">Press **Ctrl-C** tooexit hello program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="2272c-147">Na řídicím panelu řešení hello, klikněte na tlačítko **zařízení** toovisit hello **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="2272c-147">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="2272c-148">Vyberte platformy vaší malin v hello **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="2272c-148">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="2272c-149">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="2272c-149">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

1. <span data-ttu-id="2272c-151">Na hello **vyvolání metody** vyberte **InitiateFirmwareUpdate** v hello **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="2272c-151">On hello **Invoke Method** page, choose **InitiateFirmwareUpdate** in hello **Method** dropdown.</span></span>

1. <span data-ttu-id="2272c-152">V hello **FWPackageURI** zadejte **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span><span class="sxs-lookup"><span data-stu-id="2272c-152">In hello **FWPackageURI** field, enter **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span></span> <span data-ttu-id="2272c-153">Tento soubor obsahuje hello implementace 2.0 verzi firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-153">This file contains hello implementation of version 2.0 of hello firmware.</span></span>

1. <span data-ttu-id="2272c-154">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="2272c-154">Choose **InvokeMethod**.</span></span> <span data-ttu-id="2272c-155">aplikace Hello na hello malin pí odešle řídicí panel řešení back toohello potvrzení.</span><span class="sxs-lookup"><span data-stu-id="2272c-155">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard.</span></span> <span data-ttu-id="2272c-156">Pak spustí proces aktualizace firmwaru hello stažením hello novou verzi firmwaru hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-156">It then starts hello firmware update process by downloading hello new version of hello firmware:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

## <a name="observe-hello-firmware-update-process"></a><span data-ttu-id="2272c-158">Sledovat proces aktualizace firmwaru hello</span><span class="sxs-lookup"><span data-stu-id="2272c-158">Observe hello firmware update process</span></span>

<span data-ttu-id="2272c-159">Můžete sledovat proces aktualizace firmwaru hello při jeho spuštění v zařízení hello a zobrazením hello hlášené vlastnosti v řídicí panel řešení hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-159">You can observe hello firmware update process as it runs on hello device and by viewing hello reported properties in hello solution dashboard:</span></span>

1. <span data-ttu-id="2272c-160">Průběh hello v procesu aktualizace hello můžete zobrazit na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="2272c-160">You can view hello progress in of hello update process on hello Raspberry Pi:</span></span>

    ![Ukázat průběh aktualizace][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="2272c-162">vzdálené monitorování aplikace Hello restartuje bezobslužně po dokončení aktualizace hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-162">hello remote monitoring app restarts silently when hello update completes.</span></span> <span data-ttu-id="2272c-163">Použijte příkaz hello `ps -ef` tooverify je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="2272c-163">Use hello command `ps -ef` tooverify it is running.</span></span> <span data-ttu-id="2272c-164">Pokud chcete proces hello tooterminate, použijte hello `kill` s id procesu hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-164">If you want tooterminate hello process, use hello `kill` command with hello process id.</span></span>

1. <span data-ttu-id="2272c-165">Stav hello hello aktualizace firmwaru, můžete zobrazit podle hello zařízení na portálu řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-165">You can view hello status of hello firmware update, as reported by hello device, in hello solution portal.</span></span> <span data-ttu-id="2272c-166">Hello následující snímek obrazovky ukazuje stav hello a doba trvání u každé fáze procesu aktualizace hello a nová verze firmwaru hello:</span><span class="sxs-lookup"><span data-stu-id="2272c-166">hello following screenshot shows hello status and duration of each stage of hello update process, and hello new firmware version:</span></span>

    ![Zobrazit stav úlohy][img-job-status]

    <span data-ttu-id="2272c-168">Pokud přejdete zpět toohello řídicího panelu, můžete ověřit, že hello zařízení stále odesílá telemetrii následující aktualizaci firmwaru hello.</span><span class="sxs-lookup"><span data-stu-id="2272c-168">If you navigate back toohello dashboard, you can verify hello device is still sending telemetry following hello firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="2272c-169">Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="2272c-169">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="2272c-170">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="2272c-170">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="2272c-171">Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2272c-171">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2272c-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2272c-172">Next steps</span></span>

<span data-ttu-id="2272c-173">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="2272c-173">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
