---
title: "aaaConnect tooAzure brány pomocí Intel NUC IoT Suite | Microsoft Docs"
description: "Použijte hello Kit komerční brány Microsoft IoT a hello předkonfigurovaného řešení vzdáleného monitorování. Použít hello Azure IoT hraniční brány tooenable řešení vzdáleného sledování SensorTag toohello tooconnect zařízení, odesílání telemetrie toohello cloudu a reagovat toomethods volat z řídicí panel řešení hello."
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
ms.openlocfilehash: 6f98ee3c1e2311a8644da9d72d40e671e7cbcf00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-toohello-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a><span data-ttu-id="1a1a2-104">Připojení vaší brány Azure IoT Edge toohello předkonfigurovanému řešení vzdáleného monitorování a odesílat telemetrická data z SensorTag</span><span class="sxs-lookup"><span data-stu-id="1a1a2-104">Connect your Azure IoT Edge gateway toohello remote monitoring preconfigured solution and send telemetry from a SensorTag</span></span>

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

<span data-ttu-id="1a1a2-105">Tento kurz ukazuje, jak toouse data teploty a vlhkosti toosend Azure IoT Edge z SensorTag toohello vzdálené monitorování zařízení předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-105">This tutorial shows you how toouse Azure IoT Edge toosend temperature and humidity data from SensorTag device toohello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="1a1a2-106">Hello SensorTag připojí toohello Intel NUC bránu pomocí Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-106">hello SensorTag connects toohello Intel NUC gateway using Bluetooth.</span></span> <span data-ttu-id="1a1a2-107">kurz Hello používá:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-107">hello tutorial uses:</span></span>

- <span data-ttu-id="1a1a2-108">Azure IoT Edge tooimplement ukázka brány.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-108">Azure IoT Edge tooimplement a sample gateway.</span></span>
- <span data-ttu-id="1a1a2-109">vzdálené monitorování Hello IoT Suite předkonfigurované řešení jako hello cloudové back-end.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-109">hello IoT Suite remote monitoring preconfigured solution as hello cloud-based back end.</span></span>

## <a name="overview"></a><span data-ttu-id="1a1a2-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="1a1a2-110">Overview</span></span>

<span data-ttu-id="1a1a2-111">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-111">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="1a1a2-112">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-112">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="1a1a2-113">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-113">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="1a1a2-114">Nastavte vaše Intel NUC brány zařízení toocommunicate s počítačem a hello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-114">Set up your Intel NUC gateway device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="1a1a2-115">Nastavit Intel NUC brány tooreceive telemetrie ze zařízení SensorTag a odešlete ji toohello vzdálené řídicí panel monitorování.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-115">Set up your Intel NUC gateway tooreceive telemetry from a SensorTag device and send it toohello remote monitoring dashboard.</span></span>

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

<span data-ttu-id="1a1a2-116">[Texas Instruments SensorTag lit][lnk-sensortag].</span><span class="sxs-lookup"><span data-stu-id="1a1a2-116">[Texas Instruments BLE SensorTag][lnk-sensortag].</span></span> <span data-ttu-id="1a1a2-117">V tomto kurzu načte ze zařízení SensorTag hello telemetrická data přes Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-117">This tutorial retrieves telemetry data over Bluetooth from hello SensorTag device.</span></span>

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> <span data-ttu-id="1a1a2-118">Hello vzdálené monitorování zřídí řešení sadu služeb Azure ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-118">hello remote monitoring solution provisions a set of Azure services in your Azure subscription.</span></span> <span data-ttu-id="1a1a2-119">nasazení Hello odráží architektura skutečné enterprise.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-119">hello deployment reflects a real enterprise architecture.</span></span> <span data-ttu-id="1a1a2-120">tooavoid nepotřebné využití platformy Azure poplatky, odstraňte instanci hello předkonfigurované řešení na azureiotsuite.com po dokončení s ním.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-120">tooavoid unnecessary Azure consumption charges, delete your instance of hello preconfigured solution at azureiotsuite.com when you have finished with it.</span></span> <span data-ttu-id="1a1a2-121">Pokud třeba hello předkonfigurované řešení znovu, můžete ho snadno obnovit.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-121">If you need hello preconfigured solution again, you can easily recreate it.</span></span> <span data-ttu-id="1a1a2-122">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="1a1a2-122">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span>

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a><span data-ttu-id="1a1a2-123">Konfigurace připojení k Bluetooth</span><span class="sxs-lookup"><span data-stu-id="1a1a2-123">Configure Bluetooth connectivity</span></span>

