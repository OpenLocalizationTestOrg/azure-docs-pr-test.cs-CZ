---
title: "aaaUse fyzického zařízení s hranou Azure IoT | Microsoft Docs"
description: "Jak toouse rozbočovači Texas Instruments SensorTag zařízení toosend data tooan IoT prostřednictvím brány IoT Edge spuštěné na zařízení malin pí 3. Brána Hello vytvořená s využitím Azure IoT okraj."
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
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="bcb5e-104">Použití Azure IoT Edge na tooIoT zpráv typu zařízení cloud tooforward malin pí rozbočovače</span><span class="sxs-lookup"><span data-stu-id="bcb5e-104">Use Azure IoT Edge on a Raspberry Pi tooforward device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="bcb5e-105">Tento návod hello [Bluetooth nízkou energie ukázka] [ lnk-ble-samplecode] ukazuje, jak toouse [Azure IoT Edge] [ lnk-sdk] na:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-105">This walkthrough of hello [Bluetooth low energy sample][lnk-ble-samplecode] shows you how toouse [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="bcb5e-106">Předat dál zařízení cloud telemetrie tooIoT centra z fyzického zařízení.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-106">Forward device-to-cloud telemetry tooIoT Hub from a physical device.</span></span>
* <span data-ttu-id="bcb5e-107">Směrování příkazů z fyzického zařízení IoT Hub tooa.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-107">Route commands from IoT Hub tooa physical device.</span></span>

<span data-ttu-id="bcb5e-108">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-108">This walkthrough covers:</span></span>

* <span data-ttu-id="bcb5e-109">**Architektura**: důležité architektury informace o ukázkové nízkou energie hello Bluetooth.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-109">**Architecture**: important architectural information about hello Bluetooth low energy sample.</span></span>
* <span data-ttu-id="bcb5e-110">**Sestavení a spuštění**: požadované toobuild hello kroky a spusťte hello ukázková.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-110">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="bcb5e-111">Architektura</span><span class="sxs-lookup"><span data-stu-id="bcb5e-111">Architecture</span></span>

<span data-ttu-id="bcb5e-112">Hello návod ukazuje, jak toobuild a spusťte IoT vstupní brána v pí 3 malin používající Raspbian Linux.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-112">hello walkthrough shows you how toobuild and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="bcb5e-113">Hello brány je sestaven pomocí IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-113">hello gateway is built using IoT Edge.</span></span> <span data-ttu-id="bcb5e-114">Ukázka Hello používá data teploty toocollect zařízení Texas Instruments SensorTag Bluetooth nízká energie (Povolit).</span><span class="sxs-lookup"><span data-stu-id="bcb5e-114">hello sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device toocollect temperature data.</span></span>

<span data-ttu-id="bcb5e-115">Když spustíte hello IoT hraniční brána je:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-115">When you run hello IoT Edge gateway it:</span></span>

* <span data-ttu-id="bcb5e-116">Připojí zařízení SensorTag tooa pomocí protokolu hello Bluetooth nízká energie (Povolit).</span><span class="sxs-lookup"><span data-stu-id="bcb5e-116">Connects tooa SensorTag device using hello Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="bcb5e-117">Připojí tooIoT centra pomocí protokolu hello HTTP.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-117">Connects tooIoT Hub using hello HTTP protocol.</span></span>
* <span data-ttu-id="bcb5e-118">Předává telemetrie z tooIoT zařízení SensorTag hello rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-118">Forwards telemetry from hello SensorTag device tooIoT Hub.</span></span>
* <span data-ttu-id="bcb5e-119">Směrování příkazů ze zařízení SensorTag toohello IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-119">Routes commands from IoT Hub toohello SensorTag device.</span></span>

<span data-ttu-id="bcb5e-120">Brána Hello obsahuje hello následující moduly hraniční IoT:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-120">hello gateway contains hello following IoT Edge modules:</span></span>

