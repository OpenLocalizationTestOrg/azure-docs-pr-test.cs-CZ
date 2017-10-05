---
title: "Použít bránu IoT pro připojení zařízení k Azure IoT Hub | Microsoft Docs"
description: "Další informace o použití Intel NUC jako IoT bránu pro připojení TI SensorTag a odesílat data snímačů do služby Azure IoT Hub v cloudu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Brána IOT připojit zařízení do cloudu"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a><span data-ttu-id="2229e-104">Použití IoT bránu pro připojení akcí k cloudu - SensorTag do služby Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="2229e-104">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>

> [!NOTE]
> <span data-ttu-id="2229e-105">Než začnete tento kurz, ujistěte se, když jste dokončili [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span><span class="sxs-lookup"><span data-stu-id="2229e-105">Before you start this tutorial, make sure you’ve completed [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span></span> <span data-ttu-id="2229e-106">V [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), můžete nastavit zařízení Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-106">In [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), you set up the Intel NUC device as an IoT gateway.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="2229e-107">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="2229e-107">What you will learn</span></span>

<span data-ttu-id="2229e-108">Zjistíte, jak používat bránu IoT pro připojení ke službě Azure IoT Hub Texas Instruments SensorTag (CC2650STK).</span><span class="sxs-lookup"><span data-stu-id="2229e-108">You learn how to use an IoT gateway to connect a Texas Instruments SensorTag (CC2650STK) to Azure IoT Hub.</span></span> <span data-ttu-id="2229e-109">Bránu IoT odešle teploty a vlhkosti data shromážděná z SensorTag do služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-109">The IoT gateway sends temperature and humidity data collected from the SensorTag to Azure IoT Hub.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="2229e-110">Co provedete</span><span class="sxs-lookup"><span data-stu-id="2229e-110">What you will do</span></span>

- <span data-ttu-id="2229e-111">Vytvoření služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-111">Create an IoT hub.</span></span>
- <span data-ttu-id="2229e-112">Registrovat zařízení ve službě IoT hub pro SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-112">Register a device in the IoT hub for the SensorTag.</span></span>
- <span data-ttu-id="2229e-113">Povolte připojení mezi bránou rozhraní IoT a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-113">Enable the connection between the IoT gateway and the SensorTag.</span></span>
- <span data-ttu-id="2229e-114">Spuštění ukázkové aplikace zakázat k odesílání dat SensorTag do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-114">Run a BLE sample application to send SensorTag data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2229e-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2229e-115">What you need</span></span>

- <span data-ttu-id="2229e-116">Kurz [nastavit Intel NUC jako bránu IoT](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) dokončit, ve kterém můžete nastavit Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-116">Tutorial [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) completed in which you set up Intel NUC as an IoT gateway.</span></span>
- * <span data-ttu-id="2229e-117">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2229e-117">An active Azure subscription.</span></span> <span data-ttu-id="2229e-118">Pokud nemáte účet Azure [vytvořit Bezplatný zkušební účet Azure](https://azure.microsoft.com/free/) za několik minut.</span><span class="sxs-lookup"><span data-stu-id="2229e-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
- <span data-ttu-id="2229e-119">Klient SSH, který běží v hostitelském počítači.</span><span class="sxs-lookup"><span data-stu-id="2229e-119">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="2229e-120">V systému Windows se doporučuje puTTY.</span><span class="sxs-lookup"><span data-stu-id="2229e-120">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="2229e-121">Linux a systému macOS již obsahují klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="2229e-121">Linux and macOS already come with an SSH client.</span></span>
- <span data-ttu-id="2229e-122">IP adresa a uživatelské jméno a heslo pro přístup k bráně z klienta SSH.</span><span class="sxs-lookup"><span data-stu-id="2229e-122">The IP address and the username and password to access the gateway from the SSH client.</span></span>
- <span data-ttu-id="2229e-123">Připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="2229e-123">An Internet connection.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> <span data-ttu-id="2229e-124">Zde je zaregistrovat toto nové zařízení pro vaše SensorTag</span><span class="sxs-lookup"><span data-stu-id="2229e-124">Here you register this new device for your SensorTag</span></span>

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a><span data-ttu-id="2229e-125">Povolit připojení mezi bránou rozhraní IoT a SensorTag</span><span class="sxs-lookup"><span data-stu-id="2229e-125">Enable the connection between the IoT gateway and the SensorTag</span></span>

<span data-ttu-id="2229e-126">V této části můžete provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="2229e-126">In this section, you perform the following tasks:</span></span>

- <span data-ttu-id="2229e-127">Získáte adresu MAC SensorTag pro připojení Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="2229e-127">Get the MAC address of the SensorTag for Bluetooth connection.</span></span>
- <span data-ttu-id="2229e-128">Navázání připojení Bluetooth IoT brány SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-128">Initiate a Bluetooth connection from the IoT gateway to the SensorTag.</span></span>

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a><span data-ttu-id="2229e-129">Získat adresu MAC SensorTag pro připojení Bluetooth</span><span class="sxs-lookup"><span data-stu-id="2229e-129">Get the MAC address of the SensorTag for Bluetooth connection</span></span>

1. <span data-ttu-id="2229e-130">Na hostitelském počítači spusťte použije klient SSH a připojení k bráně IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-130">On the host computer, run the SSH client and connect to the IoT gateway.</span></span>
1. <span data-ttu-id="2229e-131">Odblokování Bluetooth spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2229e-131">Unblock Bluetooth by running the following command:</span></span>

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. <span data-ttu-id="2229e-132">Spusťte službu Bluetooth na bráně IoT a zadejte prostředí Bluetooth nakonfigurovat Bluetooth spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2229e-132">Start the Bluetooth service on the IoT gateway and enter a Bluetooth shell to configure Bluetooth by running the following commands:</span></span>

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. <span data-ttu-id="2229e-133">Zapnutí řadičem Bluetooth spuštěním následujícího příkazu v prostředí Bluetooth:</span><span class="sxs-lookup"><span data-stu-id="2229e-133">Power on the Bluetooth controller by running the following command at the Bluetooth shell:</span></span>

   ```bash
   power on
   ```

   ![Zapnutí řadičem Bluetooth na bráně IoT s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="2229e-135">Spustí vyhledávání blízkými zařízeními Bluetooth spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2229e-135">Start scanning for nearby Bluetooth devices by running the following command:</span></span>

   ```bash
   scan on
   ```

   ![Kontrola nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="2229e-137">Stisknutím tlačítka párování na SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-137">Press the pairing button on the SensorTag.</span></span> <span data-ttu-id="2229e-138">Zeleným VEDLA na blikání SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-138">The green LED on the SensorTag flashes.</span></span>
1. <span data-ttu-id="2229e-139">V prostředí Bluetooth měli byste vidět, že se nachází SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-139">At the Bluetooth shell, you should see the SensorTag is found.</span></span> <span data-ttu-id="2229e-140">Poznamenejte si adresu MAC SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-140">Make a note of the MAC address of the SensorTag.</span></span> <span data-ttu-id="2229e-141">V tomto příkladu je adresu MAC SensorTag `24:71:89:C0:7F:82`.</span><span class="sxs-lookup"><span data-stu-id="2229e-141">In this example, the MAC address of the SensorTag is `24:71:89:C0:7F:82`.</span></span>
1. <span data-ttu-id="2229e-142">Vypněte vyhledávání tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2229e-142">Turn off the scan by running the following command:</span></span>

   ```bash
   scan off
   ```

   ![Ukončí skenování nedaleko Bluetooth zařízení s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a><span data-ttu-id="2229e-144">Navázání připojení Bluetooth IoT brány SensorTag</span><span class="sxs-lookup"><span data-stu-id="2229e-144">Initiate a Bluetooth connection from the IoT gateway to the SensorTag</span></span>

1. <span data-ttu-id="2229e-145">Připojte k SensorTag spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="2229e-145">Connect to the SensorTag by running the following command:</span></span>

   ```bash
   connect <MAC address>
   ```

   ![Připojení k SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="2229e-147">Odpojte od SensorTag a opusťte prostředí Bluetooth spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="2229e-147">Disconnect from the SensorTag and exit the Bluetooth shell by running the following commands:</span></span>

   ```bash
   disconnect
   exit
   ```

   ![Odpojení od SensorTag s bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

<span data-ttu-id="2229e-149">Úspěšně jste povolili připojení mezi SensorTag a bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-149">You've successfully enabled the connection between the SensorTag and the IoT gateway.</span></span>

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a><span data-ttu-id="2229e-150">Spuštění ukázkové aplikace zakázat k odesílání dat SensorTag do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="2229e-150">Run a BLE sample application to send SensorTag data to your IoT hub</span></span>

<span data-ttu-id="2229e-151">Ukázkovou aplikaci Bluetooth nízká energie (Povolit) zajišťuje Azure IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="2229e-151">The Bluetooth Low Energy (BLE) sample application is provided by Azure IoT Edge.</span></span> <span data-ttu-id="2229e-152">Ukázkovou aplikaci shromažďuje data z zakázat připojení a posílat data do IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-152">The sample application collects data from BLE connection and send the data to you IoT hub.</span></span> <span data-ttu-id="2229e-153">Spuštění ukázkové aplikace, budete muset:</span><span class="sxs-lookup"><span data-stu-id="2229e-153">To run the sample application, you need to:</span></span>

1. <span data-ttu-id="2229e-154">Nakonfigurujte ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2229e-154">Configure the sample application.</span></span>
1. <span data-ttu-id="2229e-155">Spouštět ukázkovou aplikaci na bráně IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-155">Run the sample application on the IoT gateway.</span></span>

### <a name="configure-the-sample-application"></a><span data-ttu-id="2229e-156">Nakonfigurujte ukázkovou aplikaci</span><span class="sxs-lookup"><span data-stu-id="2229e-156">Configure the sample application</span></span>

1. <span data-ttu-id="2229e-157">Přejděte do složky ukázkové aplikace tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2229e-157">Go to the folder of the sample application by running the following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. <span data-ttu-id="2229e-158">Otevřete konfigurační soubor tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2229e-158">Open the configuration file by running the following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="2229e-159">V konfiguračním souboru zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2229e-159">In the configuration file, fill in the following values:</span></span>

   <span data-ttu-id="2229e-160">**IoTHubName**: název služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-160">**IoTHubName**: The name of your IoT hub.</span></span>

   <span data-ttu-id="2229e-161">**IoTHubSuffix**: získání IoTHubSuffix z primární klíč zařízení připojovací řetězec, který jste si poznamenali dolů.</span><span class="sxs-lookup"><span data-stu-id="2229e-161">**IoTHubSuffix**: Get IoTHubSuffix from the primary key of the device connection string that you noted down.</span></span> <span data-ttu-id="2229e-162">Ujistěte se, že dostanete primární klíč zařízení připojovací řetězec, není primární klíč připojovací řetězec centra IoT.</span><span class="sxs-lookup"><span data-stu-id="2229e-162">Ensure that you get the primary key of the device connection string, not the primary key of your IoT hub connection string.</span></span> <span data-ttu-id="2229e-163">Primární klíč zařízení připojovací řetězec je ve formátu `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span><span class="sxs-lookup"><span data-stu-id="2229e-163">The primary key of the device connection string is in the format of `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span></span>

   <span data-ttu-id="2229e-164">**Přenos**: výchozí hodnota je `amqp`.</span><span class="sxs-lookup"><span data-stu-id="2229e-164">**Transport**: The default value is `amqp`.</span></span> <span data-ttu-id="2229e-165">Tato hodnota se uvádí protokol během transpotation.</span><span class="sxs-lookup"><span data-stu-id="2229e-165">This value shows the protocol during transpotation.</span></span> <span data-ttu-id="2229e-166">Může to být `http`, `amqp`, nebo `mqtt`.</span><span class="sxs-lookup"><span data-stu-id="2229e-166">It could be `http`, `amqp`, or `mqtt`.</span></span>

   <span data-ttu-id="2229e-167">**macAddress**: adresa MAC SensorTag, kterou jste si poznamenali dolů.</span><span class="sxs-lookup"><span data-stu-id="2229e-167">**macAddress**: The MAC address of the SensorTag that you noted down.</span></span>

   <span data-ttu-id="2229e-168">**deviceID**: ID zařízení, které jste vytvořili ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2229e-168">**deviceID**: ID of the device that you created in your IoT hub.</span></span>

   <span data-ttu-id="2229e-169">**deviceKey**: primární klíč zařízení připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2229e-169">**deviceKey**: The primary key of the device connection string.</span></span>

   ![Dokončení konfiguračním souboru zakázat ukázkové aplikace](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. <span data-ttu-id="2229e-171">Stiskněte klávesu `ESC` a typ `:wq` k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="2229e-171">Press `ESC` and type `:wq` to save the file.</span></span>

### <a name="run-the-sample-application"></a><span data-ttu-id="2229e-172">Spuštění ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="2229e-172">Run the sample application</span></span>

1. <span data-ttu-id="2229e-173">Ujistěte se, že je zapnutý SensorTag.</span><span class="sxs-lookup"><span data-stu-id="2229e-173">Make sure the SensorTag is powered on.</span></span>
1. <span data-ttu-id="2229e-174">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="2229e-174">Run the following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="2229e-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2229e-175">Next steps</span></span>

[<span data-ttu-id="2229e-176">Použít bránu IoT pro transformaci dat snímačů s hranou Azure IoT</span><span class="sxs-lookup"><span data-stu-id="2229e-176">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
