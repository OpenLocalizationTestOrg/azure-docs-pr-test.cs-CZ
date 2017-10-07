---
title: "Simulované zařízení & brány Azure IoT - lekci 1: nastavení NUC | Microsoft Docs"
description: "Nastavte Intel NUC toowork jako bránu IoT mezi senzor a Azure IoT Hub toocollect senzor informace a odeslat tooIoT rozbočovače."
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
ms.openlocfilehash: c2146c7cd8ca54adbd0af279364ec8484be1b45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a><span data-ttu-id="30b00-104">Nastavit Intel NUC jako bránu IoT</span><span class="sxs-lookup"><span data-stu-id="30b00-104">Set up Intel NUC as an IoT gateway</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="30b00-105">Co provedete</span><span class="sxs-lookup"><span data-stu-id="30b00-105">What you will do</span></span>

- <span data-ttu-id="30b00-106">Nastavte Intel NUC jako bránu IoT.</span><span class="sxs-lookup"><span data-stu-id="30b00-106">Set up Intel NUC as an IoT gateway.</span></span>
- <span data-ttu-id="30b00-107">Nainstalujte balíček Azure IoT Edge hello Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="30b00-107">Install hello Azure IoT Edge package on Intel NUC.</span></span>
- <span data-ttu-id="30b00-108">Spuštění ukázkové aplikace na "hello_world" funkci Intel NUC tooverify hello brány.</span><span class="sxs-lookup"><span data-stu-id="30b00-108">Run a "hello_world" sample application on Intel NUC tooverify hello gateway functionality.</span></span>
<span data-ttu-id="30b00-109">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="30b00-109">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-sim-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="30b00-110">Co se dozvíte</span><span class="sxs-lookup"><span data-stu-id="30b00-110">What you will learn</span></span>

<span data-ttu-id="30b00-111">V této lekci se dozvíte:</span><span class="sxs-lookup"><span data-stu-id="30b00-111">In this lesson, you will learn:</span></span>

- <span data-ttu-id="30b00-112">Jak tooconnect Intel NUC s periferních zařízení.</span><span class="sxs-lookup"><span data-stu-id="30b00-112">How tooconnect Intel NUC with peripherals.</span></span>
- <span data-ttu-id="30b00-113">Jak tooinstall a aktualizace hello požadované balíčky na Intel NUC pomocí hello inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="30b00-113">How tooinstall and update hello required packages on Intel NUC using hello Smart Package Manager.</span></span>
- <span data-ttu-id="30b00-114">Jak toorun hello "hello_world" ukázkové aplikace tooverify hello brány funkce.</span><span class="sxs-lookup"><span data-stu-id="30b00-114">How toorun hello "hello_world" sample application tooverify hello gateway functionality.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="30b00-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="30b00-115">What you need</span></span>

- <span data-ttu-id="30b00-116">Intel NUC Kit DE3815TYKE s hello Intel IoT brány softwaru Suite (Linux větru oblasti * 7.0.0.13) předinstalována.</span><span class="sxs-lookup"><span data-stu-id="30b00-116">An Intel NUC Kit DE3815TYKE with hello Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) preinstalled.</span></span>
- <span data-ttu-id="30b00-117">Kabel Ethernet.</span><span class="sxs-lookup"><span data-stu-id="30b00-117">An Ethernet cable.</span></span>
- <span data-ttu-id="30b00-118">Klávesnice.</span><span class="sxs-lookup"><span data-stu-id="30b00-118">A keyboard.</span></span>
- <span data-ttu-id="30b00-119">HDMI nebo VGA kabel.</span><span class="sxs-lookup"><span data-stu-id="30b00-119">An HDMI or VGA cable.</span></span>
- <span data-ttu-id="30b00-120">Monitorování s k portu HDMI nebo VGA.</span><span class="sxs-lookup"><span data-stu-id="30b00-120">A monitor with an HDMI or VGA port.</span></span>

![Brána Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit_without_sensortag.png)