* <span data-ttu-id="bcb5e-121">A *zakázat modulu* , rozhraní zakázat zařízení tooreceive teploty daty ze zařízení hello a odesílání příkazů toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-121">A *BLE module* that interfaces with a BLE device tooreceive temperature data from hello device and send commands toohello device.</span></span>
* <span data-ttu-id="bcb5e-122">A *zakázat cloudu toodevice modulu* který překládá JSON zprávy odeslané ze služby IoT Hub do zakázat pokyny hello *zakázat modulu*.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-122">A *BLE cloud toodevice module* that translates JSON messages sent from IoT Hub into BLE instructions for hello *BLE module*.</span></span>
* <span data-ttu-id="bcb5e-123">A *protokoly modulu* který protokoluje všechny brány zprávy tooa místního souboru.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-123">A *logger module* that logs all gateway messages tooa local file.</span></span>
* <span data-ttu-id="bcb5e-124">*Modulem mapování identit* který překládá mezi adresy MAC povolit zařízení a identit zařízení Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="bcb5e-125">*IoT Hub modulu* které odesílá telemetrická data tooan IoT hub a přijímá příkazy zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-125">An *IoT Hub module* that uploads telemetry data tooan IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="bcb5e-126">A *zakázat tiskárny modulu* , interpretuje telemetrická data ze zařízení zakázat hello a vytiskne formátovaných dat toohello konzoly tooenable řešení potíží a ladění.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-126">A *BLE printer module* that interprets telemetry from hello BLE device and prints formatted data toohello console tooenable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-hello-gateway"></a><span data-ttu-id="bcb5e-127">Tok dat prostřednictvím brány hello</span><span class="sxs-lookup"><span data-stu-id="bcb5e-127">How data flows through hello gateway</span></span>

<span data-ttu-id="bcb5e-128">Hello následující Blokový diagram znázorňuje hello telemetrie nahrávání dat toku kanál:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-128">hello following block diagram illustrates hello telemetry upload data flow pipeline:</span></span>

