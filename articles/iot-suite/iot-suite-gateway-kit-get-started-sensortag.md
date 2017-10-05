---
title: "Připojení brány Azure IoT Suite pomocí Intel NUC | Microsoft Docs"
description: "Použijte Kit komerční brány Microsoft IoT a předkonfigurované řešení vzdáleného monitorování. Použití brány Azure IoT Edge tak, aby zařízení SensorTag pro připojení k řešení vzdáleného monitorování, odeslání telemetrie do cloudu a reagovat na metody vyvolané z řídicího panelu řešení."
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
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: bda16be1094276fcecef1e708f9d7db307d94a89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a><span data-ttu-id="66633-104">Připojte bránu Azure IoT Edge pro předkonfigurované řešení vzdáleného monitorování a odesílat telemetrická data z SensorTag</span><span class="sxs-lookup"><span data-stu-id="66633-104">Connect your Azure IoT Edge gateway to the remote monitoring preconfigured solution and send telemetry from a SensorTag</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="66633-105">V tomto kurzu se dozvíte, jak používat Azure IoT Edge k odesílání teploty a vlhkosti data ze zařízení SensorTag pro předkonfigurované řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-105">This tutorial shows you how to use Azure IoT Edge to send temperature and humidity data from SensorTag device to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="66633-106">SensorTag připojí k bráně Intel NUC pomocí Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="66633-106">The SensorTag connects to the Intel NUC gateway using Bluetooth.</span></span> <span data-ttu-id="66633-107">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="66633-107">The tutorial uses:</span></span>

- <span data-ttu-id="66633-108">Azure IoT okraj implementovat ukázka brány.</span><span class="sxs-lookup"><span data-stu-id="66633-108">Azure IoT Edge to implement a sample gateway.</span></span>
- <span data-ttu-id="66633-109">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="66633-109">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="66633-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="66633-110">Overview</span></span>

<span data-ttu-id="66633-111">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66633-111">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="66633-112">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="66633-112">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="66633-113">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="66633-113">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="66633-114">Nastavte vaše zařízení brány Intel NUC ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-114">Set up your Intel NUC gateway device to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="66633-115">Nastaví bránu Intel NUC přijímat telemetrická data ze zařízení SensorTag a odesílat je na řídicím panelu vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-115">Set up your Intel NUC gateway to receive telemetry from a SensorTag device and send it to the remote monitoring dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

<span data-ttu-id="66633-116">[Texas Instruments SensorTag lit][lnk-sensortag].</span><span class="sxs-lookup"><span data-stu-id="66633-116">[Texas Instruments BLE SensorTag][lnk-sensortag].</span></span> <span data-ttu-id="66633-117">V tomto kurzu načte ze zařízení SensorTag telemetrická data přes Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="66633-117">This tutorial retrieves telemetry data over Bluetooth from the SensorTag device.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="66633-118">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="66633-118">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="66633-119">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="66633-119">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="66633-120">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="66633-120">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="66633-121">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="66633-121">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="66633-122">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="66633-122">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a><span data-ttu-id="66633-123">Konfigurace připojení k Bluetooth</span><span class="sxs-lookup"><span data-stu-id="66633-123">Configure Bluetooth connectivity</span></span>

<span data-ttu-id="66633-124">Nakonfigurujte Bluetooth na NUC Intel tak, aby zařízení SensorTag pro připojení a odesílat telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="66633-124">Configure Bluetooth on the Intel NUC to enable the SensorTag device to connect and send telemetry.</span></span>

### <a name="find-the-mac-address-of-the-sensortag"></a><span data-ttu-id="66633-125">Najít adresu MAC SensorTag</span><span class="sxs-lookup"><span data-stu-id="66633-125">Find the MAC address of the SensorTag</span></span>

1. <span data-ttu-id="66633-126">V prostředí na Intel NUC spusťte následující příkaz, který odblokovat službu Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="66633-126">In the shell on the Intel NUC, run the following command to unblock the Bluetooth service:</span></span>

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. <span data-ttu-id="66633-127">Spusťte následující příkazy a spusťte službu Bluetooth na Intel NUC a zadejte prostředí Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="66633-127">Run the following commands to start the Bluetooth service on the Intel NUC and enter the Bluetooth shell:</span></span>

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="66633-128">Spusťte následující příkaz na mocninu na řadiči Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="66633-128">Run the following command to power on the Bluetooth controller:</span></span>

    ```bash
    power on
    ```

    <span data-ttu-id="66633-129">Pokud je na řadič, zobrazí se zpráva **změna power v úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="66633-129">When the controller is on, you see a message **Changing power on succeeded**.</span></span>

