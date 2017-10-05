---
title: "Simulované zařízení & brány Azure IoT - lekci 1: nastavení NUC | Microsoft Docs"
description: "Nastavte Intel NUC fungovat jako bránu IoT mezi senzor a Azure IoT Hub a shromažďovat informace o senzor odeslat do služby IoT Hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: "IOT brány, intel nuc nuc počítače, DE3815TYKE"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f41d6b2e-9b00-40df-90eb-17d824bea883
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b87974be9570f7d03fe84ae0a1d1fa7e346ff189
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="e6f1a-104">Nastavit Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="e6f1a-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="e6f1a-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="e6f1a-105">What you will do</span></span>

- <span data-ttu-id="e6f1a-106">Nastavte Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="e6f1a-107">Nainstalujte balíček Azure IoT Edge na Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-107">Install the Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="e6f1a-108">Spuštění ukázkové aplikace na "hello_world" Intel NUC ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-108">Run a "hello_world" sample application on Intel NUC to verify the gateway functionality.</span></span>
<span data-ttu-id="e6f1a-109">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e6f1a-109">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="e6f1a-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="e6f1a-110">What you will learn</span></span>

<span data-ttu-id="e6f1a-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="e6f1a-112">Postup připojení Intel NUC s periferních zařízení.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-112">How to connect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="e6f1a-113">Postup instalace a aktualizace požadované balíčky na Intel NUC pomocí inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-113">How to install and update the required packages on Intel NUC using the Smart Package Manager.</span></span>
- <span data-ttu-id="e6f1a-114">Postup spuštění ukázkové aplikace "hello_world" ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-114">How to run the "hello_world" sample application to verify the gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="e6f1a-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="e6f1a-115">What you need</span></span>

- <span data-ttu-id="e6f1a-116">Intel NUC Kit DE3815TYKE s sadě Intel IoT brány (Linux větru oblasti * 7.0.0.13) předinstalována.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-116">An Intel NUC Kit DE3815TYKE with the Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="e6f1a-117">Kabel Ethernet.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-117">An Ethernet cable.</span></span>
- <span data-ttu-id="e6f1a-118">Klávesnice.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-118">A keyboard.</span></span>
- <span data-ttu-id="e6f1a-119">HDMI nebo VGA kabel.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="e6f1a-120">Monitorování s k portu HDMI nebo VGA.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-120">A monitor with an HDMI or VGA port.</span></span>

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a><span data-ttu-id="e6f1a-122">Připojit Intel NUC s periferních zařízení</span><span class="sxs-lookup"><span data-stu-id="e6f1a-122">Connect Intel NUC with the peripherals</span></span>

<span data-ttu-id="e6f1a-123">Následující obrázek je příkladem NUC Intel, který je připojen k různým periferních zařízení:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-123">The image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="e6f1a-124">Připojení k klávesnici.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-124">Connected to a keyboard.</span></span>
2. <span data-ttu-id="e6f1a-125">Připojené k monitorování kabel VGA nebo HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-125">Connected to the monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="e6f1a-126">Připojený k drátové síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-126">Connected to a wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="e6f1a-127">Připojení k napájení s napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-127">Connected to the power supply with a power cable.</span></span>

