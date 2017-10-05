---
title: "Připojení brány Azure IoT Suite pomocí Intel NUC | Microsoft Docs"
description: "Použijte Kit komerční brány Microsoft IoT a předkonfigurované řešení vzdáleného monitorování. Použijte k připojení k řešení vzdáleného monitorování, odeslání simulovanou telemetrii do cloudu a reagovat na metody vyvolané z řídicího panelu řešení Azure IoT hraniční bránu."
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
ms.openlocfilehash: 9ed57d3c23e2adbd42c054f33c8ed46e3d6c9792
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a><span data-ttu-id="db5c7-104">Připojte bránu Azure IoT Edge pro předkonfigurované řešení vzdáleného monitorování a odeslat simulovanou telemetrii</span><span class="sxs-lookup"><span data-stu-id="db5c7-104">Connect your Azure IoT Edge gateway to the remote monitoring preconfigured solution and send simulated telemetry</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="db5c7-105">V tomto kurzu se dozvíte, jak používat Azure IoT Edge k simulaci teploty a vlhkosti data k odeslání do předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-105">This tutorial shows you how to use Azure IoT Edge to simulate temperature and humidity data to send to the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="db5c7-106">Tento kurz používá:</span><span class="sxs-lookup"><span data-stu-id="db5c7-106">The tutorial uses:</span></span>

- <span data-ttu-id="db5c7-107">Azure IoT okraj implementovat ukázka brány.</span><span class="sxs-lookup"><span data-stu-id="db5c7-107">Azure IoT Edge to implement a sample gateway.</span></span>
- <span data-ttu-id="db5c7-108">Sadě IoT Suite předkonfigurované řešení vzdáleného monitorování jako cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="db5c7-108">The IoT Suite remote monitoring preconfigured solution as the cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="db5c7-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="db5c7-109">Overview</span></span>

<span data-ttu-id="db5c7-110">V tomto kurzu je provést následující kroky:</span><span class="sxs-lookup"><span data-stu-id="db5c7-110">In this tutorial, you complete the following steps:</span></span>

- <span data-ttu-id="db5c7-111">Nasaďte instanci předkonfigurovaného řešení vzdáleného monitorování k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="db5c7-111">Deploy an instance of the remote monitoring preconfigured solution to your Azure subscription.</span></span> <span data-ttu-id="db5c7-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="db5c7-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="db5c7-113">Nastavte vaše zařízení brány Intel NUC ke komunikaci s vaším počítačem a řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-113">Set up your Intel NUC gateway device to communicate with your computer and the remote monitoring solution.</span></span>
- <span data-ttu-id="db5c7-114">Nakonfigurujte IoT hraniční brána k odesílání simulovanou telemetrii, která můžete zobrazit na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="db5c7-114">Configure the IoT Edge gateway to send simulated telemetry that you can view on the solution dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="db5c7-115">Řešení vzdáleného monitorování zřídí sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="db5c7-115">The remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="db5c7-116">Nasazení odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="db5c7-116">The deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="db5c7-117">Aby se zabránilo zbytečným využití platformy Azure poplatky, odstraňte instanci předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="db5c7-117">To avoid unnecessary Azure consumption charges, delete your instance of the preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="db5c7-118">Pokud budete znovu potřebovat předkonfigurované řešení, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="db5c7-118">If you need the preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="db5c7-119">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="db5c7-119">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

<span data-ttu-id="db5c7-120">Opakujte předchozí kroky pro přidání druhé zařízení pomocí ID zařízení, jako třeba **device02**.</span><span class="sxs-lookup"><span data-stu-id="db5c7-120">Repeat the previous steps to add a second device using a Device ID such as **device02**.</span></span> <span data-ttu-id="db5c7-121">Ukázka odešle data ze dvou simulované zařízení v bráně řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-121">The sample sends data from two simulated devices in the gateway to the remote monitoring solution.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a><span data-ttu-id="db5c7-122">Vytvořit vlastní modul IoT Edge</span><span class="sxs-lookup"><span data-stu-id="db5c7-122">Build the custom IoT Edge module</span></span>

<span data-ttu-id="db5c7-123">Nyní můžete vytvořit vlastní modul IoT okraj, který umožňuje brána k odesílání zpráv do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-123">You can now build the custom IoT Edge module that enables the gateway to send messages to the remote monitoring solution.</span></span> <span data-ttu-id="db5c7-124">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="db5c7-124">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="db5c7-125">Zdrojový kód pro vlastní moduly IoT Edge stáhněte z Githubu, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="db5c7-125">Download the source code for the custom IoT Edge modules from GitHub using the following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="db5c7-126">Vytvořit vlastní modul IoT Edge, pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="db5c7-126">Build the custom IoT Edge module using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="db5c7-127">Skript sestavení umístí vlastní modul IoT Edge libsimulator.so ve složce sestavení.</span><span class="sxs-lookup"><span data-stu-id="db5c7-127">The build script places the libsimulator.so custom IoT Edge module in the build folder.</span></span>

