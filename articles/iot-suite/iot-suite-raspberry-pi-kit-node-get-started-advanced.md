---
title: "Připojení k Azure IoT Suite pomocí Node.js pro podporu aktualizace firmwaru malin platformy | Microsoft Docs"
description: "Pomocí Startovní sady Microsoft Azure IoT Malinová pí 3 a sady Azure IoT Suite. Použití Node.js pro vaše platformy malin připojení k řešení vzdáleného monitorování, odesílat telemetrická data ze snímače do cloudu a provést aktualizaci firmwaru vzdálené."
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
ms.openlocfilehash: 54503d5d6a636239d240509d7d09cf334234bac7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a><span data-ttu-id="c7a84-104">Připojení k řešení vzdáleného monitorování vaší malin pí 3 a povolit vzdálenou firmware aktualizace pomocí Node.js</span><span class="sxs-lookup"><span data-stu-id="c7a84-104">Connect your Raspberry Pi 3 to the remote monitoring solution and enable remote firmware updates using Node.js</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="c7a84-105">Tento kurz ukazuje, jak pomocí Startovní sady Microsoft Azure IoT pro malin pí 3 pro:</span><span class="sxs-lookup"><span data-stu-id="c7a84-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="c7a84-106">Vývoj čtečku teploty a vlhkosti, který může komunikovat s cloudem.</span><span class="sxs-lookup"><span data-stu-id="c7a84-106">Develop a temperature and humidity reader that can communicate with the cloud.</span></span>
* <span data-ttu-id="c7a84-107">Povolit a provádět vzdálenou firmware aktualizace nebo aktualizace klientská aplikace na malin pí.</span><span class="sxs-lookup"><span data-stu-id="c7a84-107">Enable and perform a remote firmware update to update the client application on the Raspberry Pi.</span></span>

<span data-ttu-id="c7a84-108">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="c7a84-108">The tutorial uses:</span></span>

- <span data-ttu-id="c7a84-109">Raspbian operačního systému, programovací jazyk Node.js a Microsoft Azure IoT SDK pro Node.js implementovat ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="c7a84-109">Raspbian OS, the Node.js programming language, and the Microsoft Azure IoT SDK for Node.js to implement a sample device.</span></span>
- <span data-ttu-id="c7a84-110">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="c7a84-110">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="c7a84-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="c7a84-111">Overview</span></span>

<span data-ttu-id="c7a84-112">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c7a84-112">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="c7a84-113">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="c7a84-113">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="c7a84-114">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="c7a84-114">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="c7a84-115">Nastavte zařízením a senzory ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="c7a84-115">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="c7a84-116">Aktualizujte zařízení ukázkový kód pro připojení k řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="c7a84-116">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>
- <span data-ttu-id="c7a84-117">Ukázkový kód zařízení použijte k aktualizaci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="c7a84-117">Use the sample device code to update the client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="c7a84-118">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="c7a84-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="c7a84-119">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="c7a84-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="c7a84-120">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="c7a84-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="c7a84-121">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="c7a84-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="c7a84-122">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c7a84-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="c7a84-123">Stáhnout a nakonfigurovat vzorku</span><span class="sxs-lookup"><span data-stu-id="c7a84-123">Download and configure the sample</span></span>

<span data-ttu-id="c7a84-124">Teď můžete stáhnout a nakonfigurovat vzdálené monitorování klientskou aplikaci na vaše malin platformy.</span><span class="sxs-lookup"><span data-stu-id="c7a84-124">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="install-nodejs"></a><span data-ttu-id="c7a84-125">Instalovat Node.js</span><span class="sxs-lookup"><span data-stu-id="c7a84-125">Install Node.js</span></span>

<span data-ttu-id="c7a84-126">Pokud jste tak ještě neučinili, nainstalujte na vaše platformy malin Node.js.</span><span class="sxs-lookup"><span data-stu-id="c7a84-126">If you haven't done so already, install Node.js on your Raspberry Pi.</span></span> <span data-ttu-id="c7a84-127">Sada IoT SDK pro Node.js vyžaduje verzi 0.11.5 Node.js nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c7a84-127">The IoT SDK for Node.js requires version 0.11.5 of Node.js or later.</span></span> <span data-ttu-id="c7a84-128">Následující kroky vám ukážou, jak nainstalovat Node.js v6.10.2 na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="c7a84-128">The following steps show you how to install Node.js v6.10.2 on your Raspberry Pi:</span></span>

1. <span data-ttu-id="c7a84-129">Použijte následující příkaz k aktualizaci vašeho malin platformy:</span><span class="sxs-lookup"><span data-stu-id="c7a84-129">Use the following command to update your Raspberry Pi:</span></span>

    ```sh
    sudo apt-get update
    ```

1. <span data-ttu-id="c7a84-130">Stažení Node.js binární soubory pro vaše platformy malin použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7a84-130">Use the following command to download the Node.js binaries to your Raspberry Pi:</span></span>

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="c7a84-131">Použijte následující příkaz pro instalaci binárních souborů:</span><span class="sxs-lookup"><span data-stu-id="c7a84-131">Use the following command to install the binaries:</span></span>

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. <span data-ttu-id="c7a84-132">Pomocí následujícího příkazu ověřte, že jste úspěšně nainstalovali Node.js v6.10.2:</span><span class="sxs-lookup"><span data-stu-id="c7a84-132">Use the following command to verify you have installed Node.js v6.10.2 successfully:</span></span>

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a><span data-ttu-id="c7a84-133">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="c7a84-133">Clone the repositories</span></span>