1. <span data-ttu-id="66633-130">Spusťte následující příkaz pro hledání blízkými zařízeními Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="66633-130">Run the following command to scan for nearby Bluetooth devices:</span></span>

    ```bash
    scan on
    ```

1. <span data-ttu-id="66633-131">Stisknutím tlačítka napájení na SensorTag zjistitelnost.</span><span class="sxs-lookup"><span data-stu-id="66633-131">Press the power button on the SensorTag to make it discoverable.</span></span> <span data-ttu-id="66633-132">Zelená DIODU bliká.</span><span class="sxs-lookup"><span data-stu-id="66633-132">The green LED flashes.</span></span>

1. <span data-ttu-id="66633-133">Když se zobrazí zpráva v prostředí, že je řadič zjistí SensorTag, poznamenejte adresa MAC zařízení.</span><span class="sxs-lookup"><span data-stu-id="66633-133">When you see a message in the shell that the controller has discovered the SensorTag, make a note of the MAC address of the device.</span></span> <span data-ttu-id="66633-134">Adresa MAC vypadá jako **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="66633-134">The MAC address looks like **A0:E6:F8:B5:F6:00**.</span></span> <span data-ttu-id="66633-135">Adresa MAC později v tomto kurzu musíte při konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="66633-135">You need the MAC address later in the tutorial when you configure the gateway.</span></span>

1. <span data-ttu-id="66633-136">Spusťte následující příkaz k vypnutí možnosti prohledávání Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="66633-136">Run the following command to turn off Bluetooth scanning:</span></span>

    ```bash
    scan off
    ```

1. <span data-ttu-id="66633-137">Spusťte následující příkaz pro ověření, zda se můžete připojit k zařízení SensorTag:</span><span class="sxs-lookup"><span data-stu-id="66633-137">Run the following command to verify that you can connect to the SensorTag device:</span></span>

    ```bash
    connect <SensorTag MAC address>
    ```

    <span data-ttu-id="66633-138">Pokud se úspěšně připojit, prostředí zobrazuje zpráva **úspěšné připojení** a zobrazí informace o zařízení SensorTag.</span><span class="sxs-lookup"><span data-stu-id="66633-138">If you connect successfully, the shell shows the message **Connection successful** and prints information about the SensorTag device.</span></span> <span data-ttu-id="66633-139">Pokud se nemůžete připojit, zkontrolujte, že že sensortag je pořád zapnutý.</span><span class="sxs-lookup"><span data-stu-id="66633-139">If you cannot connect, check the SensorTag is still powered on.</span></span>

1. <span data-ttu-id="66633-140">Teď můžete odpojit od SensorTag a opusťte prostředí Bluetooth spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="66633-140">You can now disconnect from the SensorTag and exit the Bluetooth shell by running the following commands:</span></span>

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a><span data-ttu-id="66633-141">Vytvořit vlastní modul IoT Edge</span><span class="sxs-lookup"><span data-stu-id="66633-141">Build the custom IoT Edge module</span></span>

<span data-ttu-id="66633-142">Nyní můžete vytvořit vlastní modul IoT okraj, který umožňuje brána k odesílání zpráv do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-142">You can now build the custom IoT Edge module that enables the gateway to send messages to the remote monitoring solution.</span></span> <span data-ttu-id="66633-143">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="66633-143">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="66633-144">Zdrojový kód pro vlastní moduly IoT Edge stáhněte z Githubu, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="66633-144">Download the source code for the custom IoT Edge modules from GitHub using the following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="66633-145">Vytvořit vlastní modul IoT Edge, pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="66633-145">Build the custom IoT Edge module using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="66633-146">Skript sestavení umístí vlastní modul IoT Edge libsensor2remotemonitoring.so ve složce sestavení.</span><span class="sxs-lookup"><span data-stu-id="66633-146">The build script places the libsensor2remotemonitoring.so custom IoT Edge module in the build folder.</span></span>

## <a name="configure-and-run-the-iot-edge-gateway"></a><span data-ttu-id="66633-147">Nakonfigurujte a spusťte IoT hraniční brána</span><span class="sxs-lookup"><span data-stu-id="66633-147">Configure and run the IoT Edge gateway</span></span>