## <a name="configure-and-run-the-iot-edge-gateway"></a><span data-ttu-id="db5c7-128">Nakonfigurujte a spusťte IoT hraniční brána</span><span class="sxs-lookup"><span data-stu-id="db5c7-128">Configure and run the IoT Edge gateway</span></span>

<span data-ttu-id="db5c7-129">Teď můžete konfigurovat IoT hraniční brána k odesílání simulovanou telemetrii do řídicího panelu vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-129">You can now configure the IoT Edge gateway to send simulated telemetry to your remote monitoring dashboard.</span></span> <span data-ttu-id="db5c7-130">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="db5c7-130">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="db5c7-131">V tomto kurzu použijete standardní `vi` textového editoru na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="db5c7-131">In this tutorial, you use the standard `vi` text editor on the Intel NUC.</span></span> <span data-ttu-id="db5c7-132">Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - vi Editor kurzu] [ lnk-vi-tutorial] Seznamte se s tohoto editoru.</span><span class="sxs-lookup"><span data-stu-id="db5c7-132">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - The vi Editor Tutorial][lnk-vi-tutorial] to familiarize yourself with this editor.</span></span> <span data-ttu-id="db5c7-133">Alternativně můžete nainstalovat více uživatelsky přívětivý [nano](https://www.nano-editor.org/) editor pomocí příkazu `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="db5c7-133">Alternatively, you can install the more user-friendly [nano](https://www.nano-editor.org/) editor using the command `smart install nano -y`.</span></span>

<span data-ttu-id="db5c7-134">Otevřete konfigurační soubor ukázka v **vi** editor pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="db5c7-134">Open the sample configuration file in the **vi** editor using the following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

<span data-ttu-id="db5c7-135">V konfiguraci modulu IoTHub vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="db5c7-135">Locate the following lines in the configuration for the IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="db5c7-136">Nahraďte zástupný symbol hodnoty s informacemi IoT Hub vytvořili a uložili na začátku tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="db5c7-136">Replace the placeholder values with the IoT Hub information you created and saved at the start of this tutorial.</span></span> <span data-ttu-id="db5c7-137">Hodnota IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota IoTSuffix je obvykle **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="db5c7-137">The value for IoTHubName looks like **yourrmsolution37e08**, and the value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="db5c7-138">V konfiguraci pro mapování modulu vyhledejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="db5c7-138">Locate the following lines in the configuration for the mapping module:</span></span>

```json
args": [
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="db5c7-139">Nahraďte **deviceID** a **deviceKey** zástupné symboly pomocí ID a klíče pro dvě zařízení, které jste vytvořili dříve v řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-139">Replace the **deviceID** and **deviceKey** placeholders with the IDs and keys for the two devices you created in the remote monitoring solution previously.</span></span>

<span data-ttu-id="db5c7-140">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="db5c7-140">Save your changes.</span></span>

<span data-ttu-id="db5c7-141">Teď můžete spustit IoT hraniční bránu, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="db5c7-141">You can now run the IoT Edge gateway using the following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

<span data-ttu-id="db5c7-142">Brána se spouští v Intel NUC a odešle simulovanou telemetrii do řešení vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="db5c7-142">The gateway starts on the Intel NUC and sends simulated telemetry to the remote monitoring solution:</span></span>

![IoT hraniční brána generuje simulovanou telemetrii][img-simulated telemetry]

<span data-ttu-id="db5c7-144">Stiskněte klávesu **Ctrl-C** ukončete program kdykoli.</span><span class="sxs-lookup"><span data-stu-id="db5c7-144">Press **Ctrl-C** to exit the program at any time.</span></span>

## <a name="view-the-telemetry"></a><span data-ttu-id="db5c7-145">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="db5c7-145">View the telemetry</span></span>

<span data-ttu-id="db5c7-146">IoT hraniční brána teď odesílá simulovanou telemetrii do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="db5c7-146">The IoT Edge gateway is now sending simulated telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="db5c7-147">Můžete zobrazit telemetrii na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="db5c7-147">You can view the telemetry on the solution dashboard.</span></span>

- <span data-ttu-id="db5c7-148">Přejděte na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="db5c7-148">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="db5c7-149">Vyberte jednu ze dvou zařízení, které jste nakonfigurovali v bráně v **zařízení do zobrazení** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="db5c7-149">Select one of the two devices you configured in the gateway in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="db5c7-150">Telemetrická data ze zařízení brány se zobrazí na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="db5c7-150">The telemetry from the gateway devices displays on the dashboard.</span></span>

![Zobrazení telemetrie z brány simulované zařízení.][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="db5c7-152">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="db5c7-152">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="db5c7-153">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="db5c7-153">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="db5c7-154">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="db5c7-154">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db5c7-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db5c7-155">Next steps</span></span>

<span data-ttu-id="db5c7-156">Přejděte [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="db5c7-156">Visit the [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started