<span data-ttu-id="1a1a2-124">Konfigurovat Bluetooth v zařízení SensorTag tooconnect pro hello Intel NUC tooenable hello a odesílat telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-124">Configure Bluetooth on hello Intel NUC tooenable hello SensorTag device tooconnect and send telemetry.</span></span>

### <a name="find-hello-mac-address-of-hello-sensortag"></a><span data-ttu-id="1a1a2-125">Zjištění adresy MAC hello hello SensorTag</span><span class="sxs-lookup"><span data-stu-id="1a1a2-125">Find hello MAC address of hello SensorTag</span></span>

1. <span data-ttu-id="1a1a2-126">V prostředí hello hello Intel NUC spusťte následující příkaz toounblock hello Bluetooth služby hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-126">In hello shell on hello Intel NUC, run hello following command toounblock hello Bluetooth service:</span></span>

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. <span data-ttu-id="1a1a2-127">Hello spusťte následující příkazy toostart hello Bluetooth službu na hello Intel NUC a zadejte hello Bluetooth prostředí:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-127">Run hello following commands toostart hello Bluetooth service on hello Intel NUC and enter hello Bluetooth shell:</span></span>

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="1a1a2-128">Spusťte následující příkaz toopower na řadiči Bluetooth hello hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-128">Run hello following command toopower on hello Bluetooth controller:</span></span>

    ```bash
    power on
    ```

    <span data-ttu-id="1a1a2-129">Pokud je řadič hello na, zobrazí se zpráva **změna power v úspěšné**.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-129">When hello controller is on, you see a message **Changing power on succeeded**.</span></span>

1. <span data-ttu-id="1a1a2-130">Spusťte následující příkaz tooscan pro nedaleko zařízeními Bluetooth hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-130">Run hello following command tooscan for nearby Bluetooth devices:</span></span>

    ```bash
    scan on
    ```

1. <span data-ttu-id="1a1a2-131">Power hello stiskněte tlačítko na hello SensorTag toomake je zjistitelný.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-131">Press hello power button on hello SensorTag toomake it discoverable.</span></span> <span data-ttu-id="1a1a2-132">Hello zelená DIODU bliká.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-132">hello green LED flashes.</span></span>

1. <span data-ttu-id="1a1a2-133">Když se zobrazí zpráva v prostředí hello kontroleru hello zjistila hello SensorTag, poznamenejte si hello adresa MAC zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-133">When you see a message in hello shell that hello controller has discovered hello SensorTag, make a note of hello MAC address of hello device.</span></span> <span data-ttu-id="1a1a2-134">Hello adresa MAC vypadá jako **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-134">hello MAC address looks like **A0:E6:F8:B5:F6:00**.</span></span> <span data-ttu-id="1a1a2-135">Adresa MAC hello později v kurzu hello musíte při konfiguraci brány hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-135">You need hello MAC address later in hello tutorial when you configure hello gateway.</span></span>

1. <span data-ttu-id="1a1a2-136">Spusťte následující příkaz tooturn vypnout kontrolu Bluetooth hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-136">Run hello following command tooturn off Bluetooth scanning:</span></span>

    ```bash
    scan off
    ```

1. <span data-ttu-id="1a1a2-137">Spusťte následující příkaz tooverify, že se můžete připojit zařízení SensorTag toohello hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-137">Run hello following command tooverify that you can connect toohello SensorTag device:</span></span>

    ```bash
    connect <SensorTag MAC address>
    ```

    <span data-ttu-id="1a1a2-138">Je-li úspěšně připojit hello prostředí ukazuje uvítací zprávu **úspěšné připojení** a zobrazí informace o zařízení SensorTag hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-138">If you connect successfully, hello shell shows hello message **Connection successful** and prints information about hello SensorTag device.</span></span> <span data-ttu-id="1a1a2-139">Pokud se nemůžete připojit, zkontrolujte hello SensorTag je pořád zapnutý.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-139">If you cannot connect, check hello SensorTag is still powered on.</span></span>

