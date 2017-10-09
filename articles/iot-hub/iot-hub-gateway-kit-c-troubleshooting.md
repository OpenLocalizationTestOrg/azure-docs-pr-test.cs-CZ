---
title: "SensorTag zařízení & brány Azure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro bránu Intel NUC"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "problémy s IOT, internet věcí problémů"
ROBOTS: NOINDEX
ms.assetid: 5f742c38-0e33-465a-9a0d-1e41e8d17187
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ed6812c60412afb615012e3d694051d009b149a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="822ad-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="822ad-104">Troubleshooting</span></span>

## <a name="hardware-issues"></a><span data-ttu-id="822ad-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="822ad-105">Hardware issues</span></span>

### <a name="ti-sensortag-cannot-be-connected"></a><span data-ttu-id="822ad-106">Nelze se připojit TI SensorTag</span><span class="sxs-lookup"><span data-stu-id="822ad-106">TI SensorTag cannot be connected</span></span>

<span data-ttu-id="822ad-107">problémy s připojením SensorTag tootroubleshoot, použijte hello [SensorTag aplikace](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span><span class="sxs-lookup"><span data-stu-id="822ad-107">tootroubleshoot SensorTag connectivity issues, use hello [SensorTag app](http://processors.wiki.ti.com/index.php/SensorTag_User_Guide#SensorTag_App_user_guide).</span></span>

### <a name="have-an-issue-with-intel-nuc"></a><span data-ttu-id="822ad-108">Jít o problém s Intel NUC</span><span class="sxs-lookup"><span data-stu-id="822ad-108">Have an issue with Intel NUC</span></span>

<span data-ttu-id="822ad-109">tootroubleshoot spouštěcí problémy, najdete v příliš[řešení potíží s žádný spouštěcí na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span><span class="sxs-lookup"><span data-stu-id="822ad-109">tootroubleshoot boot issues, refer too[troubleshooting No Boot Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000005845.html).</span></span>

<span data-ttu-id="822ad-110">problémy s operačním systémem tootroubleshoot, najdete v příliš[řešení potíží s operačního systému na Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span><span class="sxs-lookup"><span data-stu-id="822ad-110">tootroubleshoot operating system issues, refer too[troubleshooting Operating System Issues on Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/000006018.html).</span></span>

<span data-ttu-id="822ad-111">tootroubleshoot další problémy, najdete v příliš[Blink kódy a zvukový signál kódy pro Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span><span class="sxs-lookup"><span data-stu-id="822ad-111">tootroubleshoot other issues, refer too[Blink Codes and Beep Codes for Intel NUC](http://www.intel.com/content/www/us/en/support/boards-and-kits/intel-nuc-boards/000005854.html).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="822ad-112">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="822ad-112">Node.js package issues</span></span>

### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="822ad-113">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="822ad-113">No response during gulp tasks</span></span>

<span data-ttu-id="822ad-114">Pokud narazíte na problémy ve spuštěné úkoly gulp, můžete přidat hello `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="822ad-114">If you encounter problems in running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="822ad-115">Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="822ad-115">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="822ad-116">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="822ad-116">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="822ad-117">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="822ad-117">Device discovery issues</span></span>

<span data-ttu-id="822ad-118">Pomoc při řešení běžných potíží s hello `discover-sensortag` příkaz, zkontrolujte hello [stránce wikiwebu](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span><span class="sxs-lookup"><span data-stu-id="822ad-118">For help in troubleshooting common problems with hello `discover-sensortag` command, check hello [wiki page](https://wiki.archlinux.org/index.php/bluetooth#Bluetoothctl).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="822ad-119">npm problémy</span><span class="sxs-lookup"><span data-stu-id="822ad-119">npm issues</span></span>

<span data-ttu-id="822ad-120">Zkuste tooupdate vašeho balíčku npm spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="822ad-120">Try tooupdate your npm package by running hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="822ad-121">Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span><span class="sxs-lookup"><span data-stu-id="822ad-121">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository](https://github.com/azure-samples/iot-hub-c-intel-nuc-gateway-getting-started).</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="822ad-122">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="822ad-122">Remote Debugging</span></span>
> <span data-ttu-id="822ad-123">Následující pokyny jsou určené pro ladění node.js skripty použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="822ad-123">Below instructions are meant for debugging node.js scripts used in this tutorial.</span></span>
### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="822ad-124">Spuštění ukázkové aplikace hello v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="822ad-124">Run hello sample application in debug mode</span></span>

<span data-ttu-id="822ad-125">Spusťte hello ukázkovou aplikaci v režimu ladění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="822ad-125">Run hello sample application in debug mode by running hello following command:</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="822ad-126">Když modul ladění hello je připraven, měli byste vidět `Debugger listening on port 5858` ve výstupu konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="822ad-126">When hello debug engine is ready, you should see `Debugger listening on port 5858` in hello console output.</span></span>

### <a name="configure-visual-studio-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="822ad-127">Konfigurace Visual Studio Code tooconnect toohello vzdáleném zařízení</span><span class="sxs-lookup"><span data-stu-id="822ad-127">Configure Visual Studio Code tooconnect toohello remote device</span></span>

1. <span data-ttu-id="822ad-128">Otevřete hello **ladění** panely na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="822ad-128">Open hello **Debug** panel on hello left side.</span></span>
2. <span data-ttu-id="822ad-129">Klikněte na zelenou hello **spustit ladění** tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="822ad-129">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="822ad-130">Otevře se Visual Studio Code `launch.json` souboru.</span><span class="sxs-lookup"><span data-stu-id="822ad-130">Visual Studio Code opens a `launch.json` file.</span></span>
3. <span data-ttu-id="822ad-131">Aktualizace hello `launch.json` soubor s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="822ad-131">Update hello `launch.json` file with hello following content.</span></span> <span data-ttu-id="822ad-132">Nahraďte `[device hostname or IP address]` s hello skutečné zařízení IP adresu nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="822ad-132">Replace `[device hostname or IP address]` with hello actual device IP address or host name.</span></span>

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

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="822ad-134">Připojte toohello vzdálené aplikace</span><span class="sxs-lookup"><span data-stu-id="822ad-134">Attach toohello remote application</span></span>

<span data-ttu-id="822ad-135">Klikněte na zelenou hello **spustit ladění** ladění toostart tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="822ad-135">Click hello green **Start Debugging** (F5) button toostart debugging.</span></span>

<span data-ttu-id="822ad-136">Čtení [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn více informací o hello ladicí program.</span><span class="sxs-lookup"><span data-stu-id="822ad-136">Read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Ukázka zakázat ladění](./media/iot-hub-gateway-kit-lessons/troubleshooting/debugging_ble_sample.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="822ad-138">Azure CLI problémy</span><span class="sxs-lookup"><span data-stu-id="822ad-138">Azure CLI issues</span></span>

<span data-ttu-id="822ad-139">Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="822ad-139">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="822ad-140">tooseek řešení, můžete použít hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span><span class="sxs-lookup"><span data-stu-id="822ad-140">tooseek solutions, you can use hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md).</span></span>

<span data-ttu-id="822ad-141">Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="822ad-141">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="822ad-142">Pomoc při řešení běžných potíží, zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="822ad-142">For help in troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="822ad-143">Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.</span><span class="sxs-lookup"><span data-stu-id="822ad-143">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="822ad-144">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="822ad-144">Python installation issues</span></span>

### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="822ad-145">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="822ad-145">Legacy installation issues (macOS)</span></span>

<span data-ttu-id="822ad-146">Když instalujete pip, oprávnění chyba se vyvolá, když starší balíčky jsou nainstalované s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="822ad-146">When you're installing pip, a permission error is thrown when older packages are installed with **su** permissions.</span></span> <span data-ttu-id="822ad-147">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="822ad-147">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="822ad-148">Některé balíčky pip z předchozí instalace byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="822ad-148">Some pip packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="822ad-149">Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="822ad-149">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="822ad-150">Tento úkol použijte následující postup toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="822ad-150">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="822ad-151">Přejděte příliš`/usr/local/lib/python2.7/site-packages`</span><span class="sxs-lookup"><span data-stu-id="822ad-151">Go too`/usr/local/lib/python2.7/site-packages`</span></span>
2. <span data-ttu-id="822ad-152">Seznam balíčky vytvořené pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="822ad-152">List packages created by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="822ad-153">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="822ad-153">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="822ad-154">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="822ad-154">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="822ad-155">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="822ad-155">Azure IoT Hub issues</span></span>

<span data-ttu-id="822ad-156">Pokud jste úspěšně zřídit služby Azure IoT hub s hello rozhraní příkazového řádku Azure, a je nutné zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje.</span><span class="sxs-lookup"><span data-stu-id="822ad-156">If you've successfully provisioned your Azure IoT hub with hello Azure CLI, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools.</span></span>

### <a name="device-explorer"></a><span data-ttu-id="822ad-157">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="822ad-157">Device Explorer</span></span>

<span data-ttu-id="822ad-158">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="822ad-158">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="822ad-159">Komunikuje s následující hello [koncové body centra IoT](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span><span class="sxs-lookup"><span data-stu-id="822ad-159">It communicates with hello following [IoT Hub endpoints](https://azure.microsoft.com/en-us/documentation/articles/iot-hub-devguide/):</span></span>

- <span data-ttu-id="822ad-160">Tooprovision správy identity zařízení a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="822ad-160">Device identity management tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="822ad-161">Přijímat zařízení cloud, takže můžete sledovat zprávy odeslané ze zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="822ad-161">Receive device-to-cloud so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="822ad-162">Odesláno cloud zařízení, můžete mohou zasílat zprávy tooyour zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="822ad-162">Send cloud-to-device so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="822ad-163">Konfigurace připojovacího řetězce centra IoT v rámci této toouse nástroj všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="822ad-163">Configure your IoT hub connection string within this tool toouse all its capabilities.</span></span>

### <a name="iothub-explorer"></a><span data-ttu-id="822ad-164">iothub-explorer</span><span class="sxs-lookup"><span data-stu-id="822ad-164">iothub-explorer</span></span>

<span data-ttu-id="822ad-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="822ad-165">[iothub-explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="822ad-166">Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="822ad-166">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="822ad-167">tooinstall hello poslední (předprodejní) verzi nástroje hello iothub-explorer, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="822ad-167">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="822ad-168">Další nápovědu tooget o všech hello iothub-explorer spolu s jejich parametry, spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="822ad-168">tooget additional help about all hello iothub-explorer commands and their parameters, run hello following command:</span></span>

```bash
iothub-explorer help
```

### <a name="hello-azure-portal"></a><span data-ttu-id="822ad-169">Hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="822ad-169">hello Azure portal</span></span>

<span data-ttu-id="822ad-170">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="822ad-170">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="822ad-171">Můžete také toouse hello [portál Azure](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="822ad-171">You might also want toouse hello [Azure portal](https://azure.microsoft.com/en-us/documentation/articles/azure-portal-overview/) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="822ad-172">Azure problémů s úložištěm</span><span class="sxs-lookup"><span data-stu-id="822ad-172">Azure Storage issues</span></span>

<span data-ttu-id="822ad-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="822ad-173">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com/) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="822ad-174">Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní.</span><span class="sxs-lookup"><span data-stu-id="822ad-174">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="822ad-175">Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.</span><span class="sxs-lookup"><span data-stu-id="822ad-175">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>
