---
title: "Connect Arduino (C) tooAzure IoT – řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro Adafruit prolnutí M0 Wi-Fi Arduino prostředí"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "řešení potíží s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: fdcc56ff-4420-463c-8a0e-5a1d215a874f
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ed793041ffa1887dbe73067f7c48d2417e982866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="4a704-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="4a704-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="4a704-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="4a704-105">Hardware issues</span></span>
<span data-ttu-id="4a704-106">Informace o řešení běžných problémů na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino najdete v tématu hello [oficiální stránka řešení potíží](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span><span class="sxs-lookup"><span data-stu-id="4a704-106">For information about solving common problems on your Adafruit Feather M0 WiFi Arduino board, see hello [official troubleshooting page](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="4a704-107">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="4a704-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="4a704-108">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="4a704-108">No response during gulp tasks</span></span>
<span data-ttu-id="4a704-109">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat hello `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="4a704-109">If you encounter problems running gulp tasks, you can add hello `--verbose` option for debugging.</span></span> <span data-ttu-id="4a704-110">Zkuste tooterminate aktuální gulp úlohy pomocí `Ctrl + C`, a pak spusťte hello následující příkaz v vaše zprávy ladění toosee okna konzoly.</span><span class="sxs-lookup"><span data-stu-id="4a704-110">Try tooterminate current gulp tasks by using `Ctrl + C`, and then run hello following command in your console window toosee debug messages.</span></span> <span data-ttu-id="4a704-111">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="4a704-111">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

<span data-ttu-id="4a704-112">Nebo můžete přidat `--listen` informace o protokolu tooopen sériového portu toooutput zařízení.</span><span class="sxs-lookup"><span data-stu-id="4a704-112">Or you can add `--listen` tooopen serial port toooutput device log information.</span></span>

```bash
gulp --listen
``` 

### <a name="npm-issues"></a><span data-ttu-id="4a704-113">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="4a704-113">NPM issues</span></span>
<span data-ttu-id="4a704-114">Zkuste tooupdate vašeho balíčku NPM s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4a704-114">Try tooupdate your NPM package with hello following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="4a704-115">Pokud problém hello stále existuje, ponechte komentáře na konci hello tohoto článku nebo vytvořit problém Githubu v našem [úložiště ukázkové][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="4a704-115">If hello problem still exists, leave your comments at hello end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="4a704-116">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="4a704-116">Azure-CLI issues</span></span>
<span data-ttu-id="4a704-117">Hello rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="4a704-117">hello Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="4a704-118">Vyhledejte řešení v hello [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek řešení.</span><span class="sxs-lookup"><span data-stu-id="4a704-118">Look for solution in hello [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) tooseek solutions.</span></span> <span data-ttu-id="4a704-119">Zkuste tooupgrade rozhraní příkazového řádku Azure toolatest verze při příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="4a704-119">Try tooupgrade Azure-cli toolatest version when commands don’t work as expected.</span></span>

<span data-ttu-id="4a704-120">Pokud narazíte na všechny chyby nástrojem hello, soubor [problém](https://github.com/Azure/azure-cli/issues) v hello **problémy** část úložiště GitHub hello.</span><span class="sxs-lookup"><span data-stu-id="4a704-120">If you encounter any bugs with hello tool, file an [issue](https://github.com/Azure/azure-cli/issues) in hello **Issues** section of hello GitHub repo.</span></span>

<span data-ttu-id="4a704-121">Nápovědu k řešení běžných potíží s zkontrolujte hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="4a704-121">For help troubleshooting common problems, check hello [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="4a704-122">Pokud jsou splněny "Nelze najít na verzi, která by splnila požadavek hello", prosím hello spusťte následující příkaz tooupgrade pip toolastest verze.</span><span class="sxs-lookup"><span data-stu-id="4a704-122">If you meet "Could not find a version that satisfies hello requirement", please run hello following command tooupgrade pip toolastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="4a704-123">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="4a704-123">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="4a704-124">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="4a704-124">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="4a704-125">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4a704-125">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="4a704-126">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="4a704-126">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="4a704-127">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění hello.</span><span class="sxs-lookup"><span data-stu-id="4a704-127">Some **pip** packages from a previous installation were created by root, which causes hello permission error.</span></span> <span data-ttu-id="4a704-128">Hello řešení je tooremove tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="4a704-128">hello solution is tooremove those packages installed by root.</span></span> <span data-ttu-id="4a704-129">Tento úkol použijte následující postup toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="4a704-129">Use hello following steps toocomplete this task:</span></span>

1. <span data-ttu-id="4a704-130">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="4a704-130">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="4a704-131">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="4a704-131">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="4a704-132">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="4a704-132">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="4a704-133">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="4a704-133">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="4a704-134">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="4a704-134">Azure IoT Hub issues</span></span>
<span data-ttu-id="4a704-135">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je třeba zařízení hello toomanage nástroj, kteří se připojují tooyour IoT hub, zkuste hello následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="4a704-135">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool toomanage hello devices that are connecting tooyour IoT hub, try hello following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="4a704-136">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="4a704-136">Device Explorer</span></span>
<span data-ttu-id="4a704-137">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí tooyour IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="4a704-137">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects tooyour IoT hub in Azure.</span></span> <span data-ttu-id="4a704-138">Komunikuje s následující hello [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="4a704-138">It communicates with hello following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="4a704-139">*Správa identit zařízení* tooprovision a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4a704-139">*Device identity management* tooprovision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="4a704-140">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="4a704-140">*Receive device-to-cloud* so you can monitor messages sent from your device tooyour IoT hub.</span></span>
* <span data-ttu-id="4a704-141">*Odeslat cloud zařízení* tak zprávy lze odesílat tooyour zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="4a704-141">*Send cloud-to-device* so you can send messages tooyour devices from your IoT hub.</span></span>

<span data-ttu-id="4a704-142">Konfigurace vaší `IoT hub connection string` v rámci této nástroj toouse všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="4a704-142">Configure your `IoT hub connection string` within this tool toouse all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="4a704-143">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="4a704-143">IoT hub Explorer</span></span>
<span data-ttu-id="4a704-144">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku toomanage klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="4a704-144">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool toomanage device clients.</span></span> <span data-ttu-id="4a704-145">Můžete používat hello nástroj toomanage hello zařízení v registru identit hello, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="4a704-145">You can use hello tool toomanage hello devices in hello identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>


<span data-ttu-id="4a704-146">tooinstall hello poslední (předprodejní) verzi nástroje iothub-explorer hello, spusťte následující příkaz v prostředí příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="4a704-146">tooinstall hello latest (prerelease) version of hello iothub-explorer tool, run hello following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="4a704-147">Můžete použít následující příkaz, že tooget další nápovědu o všech hello iothub-explorer spolu s jejich parametry hello:</span><span class="sxs-lookup"><span data-stu-id="4a704-147">You can use hello following command tooget additional help about all hello iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="4a704-148">portál Azure</span><span class="sxs-lookup"><span data-stu-id="4a704-148">Azure portal</span></span>
<span data-ttu-id="4a704-149">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="4a704-149">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="4a704-150">Můžete také toouse hello [portál Azure](../azure-portal-overview.md) toohelp zřizovat, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4a704-150">You might also want toouse hello [Azure portal](../azure-portal-overview.md) toohelp provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="4a704-151">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="4a704-151">Azure storage issues</span></span>
<span data-ttu-id="4a704-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od Microsoftu, které můžete toowork s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="4a704-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use toowork with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="4a704-153">Pomocí tohoto nástroje můžete připojit tooyour tabulky a zobrazit data hello v ní.</span><span class="sxs-lookup"><span data-stu-id="4a704-153">By using this tool, you can connect tooyour table and see hello data in it.</span></span> <span data-ttu-id="4a704-154">Můžete použít tento nástroj tootroubleshoot vaše problémů s úložištěm Azure.</span><span class="sxs-lookup"><span data-stu-id="4a704-154">You can use this tool tootroubleshoot your Azure Storage issues.</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md