1. <span data-ttu-id="1a1a2-140">Teď můžete odpojit od hello SensorTag a ukončete prostředí Bluetooth hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-140">You can now disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-hello-custom-iot-edge-module"></a><span data-ttu-id="1a1a2-141">Vytvořit vlastní modul IoT Edge hello</span><span class="sxs-lookup"><span data-stu-id="1a1a2-141">Build hello custom IoT Edge module</span></span>

<span data-ttu-id="1a1a2-142">Teď můžete sestavit hello vlastní okraje IoT modul, který umožňuje hello brány toosend zprávy toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-142">You can now build hello custom IoT Edge module that enables hello gateway toosend messages toohello remote monitoring solution.</span></span> <span data-ttu-id="1a1a2-143">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="1a1a2-143">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

<span data-ttu-id="1a1a2-144">Hello zdrojového kódu pro vlastní moduly IoT Edge hello stáhněte z webu GitHub pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-144">Download hello source code for hello custom IoT Edge modules from GitHub using hello following commands:</span></span>

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

<span data-ttu-id="1a1a2-145">Vytvořit vlastní modul IoT Edge hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-145">Build hello custom IoT Edge module using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

<span data-ttu-id="1a1a2-146">skriptu buildu Hello umístí do složky sestavení hello hello libsensor2remotemonitoring.so vlastní okraje IoT modul.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-146">hello build script places hello libsensor2remotemonitoring.so custom IoT Edge module in hello build folder.</span></span>

## <a name="configure-and-run-hello-iot-edge-gateway"></a><span data-ttu-id="1a1a2-147">Nakonfigurujte a spusťte hello IoT hraniční brána</span><span class="sxs-lookup"><span data-stu-id="1a1a2-147">Configure and run hello IoT Edge gateway</span></span>

<span data-ttu-id="1a1a2-148">Teď můžete konfigurovat hello IoT hraniční brány toosend telemetrie z vaší tooyour zařízení SensorTag pro vzdálené monitorování řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-148">You can now configure hello IoT Edge gateway toosend telemetry from your SensorTag device tooyour remote monitoring dashboard.</span></span> <span data-ttu-id="1a1a2-149">Další informace o konfiguraci brány a IoT Edge moduly, naleznete v části [koncepty Azure IoT Edge][lnk-gateway-concepts].</span><span class="sxs-lookup"><span data-stu-id="1a1a2-149">For more information about configuring a gateway and IoT Edge modules, see [Azure IoT Edge concepts][lnk-gateway-concepts].</span></span>

