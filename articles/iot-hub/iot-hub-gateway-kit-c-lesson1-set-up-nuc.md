---
title: "Zařízení SensorTag & brány Azure IoT - lekci 1: nastavení Intel NUC | Microsoft Docs"
description: "Nastavte Intel NUC fungovat jako bránu IoT mezi senzor a Azure IoT Hub a shromažďovat informace o senzor odeslat do služby IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT brány, intel nuc nuc počítače, DE3815TYKE"
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1a3a92ab8d08c6ed6f047208217c46022027157e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="a4bff-104">Nastavit Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="a4bff-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="a4bff-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="a4bff-105">What you will do</span></span>

- <span data-ttu-id="a4bff-106">Nastavte Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="a4bff-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="a4bff-107">Nainstalujte balíček Azure IoT Edge na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a4bff-107">Install the Azure IoT Edge package on the Intel NUC.</span></span>
- <span data-ttu-id="a4bff-108">Spuštění ukázkové aplikace na "hello_world" NUC Intel ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="a4bff-108">Run a "hello_world" sample application on the Intel NUC to verify the gateway functionality.</span></span>

  > <span data-ttu-id="a4bff-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a4bff-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a4bff-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="a4bff-110">What you will learn</span></span>

<span data-ttu-id="a4bff-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="a4bff-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="a4bff-112">Postup připojení Intel NUC s periferních zařízení.</span><span class="sxs-lookup"><span data-stu-id="a4bff-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="a4bff-113">Postup instalace a aktualizace požadované balíčky na Intel NUC pomocí inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="a4bff-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="a4bff-114">Postup spuštění ukázkové aplikace "hello_world" ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="a4bff-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a4bff-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="a4bff-115">What you need</span></span>