## <a name="connect-intel-nuc-with-hello-peripherals"></a><span data-ttu-id="30b00-122">Připojit Intel NUC s hello periferních zařízení</span><span class="sxs-lookup"><span data-stu-id="30b00-122">Connect Intel NUC with hello peripherals</span></span>

<span data-ttu-id="30b00-123">Následující obrázek Hello je příkladem NUC Intel, který je připojen k různým periferních zařízení:</span><span class="sxs-lookup"><span data-stu-id="30b00-123">hello image below is an example of Intel NUC that is connected with various peripherals:</span></span>

1. <span data-ttu-id="30b00-124">Připojené tooa klávesnice.</span><span class="sxs-lookup"><span data-stu-id="30b00-124">Connected tooa keyboard.</span></span>
2. <span data-ttu-id="30b00-125">Připojení toohello monitorování kabel VGA nebo HDMI kabel.</span><span class="sxs-lookup"><span data-stu-id="30b00-125">Connected toohello monitor by a VGA cable or HDMI cable.</span></span>
3. <span data-ttu-id="30b00-126">Připojené tooa drátové síti pomocí kabelu Ethernet.</span><span class="sxs-lookup"><span data-stu-id="30b00-126">Connected tooa wired network by an Ethernet cable.</span></span>
4. <span data-ttu-id="30b00-127">Zdroj napájení připojené toohello s napájecí kabel.</span><span class="sxs-lookup"><span data-stu-id="30b00-127">Connected toohello power supply with a power cable.</span></span>

