---
title: "Připojení k Azure IoT - Intel Edison (C) řešení potíží s | Microsoft Docs"
description: "Řešení potíží s stránky pro prostředí Intel Edison C"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "řešení potíží s arduino"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: fe20f2fe-490c-4910-82e1-578ed504ae86
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: dd6338ad29e0bb858c33e5bb24b8f41d3c22575a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="86289-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="86289-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="86289-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="86289-105">Hardware issues</span></span>
<span data-ttu-id="86289-106">Informace o řešení běžných problémů na Intel Edison najdete v tématu [oficiální stránka řešení potíží](https://software.intel.com/en-us/node/637974).</span><span class="sxs-lookup"><span data-stu-id="86289-106">For information about solving common problems on Intel Edison, see the [official troubleshooting page](https://software.intel.com/en-us/node/637974).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="86289-107">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="86289-107">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="86289-108">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="86289-108">No response during gulp tasks</span></span>
<span data-ttu-id="86289-109">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="86289-109">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="86289-110">Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění.</span><span class="sxs-lookup"><span data-stu-id="86289-110">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="86289-111">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="86289-111">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="npm-issues"></a><span data-ttu-id="86289-112">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="86289-112">NPM issues</span></span>
<span data-ttu-id="86289-113">Došlo k pokusu o aktualizaci vašeho balíčku NPM pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="86289-113">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="86289-114">Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [úložiště ukázkové][sample-repository].</span><span class="sxs-lookup"><span data-stu-id="86289-114">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository][sample-repository].</span></span>

## <a name="azure-cli-issues"></a><span data-ttu-id="86289-115">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="86289-115">Azure-CLI issues</span></span>
<span data-ttu-id="86289-116">Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="86289-116">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="86289-117">Vyhledejte řešení v [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) k vyhledání řešení.</span><span class="sxs-lookup"><span data-stu-id="86289-117">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="86289-118">Se pokuste o upgrade na nejnovější verzi rozhraní příkazového řádku Azure, když příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="86289-118">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="86289-119">Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="86289-119">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="86289-120">Nápovědu k řešení běžných potíží s, zkontrolujte [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="86289-120">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="86289-121">Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.</span><span class="sxs-lookup"><span data-stu-id="86289-121">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="86289-122">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="86289-122">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="86289-123">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="86289-123">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="86289-124">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="86289-124">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="86289-125">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="86289-125">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="86289-126">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění.</span><span class="sxs-lookup"><span data-stu-id="86289-126">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="86289-127">Řešení je odebrat tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="86289-127">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="86289-128">Tuto úlohu dokončit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="86289-128">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="86289-129">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="86289-129">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="86289-130">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="86289-130">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="86289-131">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="86289-131">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="86289-132">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="86289-132">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="86289-133">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="86289-133">Azure IoT Hub issues</span></span>
<span data-ttu-id="86289-134">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je potřeba nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="86289-134">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="86289-135">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="86289-135">Device Explorer</span></span>
<span data-ttu-id="86289-136">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="86289-136">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="86289-137">Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="86289-137">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

- <span data-ttu-id="86289-138">_Správa identit zařízení_ zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="86289-138">_Device identity management_ to provision and manage devices registered with your IoT hub.</span></span>
- <span data-ttu-id="86289-139">_Zobrazí zařízení cloud_ , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="86289-139">_Receive device-to-cloud_ so you can monitor messages sent from your device to your IoT hub.</span></span>
- <span data-ttu-id="86289-140">_Odeslat cloud zařízení_ tak mohou zasílat zprávy do zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="86289-140">_Send cloud-to-device_ so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="86289-141">Konfigurace vaší `IoT hub connection string` v rámci tohoto nástroje můžete použít všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="86289-141">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="86289-142">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="86289-142">IoT hub Explorer</span></span>
<span data-ttu-id="86289-143">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="86289-143">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="86289-144">Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="86289-144">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="86289-145">Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="86289-145">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```bash
npm install -g iothub-explorer@latest
```

<span data-ttu-id="86289-146">Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="86289-146">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="86289-147">portál Azure</span><span class="sxs-lookup"><span data-stu-id="86289-147">Azure portal</span></span>
<span data-ttu-id="86289-148">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="86289-148">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="86289-149">Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="86289-149">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="86289-150">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="86289-150">Azure storage issues</span></span>
<span data-ttu-id="86289-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) dat v systému Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="86289-151">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with [Azure Storage](https://azure.microsoft.com/en-us/services/storage/) data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="86289-152">Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní.</span><span class="sxs-lookup"><span data-stu-id="86289-152">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="86289-153">Tento nástroj slouží k řešení potíží vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="86289-153">You can use this tool to troubleshoot your Azure Storage issues.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86289-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86289-154">Next steps</span></span>
<span data-ttu-id="86289-155">Tato stránka obsahuje pouze nejběžnějších problémů Intel Edison kit.</span><span class="sxs-lookup"><span data-stu-id="86289-155">This page only includes the most common problems of Intel Edison kit.</span></span> <span data-ttu-id="86289-156">Můžete také ponechat dolní komentáře účelem ohlášení problémů pro odstraňování potíží.</span><span class="sxs-lookup"><span data-stu-id="86289-156">You can also leave bottom comments to report issues for further troubleshooting.</span></span>

<span data-ttu-id="86289-157">Přejděte zpět na [začít pracovat s Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="86289-157">Go back to [Get started with Intel Edison (C)](iot-hub-intel-edison-kit-c-get-started.md)</span></span>

<!-- Images and links -->

[sample-repository]: https://github.com/Azure-Samples/iot-hub-c-edison-getting-started