- <span data-ttu-id="a4bff-116">Intel NUC Kit DE3815TYKE s sadě Intel IoT brány (Linux větru oblasti * 7.0.0.13) předinstalována.</span><span class="sxs-lookup"><span data-stu-id="a4bff-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="a4bff-117">[Kliknutím sem zakoupit Groove IoT komerční brány Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="a4bff-117">[Click here to purchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="a4bff-118">Kabel Ethernet.</span><span class="sxs-lookup"><span data-stu-id="a4bff-118">An Ethernet cable.</span></span>
- <span data-ttu-id="a4bff-119">Klávesnice.</span><span class="sxs-lookup"><span data-stu-id="a4bff-119">A keyboard.</span></span>
- <span data-ttu-id="a4bff-120">HDMI nebo VGA kabel.</span><span class="sxs-lookup"><span data-stu-id="a4bff-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="a4bff-121">Monitorování s k portu HDMI nebo VGA.</span><span class="sxs-lookup"><span data-stu-id="a4bff-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="a4bff-122">Volitelné: [Texas Instruments Sensortag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="a4bff-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="a4bff-124">Připojit Intel NUC s periferních zařízení</span><span class="sxs-lookup"><span data-stu-id="a4bff-124">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="a4bff-125">Následující obrázek je příkladem NUC Intel, který je připojen k různým periferních zařízení:</span><span class="sxs-lookup"><span data-stu-id="a4bff-125">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="a4bff-126">Připojení k klávesnici.</span><span class="sxs-lookup"><span data-stu-id="a4bff-126">Connected to a keyboard.</span></span>
2. <span data-ttu-id="a4bff-127">Připojení k monitorování s kabel VGA nebo HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="a4bff-127">Connected to a monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="a4bff-128">Připojení k drátové síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="a4bff-128">Connected to a wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="a4bff-129">Připojení k napájení s napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="a4bff-129">Connected to a power supply with a power cable.</span></span>

![Intel NUC připojené k periferních zařízení](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="a4bff-131">Připojení k systému Intel NUC od hostitelského počítače pomocí protokolu Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="a4bff-131">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="a4bff-132">Budete potřebovat klávesnici a monitorování, které získat IP adresu vašeho zařízení Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a4bff-132">You will need a keyboard and a monitor to get the IP address of your Intel NUC device.</span></span> <span data-ttu-id="a4bff-133">Pokud už znáte IP adresu, můžete přeskočit ke kroku 3 v této části.</span><span class="sxs-lookup"><span data-stu-id="a4bff-133">If you already know the IP address, you can skip ahead to step 3 in this section.</span></span>

1. <span data-ttu-id="a4bff-134">Zapněte Intel NUC stisknutím tlačítka napájení a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="a4bff-134">Turn on the Intel NUC by pressing the power button and then log in.</span></span>

   <span data-ttu-id="a4bff-135">Výchozí uživatelské jméno a heslo jsou obě `root`.</span><span class="sxs-lookup"><span data-stu-id="a4bff-135">The default user name and password are both `root`.</span></span>

       > Hit the enter key on your keyboard if you see either of the following errors when you boot: 'A TPM error (7) occurred attempting to read a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="a4bff-136">Získat IP adresu Intel NUC spuštěním `ifconfig` příkazu na zařízení Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a4bff-136">Get the IP address of the Intel NUC by running the `ifconfig` command on the Intel NUC device.</span></span>

   <span data-ttu-id="a4bff-137">Tady je příklad výstupu příkazu.</span><span class="sxs-lookup"><span data-stu-id="a4bff-137">Here is an example of the command output.</span></span>

   ![výstup ifconfig zobrazující Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="a4bff-139">V tomto příkladu hodnota, která odpovídá `inet addr:` je IP adresa, které potřebujete, až se připojí k Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="a4bff-139">In this example, the value that follows `inet addr:` is the IP address that you need when connect to the Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="a4bff-140">Použijte jednu z následujících klientů SSH z hostitelského počítače pro připojení k Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="a4bff-140">Use one of the following SSH clients from your host computer to connect to Intel NUC.</span></span>

    - <span data-ttu-id="a4bff-141">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="a4bff-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="a4bff-142">Integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="a4bff-142">The built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="a4bff-143">Je efektivnější a produktivitu pracovat Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="a4bff-143">It is more efficient and productive to operate an Intel NUC from a host computer.</span></span> <span data-ttu-id="a4bff-144">Budete potřebovat Intel NUC IP adresu, uživatelské jméno a heslo pro připojení k němu prostřednictvím klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="a4bff-144">You'll need the Intel NUC's IP address, user name and password to connect to it via an SSH client.</span></span> <span data-ttu-id="a4bff-145">Tady je příklad, který používá klienta SSH v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="a4bff-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="a4bff-146">![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="a4bff-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="a4bff-147">Nainstalovat balíček Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="a4bff-147">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="a4bff-148">Balíček Azure IoT Edge obsahuje předem kompilovaném binární soubory IoT okraj a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="a4bff-148">The Azure IoT Edge package contains the pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="a4bff-149">Tyto binární soubory jsou Azure IoT Edge, sady SDK Azure IoT a odpovídající nástroje.</span><span class="sxs-lookup"><span data-stu-id="a4bff-149">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="a4bff-150">Balíček obsahuje také "hello_world" ukázkovou aplikaci slouží k ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="a4bff-150">The package also contains a "hello_world" sample application is used to validate the gateway functionality.</span></span> <span data-ttu-id="a4bff-151">IoT okraj je základní součástí brány.</span><span class="sxs-lookup"><span data-stu-id="a4bff-151">IoT Edge is the core part of the gateway.</span></span> 

<span data-ttu-id="a4bff-152">Postupujte podle těchto kroků k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="a4bff-152">Follow these steps to install the package.</span></span>

1. <span data-ttu-id="a4bff-153">Spuštěním následujících příkazů v okně terminálu přidejte IoT cloudové úložiště:</span><span class="sxs-lookup"><span data-stu-id="a4bff-153">Add the IoT Cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="a4bff-154">Zadejte "y", pokud budete vyzváni k "Zahrnovat tento kanál?"</span><span class="sxs-lookup"><span data-stu-id="a4bff-154">Enter 'y', when it prompts you to 'Include this channel?'</span></span>
   
   <span data-ttu-id="a4bff-155">Pokud se zobrazí `import read failed(-1)` chyby, použijte následující příkazy k vyřešení problému:</span><span class="sxs-lookup"><span data-stu-id="a4bff-155">If you receive an `import read failed(-1)` error, use the following commands to resolve the issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="a4bff-156">`rpm` Příkaz importuje klíč ot. / min.</span><span class="sxs-lookup"><span data-stu-id="a4bff-156">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="a4bff-157">`smart channel` Příkaz přidá rpm kanál pro inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="a4bff-157">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="a4bff-158">Před spuštěním `smart update` příkaz, zobrazí se výstup jako níže.</span><span class="sxs-lookup"><span data-stu-id="a4bff-158">Before you run the `smart update` command, you will see an output like below.</span></span>

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="a4bff-160">Spusťte příkaz inteligentní aktualizace:</span><span class="sxs-lookup"><span data-stu-id="a4bff-160">Execute the smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="a4bff-161">Instalovat balíček brány Azure IoT tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a4bff-161">Install the Azure IoT Gateway package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="a4bff-162">`packagegroup-cloud-azure`je název balíčku.</span><span class="sxs-lookup"><span data-stu-id="a4bff-162">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="a4bff-163">`smart install` Příkaz slouží k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="a4bff-163">The `smart install` command is used to install the package.</span></span>

    > <span data-ttu-id="a4bff-164">Spusťte následující příkaz, pokud se zobrazí tato chyba: veřejný klíč není k dispozici</span><span class="sxs-lookup"><span data-stu-id="a4bff-164">Run the following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="a4bff-165">Restartujte Intel NUC, pokud se zobrazí tato chyba: 'žádný balíček poskytuje util. linux vývojářů.</span><span class="sxs-lookup"><span data-stu-id="a4bff-165">Reboot the Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="a4bff-166">Po instalaci balíčku je Intel NUC fungovat jako brány.</span><span class="sxs-lookup"><span data-stu-id="a4bff-166">After the package is installed, Intel NUC is ready to function as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="a4bff-167">Spuštění ukázkové aplikace Azure IoT Edge "hello_world"</span><span class="sxs-lookup"><span data-stu-id="a4bff-167">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="a4bff-168">Následující ukázkové aplikace vytvoří brány z `hello_world.json` soubor a používá základní součásti architektury Azure IoT okraj do protokolu zprávu hello world soubor (log.txt) každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="a4bff-168">The following sample application creates a gateway from a `hello_world.json` file and uses the fundamental components of Azure IoT Edge architecture to log a hello world message to a file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="a4bff-169">Hello World vzorek můžete spustit spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="a4bff-169">You can run the Hello World sample by executing the following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="a4bff-170">Umožní aplikaci Hello, World spustit několik minut a poté stiskněte klávesu Enter zastavte ji.</span><span class="sxs-lookup"><span data-stu-id="a4bff-170">Let the Hello World application run for a few minutes and then hit the Enter key to stop it.</span></span>
<span data-ttu-id="a4bff-171">![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="a4bff-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="a4bff-172">Můžete ignorovat všechny 'handle(NULL) neplatný argument' chyby, které se zobrazí po kliknutí zadejte.</span><span class="sxs-lookup"><span data-stu-id="a4bff-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="a4bff-173">Můžete ověřit, že brána proběhla úspěšně otevřením log.txt souboru, který je teď ve složce hello_world ![log.txt zobrazení adresáře](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="a4bff-173">You can verify that the gateway ran successfully by opening the log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="a4bff-174">Otevřete log.txt pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a4bff-174">Open log.txt using the following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="a4bff-175">Zobrazí se obsah log.txt, který bude výstup naformátovaný JSON protokolování zpráv, které byly napsané každých 5 sekund modulem brány Hello, World.</span><span class="sxs-lookup"><span data-stu-id="a4bff-175">You will then see the contents of log.txt, which will be a JSON formatted output of the logging messages that were written every 5 seconds by the gateway Hello World module.</span></span>
<span data-ttu-id="a4bff-176">![zobrazení adresáře log.txt](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="a4bff-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="a4bff-177">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a4bff-177">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="a4bff-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="a4bff-178">Summary</span></span>

<span data-ttu-id="a4bff-179">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="a4bff-179">Congratulations!</span></span> <span data-ttu-id="a4bff-180">Dokončili jste nastavení Intel NUC jako brána.</span><span class="sxs-lookup"><span data-stu-id="a4bff-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="a4bff-181">Teď jste připravení přejít Další lekce k nastavení hostitelského počítače, vytvořte Azure IoT Hub a zaregistrovat logické zařízení Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="a4bff-181">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4bff-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a4bff-182">Next steps</span></span>
[<span data-ttu-id="a4bff-183">Použít bránu IoT pro připojení zařízení k Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="a4bff-183">Use an IoT gateway to connect a device to Azure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

