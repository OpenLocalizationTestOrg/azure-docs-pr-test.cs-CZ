---
title: "Malinová Pi (C) připojit k Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí malin pí Node.js"
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
ms.openlocfilehash: f9058068972ddbb674d3cd159948835dc88c4451
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="615d3-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="615d3-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="615d3-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="615d3-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="615d3-106">Aplikace běží správně, ale není blikat indikátor LED</span><span class="sxs-lookup"><span data-stu-id="615d3-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="615d3-107">Tento problém je vždy související s konektivitou okruhu hardwaru.</span><span class="sxs-lookup"><span data-stu-id="615d3-107">This issue is always related to hardware circuit connectivity.</span></span> <span data-ttu-id="615d3-108">K identifikaci problémů použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="615d3-108">Use the following steps to identify problems:</span></span>

1. <span data-ttu-id="615d3-109">Zkontrolujte, že jste vybrali správný **GPIO** vaší karty.</span><span class="sxs-lookup"><span data-stu-id="615d3-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="615d3-110">Dva porty musí být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="615d3-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="615d3-111">Zkontrolujte správnost polarita z vaší Indikátor.</span><span class="sxs-lookup"><span data-stu-id="615d3-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="615d3-112">Delší větev by měl být uveden **kladné**, anod pin.</span><span class="sxs-lookup"><span data-stu-id="615d3-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="615d3-113">Použití **3.3v připnout** a **zem Pin** na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="615d3-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="615d3-114">Pi považovat za power řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="615d3-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="615d3-115">Zkontrolujte, jestli DIODU funguje bez problémů.</span><span class="sxs-lookup"><span data-stu-id="615d3-115">Check that the LED works fine.</span></span>

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="615d3-117">Další potíže s hardwarem</span><span class="sxs-lookup"><span data-stu-id="615d3-117">Other hardware issues</span></span>
<span data-ttu-id="615d3-118">Informace o řešení běžných problémů na 3 malin platformy najdete v tématu [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="615d3-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="615d3-119">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="615d3-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="615d3-120">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="615d3-120">No response during gulp tasks</span></span>
<span data-ttu-id="615d3-121">Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="615d3-121">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="615d3-122">Zkuste ukončit aktuální gulp úlohy pomocí kombinace kláves Ctrl + C, a poté spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění.</span><span class="sxs-lookup"><span data-stu-id="615d3-122">Try to terminate current gulp tasks by using Ctrl + C, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="615d3-123">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="615d3-123">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="615d3-124">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="615d3-124">Device discovery issues</span></span>
<span data-ttu-id="615d3-125">Pomoc při řešení běžných potíží s `devdisco` příkazu, zkontrolujte [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="615d3-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="615d3-126">npm problémy</span><span class="sxs-lookup"><span data-stu-id="615d3-126">npm issues</span></span>
<span data-ttu-id="615d3-127">Zkuste aktualizovat vašeho balíčku npm pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="615d3-127">Try to update your npm package by using the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="615d3-128">Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span><span class="sxs-lookup"><span data-stu-id="615d3-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="615d3-129">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="615d3-129">Remote debugging</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="615d3-130">Spuštění ukázkové aplikace v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="615d3-130">Run the sample application in debug mode</span></span>
```bash
gulp run --debug
```

<span data-ttu-id="615d3-131">Když modul ladění je připraven, měli byste vidět ```Debugger listening on port 5858``` ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="615d3-131">When the debug engine is ready, you should see ```Debugger listening on port 5858``` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="615d3-132">Konfigurace Visual Studio Code pro připojení k vzdálené zařízení</span><span class="sxs-lookup"><span data-stu-id="615d3-132">Configure Visual Studio Code to connect to the remote device</span></span>
1. <span data-ttu-id="615d3-133">Otevřete **ladění** panely na levé straně.</span><span class="sxs-lookup"><span data-stu-id="615d3-133">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="615d3-134">Klikněte na tlačítko se zeleným **spustit ladění** tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="615d3-134">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="615d3-135">Visual Studio Code otevře soubor launch.json.</span><span class="sxs-lookup"><span data-stu-id="615d3-135">Visual Studio Code opens a launch.json file.</span></span>
3. <span data-ttu-id="615d3-136">Aktualizujte soubor launch.json s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="615d3-136">Update the launch.json file with the following content.</span></span> <span data-ttu-id="615d3-137">Nahraďte `[device hostname or IP address]` s skutečné zařízení IP adresu nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="615d3-137">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

> [!NOTE]
> <span data-ttu-id="615d3-138">Můžete najít další informace o Visual Studio, ladění, [ladění ve Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span><span class="sxs-lookup"><span data-stu-id="615d3-138">To learn more about the Visual Studio Debugging, please refer to [Debugging in Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging#_launchjson-attributes).</span></span>


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

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="615d3-140">Připojení k vzdálené aplikaci</span><span class="sxs-lookup"><span data-stu-id="615d3-140">Attach to the remote application</span></span>
<span data-ttu-id="615d3-141">Klikněte na tlačítko se zeleným **spustit ladění** (F5) tlačítko Spustit ladění.</span><span class="sxs-lookup"><span data-stu-id="615d3-141">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="615d3-142">Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) Další informace o ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="615d3-142">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Vzdálené ladění interaktivní](media/iot-hub-raspberry-pi-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="615d3-144">Azure CLI problémy</span><span class="sxs-lookup"><span data-stu-id="615d3-144">Azure CLI issues</span></span>
<span data-ttu-id="615d3-145">Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="615d3-145">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="615d3-146">K vyhledání řešení, můžete použít [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="615d3-146">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="615d3-147">Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="615d3-147">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="615d3-148">Pomoc při řešení běžných potíží, najdete [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="615d3-148">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

## <a name="python-installation-issues"></a><span data-ttu-id="615d3-149">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="615d3-149">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="615d3-150">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="615d3-150">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="615d3-151">Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="615d3-151">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="615d3-152">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="615d3-152">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="615d3-153">Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění.</span><span class="sxs-lookup"><span data-stu-id="615d3-153">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="615d3-154">Řešení je odebrat tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="615d3-154">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="615d3-155">Tuto úlohu dokončit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="615d3-155">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="615d3-156">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="615d3-156">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="615d3-157">Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="615d3-157">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="615d3-158">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="615d3-158">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="615d3-159">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="615d3-159">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="615d3-160">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="615d3-160">Azure IoT Hub issues</span></span>
<span data-ttu-id="615d3-161">Pokud jste úspěšně zřídit služby Azure IoT hub pomocí rozhraní příkazového řádku Azure, musíte nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="615d3-161">If you've successfully provisioned your Azure IoT hub with Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="615d3-162">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="615d3-162">Device explorer</span></span>
<span data-ttu-id="615d3-163">[Explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) nástroj spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="615d3-163">The [Device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) tool runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="615d3-164">Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="615d3-164">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>


* <span data-ttu-id="615d3-165">*Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="615d3-165">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="615d3-166">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="615d3-166">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="615d3-167">*Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="615d3-167">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="615d3-168">Konfigurace připojovacího řetězce IoT hub v rámci tohoto nástroje můžete použít všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="615d3-168">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="615d3-169">iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="615d3-169">iothub-explorer</span></span>
<span data-ttu-id="615d3-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě zařízení.</span><span class="sxs-lookup"><span data-stu-id="615d3-170">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage devices.</span></span> <span data-ttu-id="615d3-171">Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílání zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="615d3-171">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device messages.</span></span>

<span data-ttu-id="615d3-172">Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="615d3-172">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="615d3-173">Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="615d3-173">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="615d3-174">portál Azure</span><span class="sxs-lookup"><span data-stu-id="615d3-174">Azure portal</span></span>
<span data-ttu-id="615d3-175">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="615d3-175">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="615d3-176">Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="615d3-176">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="615d3-177">Azure problémů s úložištěm</span><span class="sxs-lookup"><span data-stu-id="615d3-177">Azure Storage issues</span></span>
<span data-ttu-id="615d3-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="615d3-178">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="615d3-179">Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní.</span><span class="sxs-lookup"><span data-stu-id="615d3-179">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="615d3-180">Tento nástroj slouží k řešení potíží vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="615d3-180">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

