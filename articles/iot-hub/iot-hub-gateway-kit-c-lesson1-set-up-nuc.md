---
title: "Zařízení SensorTag & brány Azure IoT - lekci 1: nastavení Intel NUC | Microsoft Docs"
description: "Nastavte Intel NUC toowork jako bránu IoT mezi senzor a Azure IoT Hub toocollect senzor informace a odeslat tooIoT rozbočovače."
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
ms.openlocfilehash: 7c3ab3b014713c7facb86b8e8622d70e60a960e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="f1d25-104">Nastavit Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="f1d25-104">Set up Intel NUC as an IoT gateway</span></span>
[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

## <a name="what-you-will-do"></a><span data-ttu-id="f1d25-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="f1d25-105">What you will do</span></span>

- <span data-ttu-id="f1d25-106">Nastavte Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="f1d25-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="f1d25-107">Nainstalujte balíček Azure IoT Edge hello hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="f1d25-107">Install hello Azure IoT Edge package on hello Intel NUC.</span></span>
- <span data-ttu-id="f1d25-108">Spuštění ukázkové aplikace na "hello_world" hello funkci Intel NUC tooverify hello brány.</span><span class="sxs-lookup"><span data-stu-id="f1d25-108">Run a "hello_world" sample application on hello Intel NUC tooverify hello gateway functionality.</span></span>

  > <span data-ttu-id="f1d25-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f1d25-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="f1d25-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="f1d25-110">What you will learn</span></span>

<span data-ttu-id="f1d25-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="f1d25-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="f1d25-112">Jak tooconnect Intel NUC s periferních zařízení.</span><span class="sxs-lookup"><span data-stu-id="f1d25-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="f1d25-113">Jak tooinstall a aktualizace hello požadované balíčky na Intel NUC pomocí hello inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="f1d25-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="f1d25-114">Jak toorun hello "hello_world" ukázkové aplikace tooverify hello brány funkce.</span><span class="sxs-lookup"><span data-stu-id="f1d25-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="f1d25-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="f1d25-115">What you need</span></span>

- <span data-ttu-id="f1d25-116">Intel NUC Kit DE3815TYKE s hello Intel IoT brány softwaru Suite (Linux větru oblasti * 7.0.0.13) předinstalována.</span><span class="sxs-lookup"><span data-stu-id="f1d25-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span> <span data-ttu-id="f1d25-117">[Kliknutím sem toopurchase Groove IoT komerční brány Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span><span class="sxs-lookup"><span data-stu-id="f1d25-117">[Click here toopurchase Grove IoT Commercial Gateway Kit](https://www.seeedstudio.com/Grove-IoT-Commercial-Gateway-Kit-p-2724.html).</span></span>
- <span data-ttu-id="f1d25-118">Kabel Ethernet.</span><span class="sxs-lookup"><span data-stu-id="f1d25-118">An Ethernet cable.</span></span>
- <span data-ttu-id="f1d25-119">Klávesnice.</span><span class="sxs-lookup"><span data-stu-id="f1d25-119">A keyboard.</span></span>
- <span data-ttu-id="f1d25-120">HDMI nebo VGA kabel.</span><span class="sxs-lookup"><span data-stu-id="f1d25-120">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="f1d25-121">Monitorování s k portu HDMI nebo VGA.</span><span class="sxs-lookup"><span data-stu-id="f1d25-121">A monitor with an HDMI or VGA port.</span></span>
- <span data-ttu-id="f1d25-122">Volitelné: [Texas Instruments Sensortag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span><span class="sxs-lookup"><span data-stu-id="f1d25-122">Optional: [Texas Instruments Sensor Tag (CC2650STK)](http://www.ti.com/tool/cc2650stk)</span></span>

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="f1d25-124">Připojit Intel NUC s hello periferních zařízení</span><span class="sxs-lookup"><span data-stu-id="f1d25-124">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="f1d25-125">Následující obrázek Hello je příkladem NUC Intel, který je připojen k různým periferních zařízení:</span><span class="sxs-lookup"><span data-stu-id="f1d25-125">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="f1d25-126">Připojené tooa klávesnice.</span><span class="sxs-lookup"><span data-stu-id="f1d25-126">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="f1d25-127">Připojený tooa monitorování kabel VGA nebo HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="f1d25-127">Connected tooa monitor with a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="f1d25-128">Připojené tooa drátové síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="f1d25-128">Connected tooa wired network with an Ethernet cable.</span></span>
4. <span data-ttu-id="f1d25-129">Zdroj napájení připojené tooa s napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="f1d25-129">Connected tooa power supply with a power cable.</span></span>

![Intel NUC připojené tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="f1d25-131">Připojení systému Intel NUC toohello od hostitelského počítače pomocí protokolu Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="f1d25-131">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="f1d25-132">Budete potřebovat klávesnici a monitorování tooget hello IP adresu vašeho zařízení Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="f1d25-132">You will need a keyboard and a monitor tooget hello IP address of your Intel NUC device.</span></span> <span data-ttu-id="f1d25-133">Pokud už znáte hello IP adresu, můžete přeskočit napřed toostep 3 v této části.</span><span class="sxs-lookup"><span data-stu-id="f1d25-133">If you already know hello IP address, you can skip ahead toostep 3 in this section.</span></span>

1. <span data-ttu-id="f1d25-134">Zapněte hello Intel NUC stisknutím tlačítka napájení hello a znovu přihlásili.</span><span class="sxs-lookup"><span data-stu-id="f1d25-134">Turn on hello Intel NUC by pressing hello power button and then log in.</span></span>

   <span data-ttu-id="f1d25-135">Hello výchozí uživatelské jméno a heslo jsou obě `root`.</span><span class="sxs-lookup"><span data-stu-id="f1d25-135">hello default user name and password are both `root`.</span></span>

       > Hit hello enter key on your keyboard if you see either of hello following errors when you boot: 'A TPM error (7) occurred attempting tooread a pcr value.' or 'Timeout, No TPM chip found, activating TPM-bypass!'

2. <span data-ttu-id="f1d25-136">Získat IP adresu hello hello Intel NUC spuštěním hello `ifconfig` příkazu na zařízení Intel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="f1d25-136">Get hello IP address of hello Intel NUC by running hello `ifconfig` command on hello Intel NUC device.</span></span>

   <span data-ttu-id="f1d25-137">Tady je příklad výstupu příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="f1d25-137">Here is an example of hello command output.</span></span>

   ![výstup ifconfig zobrazující Intel NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="f1d25-139">V tomto příkladu hello hodnotu, která odpovídá `inet addr:` je hello IP adresu, která je nutné, když připojujete toohello Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="f1d25-139">In this example, hello value that follows `inet addr:` is hello IP address that you need when connect toohello Intel NUC from a host computer.</span></span>

3. <span data-ttu-id="f1d25-140">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooIntel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="f1d25-140">Use one of hello following SSH clients from your host computer tooconnect tooIntel NUC.</span></span>

    - <span data-ttu-id="f1d25-141">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="f1d25-141">[PuTTY](http://www.putty.org/) for Windows.</span></span>
    - <span data-ttu-id="f1d25-142">Hello integrovaného klienta SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="f1d25-142">hello built-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="f1d25-143">Je efektivnější a produktivitu toooperate Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="f1d25-143">It is more efficient and productive toooperate an Intel NUC from a host computer.</span></span> <span data-ttu-id="f1d25-144">Budete potřebovat hello Intel NUC IP adresu, uživatelské jméno a heslo tooit tooconnect prostřednictvím klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="f1d25-144">You'll need hello Intel NUC's IP address, user name and password tooconnect tooit via an SSH client.</span></span> <span data-ttu-id="f1d25-145">Tady je příklad, který používá klienta SSH v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="f1d25-145">Here is an example that uses an SSH client on macOS.</span></span>
   <span data-ttu-id="f1d25-146">![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="f1d25-146">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="f1d25-147">Nainstalovat balíček Azure IoT Edge hello</span><span class="sxs-lookup"><span data-stu-id="f1d25-147">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="f1d25-148">balíček Azure IoT Edge Hello obsahuje předem kompilovaném binární soubory hello IoT okraj a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="f1d25-148">hello Azure IoT Edge package contains hello pre-compiled binaries of IoT Edge and its dependencies.</span></span> <span data-ttu-id="f1d25-149">Tyto binární soubory jsou Azure IoT Edge, hello sady SDK Azure IoT a odpovídající nástroje hello.</span><span class="sxs-lookup"><span data-stu-id="f1d25-149">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="f1d25-150">Hello balíček obsahuje také "hello_world" Vzorová aplikace je funkce použité toovalidate hello brány.</span><span class="sxs-lookup"><span data-stu-id="f1d25-150">hello package also contains a "hello_world" sample application is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="f1d25-151">IoT okraj je hello základní součástí hello brány.</span><span class="sxs-lookup"><span data-stu-id="f1d25-151">IoT Edge is hello core part of hello gateway.</span></span> 

<span data-ttu-id="f1d25-152">Postupujte podle těchto kroků tooinstall hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="f1d25-152">Follow these steps tooinstall hello package.</span></span>

1. <span data-ttu-id="f1d25-153">Spuštěním následujících příkazů v okně terminálu hello přidejte hello IoT cloudové úložiště:</span><span class="sxs-lookup"><span data-stu-id="f1d25-153">Add hello IoT Cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import https://iotdk.intel.com/misc/iot_pub2.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
   ```

   > <span data-ttu-id="f1d25-154">Zadejte "y", když budete vyzváni too'Include tento kanál? "</span><span class="sxs-lookup"><span data-stu-id="f1d25-154">Enter 'y', when it prompts you too'Include this channel?'</span></span>
   
   <span data-ttu-id="f1d25-155">Pokud se zobrazí `import read failed(-1)` chybu, hello použijte následující příkazy tooresolve hello problému:</span><span class="sxs-lookup"><span data-stu-id="f1d25-155">If you receive an `import read failed(-1)` error, use hello following commands tooresolve hello issue:</span></span>
   ```bash
   wget http://iotdk.intel.com/misc/iot_pub2.key 
   rpm --import iot_pub2.key  
   ```

   <span data-ttu-id="f1d25-156">Hello `rpm` příkaz importuje hello klíč ot. / min.</span><span class="sxs-lookup"><span data-stu-id="f1d25-156">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="f1d25-157">Hello `smart channel` příkaz přidá hello rpm kanálu toohello inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="f1d25-157">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="f1d25-158">Před spuštěním hello `smart update` příkaz, zobrazí se výstup jako níže.</span><span class="sxs-lookup"><span data-stu-id="f1d25-158">Before you run hello `smart update` command, you will see an output like below.</span></span>

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

2. <span data-ttu-id="f1d25-160">Spusťte příkaz hello inteligentní aktualizace:</span><span class="sxs-lookup"><span data-stu-id="f1d25-160">Execute hello smart update command:</span></span>

   ```bash
   smart update
   ```

3. <span data-ttu-id="f1d25-161">Instalovat balíček brány Azure IoT hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1d25-161">Install hello Azure IoT Gateway package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="f1d25-162">`packagegroup-cloud-azure`je název hello hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="f1d25-162">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="f1d25-163">Hello `smart install` příkaz je použité tooinstall hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="f1d25-163">hello `smart install` command is used tooinstall hello package.</span></span>

    > <span data-ttu-id="f1d25-164">Hello spusťte následující příkaz, pokud se zobrazí tato chyba: veřejný klíč není k dispozici</span><span class="sxs-lookup"><span data-stu-id="f1d25-164">Run hello following command if you see this error: 'public key not available'</span></span>

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```
    > <span data-ttu-id="f1d25-165">Restartujte hello Intel NUC, pokud se zobrazí tato chyba: 'žádný balíček poskytuje util. linux vývojářů.</span><span class="sxs-lookup"><span data-stu-id="f1d25-165">Reboot hello Intel NUC if you see this error: 'no package provides util-linux-dev'</span></span>

   <span data-ttu-id="f1d25-166">Po instalaci balíčku hello Intel NUC je připraven toofunction jako brána.</span><span class="sxs-lookup"><span data-stu-id="f1d25-166">After hello package is installed, Intel NUC is ready toofunction as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="f1d25-167">Spustit hello Azure IoT Edge "hello_world" ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="f1d25-167">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="f1d25-168">Následující ukázkové aplikace Hello vytvoří brány z `hello_world.json` soubor a používá hello základní součásti služby Azure IoT Edge architektura toolog soubor hello world zprávy tooa (log.txt) každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="f1d25-168">hello following sample application creates a gateway from a `hello_world.json` file and uses hello fundamental components of Azure IoT Edge architecture toolog a hello world message tooa file (log.txt) every 5 seconds.</span></span>

<span data-ttu-id="f1d25-169">Ukázka programu hello Hello World můžete spustit spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f1d25-169">You can run hello Hello World sample by executing hello following commands:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="f1d25-170">Umožní aplikaci Hello World hello spustit několik minut a poté stiskněte klíče toostop hello zadejte ji.</span><span class="sxs-lookup"><span data-stu-id="f1d25-170">Let hello Hello World application run for a few minutes and then hit hello Enter key toostop it.</span></span>
<span data-ttu-id="f1d25-171">![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span><span class="sxs-lookup"><span data-stu-id="f1d25-171">![application output](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)</span></span>

> <span data-ttu-id="f1d25-172">Můžete ignorovat všechny 'handle(NULL) neplatný argument' chyby, které se zobrazí po kliknutí zadejte.</span><span class="sxs-lookup"><span data-stu-id="f1d25-172">You can ignore any 'invalid argument handle(NULL)' errors that appear after you hit Enter.</span></span>

<span data-ttu-id="f1d25-173">Můžete ověřit, že brány hello proběhla úspěšně otevřením hello log.txt souboru, který je teď ve složce hello_world ![log.txt zobrazení adresáře](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span><span class="sxs-lookup"><span data-stu-id="f1d25-173">You can verify that hello gateway ran successfully by opening hello log.txt file that is now in your hello_world folder ![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtdir.png)</span></span>

<span data-ttu-id="f1d25-174">Otevřete log.txt pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1d25-174">Open log.txt using hello following command:</span></span>

```bash
vim log.txt
```

<span data-ttu-id="f1d25-175">Zobrazí se obsah hello log.txt, který bude výstup naformátovaný JSON hello protokolování zpráv, které byly napsané každých 5 sekund modulem Hello, World hello brány.</span><span class="sxs-lookup"><span data-stu-id="f1d25-175">You will then see hello contents of log.txt, which will be a JSON formatted output of hello logging messages that were written every 5 seconds by hello gateway Hello World module.</span></span>
<span data-ttu-id="f1d25-176">![zobrazení adresáře log.txt](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span><span class="sxs-lookup"><span data-stu-id="f1d25-176">![log.txt directory view](media/iot-hub-gateway-kit-lessons/lesson1/logtxtview.png)</span></span>

<span data-ttu-id="f1d25-177">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f1d25-177">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="f1d25-178">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f1d25-178">Summary</span></span>

<span data-ttu-id="f1d25-179">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="f1d25-179">Congratulations!</span></span> <span data-ttu-id="f1d25-180">Dokončili jste nastavení Intel NUC jako brána.</span><span class="sxs-lookup"><span data-stu-id="f1d25-180">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="f1d25-181">Nyní jste připraveni toomove na toohello Další lekce tooset až hostitelského počítače, vytvořte Azure IoT Hub a zaregistrovat logické zařízení Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="f1d25-181">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT Hub and register your Azure IoT Hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1d25-182">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1d25-182">Next steps</span></span>
[<span data-ttu-id="f1d25-183">Pomocí IoT brány tooconnect tooAzure zařízení IoT Hub</span><span class="sxs-lookup"><span data-stu-id="f1d25-183">Use an IoT gateway tooconnect a device tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