<span data-ttu-id="c7a84-134">Pokud jste tak ještě neučinili, klonování vyžaduje úložiště spuštěním následujících příkazů na vašeho platformy:</span><span class="sxs-lookup"><span data-stu-id="c7a84-134">If you haven't done so already, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="c7a84-135">Aktualizovat připojovací řetězec zařízení</span><span class="sxs-lookup"><span data-stu-id="c7a84-135">Update the device connection string</span></span>

<span data-ttu-id="c7a84-136">Otevřete konfigurační soubor ukázka v **nano** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="c7a84-136">Open the sample configuration file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="c7a84-137">Zástupné hodnoty nahraďte id zařízení a informace služby IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c7a84-137">Replace the placeholder values with the device id and IoT Hub information you created and saved at the start of this tutorial.</span></span>

<span data-ttu-id="c7a84-138">Až skončíte, obsah souboru deviceinfo by měl vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c7a84-138">When you are done, the contents of the deviceinfo file should look like the following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="c7a84-139">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="c7a84-139">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="c7a84-140">Spustit ukázku</span><span class="sxs-lookup"><span data-stu-id="c7a84-140">Run the sample</span></span>

<span data-ttu-id="c7a84-141">Spusťte následující příkazy pro instalaci požadovaných balíčků pro ukázku:</span><span class="sxs-lookup"><span data-stu-id="c7a84-141">Run the following commands to install the prerequisite packages for the sample:</span></span>

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

<span data-ttu-id="c7a84-142">Teď můžete spustit ukázkový program na malin pí.</span><span class="sxs-lookup"><span data-stu-id="c7a84-142">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="c7a84-143">Zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7a84-143">Enter the command:</span></span>

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

<span data-ttu-id="c7a84-144">Následující ukázkový výstup je příklad výstupu, které vidíte na příkazovém řádku na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="c7a84-144">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="c7a84-146">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="c7a84-146">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="c7a84-147">Na řídicím panelu řešení, klikněte na tlačítko **zařízení** přejděte **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="c7a84-147">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="c7a84-148">Vyberte vaše Malinová platformy v **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c7a84-148">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="c7a84-149">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="c7a84-149">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

1. <span data-ttu-id="c7a84-151">Na **vyvolání metody** vyberte **InitiateFirmwareUpdate** v **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="c7a84-151">On the **Invoke Method** page, choose **InitiateFirmwareUpdate** in the **Method** dropdown.</span></span>

1. <span data-ttu-id="c7a84-152">V **FWPackageURI** zadejte **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span><span class="sxs-lookup"><span data-stu-id="c7a84-152">In the **FWPackageURI** field, enter **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**.</span></span> <span data-ttu-id="c7a84-153">Tento soubor obsahuje implementace 2.0 verzi firmwaru.</span><span class="sxs-lookup"><span data-stu-id="c7a84-153">This file contains the implementation of version 2.0 of the firmware.</span></span>

1. <span data-ttu-id="c7a84-154">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="c7a84-154">Choose **InvokeMethod**.</span></span> <span data-ttu-id="c7a84-155">Aplikace na platformy malin odešle na potvrzení zpět na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="c7a84-155">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard.</span></span> <span data-ttu-id="c7a84-156">Pak spustí proces aktualizace firmwaru stažením novou verzi firmwaru:</span><span class="sxs-lookup"><span data-stu-id="c7a84-156">It then starts the firmware update process by downloading the new version of the firmware:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

## <a name="observe-the-firmware-update-process"></a><span data-ttu-id="c7a84-158">Sledovat firmware proces aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="c7a84-158">Observe the firmware update process</span></span>

<span data-ttu-id="c7a84-159">Můžete sledovat firmware proces aktualizovat. při jeho spuštění v zařízení a zobrazením hlášen vlastnostech na řídicím panelu řešení:</span><span class="sxs-lookup"><span data-stu-id="c7a84-159">You can observe the firmware update process as it runs on the device and by viewing the reported properties in the solution dashboard:</span></span>

1. <span data-ttu-id="c7a84-160">Postup v procesu aktualizace lze zobrazit na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="c7a84-160">You can view the progress in of the update process on the Raspberry Pi:</span></span>

    ![Ukázat průběh aktualizace][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="c7a84-162">Vzdálené monitorování aplikace se restartuje bez upozornění po dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c7a84-162">The remote monitoring app restarts silently when the update completes.</span></span> <span data-ttu-id="c7a84-163">Použijte příkaz `ps -ef` ověření je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c7a84-163">Use the command `ps -ef` to verify it is running.</span></span> <span data-ttu-id="c7a84-164">Pokud chcete ukončit proces, pomocí `kill` s id procesu.</span><span class="sxs-lookup"><span data-stu-id="c7a84-164">If you want to terminate the process, use the `kill` command with the process id.</span></span>

1. <span data-ttu-id="c7a84-165">Podle zařízení, na portálu řešení můžete zobrazit stav aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="c7a84-165">You can view the status of the firmware update, as reported by the device, in the solution portal.</span></span> <span data-ttu-id="c7a84-166">Následující snímek obrazovky ukazuje stav a doba trvání u každé fáze procesu aktualizace a nová verze firmwaru:</span><span class="sxs-lookup"><span data-stu-id="c7a84-166">The following screenshot shows the status and duration of each stage of the update process, and the new firmware version:</span></span>

    ![Zobrazit stav úlohy][img-job-status]

    <span data-ttu-id="c7a84-168">Pokud přejdete zpět na řídicí panel, můžete ověřit, že zařízení je stále odesílat telemetrii následující aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="c7a84-168">If you navigate back to the dashboard, you can verify the device is still sending telemetry following the firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="c7a84-169">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="c7a84-169">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="c7a84-170">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="c7a84-170">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="c7a84-171">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="c7a84-171">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c7a84-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c7a84-172">Next steps</span></span>

<span data-ttu-id="c7a84-173">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="c7a84-173">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
