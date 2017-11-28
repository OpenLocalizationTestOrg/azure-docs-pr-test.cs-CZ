---
title: "Připojit Intel Edison (uzel) tooAzure IoT - Lekce 4: řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí Intel Edison Node.js"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "řešení potíží s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: f11c5521-8a36-44c0-baad-f5ec26ab4618
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9502300f7f372255572b49bea45dd3e1fdaeeb37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="22840-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="22840-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="22840-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="22840-105">Hardware issues</span></span>
<span data-ttu-id="22840-106">Informace o řešení běžných problémů na Intel Edison najdete v tématu hello [oficiální stránka řešení potíží](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="22840-106">For information about solving common problems on Intel Edison, see hello [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="22840-107">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="22840-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="22840-108">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="22840-108">No response during gulp tasks</span></span>
<span data-ttu-id="22840-109">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat hello `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="22840-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="22840-110">Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="22840-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="22840-111">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="22840-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="22840-112">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="22840-112">NPM issues</span></span>
<span data-ttu-id="22840-113">Zkuste tooupdate vašeho balíčku NPM s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="22840-113">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="22840-114">Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="22840-114">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="22840-115">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="22840-115">Remote debugging</span></span>

### <a name="run-hello-sample-application-in-debug-mode"></a><span data-ttu-id="22840-116">Spuštění ukázkové aplikace hello v režimu ladění</span><span class="sxs-lookup"><span data-stu-id="22840-116">Run hello sample application in debug mode</span></span>

```bash
gulp run --debug
```

<span data-ttu-id="22840-117">Po hello ladění modul je připraven, byste měli mít toosee ```Debugger listening on port 5858``` z výstupu konzoly hello.</span><span class="sxs-lookup"><span data-stu-id="22840-117">Once hello debug engine is ready, you should be able toosee ```Debugger listening on port 5858``` from hello console output.</span></span>

### <a name="configure-vs-code-tooconnect-toohello-remote-device"></a><span data-ttu-id="22840-118">Konfigurace VS Code tooconnect toohello vzdáleném zařízení</span><span class="sxs-lookup"><span data-stu-id="22840-118">Configure VS Code tooconnect toohello remote device</span></span>

<span data-ttu-id="22840-119">Otevřete hello **ladění** panely z hello levé straně.</span><span class="sxs-lookup"><span data-stu-id="22840-119">Open hello **Debug** panel from hello left side.</span></span>

<span data-ttu-id="22840-120">Klikněte na zelenou hello **spustit ladění** tlačítko (F5).</span><span class="sxs-lookup"><span data-stu-id="22840-120">Click hello green **Start Debugging** (F5) button.</span></span> <span data-ttu-id="22840-121">By se otevřely VS Code **launch.json** souborů, které budete potřebovat tooupdate.</span><span class="sxs-lookup"><span data-stu-id="22840-121">VS Code would open a **launch.json** file, which you need tooupdate.</span></span>

<span data-ttu-id="22840-122">Aktualizace hello **launch.json** soubor s hello následující obsah, nahraďte `[device hostname or IP address]` s hello skutečné zařízení IP adresu nebo název hostitele.</span><span class="sxs-lookup"><span data-stu-id="22840-122">Update hello **launch.json** file with hello following content, replace `[device hostname or IP address]` with hello actual device IP address or hostname.</span></span>  

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

![Konfigurace vzdáleného ladění](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_configuration.png)

### <a name="attach-toohello-remote-application"></a><span data-ttu-id="22840-124">Připojte toohello vzdálené aplikace</span><span class="sxs-lookup"><span data-stu-id="22840-124">Attach toohello remote application</span></span>

<span data-ttu-id="22840-125">Klikněte na zelenou hello **spustit ladění** (F5) tlačítko a získejte ladění.</span><span class="sxs-lookup"><span data-stu-id="22840-125">Click hello green **Start Debugging** (F5) button and enjoy debugging.</span></span>

<span data-ttu-id="22840-126">Můžete si přečíst [JavaScript v produktu VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn více informací o hello ladicí program.</span><span class="sxs-lookup"><span data-stu-id="22840-126">You can read [JavaScript in VS Code](https://code.visualstudio.com/docs/languages/javascript#_debugging) toolearn more about hello debugger.</span></span>

![Vzdálené ladění interaktivní](media/iot-hub-intel-edison-lessons/troubleshooting/remote_debugging_interactive.png)

## <a name="azure-cli-issues"></a><span data-ttu-id="22840-128">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="22840-128">Azure-CLI issues</span></span>
<span data-ttu-id="22840-129">Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="22840-129">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="22840-130">Vyhledejte řešení v hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek řešení.</span><span class="sxs-lookup"><span data-stu-id="22840-130">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="22840-131">Zkuste tooupgrade rozhraní příkazového řádku Azure toolatest verze při příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="22840-131">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="22840-132">Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="22840-132">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="22840-133">Nápovědu k řešení běžných potíží s zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="22840-133">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="22840-134">Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.</span><span class="sxs-lookup"><span data-stu-id="22840-134">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="22840-135">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="22840-135">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="22840-136">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="22840-136">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="22840-137">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="22840-137">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="22840-138">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="22840-138">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="22840-139">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="22840-139">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="22840-140">Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="22840-140">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="22840-141">Tento úkol použijte následující postup toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="22840-141">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="22840-142">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="22840-142">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="22840-143">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="22840-143">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="22840-144">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="22840-144">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="22840-145">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="22840-145">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="22840-146">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="22840-146">Azure IoT Hub issues</span></span>
<span data-ttu-id="22840-147">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je třeba zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="22840-147">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="22840-148">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="22840-148">Device Explorer</span></span>
<span data-ttu-id="22840-149">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="22840-149">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="22840-150">Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="22840-150">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="22840-151">_Správa identit zařízení_ tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22840-151">_Device identity management_ tooprovision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="22840-152">_Zobrazí zařízení cloud_ , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="22840-152">_Receive device-to-cloud_ so you can monitor messages sent from your device tooyour IoT hub.</span></span>
- <span data-ttu-id="22840-153">_Odeslat cloud zařízení_ tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="22840-153">_Send cloud-to-device_ so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="22840-154">Konfigurace vaší `IoT hub connection string` v rámci této nástroj toouse všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="22840-154">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="22840-155">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="22840-155">IoT hub Explorer</span></span>
<span data-ttu-id="22840-156">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="22840-156">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="22840-157">Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="22840-157">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="22840-158">tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="22840-158">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="22840-159">Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:</span><span class="sxs-lookup"><span data-stu-id="22840-159">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="22840-160">portál Azure</span><span class="sxs-lookup"><span data-stu-id="22840-160">Azure portal</span></span>
<span data-ttu-id="22840-161">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="22840-161">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="22840-162">Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="22840-162">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="22840-163">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="22840-163">Azure storage issues</span></span>
<span data-ttu-id="22840-164">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít toowork s [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) dat v systému Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="22840-164">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="22840-165">Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní.</span><span class="sxs-lookup"><span data-stu-id="22840-165">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="22840-166">Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.</span><span class="sxs-lookup"><span data-stu-id="22840-166">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="22840-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="22840-167">Next steps</span></span>
<span data-ttu-id="22840-168">Tato stránka obsahuje pouze hello nejběžnější problémy Intel Edison Kit.</span><span class="sxs-lookup"><span data-stu-id="22840-168">This page only includes hello most common problems of Intel Edison kit.</span></span> <span data-ttu-id="22840-169">Můžete také ponechat dolní komentáře tooreport problémy pro odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="22840-169">You can also leave bottom comments tooreport issues for further troubleshooting.</span></span>

<span data-ttu-id="22840-170">Přejděte zpět příliš[začít pracovat s Intel Edison (Node.js)](iot-hub-intel-edison-kit-node-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="22840-170">Go back too[Get started with Intel Edison (Node.js)](iot-hub-intel-edison-kit-node-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-node-edison-getting-started