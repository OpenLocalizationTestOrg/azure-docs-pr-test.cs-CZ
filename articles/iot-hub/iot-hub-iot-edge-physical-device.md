---
title: "Použití fyzického zařízení s Azure IoT hranou | Microsoft Docs"
description: "Jak používat k odesílání dat do služby IoT hub prostřednictvím brány IoT Edge spuštěné na zařízení malin pí 3 zařízením Texas Instruments SensorTag. Brána je sestaven pomocí Azure IoT okraj."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: 02962a91c739a53dfcf947bcc736e5c293b9384f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-to-forward-device-to-cloud-messages-to-iot-hub"></a><span data-ttu-id="56d4c-104">Použití Azure IoT Edge na malin platformy pro předávání zpráv typu zařízení cloud do služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="56d4c-104">Use Azure IoT Edge on a Raspberry Pi to forward device-to-cloud messages to IoT Hub</span></span>

<span data-ttu-id="56d4c-105">Tento návod [Bluetooth nízkou energie ukázka] [ lnk-ble-samplecode] ukazuje, jak používat [Azure IoT Edge] [ lnk-sdk] na:</span><span class="sxs-lookup"><span data-stu-id="56d4c-105">This walkthrough of the [Bluetooth low energy sample][lnk-ble-samplecode] shows you how to use [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="56d4c-106">Předat dál telemetrie zařízení cloud do služby IoT Hub z fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-106">Forward device-to-cloud telemetry to IoT Hub from a physical device.</span></span>
* <span data-ttu-id="56d4c-107">Směrování příkazů ze služby IoT Hub fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-107">Route commands from IoT Hub to a physical device.</span></span>

<span data-ttu-id="56d4c-108">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="56d4c-108">This walkthrough covers:</span></span>

* <span data-ttu-id="56d4c-109">**Architektura**: důležité architektury informace o ukázkové nízkou energie Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="56d4c-109">**Architecture**: important architectural information about the Bluetooth low energy sample.</span></span>
* <span data-ttu-id="56d4c-110">**Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="56d4c-110">**Build and run**: the steps required to build and run the sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="56d4c-111">Architektura</span><span class="sxs-lookup"><span data-stu-id="56d4c-111">Architecture</span></span>

<span data-ttu-id="56d4c-112">Průvodce ukazuje, jak sestavit a spustit bránu IoT Edge na malin pí 3, který spouští Raspbian Linux.</span><span class="sxs-lookup"><span data-stu-id="56d4c-112">The walkthrough shows you how to build and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="56d4c-113">Brána je sestaven pomocí IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="56d4c-113">The gateway is built using IoT Edge.</span></span> <span data-ttu-id="56d4c-114">Příklad používá ke shromažďování dat teploty zařízením Texas Instruments SensorTag Bluetooth nízká energie (Povolit).</span><span class="sxs-lookup"><span data-stu-id="56d4c-114">The sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device to collect temperature data.</span></span>

<span data-ttu-id="56d4c-115">Při spuštění IoT hraniční brána je:</span><span class="sxs-lookup"><span data-stu-id="56d4c-115">When you run the IoT Edge gateway it:</span></span>

* <span data-ttu-id="56d4c-116">Připojí se k zařízení SensorTag pomocí protokolu Bluetooth nízká energie (Povolit).</span><span class="sxs-lookup"><span data-stu-id="56d4c-116">Connects to a SensorTag device using the Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="56d4c-117">Připojí se ke službě IoT Hub pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="56d4c-117">Connects to IoT Hub using the HTTP protocol.</span></span>
* <span data-ttu-id="56d4c-118">Předává telemetrie ze zařízení SensorTag pro IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-118">Forwards telemetry from the SensorTag device to IoT Hub.</span></span>
* <span data-ttu-id="56d4c-119">Směrování příkazů ze služby IoT Hub zařízení SensorTag.</span><span class="sxs-lookup"><span data-stu-id="56d4c-119">Routes commands from IoT Hub to the SensorTag device.</span></span>

<span data-ttu-id="56d4c-120">Brána obsahuje následující moduly hraniční IoT:</span><span class="sxs-lookup"><span data-stu-id="56d4c-120">The gateway contains the following IoT Edge modules:</span></span>

* <span data-ttu-id="56d4c-121">A *zakázat modulu* , rozhraní se zakázat zařízení přijímat teploty data ze zařízení a odesílání příkazů do zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-121">A *BLE module* that interfaces with a BLE device to receive temperature data from the device and send commands to the device.</span></span>
* <span data-ttu-id="56d4c-122">A *zakázat cloudu do zařízení modulu* který překládá JSON zprávy odeslané ze služby IoT Hub do zakázat pokyny *zakázat modulu*.</span><span class="sxs-lookup"><span data-stu-id="56d4c-122">A *BLE cloud to device module* that translates JSON messages sent from IoT Hub into BLE instructions for the *BLE module*.</span></span>
* <span data-ttu-id="56d4c-123">A *protokoly modulu* který zaznamenává všechny brány zprávy do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="56d4c-123">A *logger module* that logs all gateway messages to a local file.</span></span>
* <span data-ttu-id="56d4c-124">*Modulem mapování identit* který překládá mezi adresy MAC povolit zařízení a identit zařízení Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="56d4c-125">*IoT Hub modulu* které odesílá telemetrická data do služby IoT hub a přijímá příkazy zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-125">An *IoT Hub module* that uploads telemetry data to an IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="56d4c-126">A *zakázat tiskárny modulu* , interpretuje telemetrická data ze zařízení zakázat a výtisků formátovaných dat do konzoly nástroje umožňující řešení potíží a ladění.</span><span class="sxs-lookup"><span data-stu-id="56d4c-126">A *BLE printer module* that interprets telemetry from the BLE device and prints formatted data to the console to enable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-the-gateway"></a><span data-ttu-id="56d4c-127">Tok dat prostřednictvím brány</span><span class="sxs-lookup"><span data-stu-id="56d4c-127">How data flows through the gateway</span></span>

<span data-ttu-id="56d4c-128">Následující blok diagram znázorňuje kanálu toku dat odesílání telemetrie:</span><span class="sxs-lookup"><span data-stu-id="56d4c-128">The following block diagram illustrates the telemetry upload data flow pipeline:</span></span>

![Kanál brány odesílání telemetrie](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="56d4c-130">Kroky, které položky telemetrie trvá cestě z zakázat zařízení do služby IoT Hub jsou:</span><span class="sxs-lookup"><span data-stu-id="56d4c-130">The steps that an item of telemetry takes traveling from a BLE device to IoT Hub are:</span></span>

1. <span data-ttu-id="56d4c-131">Zařízení zakázat generuje ukázku teploty a odesílá přes Bluetooth zakázat modulu v bráně.</span><span class="sxs-lookup"><span data-stu-id="56d4c-131">The BLE device generates a temperature sample and sends it over Bluetooth to the BLE module in the gateway.</span></span>
1. <span data-ttu-id="56d4c-132">Zakázat modul přijímá vzorku a publikuje do zprostředkovatele spolu s adresa MAC zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-132">The BLE module receives the sample and publishes it to the broker along with the MAC address of the device.</span></span>
1. <span data-ttu-id="56d4c-133">Modul mapování identit převezme tuto zprávu a používá interní tabulku přeložit adresa MAC zařízení do služby IoT Hub identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-133">The identity mapping module picks up this message and uses an internal table to translate the MAC address of the device into an IoT Hub device identity.</span></span> <span data-ttu-id="56d4c-134">Identita zařízení IoT Hub se skládá z ID zařízení a klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="56d4c-135">Modul mapování identit publikuje novou zprávu, která obsahuje teploty ukázková data, adresa MAC zařízení, ID zařízení a klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-135">The identity mapping module publishes a new message that contains the temperature sample data, the MAC address of the device, the device ID, and the device key.</span></span>
1. <span data-ttu-id="56d4c-136">Modul služby IoT Hub přijímá tuto novou zprávu (generované modulem mapování identity) a publikuje do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-136">The IoT Hub module receives this new message (generated by the identity mapping module) and publishes it to IoT Hub.</span></span>
1. <span data-ttu-id="56d4c-137">Modul protokolovacího nástroje zaznamená všechny zprávy z zprostředkovatele do místního souboru.</span><span class="sxs-lookup"><span data-stu-id="56d4c-137">The logger module logs all messages from the broker to a local file.</span></span>

<span data-ttu-id="56d4c-138">Následující blok diagram znázorňuje kanálu toku dat příkaz zařízení:</span><span class="sxs-lookup"><span data-stu-id="56d4c-138">The following block diagram illustrates the device command data flow pipeline:</span></span>

![Zařízení příkaz brány kanálu](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="56d4c-140">Modul služby IoT Hub pravidelně se dotazuje službu IoT hub pro nové zprávy příkaz.</span><span class="sxs-lookup"><span data-stu-id="56d4c-140">The IoT Hub module periodically polls the IoT hub for new command messages.</span></span>
1. <span data-ttu-id="56d4c-141">Když v modulu služby IoT Hub přijme zprávu o nový příkaz, vydává je pro zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="56d4c-141">When the IoT Hub module receives a new command message, it publishes it to the broker.</span></span>
1. <span data-ttu-id="56d4c-142">Modul mapování identit převezme zprávou příkazu a používá interní tabulku přeložit ID zařízení IoT Hub, který má adresa MAC zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-142">The identity mapping module picks up the command message and uses an internal table to translate the IoT Hub device ID to a device MAC address.</span></span> <span data-ttu-id="56d4c-143">Pak publikuje novou zprávu, která obsahuje adresu MAC cílového zařízení v mapě vlastnosti zprávy.</span><span class="sxs-lookup"><span data-stu-id="56d4c-143">It then publishes a new message that includes the MAC address of the target device in the properties map of the message.</span></span>
1. <span data-ttu-id="56d4c-144">Modul zakázat Cloud-zařízení převezme tuto zprávu a překládá do správné zakázat instrukce pro modul zakázat.</span><span class="sxs-lookup"><span data-stu-id="56d4c-144">The BLE Cloud-to-Device module picks up this message and translates it into the proper BLE instruction for the BLE module.</span></span> <span data-ttu-id="56d4c-145">Pak publikuje novou zprávu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="56d4c-146">Modul zakázat převezme tuto zprávu a provede instrukci vstupně-výstupních operací podle komunikaci se zařízením zakázat.</span><span class="sxs-lookup"><span data-stu-id="56d4c-146">The BLE module picks up this message and executes the I/O instruction by communicating with the BLE device.</span></span>
1. <span data-ttu-id="56d4c-147">Modul protokolovacího nástroje zaznamená všechny zprávy z zprostředkovatele do souboru na disku.</span><span class="sxs-lookup"><span data-stu-id="56d4c-147">The logger module logs all messages from the broker to a disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="56d4c-148">Požadavky</span><span class="sxs-lookup"><span data-stu-id="56d4c-148">Prerequisites</span></span>

<span data-ttu-id="56d4c-149">K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="56d4c-149">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="56d4c-150">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="56d4c-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="56d4c-151">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="56d4c-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="56d4c-152">Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="56d4c-152">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="56d4c-153">Windows nezahrnuje klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="56d4c-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="56d4c-154">Doporučujeme používat [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="56d4c-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="56d4c-155">Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH.</span><span class="sxs-lookup"><span data-stu-id="56d4c-155">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="56d4c-156">Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="56d4c-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="56d4c-157">Připravte svůj hardware</span><span class="sxs-lookup"><span data-stu-id="56d4c-157">Prepare your hardware</span></span>

<span data-ttu-id="56d4c-158">Tento kurz předpokládá, že používáte [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) zařízení připojené k systémem Raspbian malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56d4c-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected to a Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="56d4c-159">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="56d4c-159">Install Raspbian</span></span>

<span data-ttu-id="56d4c-160">K instalaci Raspbian na vašem zařízení malin pí 3 můžete použít některý z následujících možností.</span><span class="sxs-lookup"><span data-stu-id="56d4c-160">You can use either of the following options to install Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="56d4c-161">Chcete-li nainstalovat nejnovější verzi Raspbian, použijte [NOOBS] [ lnk-noobs] grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="56d4c-161">To install the latest version of Raspbian, use the [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="56d4c-162">Ručně [Stáhnout] [ lnk-raspbian] a zapsat nejnovější bitovou kopii operačního systému Raspbian do SD karty.</span><span class="sxs-lookup"><span data-stu-id="56d4c-162">Manually [download][lnk-raspbian] and write the latest image of the Raspbian operating system to an SD card.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="56d4c-163">Přihlaste se a přístup k terminálu</span><span class="sxs-lookup"><span data-stu-id="56d4c-163">Sign in and access the terminal</span></span>

<span data-ttu-id="56d4c-164">Máte dvě možnosti pro přístup k Terminálové prostředí na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="56d4c-164">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="56d4c-165">Pokud máte klávesnici a monitorování, které jsou připojené k vaší malin platformy, můžete použít Raspbian grafického uživatelského rozhraní pro přístup k okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-165">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

* <span data-ttu-id="56d4c-166">Přístup na příkazovém řádku vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="56d4c-166">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="56d4c-167">Použijte okno terminálu v grafickém uživatelském rozhraní</span><span class="sxs-lookup"><span data-stu-id="56d4c-167">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="56d4c-168">Výchozí pověření pro Raspbian jsou uživatelské jméno **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="56d4c-168">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="56d4c-169">Na hlavním panelu v grafickém uživatelském rozhraní, můžete spustit **Terminálové** nástroj pomocí ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="56d4c-169">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="56d4c-170">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="56d4c-170">Sign in with SSH</span></span>

<span data-ttu-id="56d4c-171">SSH můžete použít pro příkazového řádku přístup k vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="56d4c-171">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="56d4c-172">Článek [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje postup konfigurace SSH na vaše malin platformy a jak se připojit z [Windows] [ lnk-ssh-windows] nebo [ Linux & Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="56d4c-172">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="56d4c-173">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="56d4c-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="56d4c-174">Nainstalujte BlueZ 5.37</span><span class="sxs-lookup"><span data-stu-id="56d4c-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="56d4c-175">Obraťte se na hardwaru Bluetooth prostřednictvím zásobníku BlueZ moduly zakázat.</span><span class="sxs-lookup"><span data-stu-id="56d4c-175">The BLE modules talk to the Bluetooth hardware via the BlueZ stack.</span></span> <span data-ttu-id="56d4c-176">Je nutné 5.37 verzi BlueZ pro moduly fungovala správně.</span><span class="sxs-lookup"><span data-stu-id="56d4c-176">You need version 5.37 of BlueZ for the modules to work correctly.</span></span> <span data-ttu-id="56d4c-177">Tyto pokyny Ujistěte se, že je nainstalována správná verze BlueZ.</span><span class="sxs-lookup"><span data-stu-id="56d4c-177">These instructions make sure the correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="56d4c-178">Zastavte aktuální démon bluetooth:</span><span class="sxs-lookup"><span data-stu-id="56d4c-178">Stop the current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="56d4c-179">Nainstalujte BlueZ závislosti:</span><span class="sxs-lookup"><span data-stu-id="56d4c-179">Install the BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="56d4c-180">Zdrojový kód BlueZ stáhněte z bluez.org:</span><span class="sxs-lookup"><span data-stu-id="56d4c-180">Download the BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="56d4c-181">Rozbalte zdrojový kód:</span><span class="sxs-lookup"><span data-stu-id="56d4c-181">Unzip the source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="56d4c-182">Změňte adresáře na nově vytvořená složka:</span><span class="sxs-lookup"><span data-stu-id="56d4c-182">Change directories to the newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="56d4c-183">Konfigurovat kód BlueZ má být sestaven:</span><span class="sxs-lookup"><span data-stu-id="56d4c-183">Configure the BlueZ code to be built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="56d4c-184">Sestavení BlueZ:</span><span class="sxs-lookup"><span data-stu-id="56d4c-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="56d4c-185">Až se to dokončí instalaci BlueZ stavbě:</span><span class="sxs-lookup"><span data-stu-id="56d4c-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="56d4c-186">Změna konfigurace služby systemd pro bluetooth, odkazuje na novou démon bluetooth v souboru `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="56d4c-186">Change systemd service configuration for bluetooth so it points to the new bluetooth daemon in the file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="56d4c-187">'ExecStart' řádku nahraďte následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="56d4c-187">Replace the 'ExecStart' line with the following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="56d4c-188">Povolit připojení k zařízení SensorTag ze zařízení malin pí 3</span><span class="sxs-lookup"><span data-stu-id="56d4c-188">Enable connectivity to the SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="56d4c-189">Před spuštěním ukázky, je třeba ověřit, zda vaše malin pí 3 může připojit k zařízení SensorTag.</span><span class="sxs-lookup"><span data-stu-id="56d4c-189">Before running the sample, you need to verify that your Raspberry Pi 3 can connect to the SensorTag device.</span></span>

1. <span data-ttu-id="56d4c-190">Ujistěte se, `rfkill` je nainstalován nástroj:</span><span class="sxs-lookup"><span data-stu-id="56d4c-190">Ensure the `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="56d4c-191">Odblokování bluetooth na malin pí 3 a zkontrolujte, jestli číslo verze **5.37**:</span><span class="sxs-lookup"><span data-stu-id="56d4c-191">Unblock bluetooth on the Raspberry Pi 3 and check that the version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="56d4c-192">Pokud chcete zadat prostředí interaktivní bluetooth, spusťte službu bluetooth a provést **bluetoothctl** příkaz:</span><span class="sxs-lookup"><span data-stu-id="56d4c-192">To enter the interactive bluetooth shell, start the bluetooth service and execute the **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="56d4c-193">Zadejte příkaz **zapnutí** mocninu až řadičem bluetooth.</span><span class="sxs-lookup"><span data-stu-id="56d4c-193">Enter the command **power on** to power up the bluetooth controller.</span></span> <span data-ttu-id="56d4c-194">Příkaz vrátí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="56d4c-194">The command returns output similar to the following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="56d4c-195">V prostředí interaktivní bluetooth, zadejte příkaz **kontrolu** kontrolovala zařízeními bluetooth.</span><span class="sxs-lookup"><span data-stu-id="56d4c-195">In the interactive bluetooth shell, enter the command **scan on** to scan for bluetooth devices.</span></span> <span data-ttu-id="56d4c-196">Příkaz vrátí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="56d4c-196">The command returns output similar to the following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="56d4c-197">Zařízení SensorTag zjistitelnost stisknutím tlačítka malé (zelený DIODU by měl flash).</span><span class="sxs-lookup"><span data-stu-id="56d4c-197">Make the SensorTag device discoverable by pressing the small button (the green LED should flash).</span></span> <span data-ttu-id="56d4c-198">Pí 3 malin by měl zjistit zařízení SensorTag:</span><span class="sxs-lookup"><span data-stu-id="56d4c-198">The Raspberry Pi 3 should discover the SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="56d4c-199">V tomto příkladu vidíte, že je adresa MAC zařízení SensorTag **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="56d4c-199">In this example, you can see that the MAC address of the SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="56d4c-200">Vypnout kontrolu tak, že zadáte **kontrolovat vypnout** příkaz:</span><span class="sxs-lookup"><span data-stu-id="56d4c-200">Turn off scanning by entering the **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="56d4c-201">Připojení k zařízení SensorTag pomocí adresu MAC zadáním **připojit \<adresa MAC\>**.</span><span class="sxs-lookup"><span data-stu-id="56d4c-201">Connect to your SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="56d4c-202">Následující ukázkový výstup je zkratka pro přehlednost:</span><span class="sxs-lookup"><span data-stu-id="56d4c-202">The following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > <span data-ttu-id="56d4c-203">Můžete vytvořit seznam vlastností GATT zařízení znovu s použitím **atributy seznamu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="56d4c-203">You can list the GATT characteristics of the device again using the **list-attributes** command.</span></span>

1. <span data-ttu-id="56d4c-204">Teď můžete odpojit ze zařízení pomocí **odpojit** příkaz a poté ukončete pomocí prostředí bluetooth **ukončení** příkaz:</span><span class="sxs-lookup"><span data-stu-id="56d4c-204">You can now disconnect from the device using the **disconnect** command and then exit from the bluetooth shell using the **quit** command:</span></span>

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="56d4c-205">Nyní jste připraveni lit IoT Edge ukázku spustit na vaše malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56d4c-205">You're now ready to run the BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-the-iot-edge-ble-sample"></a><span data-ttu-id="56d4c-206">Zakázat Edge IoT ukázku spustit</span><span class="sxs-lookup"><span data-stu-id="56d4c-206">Run the IoT Edge BLE sample</span></span>

<span data-ttu-id="56d4c-207">Pokud chcete zakázat Edge IoT ukázku spustit, je třeba provést tři úlohy:</span><span class="sxs-lookup"><span data-stu-id="56d4c-207">To run the IoT Edge BLE sample, you need to complete three tasks:</span></span>

* <span data-ttu-id="56d4c-208">Nakonfigurujte dva ukázkové zařízení ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="56d4c-209">Sestavení IoT Edge ve vašem zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56d4c-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="56d4c-210">Nakonfigurujte a spusťte ukázkové lit na vašem zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56d4c-210">Configure and run the BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="56d4c-211">V době psaní podporuje IoT Edge moduly zakázat pouze v brány se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="56d4c-211">At the time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="56d4c-212">Konfigurovat dva ukázkové zařízení ve službě IoT Hub</span><span class="sxs-lookup"><span data-stu-id="56d4c-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="56d4c-213">[Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, je třeba název centra pro dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need the name of your hub to complete this walkthrough.</span></span> <span data-ttu-id="56d4c-214">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="56d4c-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="56d4c-215">Přidat jednu zařízení s názvem **SensorTag_01** do služby IoT hub a zaznamenání jeho id a zařízení klíče.</span><span class="sxs-lookup"><span data-stu-id="56d4c-215">Add one device called **SensorTag_01** to your IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="56d4c-216">Můžete použít [Průzkumníka zařízení nebo iothub-explorer] [ lnk-explorer-tools] nástroje pro přidání tohoto zařízení ke službě IoT hub, který jste vytvořili v předchozím kroku a k načítání svůj klíč.</span><span class="sxs-lookup"><span data-stu-id="56d4c-216">You can use the [device explorer or iothub-explorer][lnk-explorer-tools] tools to add this device to the IoT hub you created in the previous step and to retrieve its key.</span></span> <span data-ttu-id="56d4c-217">Toto zařízení do zařízení SensorTag mapy, při konfiguraci brány.</span><span class="sxs-lookup"><span data-stu-id="56d4c-217">You map this device to the SensorTag device when you configure the gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="56d4c-218">Vytvoření Azure IoT Edge ve vaší Malinová pí 3</span><span class="sxs-lookup"><span data-stu-id="56d4c-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="56d4c-219">Nainstalujte závislosti pro hraniční Azure IoT:</span><span class="sxs-lookup"><span data-stu-id="56d4c-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="56d4c-220">Použijte následující příkazy ke klonování IoT okraj a všechny jeho submodules k domovskému adresáři:</span><span class="sxs-lookup"><span data-stu-id="56d4c-220">Use the following commands to clone IoT Edge and all its submodules to your home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="56d4c-221">Až budete mít úplnou kopii IoT Edge úložiště na vaše malin pí 3, můžete vytvořit pomocí následujícího příkazu ze složky, která obsahuje sadu SDK:</span><span class="sxs-lookup"><span data-stu-id="56d4c-221">When you have a complete copy of the IoT Edge repository on your Raspberry Pi 3, you can build it using the following command from the folder that contains the SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="56d4c-222">Nakonfigurujte a spusťte ukázkové lit na vaše malin pí 3</span><span class="sxs-lookup"><span data-stu-id="56d4c-222">Configure and run the BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="56d4c-223">Bootstrap a spustit ukázku, musíte nakonfigurovat každý IoT Edge modul, který se účastní v bráně.</span><span class="sxs-lookup"><span data-stu-id="56d4c-223">To bootstrap and run the sample, you must configure each IoT Edge module that participates in the gateway.</span></span> <span data-ttu-id="56d4c-224">Tato konfigurace je součástí souboru JSON a je nutné nakonfigurovat pět zúčastněných moduly IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="56d4c-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="56d4c-225">Není k dispozici ukázkový soubor JSON v úložišti názvem **brány\_sample.json** , můžete použít jako výchozí bod pro vytváření konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="56d4c-225">There is a sample JSON file in the repository called **gateway\_sample.json** that you can use as the starting point for building your own configuration file.</span></span> <span data-ttu-id="56d4c-226">Tento soubor je v **ukázky/ble_gateway/src** složky v místní kopii úložiště IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="56d4c-226">This file is in the **samples/ble_gateway/src** folder in local copy of the IoT Edge repository.</span></span>

<span data-ttu-id="56d4c-227">Následující části popisují, jak upravit tento konfigurační soubor pro ukázku zakázat a předpokládá, že je úložiště IoT Edge v **/home/pi/iot-edge /** složky na vaše malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="56d4c-227">The following sections describe how to edit this configuration file for the BLE sample and assume that the IoT Edge repository is in the **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="56d4c-228">Pokud úložiště je jinde, odpovídajícím způsobem upravte cesty.</span><span class="sxs-lookup"><span data-stu-id="56d4c-228">If the repository is elsewhere, adjust the paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="56d4c-229">Konfigurace protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="56d4c-229">Logger configuration</span></span>

<span data-ttu-id="56d4c-230">Za předpokladu, že úložiště brány se nachází v **/home/pi/iot-edge /** složku, nakonfigurujte modul protokolovacího nástroje takto:</span><span class="sxs-lookup"><span data-stu-id="56d4c-230">Assuming the gateway repository is located in the **/home/pi/iot-edge/** folder, configure the logger module as follows:</span></span>

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a><span data-ttu-id="56d4c-231">Konfigurace modulu zakázat</span><span class="sxs-lookup"><span data-stu-id="56d4c-231">BLE module configuration</span></span>

<span data-ttu-id="56d4c-232">Ukázka konfigurace zařízení zakázat předpokládá zařízením Texas Instruments SensorTag.</span><span class="sxs-lookup"><span data-stu-id="56d4c-232">The sample configuration for the BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="56d4c-233">Jakékoli standardní zakázat zařízení, které může fungovat jako GATT periferní by měla fungovat, ale pravděpodobně vyžadovat aktualizaci GATT charakteristik ID a data.</span><span class="sxs-lookup"><span data-stu-id="56d4c-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need to update the GATT characteristic IDs and data.</span></span> <span data-ttu-id="56d4c-234">Přidáte adresu MAC svého zařízení SensorTag:</span><span class="sxs-lookup"><span data-stu-id="56d4c-234">Add the MAC address of your SensorTag device:</span></span>

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

<span data-ttu-id="56d4c-235">Pokud nepoužíváte zařízení SensorTag, přečtěte si dokumentaci k zařízení zakázat k určení, jestli je potřeba aktualizovat vlastnosti GATT ID a hodnot dat.</span><span class="sxs-lookup"><span data-stu-id="56d4c-235">If you are not using a SensorTag device, review the documentation for your BLE device to determine whether you need to update the GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="56d4c-236">Modul služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="56d4c-236">IoT Hub module</span></span>

<span data-ttu-id="56d4c-237">Přidejte název služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-237">Add the name of your IoT Hub.</span></span> <span data-ttu-id="56d4c-238">Hodnota přípony je obvykle **azure devices.net**:</span><span class="sxs-lookup"><span data-stu-id="56d4c-238">The suffix value is typically **azure-devices.net**:</span></span>

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="56d4c-239">Konfigurace modulu mapování identit</span><span class="sxs-lookup"><span data-stu-id="56d4c-239">Identity mapping module configuration</span></span>

<span data-ttu-id="56d4c-240">Přidat adresa MAC zařízení SensorTag a ID zařízení a klíč **SensorTag_01** zařízení, které jste přidali do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="56d4c-240">Add the MAC address of your SensorTag device and the device ID and key of the **SensorTag_01** device you added to your IoT Hub:</span></span>

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="56d4c-241">Konfigurace modulu zakázat tiskárny</span><span class="sxs-lookup"><span data-stu-id="56d4c-241">BLE Printer module configuration</span></span>

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="56d4c-242">Konfigurace modulu BLEC2D</span><span class="sxs-lookup"><span data-stu-id="56d4c-242">BLEC2D Module Configuration</span></span>

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a><span data-ttu-id="56d4c-243">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="56d4c-243">Routing Configuration</span></span>

<span data-ttu-id="56d4c-244">Následující konfigurace zajišťuje následující směrování mezi moduly hraniční IoT:</span><span class="sxs-lookup"><span data-stu-id="56d4c-244">The following configuration ensures the following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="56d4c-245">**Protokolovač** modul přijímá a zaznamenává všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="56d4c-245">The **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="56d4c-246">**SensorTag** modul odesílá zprávy i **mapování** a **zakázat tiskárny** moduly.</span><span class="sxs-lookup"><span data-stu-id="56d4c-246">The **SensorTag** module sends messages to both the **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="56d4c-247">**Mapování** modul odesílá zprávy a pokuste se **IoTHub** modulu k odeslání služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-247">The **mapping** module sends messages to the **IoTHub** module to be sent up to your IoT Hub.</span></span>
* <span data-ttu-id="56d4c-248">**IoTHub** modul odesílá zprávy zpět do **mapování** modulu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-248">The **IoTHub** module sends messages back to the **mapping** module.</span></span>
* <span data-ttu-id="56d4c-249">**Mapování** modul odesílá zprávy a pokuste se **BLEC2D** modulu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-249">The **mapping** module sends messages to the **BLEC2D** module.</span></span>
* <span data-ttu-id="56d4c-250">**BLEC2D** modul odesílá zprávy zpět do **Sensortag** modulu.</span><span class="sxs-lookup"><span data-stu-id="56d4c-250">The **BLEC2D** module sends messages back to the **Sensor Tag** module.</span></span>

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

<span data-ttu-id="56d4c-251">Ke spuštění ukázky, předat konfigurační soubor JSON cestu jako parametr, který se **lit\_brány** binární.</span><span class="sxs-lookup"><span data-stu-id="56d4c-251">To run the sample, pass the path to the JSON configuration file as a parameter to the **ble\_gateway** binary.</span></span> <span data-ttu-id="56d4c-252">Tento příkaz předpokládá, že používáte **gateway_sample.json** konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="56d4c-252">The following command assumes you are using the **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="56d4c-253">Provedení tohoto příkazu z **iot hranou** složky na malin platformy:</span><span class="sxs-lookup"><span data-stu-id="56d4c-253">Execute this command from the **iot-edge** folder on the Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="56d4c-254">Potřebujete stiskněte tlačítko malé na zařízení SensorTag zjistitelnost před spuštěním ukázky.</span><span class="sxs-lookup"><span data-stu-id="56d4c-254">You may need to press the small button on the SensorTag device to make it discoverable before you run the sample.</span></span>

<span data-ttu-id="56d4c-255">Když spustíte ukázku, můžete použít [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroje ke sledování IoT hraniční brána předává ze zařízení SensorTag zprávy.</span><span class="sxs-lookup"><span data-stu-id="56d4c-255">When you run the sample, you can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to monitor the messages the IoT Edge gateway forwards from the SensorTag device.</span></span> <span data-ttu-id="56d4c-256">Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="56d4c-256">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="56d4c-257">Odesílání zpráv z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="56d4c-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="56d4c-258">Zakázat modul rovněž podporuje odesílání příkazů ze služby IoT Hub zařízení.</span><span class="sxs-lookup"><span data-stu-id="56d4c-258">The BLE module also supports sending commands from IoT Hub to the device.</span></span> <span data-ttu-id="56d4c-259">Můžete použít [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj k odeslání zprávy JSON, které modul zakázat brány předá k zařízení zakázat.</span><span class="sxs-lookup"><span data-stu-id="56d4c-259">You can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to send JSON messages that the BLE gateway module forwards on to the BLE device.</span></span>

<span data-ttu-id="56d4c-260">Pokud používáte zařízení Texas Instruments SensorTag, můžete zapnout červené DIODU, zelená DIODU nebo bzučák odesláním příkazů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="56d4c-260">If you are using the Texas Instruments SensorTag device, you can turn on the red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="56d4c-261">Před odesláním příkazy ze služby IoT Hub, nejprve odeslat následující dvě zprávy JSON v pořadí.</span><span class="sxs-lookup"><span data-stu-id="56d4c-261">Before you send commands from IoT Hub, first send the following two JSON messages in order.</span></span> <span data-ttu-id="56d4c-262">Potom můžete odeslat žádné příkazy zapnutí indikátory nebo bzučák.</span><span class="sxs-lookup"><span data-stu-id="56d4c-262">Then you can send any of the commands to turn on the lights or buzzer.</span></span>

1. <span data-ttu-id="56d4c-263">Resetovat všechny LED a a odpověď (jejich vypnutím):</span><span class="sxs-lookup"><span data-stu-id="56d4c-263">Reset all LEDs and the buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="56d4c-264">Nakonfigurujte vstupně-výstupních operací jako 'vzdálené':</span><span class="sxs-lookup"><span data-stu-id="56d4c-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="56d4c-265">Nyní můžete odeslat žádný z následujících příkazů pro zapnutí indikátory nebo bzučák na zařízení SensorTag:</span><span class="sxs-lookup"><span data-stu-id="56d4c-265">Now you can send any of the following commands to turn on the lights or buzzer on the SensorTag device:</span></span>

* <span data-ttu-id="56d4c-266">Zapněte red DIODU:</span><span class="sxs-lookup"><span data-stu-id="56d4c-266">Turn on the red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="56d4c-267">Zapněte zelená DIODU:</span><span class="sxs-lookup"><span data-stu-id="56d4c-267">Turn on the green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="56d4c-268">Zapněte a odpověď:</span><span class="sxs-lookup"><span data-stu-id="56d4c-268">Turn on the buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="56d4c-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="56d4c-269">Next Steps</span></span>

<span data-ttu-id="56d4c-270">Pokud chcete získat rozsáhlejšími znalostmi IoT okraj a Experimentujte s příklady kódu, přejděte na následující kurzy developer a prostředky:</span><span class="sxs-lookup"><span data-stu-id="56d4c-270">If you want to gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="56d4c-271">[Azure IoT Edge][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="56d4c-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="56d4c-272">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="56d4c-272">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="56d4c-273">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="56d4c-273">[IoT Hub developer guide][lnk-devguide]</span></span>

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
