---
title: "Arduino (C) se připojit k Azure IoT – řešení potíží s | Microsoft Docs"
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
ms.openlocfilehash: 3bb369da9148300a80e7d351522a4d908e926996
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="21e25-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="21e25-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="21e25-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="21e25-105">Hardware issues</span></span>
<span data-ttu-id="21e25-106">Informace o řešení běžných problémů na vaší kartě Adafruit prolnutí M0 Wi-Fi Arduino najdete v tématu [oficiální stránka řešení potíží](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span><span class="sxs-lookup"><span data-stu-id="21e25-106">For information about solving common problems on your Adafruit Feather M0 WiFi Arduino board, see the [official troubleshooting page](https://learn.adafruit.com/adafruit-feather-m0-wifi-atwinc1500?view=all#faq).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="21e25-107">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="21e25-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="21e25-108">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="21e25-108">No response during gulp tasks</span></span>
<span data-ttu-id="21e25-109">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="21e25-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="21e25-110">Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění.</span><span class="sxs-lookup"><span data-stu-id="21e25-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="21e25-111">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="21e25-111">You might see detailed error messages in your console output.</span></span>

```bash
gulp --verbose
```

<span data-ttu-id="21e25-112">Nebo můžete přidat `--listen` otevřete sériového portu na informace o protokolu výstupní zařízení.</span><span class="sxs-lookup"><span data-stu-id="21e25-112">Or you can add `--listen` to open serial port to output device log information.</span></span>

```bash
gulp --listen
``` 

### <a name="npm-issues"></a><span data-ttu-id="21e25-113">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="21e25-113">NPM issues</span></span>
<span data-ttu-id="21e25-114">Došlo k pokusu o aktualizaci vašeho balíčku NPM pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="21e25-114">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="21e25-115">Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="21e25-115">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="21e25-116">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="21e25-116">Azure-CLI issues</span></span>
<span data-ttu-id="21e25-117">Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="21e25-117">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="21e25-118">Vyhledejte řešení v [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) k vyhledání řešení.</span><span class="sxs-lookup"><span data-stu-id="21e25-118">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="21e25-119">Se pokuste o upgrade na nejnovější verzi rozhraní příkazového řádku Azure, když příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="21e25-119">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="21e25-120">Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="21e25-120">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="21e25-121">Nápovědu k řešení běžných potíží s, zkontrolujte [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="21e25-121">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="21e25-122">Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.</span><span class="sxs-lookup"><span data-stu-id="21e25-122">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="21e25-123">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="21e25-123">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="21e25-124">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="21e25-124">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="21e25-125">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="21e25-125">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="21e25-126">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="21e25-126">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="21e25-127">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění.</span><span class="sxs-lookup"><span data-stu-id="21e25-127">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="21e25-128">Řešení je odebrat tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="21e25-128">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="21e25-129">Tuto úlohu dokončit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="21e25-129">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="21e25-130">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="21e25-130">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="21e25-131">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="21e25-131">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="21e25-132">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="21e25-132">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="21e25-133">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="21e25-133">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="21e25-134">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="21e25-134">Azure IoT Hub issues</span></span>
<span data-ttu-id="21e25-135">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je potřeba nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="21e25-135">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="21e25-136">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="21e25-136">Device Explorer</span></span>
<span data-ttu-id="21e25-137">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="21e25-137">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="21e25-138">Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="21e25-138">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="21e25-139">*Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="21e25-139">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="21e25-140">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="21e25-140">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="21e25-141">*Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="21e25-141">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="21e25-142">Konfigurace vaší `IoT hub connection string` v rámci tohoto nástroje můžete použít všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="21e25-142">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="21e25-143">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="21e25-143">IoT hub Explorer</span></span>
<span data-ttu-id="21e25-144">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="21e25-144">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="21e25-145">Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="21e25-145">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>


<span data-ttu-id="21e25-146">Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="21e25-146">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="21e25-147">Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="21e25-147">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="21e25-148">portál Azure</span><span class="sxs-lookup"><span data-stu-id="21e25-148">Azure portal</span></span>
<span data-ttu-id="21e25-149">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="21e25-149">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="21e25-150">Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="21e25-150">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="21e25-151">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="21e25-151">Azure storage issues</span></span>
<span data-ttu-id="21e25-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="21e25-152">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="21e25-153">Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní.</span><span class="sxs-lookup"><span data-stu-id="21e25-153">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="21e25-154">Tento nástroj slouží k řešení potíží vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="21e25-154">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md