![Intel NUC připojené tooperipherals](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-toohello-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a><span data-ttu-id="30b00-129">Připojení systému Intel NUC toohello od hostitelského počítače pomocí protokolu Secure Shell (SSH)</span><span class="sxs-lookup"><span data-stu-id="30b00-129">Connect toohello Intel NUC system from host computer via Secure Shell (SSH)</span></span>

<span data-ttu-id="30b00-130">Zde je třeba klávesnice a monitorování tooget hello IP adresa vašeho NUC zařízení.</span><span class="sxs-lookup"><span data-stu-id="30b00-130">Here you need keyboard and monitor tooget hello IP address of your NUC device.</span></span> <span data-ttu-id="30b00-131">Pokud už znáte hello IP adresu, můžete přeskočit toostep 3 v této části.</span><span class="sxs-lookup"><span data-stu-id="30b00-131">If you already know hello IP address, you can skip toostep 3 in this section.</span></span>

1. <span data-ttu-id="30b00-132">Zapněte Intel NUC stisknutím tlačítka napájení hello a protokolu systému hello.</span><span class="sxs-lookup"><span data-stu-id="30b00-132">Turn on Intel NUC by pressing hello Power button and log in hello system.</span></span>

   <span data-ttu-id="30b00-133">Hello výchozí uživatelské jméno a heslo jsou obě `root`.</span><span class="sxs-lookup"><span data-stu-id="30b00-133">hello default user name and password are both `root`.</span></span>

2. <span data-ttu-id="30b00-134">Získat IP adresu hello NUC spuštěním hello `ifconfig` příkaz.</span><span class="sxs-lookup"><span data-stu-id="30b00-134">Get hello IP address of NUC by running hello `ifconfig` command.</span></span> <span data-ttu-id="30b00-135">Tento krok se provádí na hello NUC zařízení.</span><span class="sxs-lookup"><span data-stu-id="30b00-135">This step is done on hello NUC device.</span></span>

   <span data-ttu-id="30b00-136">Tady je příklad výstupu příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="30b00-136">Here is an example of hello command output.</span></span>

   ![výstup ifconfig zobrazující NUC IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   <span data-ttu-id="30b00-138">V tomto příkladu hello hodnotu, která odpovídá `inet addr:` je hello IP adresu, které musíte při plánování tooconnect vzdáleně z počítače tooIntel hostitele NUC.</span><span class="sxs-lookup"><span data-stu-id="30b00-138">In this example, hello value that follows `inet addr:` is hello IP address that you need when you plan tooconnect remotely from a host computer tooIntel NUC.</span></span>

3. <span data-ttu-id="30b00-139">Použijte jeden z následujících klientů SSH z vaší hostitele počítače tooconnect tooIntel NUC hello.</span><span class="sxs-lookup"><span data-stu-id="30b00-139">Use one of hello following SSH clients from your host machine tooconnect tooIntel NUC.</span></span>

   - <span data-ttu-id="30b00-140">[PuTTY](http://www.putty.org/) pro systém Windows.</span><span class="sxs-lookup"><span data-stu-id="30b00-140">[PuTTY](http://www.putty.org/) for Windows.</span></span>
   - <span data-ttu-id="30b00-141">Hello sestavení v klient SSH na Ubuntu nebo systému macOS.</span><span class="sxs-lookup"><span data-stu-id="30b00-141">hello build-in SSH client on Ubuntu or macOS.</span></span>

   <span data-ttu-id="30b00-142">Je efektivnější a produktivitu toooperate na Intel NUC z hostitelského počítače.</span><span class="sxs-lookup"><span data-stu-id="30b00-142">It is more efficient and productive toooperate on Intel NUC from a host computer.</span></span> <span data-ttu-id="30b00-143">Budete potřebovat hello hello IP adresu, uživatelské jméno a heslo tooconnect hello NUC prostřednictvím klient SSH.</span><span class="sxs-lookup"><span data-stu-id="30b00-143">You need hello hello IP address, user name and password tooconnect hello NUC via SSH client.</span></span> <span data-ttu-id="30b00-144">Zde je klient hello příklad použití SSH v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="30b00-144">Here is hello example use SSH client on macOS.</span></span>
   <span data-ttu-id="30b00-145">![Klient SSH, které jsou spuštěné v systému macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span><span class="sxs-lookup"><span data-stu-id="30b00-145">![SSH client running on macOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)</span></span>

## <a name="install-hello-azure-iot-edge-package"></a><span data-ttu-id="30b00-146">Nainstalovat balíček Azure IoT Edge hello</span><span class="sxs-lookup"><span data-stu-id="30b00-146">Install hello Azure IoT Edge package</span></span>

<span data-ttu-id="30b00-147">balíček Azure IoT Edge Hello obsahuje hello předem kompilovaném binární soubory hello SDK a jeho závislosti.</span><span class="sxs-lookup"><span data-stu-id="30b00-147">hello Azure IoT Edge package contains hello pre-compiled binaries of hello SDK and its dependencies.</span></span> <span data-ttu-id="30b00-148">Tyto binární soubory jsou Azure IoT Edge, hello sady SDK Azure IoT a odpovídající nástroje hello.</span><span class="sxs-lookup"><span data-stu-id="30b00-148">These binaries are Azure IoT Edge, hello Azure IoT SDK and hello corresponding tools.</span></span> <span data-ttu-id="30b00-149">balíček Hello také obsahuje ukázkovou aplikaci "hello_world", která je funkcí brány použité toovalidate hello.</span><span class="sxs-lookup"><span data-stu-id="30b00-149">hello package also contains a "hello_world" sample application that is used toovalidate hello gateway functionality.</span></span> <span data-ttu-id="30b00-150">IoT okraj je hello základní součástí hello brány.</span><span class="sxs-lookup"><span data-stu-id="30b00-150">IoT Edge is hello core part of hello gateway.</span></span> <span data-ttu-id="30b00-151">tooinstall hello balíček, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="30b00-151">tooinstall hello package, follow these steps:</span></span>

1. <span data-ttu-id="30b00-152">Spuštěním následujících příkazů v okně terminálu hello přidejte hello IoT cloudové úložiště:</span><span class="sxs-lookup"><span data-stu-id="30b00-152">Add hello IoT cloud repository by running hello following commands in a terminal window:</span></span>

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   <span data-ttu-id="30b00-153">Hello `rpm` příkaz importuje hello klíč ot. / min.</span><span class="sxs-lookup"><span data-stu-id="30b00-153">hello `rpm` command imports hello rpm key.</span></span> <span data-ttu-id="30b00-154">Hello `smart channel` příkaz přidá hello rpm kanálu toohello inteligentní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="30b00-154">hello `smart channel` command adds hello rpm channel toohello Smart Package Manager.</span></span> <span data-ttu-id="30b00-155">Před spuštěním hello `smart update` příkazu, uvidíte výstup jako níže.</span><span class="sxs-lookup"><span data-stu-id="30b00-155">Before you run hello `smart update` command, you see an output like below.</span></span>

   ![ot. / min a inteligentní kanál příkazy výstup](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. <span data-ttu-id="30b00-157">Instalovat balíček hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="30b00-157">Install hello package by running hello following command:</span></span>

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   <span data-ttu-id="30b00-158">`packagegroup-cloud-azure`je název hello hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="30b00-158">`packagegroup-cloud-azure` is hello name of hello package.</span></span> <span data-ttu-id="30b00-159">Hello `smart install` příkaz je použité tooinstall hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="30b00-159">hello `smart install` command is used tooinstall hello package.</span></span>

   <span data-ttu-id="30b00-160">Po instalaci balíčku hello Intel NUC je očekávané toowork jako brána.</span><span class="sxs-lookup"><span data-stu-id="30b00-160">After hello package is installed, Intel NUC is expected toowork as a gateway.</span></span>

## <a name="run-hello-azure-iot-edge-helloworld-sample-application"></a><span data-ttu-id="30b00-161">Spustit hello Azure IoT Edge "hello_world" ukázkové aplikace</span><span class="sxs-lookup"><span data-stu-id="30b00-161">Run hello Azure IoT Edge "hello_world" sample application</span></span>

<span data-ttu-id="30b00-162">Přejděte příliš`azureiotgatewaysdk/samples` a spusťte hello ukázka "hello_world" ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="30b00-162">Go too`azureiotgatewaysdk/samples` and run hello sample "hello_world" sample application.</span></span> <span data-ttu-id="30b00-163">Tato ukázková aplikace vytvoří brány z hello `hello_world.json` soubor a používá základní součástí hello Azure IoT Edge architektura toolog souboru tooa zprávy hello world hello každých 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="30b00-163">This sample application creates a gateway from hello `hello_world.json` file and uses hello fundamental components of hello Azure IoT Edge architecture toolog a hello world message tooa file every 5 seconds.</span></span>

<span data-ttu-id="30b00-164">Hello ukázka "hello_world" ukázkovou aplikaci můžete spustit spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="30b00-164">You can run hello sample "hello_world" sample application by running hello following command:</span></span>

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

<span data-ttu-id="30b00-165">Ukázková aplikace Hello vytváří hello následující výstup, pokud funkci brány hello funguje správně:</span><span class="sxs-lookup"><span data-stu-id="30b00-165">hello sample application produces hello following output if hello gateway functionality is working correctly:</span></span>

![výstup aplikace](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

<span data-ttu-id="30b00-167">Pokud máte potíže, vyhledejte řešení na hello [řešení potíží s stránky](iot-hub-gateway-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="30b00-167">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-gateway-kit-c-troubleshooting.md).</span></span>

## <a name="summary"></a><span data-ttu-id="30b00-168">Souhrn</span><span class="sxs-lookup"><span data-stu-id="30b00-168">Summary</span></span>

<span data-ttu-id="30b00-169">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="30b00-169">Congratulations!</span></span> <span data-ttu-id="30b00-170">Dokončili jste nastavení Intel NUC jako brána.</span><span class="sxs-lookup"><span data-stu-id="30b00-170">You've finished setting up Intel NUC as a gateway.</span></span> <span data-ttu-id="30b00-171">Nyní jste připraveni toomove na toohello Další lekce tooset až hostitelského počítače vytvoření služby Azure IoT hub a zaregistrujte logické zařízení Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="30b00-171">Now you're ready toomove on toohello next lesson tooset up your host computer, create an Azure IoT hub and register your Azure IoT hub logical device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="30b00-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="30b00-172">Next steps</span></span>
[<span data-ttu-id="30b00-173">Příprava hostitelský počítač a Azure IoT hub</span><span class="sxs-lookup"><span data-stu-id="30b00-173">Get your host computer and Azure IoT hub ready</span></span>](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
