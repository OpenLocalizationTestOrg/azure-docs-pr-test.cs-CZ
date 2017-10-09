---
title: "Connect Raspberry PI (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí malin pí Node.js hello"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 22cf50dc-8206-42a2-a1fc-f75fa85135fa
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f0807104550e8e53a132f7741564b33f1db17ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="df391-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="df391-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="df391-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="df391-105">Hardware issues</span></span>
### <a name="hello-application-runs-well-but-hello-led-is-not-blinking"></a><span data-ttu-id="df391-106">aplikace Hello spustí i ale hello DIODU není blikat</span><span class="sxs-lookup"><span data-stu-id="df391-106">hello application runs well but hello LED is not blinking</span></span>
<span data-ttu-id="df391-107">Tento problém je vždy související toohardware okruhu připojení.</span><span class="sxs-lookup"><span data-stu-id="df391-107">This issue is always related toohardware circuit connectivity.</span></span> <span data-ttu-id="df391-108">Použijte následující kroky tooidentify problémy hello:</span><span class="sxs-lookup"><span data-stu-id="df391-108">Use hello following steps tooidentify problems:</span></span>

1. <span data-ttu-id="df391-109">Zkontrolujte, že jste vybrali správný hello **GPIO** vaší karty.</span><span class="sxs-lookup"><span data-stu-id="df391-109">Check that you chose hello correct **GPIO** on your board.</span></span> <span data-ttu-id="df391-110">Hello dva porty by měl být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="df391-110">hello two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="df391-111">Zkontrolujte správnost hello polarita z vaší Indikátor.</span><span class="sxs-lookup"><span data-stu-id="df391-111">Check that hello polarity of your LED is correct.</span></span> <span data-ttu-id="df391-112">Hello delší větev by měl být uveden hello **kladné**, anod pin.</span><span class="sxs-lookup"><span data-stu-id="df391-112">hello longer leg should indicate hello **positive**, anode pin.</span></span>
3. <span data-ttu-id="df391-113">Použití hello **3.3v připnout** a **zem Pin** na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="df391-113">Use hello **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="df391-114">Pi považovat za hello power řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="df391-114">Treat Pi as hello DC power.</span></span> <span data-ttu-id="df391-115">Zkontrolujte, zda že tento hello DIODU funguje bez problémů.</span><span class="sxs-lookup"><span data-stu-id="df391-115">Check that hello LED works fine.</span></span>

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="df391-117">Další potíže s hardwarem</span><span class="sxs-lookup"><span data-stu-id="df391-117">Other hardware issues</span></span>
<span data-ttu-id="df391-118">Informace o řešení běžných problémů na 3 malin platformy najdete v tématu hello [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="df391-118">For information about solving common problems on Raspberry Pi 3, see hello [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="df391-119">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="df391-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="df391-120">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="df391-120">No response during gulp tasks</span></span>
<span data-ttu-id="df391-121">Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat hello `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="df391-121">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="df391-122">Zkuste tooterminate aktuální gulp úlohy pomocí kombinace kláves Ctrl + C a poté spusťte následující příkaz v vaše zprávy ladění toosee okna konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="df391-122">Try tooterminate current gulp tasks by using Ctrl + C, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="df391-123">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="df391-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="df391-124">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="df391-124">Device discovery issues</span></span>
<span data-ttu-id="df391-125">Pomoc při řešení běžných potíží s hello `devdisco` příkaz, zkontrolujte hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="df391-125">For help in troubleshooting common problems with hello `devdisco` command, check hello [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="df391-126">npm problémy</span><span class="sxs-lookup"><span data-stu-id="df391-126">npm issues</span></span>
<span data-ttu-id="df391-127">Zkuste tooupdate vašeho balíčku npm pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="df391-127">Try tooupdate your npm package by using hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="df391-128">Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="df391-128">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="df391-129">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="df391-129">Remote debugging</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="df391-130">Spuštění ukázkové aplikace hello v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="df391-130">Run hello sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="df391-131">Když modul ladění hello je připraven, měli byste vidět ```Debugger listening on port 5858``` ve výstupu konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="df391-131">When hello debug engine is ready, you should see ```Debugger listening on port 5858``` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="df391-132">Konfigurace Visual Studio Code tooconnect toohello vzdáleném zařízení</span><span class="sxs-lookup"><span data-stu-id="df391-132">Configure Visual Studio Code tooconnect toohello remote device</span></span>
1. <span data-ttu-id="df391-133">Otevřete hello **ladění** panely na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="df391-133">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="df391-134">Klikněte na zelenou hello **spustit ladění** tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="df391-134">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="df391-135">Visual Studio Code otevře soubor launch.json.</span><span class="sxs-lookup"><span data-stu-id="df391-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="df391-136">Aktualizujte soubor launch.json hello hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="df391-136">Update hello launch.json file with hello following content.</span></span> <span data-ttu-id="df391-137">Nahraďte `[device hostname or IP address]` s hello skutečné zařízení IP adresu nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="df391-137">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="df391-138">Další informace o ladění Visual Studio, hello toolearn naleznete příliš[ladění ve Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="df391-138">toolearn more about hello Visual Studio Debugging, please refer too[Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Attach",
            "type": "node",
            "request": "attach",
            "port": 5858,
            "address": "[device hostname or IP address]",
            "restart": false,
            "sourceMaps": false,
            "outDir": null,
            "localRoot": "${workspaceRoot}",
            "remoteRoot": null
        }
    ]
}
```

![Konfigurace vzdáleného ladění](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="df391-140">Připojte toohello vzdálené aplikace</span><span class="sxs-lookup"><span data-stu-id="df391-140">Attach toohello remote application</span></span>
<span data-ttu-id="df391-141">Klikněte na zelenou hello **spustit ladění** ladění toostart tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="df391-141">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="df391-142">Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn více informací o hello ladicí program.</span><span class="sxs-lookup"><span data-stu-id="df391-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Vzdálené ladění interaktivní](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="df391-144">Azure CLI problémy</span><span class="sxs-lookup"><span data-stu-id="df391-144">Azure CLI issues</span></span>
<span data-ttu-id="df391-145">Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="df391-145">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="df391-146">tooseek řešení, můžete použít hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="df391-146">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="df391-147">Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="df391-147">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="df391-148">Pomoc při řešení běžných potíží, zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="df391-148">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="df391-149">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="df391-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="df391-150">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="df391-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="df391-151">Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="df391-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="df391-152">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="df391-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="df391-153">Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="df391-153">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="df391-154">Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="df391-154">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="df391-155">Tento úkol použijte následující postup toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="df391-155">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="df391-156">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="df391-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="df391-157">Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="df391-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="df391-158">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="df391-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="df391-159">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="df391-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="df391-160">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="df391-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="df391-161">Pokud jste úspěšně zřídit služby Azure IoT hub pomocí rozhraní příkazového řádku Azure, a je nutné zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="df391-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="df391-162">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="df391-162">Device explorer</span></span>
<span data-ttu-id="df391-163">Hello [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nástroj spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="df391-163">hello [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="df391-164">Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="df391-164">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="df391-165">*Správa identit zařízení* tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="df391-165">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="df391-166">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="df391-166">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="df391-167">*Odeslat cloud zařízení* tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="df391-167">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="df391-168">Konfigurace připojovacího řetězce centra IoT v rámci této toouse nástroj všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="df391-168">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="df391-169">iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="df391-169">iothub-explorer</span></span>
<span data-ttu-id="df391-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) je nástroj příkazového řádku s více platformami ukázkové toomanage zařízení.</span><span class="sxs-lookup"><span data-stu-id="df391-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage devices.</span></span> <span data-ttu-id="df391-171">Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="df391-171">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="df391-172">tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="df391-172">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="df391-173">Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:</span><span class="sxs-lookup"><span data-stu-id="df391-173">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="df391-174">portál Azure</span><span class="sxs-lookup"><span data-stu-id="df391-174">Azure portal</span></span>
<span data-ttu-id="df391-175">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="df391-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="df391-176">Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="df391-176">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="df391-177">Azure problémů s úložištěm</span><span class="sxs-lookup"><span data-stu-id="df391-177">Azure Storage issues</span></span>
<span data-ttu-id="df391-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="df391-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="df391-179">Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní.</span><span class="sxs-lookup"><span data-stu-id="df391-179">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="df391-180">Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.</span><span class="sxs-lookup"><span data-stu-id="df391-180">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

