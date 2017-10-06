---
title: "aaaConnect tooAzure brány pomocí Intel NUC IoT Suite | Microsoft Docs"
description: "Použijte hello Kit komerční brány Microsoft IoT a hello předkonfigurovaného řešení vzdáleného monitorování. Použití hello Azure IoT hraniční brány tooconnect toohello řešení vzdáleného monitorování, odešle simulovanou telemetrii toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 46b545fc21b054191c8f78ace20fc628f839a819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a><span data-ttu-id="85b73-104">Připojení vaší brány Azure IoT Edge toohello předkonfigurovanému řešení vzdáleného monitorování a odeslat simulovanou telemetrii</span><span class="sxs-lookup"><span data-stu-id="85b73-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send simulated telemetry</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="85b73-105">Tento kurz ukazuje, jak toouse Azure IoT Edge toosimulate teploty a vlhkosti data toosend toohello vzdálené monitorování předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="85b73-105">This tutorial shows you how toouse Azure IoT Edge toosimulate temperature and humidity data toosend toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="85b73-106">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="85b73-106">hello tutorial uses:</span></span>

- <span data-ttu-id="85b73-107">Azure IoT Edge tooimplement ukázka brány.</span><span class="sxs-lookup"><span data-stu-id="85b73-107">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="85b73-108">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="85b73-108">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="85b73-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="85b73-109">Overview</span></span>

<span data-ttu-id="85b73-110">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="85b73-110">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="85b73-111">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="85b73-111">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="85b73-112">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="85b73-112">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="85b73-113">Nastavte vaše Intel NUC brány zařízení toocommunicate s počítačem a hello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="85b73-113">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="85b73-114">Nakonfigurujte hello IoT hraniční brány toosend simulated telemetrii, kterou můžete zobrazit na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="85b73-114">Configure hello IoT Edge gateway toosend simulated telemetry that you can view on hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="85b73-115">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="85b73-115">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="85b73-116">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="85b73-116">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="85b73-117">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="85b73-117">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="85b73-118">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="85b73-118">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="85b73-119">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="85b73-119">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

<span data-ttu-id="85b73-120">Opakujte předchozí kroky tooadd hello druhé zařízení pomocí ID zařízení, jako třeba **device02**.</span><span class="sxs-lookup"><span data-stu-id="85b73-120">Repeat hello previous steps tooadd a second device using a Device ID such as **device02**.</span></span> <span data-ttu-id="85b73-121">Ukázka Hello odešle data ze dvou simulované zařízení v hello brány toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="85b73-121">hello sample sends data from two simulated devices in hello gateway toohello remote monitoring solution.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="85b73-122">Vytvořit vlastní modul IoT Edge hello</span><span class="sxs-lookup"><span data-stu-id="85b73-122">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="85b73-123">Teď můžete sestavit hello vlastní okraje IoT modul, který umožňuje hello brány toosend zprávy toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="85b73-123">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="85b73-124">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="85b73-124">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="85b73-125">Hello zdrojového kódu pro vlastní moduly IoT Edge hello stáhněte z webu GitHub pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="85b73-125">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="85b73-126">Vytvořit vlastní modul IoT Edge hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="85b73-126">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="85b73-127">skriptu buildu Hello umístí do složky sestavení hello hello libsimulator.so vlastní okraje IoT modul.</span><span class="sxs-lookup"><span data-stu-id="85b73-127">hello build script places hello libsimulator.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="85b73-128">Nakonfigurujte a spusťte hello IoT hraniční brána</span><span class="sxs-lookup"><span data-stu-id="85b73-128">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="85b73-129">Teď můžete konfigurovat hello IoT hraniční brány toosend simulovanou telemetrii tooyour vzdálené monitorování řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="85b73-129">You can now configure hello IoT Edge gateway toosend simulated telemetry tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="85b73-130">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="85b73-130">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="85b73-131">V tomto kurzu použijete standardní hello `vi` textového editoru na hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="85b73-131">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="85b73-132">Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - hello vi Editor kurzu] [ lnk-vi-tutorial] toofamiliarize sami pomocí tohoto editoru.</span><span class="sxs-lookup"><span data-stu-id="85b73-132">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="85b73-133">Alternativně můžete nainstalovat hello přívětivější [nano](https://www.nano-editor.org/) editor pomocí příkazu hello `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="85b73-133">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="85b73-134">Otevřete hello vzorový konfigurační soubor v hello **vi** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="85b73-134">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
```

<span data-ttu-id="85b73-135">Vyhledejte následující řádky v hello konfiguraci pro modul IoTHub hello hello:</span><span class="sxs-lookup"><span data-stu-id="85b73-135">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="85b73-136">Nahraďte zástupný symbol hello hodnoty s hello informace služby IoT Hub vytvořili a uložili v hello spuštění tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="85b73-136">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="85b73-137">Hodnota Hello IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota hello IoTSuffix je obvykle **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="85b73-137">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="85b73-138">Vyhledejte následující řádky v konfiguraci hello hello mapování modulu hello:</span><span class="sxs-lookup"><span data-stu-id="85b73-138">Locate hello following lines in hello configuration for hello mapping module:</span></span>

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

<span data-ttu-id="85b73-139">Nahraďte hello **deviceID** a **deviceKey** zástupné symboly hello ID a klíče pro hello dva zařízení, které jste vytvořili dříve v hello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="85b73-139">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="85b73-140">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="85b73-140">Save your changes.</span></span>

<span data-ttu-id="85b73-141">Teď můžete spustit hello IoT hraniční bránu pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="85b73-141">You can now run hello IoT Edge gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

<span data-ttu-id="85b73-142">Hello brány se spouští v hello Intel NUC a odešle simulovanou telemetrii toohello řešení vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="85b73-142">hello gateway starts on hello Intel NUC and sends simulated telemetry toohello remote monitoring solution:</span></span>

![IoT hraniční brána generuje simulovanou telemetrii][img-simulated telemetry]

<span data-ttu-id="85b73-144">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="85b73-144">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="85b73-145">Zobrazení telemetrie hello</span><span class="sxs-lookup"><span data-stu-id="85b73-145">View hello telemetry</span></span>

<span data-ttu-id="85b73-146">Hello IoT hraniční brána je nyní odesílání simulovanou telemetrii toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="85b73-146">hello IoT Edge gateway is now sending simulated telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="85b73-147">Můžete zobrazit telemetrii hello na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="85b73-147">You can view hello telemetry on hello solution dashboard.</span></span>

- <span data-ttu-id="85b73-148">Přejděte toohello řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="85b73-148">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="85b73-149">Vyberte jednu z hello dva zařízení, které jste nakonfigurovali v hello brány v hello **zařízení tooView** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="85b73-149">Select one of hello two devices you configured in hello gateway in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="85b73-150">Hello telemetrie ze zařízení brány hello se zobrazí na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="85b73-150">hello telemetry from hello gateway devices displays on hello dashboard.</span></span>

![Zobrazení telemetrie z hello simulované zařízení brány][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="85b73-152">Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="85b73-152">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="85b73-153">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="85b73-153">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="85b73-154">Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="85b73-154">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85b73-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="85b73-155">Next steps</span></span>

<span data-ttu-id="85b73-156">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="85b73-156">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-simulated telemetry]: ./media/iot-suite-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started