<span data-ttu-id="66633-148">Teď můžete konfigurovat IoT hraniční brána k odesílání telemetrie ze zařízení SensorTag na řídicím panelu vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-148">You can now configure the IoT Edge gateway to send telemetry from your SensorTag device to your remote monitoring dashboard.</span></span> <span data-ttu-id="66633-149">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="66633-149">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="66633-150">V tomto kurzu použijete standardní `vi` textového editoru na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="66633-150">In this tutorial, you use the standard `vi` text editor on the Intel NUC.</span></span> <span data-ttu-id="66633-151">Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - vi Editor kurzu] [ lnk-vi-tutorial] Seznamte se s tohoto editoru.</span><span class="sxs-lookup"><span data-stu-id="66633-151">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - The vi Editor Tutorial][lnk-vi-tutorial] to familiarize yourself with this editor.</span></span> <span data-ttu-id="66633-152">Alternativně můžete nainstalovat více uživatelsky přívětivý [nano](https://www.nano-editor.org/) editor pomocí příkazu `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="66633-152">Alternatively, you can install the more user-friendly [nano](https://www.nano-editor.org/) editor using the command `smart install nano -y`.</span></span>

<span data-ttu-id="66633-153">Otevřete konfigurační soubor ukázka v **vi** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="66633-153">Open the sample configuration file in the **vi** editor using the following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

<span data-ttu-id="66633-154">V konfiguraci modulu IoTHub vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="66633-154">Locate the following lines in the configuration for the IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="66633-155">Nahraďte zástupný symbol hodnoty s informacemi IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="66633-155">Replace the placeholder values with the IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="66633-156">Hodnota IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota IoTSuffix je obvykle **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="66633-156">The value for IoTHubName looks like **yourrmsolution37e08**, and the value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="66633-157">V konfiguraci pro mapování modulu vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="66633-157">Locate the following lines in the configuration for the mapping module:</span></span>

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="66633-158">Nahraďte **macAddress** zástupný symbol adresu MAC vaší SensorTag jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="66633-158">Replace the **macAddress** placeholder with the MAC address of your SensorTag you noted previously.</span></span> <span data-ttu-id="66633-159">Nahraďte **deviceID** a **deviceKey** zástupné symboly pomocí ID a klíče pro dvě zařízení, které jste vytvořili dříve v řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-159">Replace the **deviceID** and **deviceKey** placeholders with the IDs and keys for the two devices you created in the remote monitoring solution previously.</span></span>

<span data-ttu-id="66633-160">V konfiguraci modulu SensorTag vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="66633-160">Locate the following lines in the configuration for the SensorTag module:</span></span>

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

<span data-ttu-id="66633-161">Nahraďte **zařízení\_mac\_adresu** zástupný symbol adresu MAC vaší SensorTag jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="66633-161">Replace the **device\_mac\_address** placeholder  with the MAC address of your SensorTag you noted previously.</span></span>

<span data-ttu-id="66633-162">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="66633-162">Save your changes.</span></span>

<span data-ttu-id="66633-163">Teď můžete spustit bránu pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="66633-163">You can now run the gateway using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

<span data-ttu-id="66633-164">IoT hraniční brána se spouští v Intel NUC a odesílá telemetrická data z SensorTag do řešení vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="66633-164">The IoT Edge gateway starts on the Intel NUC and sends telemetry from the SensorTag to the remote monitoring solution:</span></span>

![IoT hraniční brána odesílá telemetrii z SensorTag][img-telemetry]

<span data-ttu-id="66633-166">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="66633-166">Press **Ctrl-C** to exit the program at any time.</span></span>

## <a name="view-the-telemetry"></a><span data-ttu-id="66633-167">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="66633-167">View the telemetry</span></span>

<span data-ttu-id="66633-168">Brána teď odesílá telemetrická data ze zařízení SensorTag do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="66633-168">The gateway is now sending telemetry from the SensorTag device to the remote monitoring solution.</span></span> <span data-ttu-id="66633-169">Můžete zobrazit telemetrii na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="66633-169">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="66633-170">Můžete také odesílat příkazy na vaše zařízení SensorTag prostřednictvím brány na řídicím panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="66633-170">You can also send commands to your SensorTag device through the gateway from the solution dashboard.</span></span>

- <span data-ttu-id="66633-171">Přejděte na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="66633-171">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="66633-172">Vyberte zařízení, které jste nakonfigurovali v brány, kterou představuje SensorTag v **zařízení do zobrazení** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="66633-172">Select the device you configured in the gateway that represents the SensorTag in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="66633-173">Telemetrická data ze zařízení SensorTag zobrazí na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="66633-173">The telemetry from the SensorTag device displays on the dashboard.</span></span>

![Zobrazení telemetrie z zařízení SensorTag][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="66633-175">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="66633-175">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="66633-176">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="66633-176">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="66633-177">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="66633-177">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="66633-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66633-178">Next steps</span></span>

<span data-ttu-id="66633-179">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="66633-179">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started