![Kanál brány odesílání telemetrie](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="bcb5e-130">Hello kroky, které položky telemetrie trvá, cestování z tooIoT zařízení povolit centru jsou:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-130">hello steps that an item of telemetry takes traveling from a BLE device tooIoT Hub are:</span></span>

1. <span data-ttu-id="bcb5e-131">zařízení zakázat Hello generuje ukázku teploty a odešle přes Bluetooth toohello zakázat modulu v bráně hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-131">hello BLE device generates a temperature sample and sends it over Bluetooth toohello BLE module in hello gateway.</span></span>
1. <span data-ttu-id="bcb5e-132">modul zakázat Hello obdrží hello ukázka a vydává je toohello zprostředkovatele spolu s hello adresa MAC zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-132">hello BLE module receives hello sample and publishes it toohello broker along with hello MAC address of hello device.</span></span>
1. <span data-ttu-id="bcb5e-133">modul mapování identit Hello převezme tuto zprávu a používá k interní tabulce tootranslate hello adresa MAC zařízení hello do identity zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-133">hello identity mapping module picks up this message and uses an internal table tootranslate hello MAC address of hello device into an IoT Hub device identity.</span></span> <span data-ttu-id="bcb5e-134">Identita zařízení IoT Hub se skládá z ID zařízení a klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="bcb5e-135">modul mapování identit Hello publikuje novou zprávu, která obsahuje hello teploty ukázková data, adresa MAC hello hello zařízení, hello ID zařízení a klíč zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-135">hello identity mapping module publishes a new message that contains hello temperature sample data, hello MAC address of hello device, hello device ID, and hello device key.</span></span>
1. <span data-ttu-id="bcb5e-136">Hello modulu služby IoT Hub přijímá tuto novou zprávu (generované modulem mapování identit hello) a vydává je tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-136">hello IoT Hub module receives this new message (generated by hello identity mapping module) and publishes it tooIoT Hub.</span></span>
1. <span data-ttu-id="bcb5e-137">modul protokolovacího nástroje Hello zaznamená všechny zprávy z místního souboru tooa hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-137">hello logger module logs all messages from hello broker tooa local file.</span></span>

<span data-ttu-id="bcb5e-138">Hello následující Blokový diagram znázorňuje hello zařízení příkaz datovém toku kanálu:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-138">hello following block diagram illustrates hello device command data flow pipeline:</span></span>

![Zařízení příkaz brány kanálu](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="bcb5e-140">Hello modulu pravidelně se dotazuje služby IoT Hub hello IoT hub pro nové zprávy příkaz.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-140">hello IoT Hub module periodically polls hello IoT hub for new command messages.</span></span>
1. <span data-ttu-id="bcb5e-141">Když hello IoT Hub modulu přijme zprávu o nový příkaz, tato možnost publikuje ho toohello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-141">When hello IoT Hub module receives a new command message, it publishes it toohello broker.</span></span>
1. <span data-ttu-id="bcb5e-142">modul mapování identit Hello převezme zprávou příkazu hello a používá k interní tabulce tootranslate hello IoT Hub zařízení ID tooa zařízení adresa MAC.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-142">hello identity mapping module picks up hello command message and uses an internal table tootranslate hello IoT Hub device ID tooa device MAC address.</span></span> <span data-ttu-id="bcb5e-143">Pak publikuje novou zprávu, která obsahuje adresu MAC hello hello cílové zařízení v mapě vlastnosti hello hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-143">It then publishes a new message that includes hello MAC address of hello target device in hello properties map of hello message.</span></span>
1. <span data-ttu-id="bcb5e-144">modul zakázat Cloud-zařízení Hello převezme tuto zprávu a překládá do hello správné zakázat instrukce pro modul zakázat hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-144">hello BLE Cloud-to-Device module picks up this message and translates it into hello proper BLE instruction for hello BLE module.</span></span> <span data-ttu-id="bcb5e-145">Pak publikuje novou zprávu.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="bcb5e-146">modul zakázat Hello převezme tuto zprávu a provede hello vstupně-výstupních operací instrukci navázat komunikaci s hello zakázat zařízení.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-146">hello BLE module picks up this message and executes hello I/O instruction by communicating with hello BLE device.</span></span>
1. <span data-ttu-id="bcb5e-147">Hello protokoly modulu zaznamená všechny zprávy ze souboru disku tooa hello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-147">hello logger module logs all messages from hello broker tooa disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bcb5e-148">Požadavky</span><span class="sxs-lookup"><span data-stu-id="bcb5e-148">Prerequisites</span></span>

<span data-ttu-id="bcb5e-149">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-149">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="bcb5e-150">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="bcb5e-151">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="bcb5e-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="bcb5e-152">Musíte klient SSH na vaše tooenable stolní počítače můžete tooremotely hello se příkaz access řádek na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-152">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="bcb5e-153">Windows nezahrnuje klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="bcb5e-154">Doporučujeme používat [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="bcb5e-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="bcb5e-155">Většina Linuxových distribucích a Mac OS, obsahují hello nástroj příkazového řádku SSH.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-155">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="bcb5e-156">Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="bcb5e-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="bcb5e-157">Připravte svůj hardware</span><span class="sxs-lookup"><span data-stu-id="bcb5e-157">Prepare your hardware</span></span>

<span data-ttu-id="bcb5e-158">Tento kurz předpokládá, že používáte [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) zařízení připojené tooa malin pí 3 systémem Raspbian.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected tooa Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="bcb5e-159">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="bcb5e-159">Install Raspbian</span></span>

<span data-ttu-id="bcb5e-160">Můžete použít buď hello následující možnosti tooinstall Raspbian na vašem zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-160">You can use either of hello following options tooinstall Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="bcb5e-161">tooinstall hello nejnovější verzi Raspbian, použijte hello [NOOBS] [ lnk-noobs] grafické uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-161">tooinstall hello latest version of Raspbian, use hello [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="bcb5e-162">Ručně [Stáhnout] [ lnk-raspbian] a zapsat nejnovější bitovou kopii hello hello Raspbian operačního systému tooan SD karty.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-162">Manually [download][lnk-raspbian] and write hello latest image of hello Raspbian operating system tooan SD card.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="bcb5e-163">Přihlaste se a přístup k Terminálové hello</span><span class="sxs-lookup"><span data-stu-id="bcb5e-163">Sign in and access hello terminal</span></span>

<span data-ttu-id="bcb5e-164">Dvě možnosti tooaccess máte na vaše platformy malin terminálu prostředí:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-164">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="bcb5e-165">Pokud máte klávesnici a sledování připojených tooyour malin platformy, můžete použít grafické uživatelské rozhraní Raspbian tooaccess hello okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-165">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

* <span data-ttu-id="bcb5e-166">Přístup hello příkazového řádku na vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-166">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="bcb5e-167">Použijte okno terminálu v hello grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="bcb5e-167">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="bcb5e-168">uživatelské jméno jsou Hello výchozí pověření pro Raspbian **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-168">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="bcb5e-169">Hello hlavním panelu v hello grafického uživatelského rozhraní, můžete spustit hello **Terminálové** nástroj pomocí hello ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-169">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="bcb5e-170">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="bcb5e-170">Sign in with SSH</span></span>

<span data-ttu-id="bcb5e-171">SSH můžete použít pro přístup přes příkazový řádek tooyour malin pí.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-171">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="bcb5e-172">článek Hello [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje, jak tooconfigure SSH na vaše malin platformy a jak tooconnect z [Windows] [ lnk-ssh-windows] nebo [Operačního systému Linux & Mac][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="bcb5e-172">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="bcb5e-173">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="bcb5e-174">Nainstalujte BlueZ 5.37</span><span class="sxs-lookup"><span data-stu-id="bcb5e-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="bcb5e-175">moduly zakázat Hello komunikovat hardwaru Bluetooth toohello prostřednictvím hello BlueZ zásobníku.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-175">hello BLE modules talk toohello Bluetooth hardware via hello BlueZ stack.</span></span> <span data-ttu-id="bcb5e-176">Potřebujete 5.37 verzi BlueZ pro hello moduly toowork správně.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-176">You need version 5.37 of BlueZ for hello modules toowork correctly.</span></span> <span data-ttu-id="bcb5e-177">Tyto pokyny Ujistěte se, že je nainstalována správná verze BlueZ hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-177">These instructions make sure hello correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="bcb5e-178">Zastavte hello aktuální démon bluetooth:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-178">Stop hello current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="bcb5e-179">Nainstalujte hello BlueZ závislosti:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-179">Install hello BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="bcb5e-180">Stáhněte hello BlueZ zdrojového kódu z bluez.org:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-180">Download hello BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="bcb5e-181">Rozbalte hello zdrojového kódu:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-181">Unzip hello source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="bcb5e-182">Změna složky pro nově vytvořený toohello adresáře:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-182">Change directories toohello newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="bcb5e-183">Nakonfigurujte hello BlueZ kód toobe vytvořené:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-183">Configure hello BlueZ code toobe built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="bcb5e-184">Sestavení BlueZ:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="bcb5e-185">Až se to dokončí instalaci BlueZ stavbě:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="bcb5e-186">Změna konfigurace služby systemd pro bluetooth tak odkazuje toohello nové démon bluetooth v souboru hello `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-186">Change systemd service configuration for bluetooth so it points toohello new bluetooth daemon in hello file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="bcb5e-187">Nahraďte řádku 'ExecStart' hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-187">Replace hello 'ExecStart' line with hello following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="bcb5e-188">Povolit zařízení SensorTag toohello připojení ze zařízení malin pí 3</span><span class="sxs-lookup"><span data-stu-id="bcb5e-188">Enable connectivity toohello SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="bcb5e-189">Před spuštěné ukázkový text hello musíte tooverify, že vaše malin pí 3 se může připojit zařízení SensorTag toohello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-189">Before running hello sample, you need tooverify that your Raspberry Pi 3 can connect toohello SensorTag device.</span></span>

1. <span data-ttu-id="bcb5e-190">Ujistěte se, hello `rfkill` je nainstalován nástroj:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-190">Ensure hello `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="bcb5e-191">Odblokování bluetooth na hello malin pí 3 a zkontrolujte, zda text hello číslo verze **5.37**:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-191">Unblock bluetooth on hello Raspberry Pi 3 and check that hello version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="bcb5e-192">spuštění služby bluetooth hello tooenter hello interaktivní bluetooth prostředí a spusťte hello **bluetoothctl** příkaz:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-192">tooenter hello interactive bluetooth shell, start hello bluetooth service and execute hello **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="bcb5e-193">Zadejte příkaz hello **zapnutí** toopower až hello bluetooth řadiče.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-193">Enter hello command **power on** toopower up hello bluetooth controller.</span></span> <span data-ttu-id="bcb5e-194">příkaz Hello vrátí podobné toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-194">hello command returns output similar toohello following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="bcb5e-195">V prostředí hello interaktivní bluetooth, zadejte příkaz hello **kontrolu** tooscan pro zařízeními bluetooth.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-195">In hello interactive bluetooth shell, enter hello command **scan on** tooscan for bluetooth devices.</span></span> <span data-ttu-id="bcb5e-196">příkaz Hello vrátí podobné toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-196">hello command returns output similar toohello following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="bcb5e-197">Zařízení SensorTag hello zjistitelnost stisknutím tlačítka na malé hello (hello, které by měl zelená DIODU flash).</span><span class="sxs-lookup"><span data-stu-id="bcb5e-197">Make hello SensorTag device discoverable by pressing hello small button (hello green LED should flash).</span></span> <span data-ttu-id="bcb5e-198">Hello malin pí 3 by měl zjistit zařízení SensorTag hello:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-198">hello Raspberry Pi 3 should discover hello SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="bcb5e-199">V tomto příkladu uvidíte, že hello adresu MAC hello zařízení SensorTag **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-199">In this example, you can see that hello MAC address of hello SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="bcb5e-200">Vypnout kontrolu zadáním hello **kontrolovat vypnout** příkaz:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-200">Turn off scanning by entering hello **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="bcb5e-201">Připojení zařízení SensorTag tooyour pomocí adresu MAC zadáním **připojit \<adresa MAC\>**.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-201">Connect tooyour SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="bcb5e-202">Následující ukázkový výstup Hello je zkratka pro přehlednost:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-202">hello following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
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

    > <span data-ttu-id="bcb5e-203">Můžete vytvořit seznam vlastností GATT hello hello zařízení znovu s použitím hello **atributy seznamu** příkaz.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-203">You can list hello GATT characteristics of hello device again using hello **list-attributes** command.</span></span>

1. <span data-ttu-id="bcb5e-204">Teď můžete odpojit od hello zařízení pomocí hello **odpojit** příkaz a poté ukončete z prostředí bluetooth hello pomocí hello **ukončení** příkaz:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-204">You can now disconnect from hello device using hello **disconnect** command and then exit from hello bluetooth shell using hello **quit** command:</span></span>

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="bcb5e-205">Nyní jste připravené toorun hello lit IoT Edge ukázka na vaše malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-205">You're now ready toorun hello BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-hello-iot-edge-ble-sample"></a><span data-ttu-id="bcb5e-206">Spuštění ukázkové IoT Edge zakázat hello</span><span class="sxs-lookup"><span data-stu-id="bcb5e-206">Run hello IoT Edge BLE sample</span></span>

<span data-ttu-id="bcb5e-207">Ukázka IoT Edge zakázat hello toorun, je třeba toocomplete tři úlohy:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-207">toorun hello IoT Edge BLE sample, you need toocomplete three tasks:</span></span>

* <span data-ttu-id="bcb5e-208">Nakonfigurujte dva ukázkové zařízení ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="bcb5e-209">Sestavení IoT Edge ve vašem zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="bcb5e-210">Nakonfigurujte a spusťte ukázkové zakázat hello na vašem zařízení malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-210">Configure and run hello BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="bcb5e-211">V době psaní textu hello podporuje IoT Edge moduly zakázat pouze v brány se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-211">At hello time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="bcb5e-212">Konfigurovat dva ukázkové zařízení ve službě IoT Hub</span><span class="sxs-lookup"><span data-stu-id="bcb5e-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="bcb5e-213">[Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, potřebujete hello název vašeho centra toocomplete tento návod.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="bcb5e-214">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="bcb5e-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="bcb5e-215">Přidat jednu zařízení s názvem **SensorTag_01** tooyour IoT hub a poznamenejte si jeho id a zařízení klíče.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-215">Add one device called **SensorTag_01** tooyour IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="bcb5e-216">Můžete použít hello [Průzkumníka zařízení nebo iothub-explorer] [ lnk-explorer-tools] nástroje tooadd toto centrum IoT zařízení toohello jste vytvořili v předchozím kroku hello a tooretrieve svůj klíč.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-216">You can use hello [device explorer or iothub-explorer][lnk-explorer-tools] tools tooadd this device toohello IoT hub you created in hello previous step and tooretrieve its key.</span></span> <span data-ttu-id="bcb5e-217">Toto zařízení SensorTag toohello zařízení mapy, při konfiguraci brány hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-217">You map this device toohello SensorTag device when you configure hello gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="bcb5e-218">Vytvoření Azure IoT Edge ve vaší Malinová pí 3</span><span class="sxs-lookup"><span data-stu-id="bcb5e-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="bcb5e-219">Nainstalujte závislosti pro hraniční Azure IoT:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="bcb5e-220">Použití hello následující příkazy tooclone IoT okraj a všechny jeho submodules tooyour domovského adresáře:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-220">Use hello following commands tooclone IoT Edge and all its submodules tooyour home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="bcb5e-221">Až budete mít úplnou kopii hello IoT Edge úložiště na vaše malin pí 3, můžete vytvořit pomocí hello následující příkaz z hello složku, která obsahuje hello SDK:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-221">When you have a complete copy of hello IoT Edge repository on your Raspberry Pi 3, you can build it using hello following command from hello folder that contains hello SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="bcb5e-222">Nakonfigurujte a spusťte ukázkové zakázat hello na vaše malin pí 3</span><span class="sxs-lookup"><span data-stu-id="bcb5e-222">Configure and run hello BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="bcb5e-223">toobootstrap a spuštění hello ukázková, musíte nakonfigurovat každý IoT Edge modul, který se účastní v bráně hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-223">toobootstrap and run hello sample, you must configure each IoT Edge module that participates in hello gateway.</span></span> <span data-ttu-id="bcb5e-224">Tato konfigurace je součástí souboru JSON a je nutné nakonfigurovat pět zúčastněných moduly IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="bcb5e-225">Není k dispozici ukázkový soubor JSON v úložišti hello názvem **brány\_sample.json** , můžete použít jako výchozí bod pro vytváření konfiguračního souboru hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-225">There is a sample JSON file in hello repository called **gateway\_sample.json** that you can use as hello starting point for building your own configuration file.</span></span> <span data-ttu-id="bcb5e-226">Tento soubor je v hello **ukázky/ble_gateway/src** složky v místní kopii hello IoT Edge úložiště.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-226">This file is in hello **samples/ble_gateway/src** folder in local copy of hello IoT Edge repository.</span></span>

<span data-ttu-id="bcb5e-227">Hello následující části popisují, jak je tooedit tato konfigurace soubor pro ukázku zakázat hello a předpokládá, že hello IoT Edge úložiště v hello **/home/pi/iot-edge /** složky na vaše malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-227">hello following sections describe how tooedit this configuration file for hello BLE sample and assume that hello IoT Edge repository is in hello **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="bcb5e-228">Pokud je úložiště hello jinde, odpovídajícím způsobem upravte hello cesty.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-228">If hello repository is elsewhere, adjust hello paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="bcb5e-229">Konfigurace protokolovacího nástroje</span><span class="sxs-lookup"><span data-stu-id="bcb5e-229">Logger configuration</span></span>

<span data-ttu-id="bcb5e-230">Za předpokladu, že úložiště brány hello se nachází v hello **/home/pi/iot-edge /** složku, nakonfigurujte hello protokoly modulu takto:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-230">Assuming hello gateway repository is located in hello **/home/pi/iot-edge/** folder, configure hello logger module as follows:</span></span>

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

#### <a name="ble-module-configuration"></a><span data-ttu-id="bcb5e-231">Konfigurace modulu zakázat</span><span class="sxs-lookup"><span data-stu-id="bcb5e-231">BLE module configuration</span></span>

<span data-ttu-id="bcb5e-232">Hello Ukázková konfigurace pro zařízení zakázat hello předpokládá zařízením Texas Instruments SensorTag.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-232">hello sample configuration for hello BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="bcb5e-233">Jakékoli standardní zakázat zařízení, které může fungovat jako by měla fungovat GATT periferní však můžete potřebovat tooupdate hello GATT vlastnosti ID a data.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need tooupdate hello GATT characteristic IDs and data.</span></span> <span data-ttu-id="bcb5e-234">Přidáte adresu MAC hello zařízení SensorTag:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-234">Add hello MAC address of your SensorTag device:</span></span>

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

<span data-ttu-id="bcb5e-235">Pokud nepoužíváte zařízení SensorTag, najdete v dokumentaci hello pro vaše zařízení toodetermine zakázat jestli potřebujete tooupdate hello GATT vlastnosti ID a datové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-235">If you are not using a SensorTag device, review hello documentation for your BLE device toodetermine whether you need tooupdate hello GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="bcb5e-236">Modul služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="bcb5e-236">IoT Hub module</span></span>

<span data-ttu-id="bcb5e-237">Přidáte hello název služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-237">Add hello name of your IoT Hub.</span></span> <span data-ttu-id="bcb5e-238">Hodnota přípony Hello je obvykle **azure devices.net**:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-238">hello suffix value is typically **azure-devices.net**:</span></span>

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

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="bcb5e-239">Konfigurace modulu mapování identit</span><span class="sxs-lookup"><span data-stu-id="bcb5e-239">Identity mapping module configuration</span></span>

<span data-ttu-id="bcb5e-240">Přidat adresu MAC hello zařízení SensorTag a hello ID zařízení a klíč hello **SensorTag_01** zařízení, které jste přidali tooyour IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-240">Add hello MAC address of your SensorTag device and hello device ID and key of hello **SensorTag_01** device you added tooyour IoT Hub:</span></span>

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

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="bcb5e-241">Konfigurace modulu zakázat tiskárny</span><span class="sxs-lookup"><span data-stu-id="bcb5e-241">BLE Printer module configuration</span></span>

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

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="bcb5e-242">Konfigurace modulu BLEC2D</span><span class="sxs-lookup"><span data-stu-id="bcb5e-242">BLEC2D Module Configuration</span></span>

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

#### <a name="routing-configuration"></a><span data-ttu-id="bcb5e-243">Konfigurace směrování</span><span class="sxs-lookup"><span data-stu-id="bcb5e-243">Routing Configuration</span></span>

<span data-ttu-id="bcb5e-244">Hello následující konfigurace zajišťuje hello následující směrování mezi moduly hraniční IoT:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-244">hello following configuration ensures hello following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="bcb5e-245">Hello **Protokolovač** modul přijímá a zaznamenává všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-245">hello **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="bcb5e-246">Hello **SensorTag** modul odesílá zprávy tooboth hello **mapování** a **zakázat tiskárny** moduly.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-246">hello **SensorTag** module sends messages tooboth hello **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="bcb5e-247">Hello **mapování** modul odesílá zprávy toohello **IoTHub** toobe modulu správně tooyour IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-247">hello **mapping** module sends messages toohello **IoTHub** module toobe sent up tooyour IoT Hub.</span></span>
* <span data-ttu-id="bcb5e-248">Hello **IoTHub** modul odesílá zprávy zpět toohello **mapování** modulu.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-248">hello **IoTHub** module sends messages back toohello **mapping** module.</span></span>
* <span data-ttu-id="bcb5e-249">Hello **mapování** modul odesílá zprávy toohello **BLEC2D** modulu.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-249">hello **mapping** module sends messages toohello **BLEC2D** module.</span></span>
* <span data-ttu-id="bcb5e-250">Hello **BLEC2D** modul odesílá zprávy zpět toohello **Sensortag** modulu.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-250">hello **BLEC2D** module sends messages back toohello **Sensor Tag** module.</span></span>

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

<span data-ttu-id="bcb5e-251">Ukázka hello toorun, pass hello cesta toohello JSON konfiguračním souboru jako parametr toohello **lit\_brány** binární.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-251">toorun hello sample, pass hello path toohello JSON configuration file as a parameter toohello **ble\_gateway** binary.</span></span> <span data-ttu-id="bcb5e-252">Hello následující příkaz předpokládá, že používáte hello **gateway_sample.json** konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-252">hello following command assumes you are using hello **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="bcb5e-253">Provedení tohoto příkazu z hello **iot hranou** složky na hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-253">Execute this command from hello **iot-edge** folder on hello Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="bcb5e-254">Může být nutné toopress hello malé tlačítko toomake zařízení SensorTag hello je zjistitelný před spuštěním ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-254">You may need toopress hello small button on hello SensorTag device toomake it discoverable before you run hello sample.</span></span>

<span data-ttu-id="bcb5e-255">Když spustíte ukázkový text hello, můžete použít hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo hello [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj toomonitor hello zprávy hello IoT hraniční brána předává ze zařízení SensorTag hello.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-255">When you run hello sample, you can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toomonitor hello messages hello IoT Edge gateway forwards from hello SensorTag device.</span></span> <span data-ttu-id="bcb5e-256">Například pomocí iothub-explorer můžete monitorovat zpráv typu zařízení cloud pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-256">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="bcb5e-257">Odesílání zpráv z cloudu do zařízení</span><span class="sxs-lookup"><span data-stu-id="bcb5e-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="bcb5e-258">Hello zakázat modul rovněž podporuje odesílání příkazů z toohello zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-258">hello BLE module also supports sending commands from IoT Hub toohello device.</span></span> <span data-ttu-id="bcb5e-259">Můžete použít hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nebo hello [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroj toosend JSON zprávy Tenhle modul hello zakázat brány předá toohello zakázat zařízení.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-259">You can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toosend JSON messages that hello BLE gateway module forwards on toohello BLE device.</span></span>

<span data-ttu-id="bcb5e-260">Pokud používáte hello Texas Instruments SensorTag zařízení, můžete zapnout hello red DIODU, zelená DIODU nebo bzučák odesláním příkazů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-260">If you are using hello Texas Instruments SensorTag device, you can turn on hello red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="bcb5e-261">Před odesláním příkazy ze služby IoT Hub, nejprve odešlete hello následující dvě zprávy JSON v pořadí.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-261">Before you send commands from IoT Hub, first send hello following two JSON messages in order.</span></span> <span data-ttu-id="bcb5e-262">Potom můžete odeslat všechny příkazy tooturn hello na hello indikátory nebo bzučák.</span><span class="sxs-lookup"><span data-stu-id="bcb5e-262">Then you can send any of hello commands tooturn on hello lights or buzzer.</span></span>

1. <span data-ttu-id="bcb5e-263">Resetovat všechny LED a bzučák hello (jejich vypnutím):</span><span class="sxs-lookup"><span data-stu-id="bcb5e-263">Reset all LEDs and hello buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="bcb5e-264">Nakonfigurujte vstupně-výstupních operací jako 'vzdálené':</span><span class="sxs-lookup"><span data-stu-id="bcb5e-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="bcb5e-265">Nyní můžete odeslat žádný z následujících příkazů tooturn na hello indikátory nebo bzučák na zařízení SensorTag hello hello:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-265">Now you can send any of hello following commands tooturn on hello lights or buzzer on hello SensorTag device:</span></span>

* <span data-ttu-id="bcb5e-266">Zapněte hello red DIODU:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-266">Turn on hello red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="bcb5e-267">Zapněte hello zelená DIODU:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-267">Turn on hello green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="bcb5e-268">Zapněte bzučák hello:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-268">Turn on hello buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="bcb5e-269">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bcb5e-269">Next Steps</span></span>

<span data-ttu-id="bcb5e-270">Pokud chcete toogain rozsáhlejšími znalostmi IoT okraj a experimentovat s příklady kódu, navštivte hello následující kurzy developer a prostředky:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-270">If you want toogain a more advanced understanding of IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="bcb5e-271">[Azure IoT Edge][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="bcb5e-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="bcb5e-272">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="bcb5e-272">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="bcb5e-273">[Příručka vývojáře pro službu IoT Hub][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="bcb5e-273">[IoT Hub developer guide][lnk-devguide]</span></span>

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
