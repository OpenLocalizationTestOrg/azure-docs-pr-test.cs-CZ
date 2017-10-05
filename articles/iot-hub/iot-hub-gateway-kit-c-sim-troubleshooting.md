---
title: "Simulované zařízení & brány Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro bránu Intel NUC"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
ms.assetid: 3ee8f4b0-5799-40a3-8cf0-8d5aa44dbc2b
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: eae4c112accaefa8bd1bf85f7b43badc2f491dfd
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="a806c-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="a806c-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="a806c-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="a806c-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="a806c-106">Nelze se připojit TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="a806c-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="a806c-107">Chcete-li vyřešit potíže s připojením k SensorTag, použijte [SensorTag aplikace](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="a806c-107">To troubleshoot SensorTag connectivity issues, use the [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="a806c-108">Jít o problém s Intel NUC</span><span class="sxs-lookup"><span data-stu-id="a806c-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="a806c-109">Odstraňování problémů, spouštění, najdete v tématu [řešení potíží s žádný spouštěcí na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="a806c-109">To troubleshoot boot issues, refer to [troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="a806c-110">Odstraňování problémů, operační systém, najdete v tématu [řešení potíží s operačního systému na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="a806c-110">To troubleshoot operating system issues, refer to [troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="a806c-111">Řešení potíží s další problémy, najdete v tématu [Blink kódy a zvukový signál kódy pro Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="a806c-111">To troubleshoot other issues, refer to [Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="a806c-112">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="a806c-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="a806c-113">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="a806c-113">No response during gulp tasks</span></span>

<span data-ttu-id="a806c-114">Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="a806c-114">If you encounter problems in running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="a806c-115">Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění.</span><span class="sxs-lookup"><span data-stu-id="a806c-115">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="a806c-116">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="a806c-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="a806c-117">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="a806c-117">Device discovery issues</span></span>

<span data-ttu-id="a806c-118">Pomoc při řešení běžných potíží s `discover-sensortag` příkazu, zkontrolujte [stránce wikiwebu](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="a806c-118">For help in troubleshooting common problems with the `discover-sensortag` command, check the [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="a806c-119">npm problémy</span><span class="sxs-lookup"><span data-stu-id="a806c-119">npm issues</span></span>

<span data-ttu-id="a806c-120">Došlo k pokusu o aktualizaci vašeho balíčku npm spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="a806c-120">Try to update your npm package by running the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="a806c-121">Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="a806c-121">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="a806c-122">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="a806c-122">Remote Debugging</span></span>
> <span data-ttu-id="a806c-123">Následující pokyny jsou určené pro ladění node.js skripty použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="a806c-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-the-sample-application-in-debug-mode"></a><span data-ttu-id="a806c-124">Spuštění ukázkové aplikace v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="a806c-124">Run the sample application in debug mode</span></span>

<span data-ttu-id="a806c-125">Spuštění ukázkové aplikace v režimu ladění tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a806c-125">Run the sample application in debug mode by running the following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="a806c-126">Když modul ladění je připraven, měli byste vidět `Debugger listening on port 5858` ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="a806c-126">When the debug engine is ready, you should see `Debugger listening on port 5858` in the console output.</span></span>

### <a name="configure-visual-studio-code-to-connect-to-the-remote-device"></a><span data-ttu-id="a806c-127">Konfigurace Visual Studio Code pro připojení k vzdálené zařízení</span><span class="sxs-lookup"><span data-stu-id="a806c-127">Configure Visual Studio Code to connect to the remote device</span></span>

1. <span data-ttu-id="a806c-128">Otevřete **ladění** panely na levé straně.</span><span class="sxs-lookup"><span data-stu-id="a806c-128">Open the **Debug** panel on the left side.</span></span>
2. <span data-ttu-id="a806c-129">Klikněte na tlačítko se zeleným **spustit ladění** tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="a806c-129">Click the green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="a806c-130">Otevře se Visual Studio Code `launch.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="a806c-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="a806c-131">Aktualizace `launch.json` soubor s následujícím obsahem.</span><span class="sxs-lookup"><span data-stu-id="a806c-131">Update the `launch.json` file with the following content.</span></span> <span data-ttu-id="a806c-132">Nahraďte `[device hostname or IP address]` s skutečné zařízení IP adresu nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="a806c-132">Replace `[device hostname or IP address]` with the actual device IP address or host name.</span></span>

   ``` json
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
            "remoteRoot": "~/ble_sample"
        }
     ]
   }
   ```

![Konfigurace vzdáleného ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-to-the-remote-application"></a><span data-ttu-id="a806c-134">Připojení k vzdálené aplikaci</span><span class="sxs-lookup"><span data-stu-id="a806c-134">Attach to the remote application</span></span>

<span data-ttu-id="a806c-135">Klikněte na tlačítko se zeleným **spustit ladění** (F5) tlačítko Spustit ladění.</span><span class="sxs-lookup"><span data-stu-id="a806c-135">Click the green **Start Debugging** (F5) button to start debugging.</span></span>

<span data-ttu-id="a806c-136">Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) Další informace o ladicího programu.</span><span class="sxs-lookup"><span data-stu-id="a806c-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) to learn more about the debugger.</span></span>

![Ukázka zakázat ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="a806c-138">Azure CLI problémy</span><span class="sxs-lookup"><span data-stu-id="a806c-138">Azure CLI issues</span></span>

<span data-ttu-id="a806c-139">Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="a806c-139">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="a806c-140">K vyhledání řešení, můžete použít [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="a806c-140">To seek solutions, you can use the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="a806c-141">Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="a806c-141">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="a806c-142">Pomoc při řešení běžných potíží, najdete [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="a806c-142">For help in troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="a806c-143">Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.</span><span class="sxs-lookup"><span data-stu-id="a806c-143">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="a806c-144">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="a806c-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="a806c-145">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="a806c-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="a806c-146">Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a806c-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="a806c-147">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="a806c-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="a806c-148">Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a806c-148">Some pip packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="a806c-149">Řešení je odebrat tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="a806c-149">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="a806c-150">Tuto úlohu dokončit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="a806c-150">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="a806c-151">Přejděte na `/usr/local/lib/python2.7/site-packages`.</span><span class="sxs-lookup"><span data-stu-id="a806c-151">Go to `/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="a806c-152">Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="a806c-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="a806c-153">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="a806c-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="a806c-154">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="a806c-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="a806c-155">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="a806c-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="a806c-156">Pokud jste úspěšně zřídit služby Azure IoT hub pomocí Azure CLI, musíte nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="a806c-156">If you've successfully provisioned your Azure IoT hub with the Azure CLI, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="a806c-157">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="a806c-157">Device Explorer</span></span>

<span data-ttu-id="a806c-158">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="a806c-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="a806c-159">Komunikuje s následující [koncové body centra IoT](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="a806c-159">It communicates with the following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="a806c-160">Správa identit zařízení a zřizovat a spravovat zařízení zaregistrován u služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a806c-160">Device identity management to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="a806c-161">Zobrazí zařízení cloud, můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a806c-161">Receive device-to-cloud so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="a806c-162">Cloud zařízení odešlete, tak mohou zasílat zprávy do zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="a806c-162">Send cloud-to-device so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="a806c-163">Konfigurace připojovacího řetězce IoT hub v rámci tohoto nástroje můžete použít všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="a806c-163">Configure your IoT hub connection string within this tool to use all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="a806c-164">iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="a806c-164">iothub-explorer</span></span>

<span data-ttu-id="a806c-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="a806c-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="a806c-166">Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="a806c-166">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="a806c-167">Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a806c-167">To install the latest (prerelease) version of the iothub-explorer tool, run the following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="a806c-168">Chcete-li získat další nápovědu k všechny příkazy iothub-explorer a jejich parametrů, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="a806c-168">To get additional help about all the iothub-explorer commands and their parameters, run the following command:</span></span>

```bash
iothub-explorer help
```

### <a name="the-azure-portal"></a><span data-ttu-id="a806c-169">Portál Azure</span><span class="sxs-lookup"><span data-stu-id="a806c-169">The Azure portal</span></span>

<span data-ttu-id="a806c-170">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="a806c-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="a806c-171">Můžete také použít [portál Azure](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) pomoci zřizování, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="a806c-171">You might also want to use the [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="a806c-172">Azure problémů s úložištěm</span><span class="sxs-lookup"><span data-stu-id="a806c-172">Azure Storage issues</span></span>

<span data-ttu-id="a806c-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="a806c-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="a806c-174">Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní.</span><span class="sxs-lookup"><span data-stu-id="a806c-174">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="a806c-175">Tento nástroj slouží k řešení potíží vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a806c-175">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
