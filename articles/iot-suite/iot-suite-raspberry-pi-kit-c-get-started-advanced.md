---
title: "Připojení k Azure IoT Suite pomocí jazyka C pro podporu aktualizace firmwaru malin platformy | Microsoft Docs"
description: "Pomocí Startovní sady Microsoft Azure IoT Malinová pí 3 a sady Azure IoT Suite. Použití jazyka C pro vaše platformy malin připojení k řešení vzdáleného monitorování, odesílat telemetrická data ze senzorů do cloudu a provést aktualizaci firmwaru vzdálené."
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
ms.openlocfilehash: f36f6512bb30e4b109b1bd1c3cdab10300f4edc9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a><span data-ttu-id="9b73d-104">Připojení k řešení vzdáleného monitorování vaší malin pí 3 a povolit vzdálenou firmware aktualizace pomocí jazyka C</span><span class="sxs-lookup"><span data-stu-id="9b73d-104">Connect your Raspberry Pi 3 to the remote monitoring solution and enable remote firmware updates using C</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-selector](../../includes/iot-suite-raspberry-pi-kit-selector.md)]

<span data-ttu-id="9b73d-105">Tento kurz ukazuje, jak pomocí Startovní sady Microsoft Azure IoT pro malin pí 3 pro:</span><span class="sxs-lookup"><span data-stu-id="9b73d-105">This tutorial shows you how to use the Microsoft Azure IoT Starter Kit for Raspberry Pi 3 to:</span></span>

* <span data-ttu-id="9b73d-106">Vývoj čtečku teploty a vlhkosti, který může komunikovat s cloudem.</span><span class="sxs-lookup"><span data-stu-id="9b73d-106">Develop a temperature and humidity reader that can communicate with the cloud.</span></span>
* <span data-ttu-id="9b73d-107">Povolit a provádět vzdálenou firmware aktualizace nebo aktualizace klientská aplikace na malin pí.</span><span class="sxs-lookup"><span data-stu-id="9b73d-107">Enable and perform a remote firmware update to update the client application on the Raspberry Pi.</span></span>

<span data-ttu-id="9b73d-108">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="9b73d-108">The tutorial uses:</span></span>

* <span data-ttu-id="9b73d-109">Raspbian operačního systému, programovací jazyk C a Microsoft Azure IoT SDK pro jazyk C implementovat ukázka zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b73d-109">Raspbian OS, the C programming language, and the Microsoft Azure IoT SDK for C to implement a sample device.</span></span>
* <span data-ttu-id="9b73d-110">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="9b73d-110">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="9b73d-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="9b73d-111">Overview</span></span>

<span data-ttu-id="9b73d-112">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="9b73d-112">In this tutorial, you complete the following steps:</span></span>

