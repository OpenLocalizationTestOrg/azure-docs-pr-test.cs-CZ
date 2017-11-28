---
title: "Connect Raspberry PI (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro Node.js pí malin prostředí"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 3653c79b-8a0c-41d4-b0bf-f6b4edb4d233
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 4f1ea81dd25d10a39c2939f5ee5f19f6d2ba2b2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="e802f-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="e802f-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="e802f-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="e802f-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="e802f-106">aplikace Hello spustí i ale hello DIODU není blikat</span><span class="sxs-lookup"><span data-stu-id="e802f-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="e802f-107">Tento problém je vždy související toohello hardwaru okruh připojení.</span><span class="sxs-lookup"><span data-stu-id="e802f-107">This issue is always related toohello hardware circuit connectivity.</span></span> <span data-ttu-id="e802f-108">Použijte následující kroky tooidentify problémy hello.</span><span class="sxs-lookup"><span data-stu-id="e802f-108">Use hello following steps tooidentify problems.</span></span>

1. <span data-ttu-id="e802f-109">Zkontrolujte, že jste vybrali správný hello **GPIO** vaší karty.</span><span class="sxs-lookup"><span data-stu-id="e802f-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="e802f-110">Hello dva porty by měl být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="e802f-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="e802f-111">Zkontrolujte správnost hello polarita z vaší Indikátor.</span><span class="sxs-lookup"><span data-stu-id="e802f-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="e802f-112">Hello delší větev by měl být uveden hello **kladné**, anod pin.</span><span class="sxs-lookup"><span data-stu-id="e802f-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="e802f-113">Použití hello **3.3v připnout** a **zem Pin** na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="e802f-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="e802f-114">Pi považovat za hello power řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="e802f-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="e802f-115">Zkontrolujte, zda že tento hello DIODU funguje bez problémů.</span><span class="sxs-lookup"><span data-stu-id="e802f-115">Check that hello LED works fine.</span></span>

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="e802f-117">Další potíže s hardwarem</span><span class="sxs-lookup"><span data-stu-id="e802f-117">Other hardware issues</span></span>
<span data-ttu-id="e802f-118">Informace o řešení běžných problémů na 3 malin platformy najdete v tématu hello [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="e802f-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="e802f-119">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="e802f-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="e802f-120">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="e802f-120">No response during gulp tasks</span></span>
<span data-ttu-id="e802f-121">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat hello `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="e802f-121">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="e802f-122">Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="e802f-122">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="e802f-123">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="e802f-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="e802f-124">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="e802f-124">Device discovery issues</span></span>
<span data-ttu-id="e802f-125">Pomoc při řešení běžných potíží s hello `devdisco` příkaz, zkontrolujte hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="e802f-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="e802f-126">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="e802f-126">NPM issues</span></span>
<span data-ttu-id="e802f-127">Zkuste tooupdate vašeho balíčku NPM s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e802f-127">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="e802f-128">Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [Ukázka úložiště](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="e802f-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="e802f-129">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="e802f-129">Remote debugging</span></span>

<span data-ttu-id="e802f-130">Vzdálené ladění podpora bude brzy k dispozici v rozšíření kódu C/C++ sady Visual Studio k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e802f-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="e802f-131">V současně můžete GDB pomocí Oblíbené terminálu SSH:</span><span class="sxs-lookup"><span data-stu-id="e802f-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="e802f-132">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="e802f-132">Azure-CLI issues</span></span>
<span data-ttu-id="e802f-133">Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="e802f-133">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="e802f-134">Vyhledejte řešení v hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek řešení.</span><span class="sxs-lookup"><span data-stu-id="e802f-134">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="e802f-135">Zkuste tooupgrade rozhraní příkazového řádku Azure toolatest verze při příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="e802f-135">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="e802f-136">Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="e802f-136">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="e802f-137">Nápovědu k řešení běžných potíží s zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="e802f-137">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="e802f-138">Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.</span><span class="sxs-lookup"><span data-stu-id="e802f-138">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="e802f-139">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="e802f-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="e802f-140">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="e802f-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="e802f-141">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e802f-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="e802f-142">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="e802f-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="e802f-143">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="e802f-143">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="e802f-144">Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="e802f-144">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="e802f-145">Tento úkol použijte následující postup toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="e802f-145">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="e802f-146">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="e802f-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="e802f-147">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="e802f-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="e802f-148">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="e802f-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="e802f-149">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="e802f-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="e802f-150">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="e802f-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="e802f-151">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je třeba zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="e802f-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="e802f-152">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="e802f-152">Device Explorer</span></span>
<span data-ttu-id="e802f-153">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="e802f-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="e802f-154">Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="e802f-154">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="e802f-155">*Správa identit zařízení* tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e802f-155">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="e802f-156">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="e802f-156">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="e802f-157">*Odeslat cloud zařízení* tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e802f-157">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="e802f-158">Konfigurace vaší `IoT hub connection string` v rámci této nástroj toouse všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="e802f-158">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="e802f-159">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="e802f-159">IoT hub Explorer</span></span>
<span data-ttu-id="e802f-160">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="e802f-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="e802f-161">Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="e802f-161">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="e802f-162">tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="e802f-162">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="e802f-163">Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:</span><span class="sxs-lookup"><span data-stu-id="e802f-163">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="e802f-164">portál Azure</span><span class="sxs-lookup"><span data-stu-id="e802f-164">Azure portal</span></span>
<span data-ttu-id="e802f-165">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="e802f-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="e802f-166">Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e802f-166">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="e802f-167">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="e802f-167">Azure storage issues</span></span>
<span data-ttu-id="e802f-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="e802f-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="e802f-169">Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní.</span><span class="sxs-lookup"><span data-stu-id="e802f-169">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="e802f-170">Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.</span><span class="sxs-lookup"><span data-stu-id="e802f-170">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