![Intel NUC připojené k periferních zařízení](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="e6f1a-129">Připojení k systému Intel NUC od hostitelského počítače pomocí protokolu Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="e6f1a-129">Connect to the Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="e6f1a-130">Zde je třeba klávesnice a monitorování, který chcete získat IP adresu vašeho NUC zařízení.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-130">Here you need keyboard and monitor to get the IP address of your NUC device.</span></span> <span data-ttu-id="e6f1a-131">Pokud už znáte IP adresu, můžete přeskočit ke kroku 3 v této části.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-131">If you already know the IP address, you can skip to step 3 in this section.</span></span>

1. <span data-ttu-id="e6f1a-132">Zapněte Intel NUC stisknutím tlačítka napájení a protokolu v systému.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-132">Turn on Intel NUC by pressing the Power button and log in the system.</span></span>

   <span data-ttu-id="e6f1a-133">Výchozí uživatelské jméno a heslo jsou obě `root`.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-133">The default user name and password are both `root`.</span></span>

2. <span data-ttu-id="e6f1a-134">Získat IP adresu NUC spuštěním `ifconfig` příkaz.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-134">Get the IP address of NUC by running the `ifconfig` command.</span></span> <span data-ttu-id="e6f1a-135">Tento krok se provádí na NUC zařízení.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-135">This step is done on the NUC device.</span></span>

   <span data-ttu-id="e6f1a-136">Tady je příklad výstupu příkazu.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-136">Here is an example of the command output.</span></span>

   ![výstup ifconfig zobrazující NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="e6f1a-138">V tomto příkladu hodnota, která odpovídá `inet addr:` je IP adresa, která musíte, když máte v plánu připojit se k Intel NUC vzdáleně z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-138">In this example, the value that follows `inet addr:` is the IP address that you need when you plan to connect remotely from a host computer to Intel NUC.</span></span>

3. <span data-ttu-id="e6f1a-139">Použijte jeden z následující klienti SSH ze hostitelský počítač připojit k Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-139">Use one of the following SSH clients from your host machine to connect to Intel NUC.</span></span>

   - <span data-ttu-id="e6f1a-140">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="e6f1a-141">Sestavení v SSH klienta na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-141">The build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="e6f1a-142">Je efektivnější a produktivitu pracovat Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-142">It is more efficient and productive to operate on Intel NUC from a host computer.</span></span> <span data-ttu-id="e6f1a-143">Je nutné IP adresu, uživatelské jméno a heslo pro připojení NUC prostřednictvím klient SSH.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-143">You need the the IP address, user name and password to connect the NUC via SSH client.</span></span> <span data-ttu-id="e6f1a-144">Tady je příklad použití SSH klienta v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-144">Here is the example use SSH client on macOS.</span></span>
   <span data-ttu-id="e6f1a-145">![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="e6f1a-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-the-azure-iot-edge-package"></a><span data-ttu-id="e6f1a-146">Nainstalovat balíček Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="e6f1a-146">Install the Azure IoT Edge package</span></span>

<span data-ttu-id="e6f1a-147">Balíček Azure IoT Edge obsahuje předem kompilovaném binární soubory sady SDK a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-147">The Azure IoT Edge package contains the pre-compiled binaries of the SDK and its dependencies.</span></span> <span data-ttu-id="e6f1a-148">Tyto binární soubory jsou Azure IoT Edge, sady SDK Azure IoT a odpovídající nástroje.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-148">These binaries are Azure IoT Edge, the Azure IoT SDK and the corresponding tools.</span></span> <span data-ttu-id="e6f1a-149">Balíček obsahuje také "hello_world" ukázkovou aplikaci, která slouží k ověření funkci brány.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-149">The package also contains a "hello_world" sample application that is used to validate the gateway functionality.</span></span> <span data-ttu-id="e6f1a-150">IoT okraj je základní součástí brány.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-150">IoT Edge is the core part of the gateway.</span></span> <span data-ttu-id="e6f1a-151">K instalaci balíčku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-151">To install the package, follow these steps:</span></span>

1. <span data-ttu-id="e6f1a-152">Přidejte úložiště cloudu IoT spuštěním následujících příkazů v okně terminálu:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-152">Add the IoT cloud repository by running the following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="e6f1a-153">`rpm` Příkaz importuje klíč ot. / min.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-153">The `rpm` command imports the rpm key.</span></span> <span data-ttu-id="e6f1a-154">`smart channel` Příkaz přidá rpm kanál pro inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-154">The `smart channel` command adds the rpm channel to the Smart Package Manager.</span></span> <span data-ttu-id="e6f1a-155">Před spuštěním `smart update` příkazu, uvidíte výstup jako níže.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-155">Before you run the `smart update` command, you see an output like below.</span></span>

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="e6f1a-157">Nainstalujte balíček spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-157">Install the package by running the following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="e6f1a-158">`packagegroup-cloud-azure`je název balíčku.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-158">`packagegroup-cloud-azure` is the name of the package.</span></span> <span data-ttu-id="e6f1a-159">`smart install` Příkaz slouží k instalaci balíčku.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-159">The `smart install` command is used to install the package.</span></span>

   <span data-ttu-id="e6f1a-160">Po instalaci balíčku Intel NUC budou fungovat jako brána.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-160">After the package is installed, Intel NUC is expected to work as a gateway.</span></span>

## <a name="run-the-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="e6f1a-161">Spuštění ukázkové aplikace Azure IoT Edge "hello_world"</span><span class="sxs-lookup"><span data-stu-id="e6f1a-161">Run the Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="e6f1a-162">Přejděte na `azureiotgatewaysdk/samples` a spusťte ukázkové "hello_world" ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-162">Go to `azureiotgatewaysdk/samples` and run the sample "hello_world" sample application.</span></span> <span data-ttu-id="e6f1a-163">Tato ukázková aplikace vytvoří brány z `hello_world.json` soubor a používá základní součásti architektury Azure IoT okraj do protokolu zprávu hello world soubor každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-163">This sample application creates a gateway from the `hello_world.json` file and uses the fundamental components of the Azure IoT Edge architecture to log a hello world message to a file every 5 seconds.</span></span>

<span data-ttu-id="e6f1a-164">Ukázka "hello_world" ukázkovou aplikaci můžete spustit tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-164">You can run the sample "hello_world" sample application by running the following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="e6f1a-165">Pokud funkci brány funguje správně, vytvoří ukázkovou aplikaci následující výstup:</span><span class="sxs-lookup"><span data-stu-id="e6f1a-165">The sample application produces the following output if the gateway functionality is working correctly:</span></span>

![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="e6f1a-167">Pokud máte potíže, vyhledejte řešení na [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e6f1a-167">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="e6f1a-168">Souhrn</span><span class="sxs-lookup"><span data-stu-id="e6f1a-168">Summary</span></span>

<span data-ttu-id="e6f1a-169">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="e6f1a-169">Congratulations!</span></span> <span data-ttu-id="e6f1a-170">Dokončili jste nastavení Intel NUC jako brána.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="e6f1a-171">Teď jste připravení přejít Další lekce nastavit hostitelského počítače, vytvoření služby Azure IoT hub a zaregistrovat logické zařízení Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e6f1a-171">Now you're ready to move on to the next lesson to set up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6f1a-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6f1a-172">Next steps</span></span>
[<span data-ttu-id="e6f1a-173">Příprava hostitelský počítač a Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="e6f1a-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