* <span data-ttu-id="9b73d-113">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="9b73d-113">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="9b73d-114">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="9b73d-114">This step automatically deploys and configures multiple Azure services.</span></span>
* <span data-ttu-id="9b73d-115">Nastavte zařízením a senzory ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="9b73d-115">Set up your device and sensors to communicate with your computer and the remote monitoring solution.</span></span>
* <span data-ttu-id="9b73d-116">Aktualizujte zařízení ukázkový kód pro připojení k řešení vzdáleného monitorování a odesílat telemetrická data, která můžete zobrazit na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="9b73d-116">Update the sample device code to connect to the remote monitoring solution, and send telemetry that you can view on the solution dashboard.</span></span>
* <span data-ttu-id="9b73d-117">Ukázkový kód zařízení použijte k aktualizaci klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b73d-117">Use the sample device code to update the client application.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-prerequisites](../../includes/iot-suite-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="9b73d-118">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="9b73d-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="9b73d-119">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="9b73d-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="9b73d-120">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="9b73d-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="9b73d-121">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="9b73d-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="9b73d-122">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="9b73d-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-solution](../../includes/iot-suite-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a><span data-ttu-id="9b73d-123">Stáhnout a nakonfigurovat vzorku</span><span class="sxs-lookup"><span data-stu-id="9b73d-123">Download and configure the sample</span></span>

<span data-ttu-id="9b73d-124">Teď můžete stáhnout a nakonfigurovat vzdálené monitorování klientskou aplikaci na vaše malin platformy.</span><span class="sxs-lookup"><span data-stu-id="9b73d-124">You can now download and configure the remote monitoring client application on your Raspberry Pi.</span></span>

### <a name="clone-the-repositories"></a><span data-ttu-id="9b73d-125">Klonování úložiště</span><span class="sxs-lookup"><span data-stu-id="9b73d-125">Clone the repositories</span></span>

<span data-ttu-id="9b73d-126">Pokud jste tak ještě neučinili, klonování vyžaduje úložiště spuštěním následujících příkazů na vašeho platformy:</span><span class="sxs-lookup"><span data-stu-id="9b73d-126">If you haven't done so already, clone the required repositories by running the following commands on your Pi:</span></span>

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a><span data-ttu-id="9b73d-127">Aktualizovat připojovací řetězec zařízení</span><span class="sxs-lookup"><span data-stu-id="9b73d-127">Update the device connection string</span></span>

<span data-ttu-id="9b73d-128">Otevřete konfigurační soubor ukázka v **nano** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="9b73d-128">Open the sample configuration file in the **nano** editor using the following command:</span></span>

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

<span data-ttu-id="9b73d-129">Zástupné hodnoty nahraďte ID a IoT Hub informace o zařízení jste vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9b73d-129">Replace the placeholder values with the device ID and IoT Hub information you created and saved at the start of this tutorial.</span></span>

<span data-ttu-id="9b73d-130">Až skončíte, obsah souboru deviceinfo by měl vypadat jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="9b73d-130">When you are done, the contents of the deviceinfo file should look like the following example:</span></span>

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

<span data-ttu-id="9b73d-131">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="9b73d-131">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>

## <a name="build-the-sample"></a><span data-ttu-id="9b73d-132">Sestavit ukázku</span><span class="sxs-lookup"><span data-stu-id="9b73d-132">Build the sample</span></span>

<span data-ttu-id="9b73d-133">Pokud jste tak již neučinili, nainstalujte požadované balíčky pro Microsoft Azure IoT zařízení SDK pro jazyk C spuštěním následujících příkazů v terminálu na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="9b73d-133">If you have not already done so, install the prerequisite packages for the Microsoft Azure IoT Device SDK for C by running the following commands in a terminal on the Raspberry Pi:</span></span>

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

<span data-ttu-id="9b73d-134">Teď můžete sestavit ukázkové řešení na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="9b73d-134">You can now build the sample solution on the Raspberry Pi:</span></span>

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

<span data-ttu-id="9b73d-135">Teď můžete spustit ukázkový program na malin pí.</span><span class="sxs-lookup"><span data-stu-id="9b73d-135">You can now run the sample program on the Raspberry Pi.</span></span> <span data-ttu-id="9b73d-136">Zadejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="9b73d-136">Enter the command:</span></span>

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

<span data-ttu-id="9b73d-137">Následující ukázkový výstup je příklad výstupu, které vidíte na příkazovém řádku na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="9b73d-137">The following sample output is an example of the output you see at the command prompt on the Raspberry Pi:</span></span>

![Výstup z aplikace Malinová platformy][img-raspberry-output]

<span data-ttu-id="9b73d-139">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="9b73d-139">Press **Ctrl-C** to exit the program at any time.</span></span>

[!INCLUDE [iot-suite-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-raspberry-pi-kit-view-telemetry-advanced.md)]

1. <span data-ttu-id="9b73d-140">Na řídicím panelu řešení, klikněte na tlačítko **zařízení** přejděte **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="9b73d-140">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="9b73d-141">Vyberte vaše Malinová platformy v **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-141">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="9b73d-142">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="9b73d-142">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

1. <span data-ttu-id="9b73d-144">Na **vyvolání metody** vyberte **InitiateFirmwareUpdate** v **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="9b73d-144">On the **Invoke Method** page, choose **InitiateFirmwareUpdate** in the **Method** dropdown.</span></span>

1. <span data-ttu-id="9b73d-145">V **FWPackageURI** zadejte **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-145">In the **FWPackageURI** field, enter **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**.</span></span> <span data-ttu-id="9b73d-146">Tento soubor archivu obsahuje implementace 2.0 verzi firmwaru.</span><span class="sxs-lookup"><span data-stu-id="9b73d-146">This archive file contains the implementation of version 2.0 of the firmware.</span></span>

1. <span data-ttu-id="9b73d-147">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-147">Choose **InvokeMethod**.</span></span> <span data-ttu-id="9b73d-148">Aplikace na platformy malin odešle na potvrzení zpět na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="9b73d-148">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard.</span></span> <span data-ttu-id="9b73d-149">Pak spustí proces aktualizace firmwaru stažením novou verzi firmwaru:</span><span class="sxs-lookup"><span data-stu-id="9b73d-149">It then starts the firmware update process by downloading the new version of the firmware:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

## <a name="observe-the-firmware-update-process"></a><span data-ttu-id="9b73d-151">Sledovat firmware proces aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="9b73d-151">Observe the firmware update process</span></span>

<span data-ttu-id="9b73d-152">Můžete sledovat firmware proces aktualizovat. při jeho spuštění v zařízení a zobrazením hlášen vlastnostech na řídicím panelu řešení:</span><span class="sxs-lookup"><span data-stu-id="9b73d-152">You can observe the firmware update process as it runs on the device and by viewing the reported properties in the solution dashboard:</span></span>

1. <span data-ttu-id="9b73d-153">Postup v procesu aktualizace lze zobrazit na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="9b73d-153">You can view the progress in of the update process on the Raspberry Pi:</span></span>

    ![Ukázat průběh aktualizace][img-update-progress]

    > [!NOTE]
    > <span data-ttu-id="9b73d-155">Vzdálené monitorování aplikace se restartuje bez upozornění po dokončení aktualizace.</span><span class="sxs-lookup"><span data-stu-id="9b73d-155">The remote monitoring app restarts silently when the update completes.</span></span> <span data-ttu-id="9b73d-156">Použijte příkaz `ps -ef` ověření je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="9b73d-156">Use the command `ps -ef` to verify it is running.</span></span> <span data-ttu-id="9b73d-157">Pokud chcete ukončit proces, pomocí `kill` s id procesu.</span><span class="sxs-lookup"><span data-stu-id="9b73d-157">If you want to terminate the process, use the `kill` command with the process id.</span></span>

1. <span data-ttu-id="9b73d-158">Podle zařízení, na portálu řešení můžete zobrazit stav aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="9b73d-158">You can view the status of the firmware update, as reported by the device, in the solution portal.</span></span> <span data-ttu-id="9b73d-159">Následující snímek obrazovky ukazuje stav a doba trvání u každé fáze procesu aktualizace a nová verze firmwaru:</span><span class="sxs-lookup"><span data-stu-id="9b73d-159">The following screenshot shows the status and duration of each stage of the update process, and the new firmware version:</span></span>

    ![Zobrazit stav úlohy][img-job-status]

    <span data-ttu-id="9b73d-161">Pokud přejdete zpět na řídicí panel, můžete ověřit, že zařízení je stále odesílat telemetrii následující aktualizace firmwaru.</span><span class="sxs-lookup"><span data-stu-id="9b73d-161">If you navigate back to the dashboard, you can verify the device is still sending telemetry following the firmware update.</span></span>

> [!WARNING]
> <span data-ttu-id="9b73d-162">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="9b73d-162">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="9b73d-163">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="9b73d-163">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="9b73d-164">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="9b73d-164">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b73d-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9b73d-165">Next steps</span></span>

<span data-ttu-id="9b73d-166">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="9b73d-166">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>


[img-raspberry-output]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md