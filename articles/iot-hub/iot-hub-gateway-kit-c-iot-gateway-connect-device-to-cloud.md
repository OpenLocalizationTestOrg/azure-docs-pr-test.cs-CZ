---
title: "aaaUse IoT brány tooconnect tooAzure zařízení IoT Hub | Microsoft Docs"
description: "Zjistěte, jak cloudové toouse Intel NUC jako tooconnect brány IoT TI SensorTag a odesílání senzor data tooAzure IoT Hub v hello."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Brána IOT připojit toocloud zařízení"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a><span data-ttu-id="f4f28-104">Použít IoT brány tooconnect věcí toohello cloudu - SensorTag tooAzure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="f4f28-104">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>

> [!NOTE]
> <span data-ttu-id="f4f28-105">Než začnete tento kurz, ujistěte se, když jste dokončili [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span><span class="sxs-lookup"><span data-stu-id="f4f28-105">Before you start this tutorial, make sure you’ve completed [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span></span> <span data-ttu-id="f4f28-106">V [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), můžete nastavit zařízení Intel NUC hello jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="f4f28-106">In [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), you set up hello Intel NUC device as an IoT gateway.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f4f28-107">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f4f28-107">What you will learn</span></span>

<span data-ttu-id="f4f28-108">Zjistíte, jak toouse IoT brány tooconnect Texas Instruments SensorTag (CC2650STK) tooAzure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-108">You learn how toouse an IoT gateway tooconnect a Texas Instruments SensorTag (CC2650STK) tooAzure IoT Hub.</span></span> <span data-ttu-id="f4f28-109">Brána IoT Hello odešle teploty a vlhkosti data shromážděná z hello SensorTag tooAzure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-109">hello IoT gateway sends temperature and humidity data collected from hello SensorTag tooAzure IoT Hub.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="f4f28-110">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f4f28-110">What you will do</span></span>

- <span data-ttu-id="f4f28-111">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-111">Create an IoT hub.</span></span>
- <span data-ttu-id="f4f28-112">Registrovat zařízení ve hello IoT hub pro hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="f4f28-112">Register a device in hello IoT hub for hello SensorTag.</span></span>
- <span data-ttu-id="f4f28-113">Povolte hello připojení mezi bránou hello IoT a hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="f4f28-113">Enable hello connection between hello IoT gateway and hello SensorTag.</span></span>
- <span data-ttu-id="f4f28-114">Spusťte zakázat ukázkové aplikace toosend SensorTag data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-114">Run a BLE sample application toosend SensorTag data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f4f28-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f4f28-115">What you need</span></span>

- <span data-ttu-id="f4f28-116">Kurz [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) dokončit, ve kterém můžete nastavit Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="f4f28-116">Tutorial [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) completed in which you set up Intel NUC as an IoT gateway.</span></span>
- * <span data-ttu-id="f4f28-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f4f28-117">An active Azure subscription.</span></span> <span data-ttu-id="f4f28-118">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="f4f28-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
- <span data-ttu-id="f4f28-119">Klient SSH, který běží v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="f4f28-119">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="f4f28-120">V systému Windows se doporučuje puTTY.</span><span class="sxs-lookup"><span data-stu-id="f4f28-120">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="f4f28-121">Linux a systému macOS již obsahují klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="f4f28-121">Linux and macOS already come with an SSH client.</span></span>
- <span data-ttu-id="f4f28-122">Hello IP adresa a hello uživatelské jméno a heslo tooaccess hello brány z klienta SSH hello.</span><span class="sxs-lookup"><span data-stu-id="f4f28-122">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
- <span data-ttu-id="f4f28-123">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="f4f28-123">An Internet connection.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> <span data-ttu-id="f4f28-124">Zde je zaregistrovat toto nové zařízení pro vaše SensorTag</span><span class="sxs-lookup"><span data-stu-id="f4f28-124">Here you register this new device for your SensorTag</span></span>

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a><span data-ttu-id="f4f28-125">Povolit hello připojení mezi bránou hello IoT a hello SensorTag</span><span class="sxs-lookup"><span data-stu-id="f4f28-125">Enable hello connection between hello IoT gateway and hello SensorTag</span></span>

<span data-ttu-id="f4f28-126">V této části provedete hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f4f28-126">In this section, you perform hello following tasks:</span></span>

- <span data-ttu-id="f4f28-127">Získáte adresu MAC hello hello SensorTag pro připojení Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="f4f28-127">Get hello MAC address of hello SensorTag for Bluetooth connection.</span></span>
- <span data-ttu-id="f4f28-128">Navázání připojení Bluetooth z hello IoT brány toohello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="f4f28-128">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag.</span></span>

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a><span data-ttu-id="f4f28-129">Získat adresu MAC hello hello SensorTag pro připojení Bluetooth</span><span class="sxs-lookup"><span data-stu-id="f4f28-129">Get hello MAC address of hello SensorTag for Bluetooth connection</span></span>

1. <span data-ttu-id="f4f28-130">Na hostitelském počítači hello spusťte hello klient SSH a připojit toohello IoT bránu.</span><span class="sxs-lookup"><span data-stu-id="f4f28-130">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="f4f28-131">Odblokování Bluetooth spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-131">Unblock Bluetooth by running hello following command:</span></span>

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. <span data-ttu-id="f4f28-132">Spusťte službu hello Bluetooth na hello IoT brány a zadejte tooconfigure prostředí Bluetooth hello Bluetooth spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="f4f28-132">Start hello Bluetooth service on hello IoT gateway and enter a Bluetooth shell tooconfigure Bluetooth by running hello following commands:</span></span>

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. <span data-ttu-id="f4f28-133">Zapnutí řadiče Bluetooth hello spuštěním hello následující příkaz v hello prostředí Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="f4f28-133">Power on hello Bluetooth controller by running hello following command at hello Bluetooth shell:</span></span>

   ```bash
   power on
   ```

   ![Zapnutí v hello IoT bránu s bluetoothctl hello řadič Bluetooth](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="f4f28-135">Spustí vyhledávání nedaleko zařízeními Bluetooth spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-135">Start scanning for nearby Bluetooth devices by running hello following command:</span></span>

   ```bash
   scan on
   ```

   ![Kontrola nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="f4f28-137">Stiskněte tlačítko na hello SensorTag párování hello.</span><span class="sxs-lookup"><span data-stu-id="f4f28-137">Press hello pairing button on hello SensorTag.</span></span> <span data-ttu-id="f4f28-138">Hello zelená VEDLA na bliká SensorTag hello.</span><span class="sxs-lookup"><span data-stu-id="f4f28-138">hello green LED on hello SensorTag flashes.</span></span>
1. <span data-ttu-id="f4f28-139">V hello Bluetooth prostředí měli byste vidět hello SensorTag je nalezen.</span><span class="sxs-lookup"><span data-stu-id="f4f28-139">At hello Bluetooth shell, you should see hello SensorTag is found.</span></span> <span data-ttu-id="f4f28-140">Poznamenejte si adresu MAC hello SensorTag hello.</span><span class="sxs-lookup"><span data-stu-id="f4f28-140">Make a note of hello MAC address of hello SensorTag.</span></span> <span data-ttu-id="f4f28-141">V tomto příkladu je hello adresu MAC hello SensorTag `24:71:89:C0:7F:82`.</span><span class="sxs-lookup"><span data-stu-id="f4f28-141">In this example, hello MAC address of hello SensorTag is `24:71:89:C0:7F:82`.</span></span>
1. <span data-ttu-id="f4f28-142">Vypněte kontrolu hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-142">Turn off hello scan by running hello following command:</span></span>

   ```bash
   scan off
   ```

   ![Ukončí skenování nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a><span data-ttu-id="f4f28-144">Navázání připojení Bluetooth z hello IoT brány toohello SensorTag</span><span class="sxs-lookup"><span data-stu-id="f4f28-144">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag</span></span>

1. <span data-ttu-id="f4f28-145">Připojte toohello SensorTag spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-145">Connect toohello SensorTag by running hello following command:</span></span>

   ```bash
   connect <MAC address>
   ```

   ![Spojte se s bluetoothctl toohello SensorTag](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="f4f28-147">Odpojte od hello SensorTag a ukončete prostředí Bluetooth hello spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f4f28-147">Disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

   ```bash
   disconnect
   exit
   ```

   ![Odpojení od hello SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

<span data-ttu-id="f4f28-149">Úspěšně jste povolili hello připojení mezi hello SensorTag a hello IoT brány.</span><span class="sxs-lookup"><span data-stu-id="f4f28-149">You've successfully enabled hello connection between hello SensorTag and hello IoT gateway.</span></span>

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a><span data-ttu-id="f4f28-150">Spuštění zakázat ukázkové aplikace toosend SensorTag data tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="f4f28-150">Run a BLE sample application toosend SensorTag data tooyour IoT hub</span></span>

<span data-ttu-id="f4f28-151">Azure IoT Edge zajišťuje Hello ukázkovou aplikaci Bluetooth nízká energie (Povolit).</span><span class="sxs-lookup"><span data-stu-id="f4f28-151">hello Bluetooth Low Energy (BLE) sample application is provided by Azure IoT Edge.</span></span> <span data-ttu-id="f4f28-152">Ukázková aplikace Hello shromažďuje data z zakázat připojení a odeslat hello data tooyou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-152">hello sample application collects data from BLE connection and send hello data tooyou IoT hub.</span></span> <span data-ttu-id="f4f28-153">toorun hello ukázkovou aplikaci, musíte:</span><span class="sxs-lookup"><span data-stu-id="f4f28-153">toorun hello sample application, you need to:</span></span>

1. <span data-ttu-id="f4f28-154">Nakonfigurujte hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f4f28-154">Configure hello sample application.</span></span>
1. <span data-ttu-id="f4f28-155">Spusťte hello ukázkovou aplikaci na hello IoT brány.</span><span class="sxs-lookup"><span data-stu-id="f4f28-155">Run hello sample application on hello IoT gateway.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="f4f28-156">Konfigurace hello ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f4f28-156">Configure hello sample application</span></span>

1. <span data-ttu-id="f4f28-157">Složky přejděte toohello hello ukázkovou aplikaci spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-157">Go toohello folder of hello sample application by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. <span data-ttu-id="f4f28-158">Otevřete konfigurační soubor hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f4f28-158">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="f4f28-159">V hello konfigurační soubor zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="f4f28-159">In hello configuration file, fill in hello following values:</span></span>

   <span data-ttu-id="f4f28-160">**IoTHubName**: hello název služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-160">**IoTHubName**: hello name of your IoT hub.</span></span>

   <span data-ttu-id="f4f28-161">**IoTHubSuffix**: získání IoTHubSuffix z hello primární klíč hello zařízení připojovací řetězec, který jste si poznamenali dolů.</span><span class="sxs-lookup"><span data-stu-id="f4f28-161">**IoTHubSuffix**: Get IoTHubSuffix from hello primary key of hello device connection string that you noted down.</span></span> <span data-ttu-id="f4f28-162">Zkontrolujte, zda můžete získat hello primární klíč hello zařízení připojovací řetězec, není hello primární klíč připojovací řetězec centra IoT.</span><span class="sxs-lookup"><span data-stu-id="f4f28-162">Ensure that you get hello primary key of hello device connection string, not hello primary key of your IoT hub connection string.</span></span> <span data-ttu-id="f4f28-163">primární klíč Hello hello zařízení připojovacího řetězce je ve formátu hello `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span><span class="sxs-lookup"><span data-stu-id="f4f28-163">hello primary key of hello device connection string is in hello format of `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span></span>

   <span data-ttu-id="f4f28-164">**Přenos**: hello výchozí hodnota je `amqp`.</span><span class="sxs-lookup"><span data-stu-id="f4f28-164">**Transport**: hello default value is `amqp`.</span></span> <span data-ttu-id="f4f28-165">Tato hodnota se uvádí hello protokol během transpotation.</span><span class="sxs-lookup"><span data-stu-id="f4f28-165">This value shows hello protocol during transpotation.</span></span> <span data-ttu-id="f4f28-166">Může to být `http`, `amqp`, nebo `mqtt`.</span><span class="sxs-lookup"><span data-stu-id="f4f28-166">It could be `http`, `amqp`, or `mqtt`.</span></span>

   <span data-ttu-id="f4f28-167">**macAddress**: hello adresu MAC hello SensorTag, kterou jste si poznamenali dolů.</span><span class="sxs-lookup"><span data-stu-id="f4f28-167">**macAddress**: hello MAC address of hello SensorTag that you noted down.</span></span>

   <span data-ttu-id="f4f28-168">**deviceID**: ID hello zařízení, které jste vytvořili ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f4f28-168">**deviceID**: ID of hello device that you created in your IoT hub.</span></span>

   <span data-ttu-id="f4f28-169">**deviceKey**: primární klíč hello hello zařízení připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="f4f28-169">**deviceKey**: hello primary key of hello device connection string.</span></span>

   ![Soubor konfigurace dokončení hello hello zakázat ukázkové aplikace](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. <span data-ttu-id="f4f28-171">Stiskněte klávesu `ESC` a typ `:wq` toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="f4f28-171">Press `ESC` and type `:wq` toosave hello file.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="f4f28-172">Spuštění ukázkové aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f4f28-172">Run hello sample application</span></span>

1. <span data-ttu-id="f4f28-173">Ujistěte se, zda text hello, které SensorTag je zapnutý.</span><span class="sxs-lookup"><span data-stu-id="f4f28-173">Make sure hello SensorTag is powered on.</span></span>
1. <span data-ttu-id="f4f28-174">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f4f28-174">Run hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="f4f28-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f4f28-175">Next steps</span></span>

[<span data-ttu-id="f4f28-176">Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT</span><span class="sxs-lookup"><span data-stu-id="f4f28-176">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