> [!TIP]
> <span data-ttu-id="1a1a2-150">V tomto kurzu použijete standardní hello `vi` textového editoru na hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-150">In this tutorial, you use hello standard `vi` text editor on hello Intel NUC.</span></span> <span data-ttu-id="1a1a2-151">Pokud jste nepoužili `vi` před, musíte provést úvodní kurz, jako například [Unix - hello vi Editor kurzu] [ lnk-vi-tutorial] toofamiliarize sami pomocí tohoto editoru.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-151">If you have not used `vi` before, you should complete an introductory tutorial, such as [Unix - hello vi Editor Tutorial][lnk-vi-tutorial] toofamiliarize yourself with this editor.</span></span> <span data-ttu-id="1a1a2-152">Alternativně můžete nainstalovat hello přívětivější [nano](https://www.nano-editor.org/) editor pomocí příkazu hello `smart install nano -y`.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-152">Alternatively, you can install hello more user-friendly [nano](https://www.nano-editor.org/) editor using hello command `smart install nano -y`.</span></span>

<span data-ttu-id="1a1a2-153">Otevřete hello vzorový konfigurační soubor v hello **vi** editor pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-153">Open hello sample configuration file in hello **vi** editor using hello following command:</span></span>

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

<span data-ttu-id="1a1a2-154">Vyhledejte následující řádky v hello konfiguraci pro modul IoTHub hello hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-154">Locate hello following lines in hello configuration for hello IoTHub module:</span></span>

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

<span data-ttu-id="1a1a2-155">Nahraďte zástupný symbol hello hodnoty s hello informace služby IoT Hub vytvořili a uložili v hello spuštění tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-155">Replace hello placeholder values with hello IoT Hub information you created and saved at hello start of this tutorial.</span></span> <span data-ttu-id="1a1a2-156">Hodnota Hello IoTHubName vypadá jako **yourrmsolution37e08**, a hodnota hello IoTSuffix je obvykle **azure devices.net**.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-156">hello value for IoTHubName looks like **yourrmsolution37e08**, and hello value for IoTSuffix is typically **azure-devices.net**.</span></span>

<span data-ttu-id="1a1a2-157">Vyhledejte následující řádky v konfiguraci hello hello mapování modulu hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-157">Locate hello following lines in hello configuration for hello mapping module:</span></span>

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

<span data-ttu-id="1a1a2-158">Nahraďte hello **macAddress** zástupný symbol hello adresu MAC vaší SensorTag jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-158">Replace hello **macAddress** placeholder with hello MAC address of your SensorTag you noted previously.</span></span> <span data-ttu-id="1a1a2-159">Nahraďte hello **deviceID** a **deviceKey** zástupné symboly hello ID a klíče pro hello dva zařízení, které jste vytvořili dříve v hello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-159">Replace hello **deviceID** and **deviceKey** placeholders with hello IDs and keys for hello two devices you created in hello remote monitoring solution previously.</span></span>

<span data-ttu-id="1a1a2-160">Vyhledejte následující řádky v hello konfiguraci pro modul SensorTag hello hello:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-160">Locate hello following lines in hello configuration for hello SensorTag module:</span></span>

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

<span data-ttu-id="1a1a2-161">Nahraďte hello **zařízení\_mac\_adresu** zástupný symbol hello adresu MAC vaší SensorTag jste si poznamenali dříve.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-161">Replace hello **device\_mac\_address** placeholder  with hello MAC address of your SensorTag you noted previously.</span></span>

<span data-ttu-id="1a1a2-162">Uložte provedené změny.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-162">Save your changes.</span></span>

<span data-ttu-id="1a1a2-163">Teď můžete spustit bránu hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-163">You can now run hello gateway using hello following commands:</span></span>

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

<span data-ttu-id="1a1a2-164">Hello IoT hraniční brána se spouští v hello Intel NUC a odesílá telemetrii z hello SensorTag toohello řešení vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="1a1a2-164">hello IoT Edge gateway starts on hello Intel NUC and sends telemetry from hello SensorTag toohello remote monitoring solution:</span></span>

![IoT hraniční brána odesílá telemetrii z hello SensorTag][img-telemetry]

<span data-ttu-id="1a1a2-166">Stiskněte klávesu **Ctrl-C** programu hello tooexit kdykoli.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-166">Press **Ctrl-C** tooexit hello program at any time.</span></span>

## <a name="view-hello-telemetry"></a><span data-ttu-id="1a1a2-167">Zobrazení telemetrie hello</span><span class="sxs-lookup"><span data-stu-id="1a1a2-167">View hello telemetry</span></span>

<span data-ttu-id="1a1a2-168">Hello brána teď odesílá telemetrii z hello SensorTag zařízení toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-168">hello gateway is now sending telemetry from hello SensorTag device toohello remote monitoring solution.</span></span> <span data-ttu-id="1a1a2-169">Můžete zobrazit telemetrii hello na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-169">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="1a1a2-170">Můžete také odeslat příkazy tooyour SensorTag zařízení prostřednictvím brány hello z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-170">You can also send commands tooyour SensorTag device through hello gateway from hello solution dashboard.</span></span>

- <span data-ttu-id="1a1a2-171">Přejděte toohello řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-171">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="1a1a2-172">Vyberte hello zařízení, které jste nakonfigurovali v hello brány, který představuje hello SensorTag v hello **zařízení tooView** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-172">Select hello device you configured in hello gateway that represents hello SensorTag in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="1a1a2-173">Hello telemetrie ze zařízení SensorTag hello se zobrazí na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-173">hello telemetry from hello SensorTag device displays on hello dashboard.</span></span>

![Zobrazení telemetrie z zařízení SensorTag hello][img-telemetry-display]

> [!WARNING]
> <span data-ttu-id="1a1a2-175">Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-175">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="1a1a2-176">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="1a1a2-176">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="1a1a2-177">Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-177">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1a1a2-178">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a1a2-178">Next steps</span></span>

<span data-ttu-id="1a1a2-179">Navštivte hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) Další ukázky a dokumentace na Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="1a1a2-179">Visit hello [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/) for more samples and documentation on Azure IoT.</span></span>

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started