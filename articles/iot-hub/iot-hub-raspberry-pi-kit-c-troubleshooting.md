---
title: "Malinová Pi (C) připojit k Azure IoT – řešení potíží s | Microsoft Docs"
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
ms.openlocfilehash: 828669db23fa8d608029134fbe364033456d935a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting"></a><span data-ttu-id="f5b1d-104">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f5b1d-104">Troubleshooting</span></span>
## <a name="hardware-issues"></a><span data-ttu-id="f5b1d-105">Problémy s hardwarem</span><span class="sxs-lookup"><span data-stu-id="f5b1d-105">Hardware issues</span></span>
### <a name="the-application-runs-well-but-the-led-is-not-blinking"></a><span data-ttu-id="f5b1d-106">Aplikace běží správně, ale není blikat indikátor LED</span><span class="sxs-lookup"><span data-stu-id="f5b1d-106">The application runs well but the LED is not blinking</span></span>
<span data-ttu-id="f5b1d-107">Tento problém je vždy související s konektivitou okruhu hardwaru.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-107">This issue is always related to the hardware circuit connectivity.</span></span> <span data-ttu-id="f5b1d-108">Identifikaci problémů použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-108">Use the following steps to identify problems.</span></span>

1. <span data-ttu-id="f5b1d-109">Zkontrolujte, že jste vybrali správný **GPIO** vaší karty.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-109">Check that you chose the correct **GPIO** on your board.</span></span> <span data-ttu-id="f5b1d-110">Dva porty musí být **GPIO zem (Pin 6)** a **GPIO 04 (Pin 7)**.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-110">The two ports should be **GPIO GND (Pin 6)** and **GPIO 04 (Pin 7)**.</span></span>
2. <span data-ttu-id="f5b1d-111">Zkontrolujte správnost polarita z vaší Indikátor.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-111">Check that the polarity of your LED is correct.</span></span> <span data-ttu-id="f5b1d-112">Delší větev by měl být uveden **kladné**, anod pin.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-112">The longer leg should indicate the **positive**, anode pin.</span></span>
3. <span data-ttu-id="f5b1d-113">Použití **3.3v připnout** a **zem Pin** na malin pí 3.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-113">Use the **3.3V Pin** and **GND Pin** on Raspberry Pi 3.</span></span> <span data-ttu-id="f5b1d-114">Pi považovat za power řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-114">Treat Pi as the DC power.</span></span> <span data-ttu-id="f5b1d-115">Zkontrolujte, jestli DIODU funguje bez problémů.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-115">Check that the LED works fine.</span></span>

![Specifikace DIODU](media/iot-hub-raspberry-pi-lessons/troubleshooting/led_spec.png)

### <a name="other-hardware-issues"></a><span data-ttu-id="f5b1d-117">Další potíže s hardwarem</span><span class="sxs-lookup"><span data-stu-id="f5b1d-117">Other hardware issues</span></span>
<span data-ttu-id="f5b1d-118">Informace o řešení běžných problémů na 3 malin platformy najdete v tématu [oficiální stránka řešení potíží](http://elinux.org/R-Pi_Troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="f5b1d-118">For information about solving common problems on Raspberry Pi 3, see the [official troubleshooting page](http://elinux.org/R-Pi_Troubleshooting).</span></span>

## <a name="nodejs-package-issues"></a><span data-ttu-id="f5b1d-119">Problémy balíčku Node.js</span><span class="sxs-lookup"><span data-stu-id="f5b1d-119">Node.js package issues</span></span>
### <a name="no-response-during-gulp-tasks"></a><span data-ttu-id="f5b1d-120">Žádná odpověď během gulp úlohy</span><span class="sxs-lookup"><span data-stu-id="f5b1d-120">No response during gulp tasks</span></span>
<span data-ttu-id="f5b1d-121">Pokud narazíte na potíže se spouštěním gulp úlohy, můžete přidat `--verbose` možnost pro ladění.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-121">If you encounter problems running gulp tasks, you can add the `--verbose` option for debugging.</span></span> <span data-ttu-id="f5b1d-122">Zkuste ukončit aktuální úlohy gulp pomocí `Ctrl + C`a potom spusťte následující příkaz v okně konzoly zobrazíte zprávy ladění.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-122">Try to terminate current gulp tasks by using `Ctrl + C`, and then run the following command in your console window to see debug messages.</span></span> <span data-ttu-id="f5b1d-123">Může se zobrazit podrobné chybové zprávy ve výstupu konzoly.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-123">You might see detailed error messages in your console output.</span></span> 

```bash
gulp --verbose
```

### <a name="device-discovery-issues"></a><span data-ttu-id="f5b1d-124">Problémy zjišťování zařízení</span><span class="sxs-lookup"><span data-stu-id="f5b1d-124">Device discovery issues</span></span>
<span data-ttu-id="f5b1d-125">Pomoc při řešení běžných potíží s `devdisco` příkazu, zkontrolujte [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span><span class="sxs-lookup"><span data-stu-id="f5b1d-125">For help in troubleshooting common problems with the `devdisco` command, check the [readme](https://github.com/Azure/device-discovery-cli/blob/develop/readme.md).</span></span>

### <a name="npm-issues"></a><span data-ttu-id="f5b1d-126">NPM problémy</span><span class="sxs-lookup"><span data-stu-id="f5b1d-126">NPM issues</span></span>
<span data-ttu-id="f5b1d-127">Došlo k pokusu o aktualizaci vašeho balíčku NPM pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-127">Try to update your NPM package with the following command:</span></span>

```bash
npm install -g npm
```

<span data-ttu-id="f5b1d-128">Pokud problém přetrvává, nechte komentáře na konci tohoto článku nebo vytvořte potíže Githubu v našem [Ukázka úložiště](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span><span class="sxs-lookup"><span data-stu-id="f5b1d-128">If the problem still exists, leave your comments at the end of this article or create a GitHub issue in our [sample repository](https://github.com/Azure-Samples/iot-hub-c-raspberrypi-getting-started)</span></span>

## <a name="remote-debugging"></a><span data-ttu-id="f5b1d-129">Vzdálené ladění</span><span class="sxs-lookup"><span data-stu-id="f5b1d-129">Remote debugging</span></span>

<span data-ttu-id="f5b1d-130">Vzdálené ladění podpora bude brzy k dispozici v rozšíření kódu C/C++ sady Visual Studio k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-130">Remote debugging support will be available soon in Visual Studio Code C/C++ Extension.</span></span>

<span data-ttu-id="f5b1d-131">V současně můžete GDB pomocí Oblíbené terminálu SSH:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-131">In a meanwhile you can use GDB via your favourite SSH terminal:</span></span>

```bash
cd c-pi-lesson-x
sudo gdb app
```

## <a name="azure-cli-issues"></a><span data-ttu-id="f5b1d-132">Problémy rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="f5b1d-132">Azure-CLI issues</span></span>
<span data-ttu-id="f5b1d-133">Rozhraní příkazového řádku Azure (Azure CLI) je buildu preview.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-133">The Azure command-line interface (Azure CLI) is a preview build.</span></span> <span data-ttu-id="f5b1d-134">Vyhledejte řešení v [průvodci instalaci Preview](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) k vyhledání řešení.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-134">Look for solution in the [Preview Install Guide](https://github.com/Azure/azure-cli/blob/master/doc/preview_install_guide.md) to seek solutions.</span></span> <span data-ttu-id="f5b1d-135">Se pokuste o upgrade na nejnovější verzi rozhraní příkazového řádku Azure, když příkazy nefungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-135">Try to upgrade Azure-cli to latest version when commands don’t work as expected.</span></span>

<span data-ttu-id="f5b1d-136">Pokud narazíte na všechny chyby pomocí nástroje souboru [problém](https://github.com/Azure/azure-cli/issues) v **problémy** část úložiště GitHub.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-136">If you encounter any bugs with the tool, file an [issue](https://github.com/Azure/azure-cli/issues) in the **Issues** section of the GitHub repo.</span></span>

<span data-ttu-id="f5b1d-137">Nápovědu k řešení běžných potíží s, zkontrolujte [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span><span class="sxs-lookup"><span data-stu-id="f5b1d-137">For help troubleshooting common problems, check the [readme](https://github.com/Azure/azure-cli/blob/master/README.rst).</span></span>

<span data-ttu-id="f5b1d-138">Pokud jsou splněny "Nelze najít na verzi, která splňuje požadavek", spusťte následující příkaz pro upgrade na nejnovější verzi pip.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-138">If you meet "Could not find a version that satisfies the requirement", please run the following command to upgrade pip to lastest version.</span></span>

```bash
python -m pip install --upgrade pip
```

## <a name="python-installation-issues"></a><span data-ttu-id="f5b1d-139">Problémy instalace Python</span><span class="sxs-lookup"><span data-stu-id="f5b1d-139">Python installation issues</span></span>
### <a name="legacy-installation-issues-macos"></a><span data-ttu-id="f5b1d-140">Problémy instalace starší verze (macOS)</span><span class="sxs-lookup"><span data-stu-id="f5b1d-140">Legacy installation issues (macOS)</span></span>
<span data-ttu-id="f5b1d-141">Když instalujete **pip**, oprávnění chyba se vyvolá, když starší balíčky, které jsou nainstalovány s **su** oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-141">When you're installing **pip**, a permission error is thrown when older packages that are installed with **su** permissions.</span></span> <span data-ttu-id="f5b1d-142">K této situaci dochází, protože předchozí instalaci jazyka Python pomocí brew (macOS) není zcela odinstalována.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-142">This situation occurs because a previous installation of Python using brew (macOS) is not uninstalled completely.</span></span> <span data-ttu-id="f5b1d-143">Některé **pip** balíčky z předchozí instalace, které byly vytvořeny pomocí kořenového, což způsobí, že chyba oprávnění.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-143">Some **pip** packages from a previous installation were created by root, which causes the permission error.</span></span> <span data-ttu-id="f5b1d-144">Řešení je odebrat tyto balíčky nainstalované pomocí kořenového.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-144">The solution is to remove those packages installed by root.</span></span> <span data-ttu-id="f5b1d-145">Tuto úlohu dokončit pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-145">Use the following steps to complete this task:</span></span>

1. <span data-ttu-id="f5b1d-146">Přejděte na: /usr/local/lib/python2.7/site-packages</span><span class="sxs-lookup"><span data-stu-id="f5b1d-146">Go to: /usr/local/lib/python2.7/site-packages</span></span>
2. <span data-ttu-id="f5b1d-147">Seznam balíčků vytvořit pomocí kořenového:`ls -l | grep root`</span><span class="sxs-lookup"><span data-stu-id="f5b1d-147">List packages create by root: `ls -l | grep root`</span></span>
3. <span data-ttu-id="f5b1d-148">Odinstalaci balíčků z kroku 2:`sudo rm -rf {package name}`</span><span class="sxs-lookup"><span data-stu-id="f5b1d-148">Uninstall packages from step 2: `sudo rm -rf {package name}`</span></span>
4. <span data-ttu-id="f5b1d-149">Přeinstalujte Python.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-149">Reinstall Python.</span></span>

## <a name="azure-iot-hub-issues"></a><span data-ttu-id="f5b1d-150">Azure IoT Hub problémy</span><span class="sxs-lookup"><span data-stu-id="f5b1d-150">Azure IoT Hub issues</span></span>
<span data-ttu-id="f5b1d-151">Pokud jste úspěšně zřídit služby Azure IoT hub s `azure-cli`, a je potřeba nástroj pro správu zařízení, které se připojují ke službě IoT hub, zkuste následující nástroje:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-151">If you've successfully provisioned your Azure IoT hub with `azure-cli`, and you need a tool to manage the devices that are connecting to your IoT hub, try the following tools:</span></span>

### <a name="device-explorer"></a><span data-ttu-id="f5b1d-152">Průzkumník zařízení</span><span class="sxs-lookup"><span data-stu-id="f5b1d-152">Device Explorer</span></span>
<span data-ttu-id="f5b1d-153">[Průzkumník zařízení](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) spouští na místní počítač se systémem Windows a připojí se ke službě IoT hub v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-153">[Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) runs on your Windows local machine and connects to your IoT hub in Azure.</span></span> <span data-ttu-id="f5b1d-154">Komunikuje s následující [koncové body centra IoT](iot-hub-devguide.md):</span><span class="sxs-lookup"><span data-stu-id="f5b1d-154">It communicates with the following [IoT Hub endpoints](iot-hub-devguide.md):</span></span>

* <span data-ttu-id="f5b1d-155">*Správa identit zařízení* zřizovat a spravovat zařízení zaregistrovaná službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-155">*Device identity management* to provision and manage devices registered with your IoT hub.</span></span>
* <span data-ttu-id="f5b1d-156">*Zobrazí zařízení cloud* , můžete monitorovat zprávy odeslané ze zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-156">*Receive device-to-cloud* so you can monitor messages sent from your device to your IoT hub.</span></span>
* <span data-ttu-id="f5b1d-157">*Odeslat cloud zařízení* tak mohou zasílat zprávy do zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-157">*Send cloud-to-device* so you can send messages to your devices from your IoT hub.</span></span>

<span data-ttu-id="f5b1d-158">Konfigurace vaší `IoT hub connection string` v rámci tohoto nástroje můžete použít všechny jeho možnosti.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-158">Configure your `IoT hub connection string` within this tool to use all its capabilities.</span></span>

### <a name="iot-hub-explorer"></a><span data-ttu-id="f5b1d-159">Centrum IoT Explorer</span><span class="sxs-lookup"><span data-stu-id="f5b1d-159">IoT hub Explorer</span></span>
<span data-ttu-id="f5b1d-160">[Centrum IoT Explorer](https://github.com/Azure/iothub-explorer) je ukázkový nástroj víceplatformového rozhraní příkazového řádku ke správě klientů zařízení.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-160">[IoT hub Explorer](https://github.com/Azure/iothub-explorer) is a sample multiplatform CLI tool to manage device clients.</span></span> <span data-ttu-id="f5b1d-161">Nástroj můžete použít ke správě zařízení v registru identit, sledování zpráv typu zařízení cloud a odesílat příkazy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-161">You can use the tool to manage the devices in the identity registry, monitor device-to-cloud messages, and send cloud-to-device commands.</span></span>

<span data-ttu-id="f5b1d-162">Chcete-li nainstalovat nejnovější verzi (předprodejní) nástroj iothub Průzkumník, spusťte následující příkaz v prostředí příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-162">To install the latest (prerelease) version of the iothub-explorer tool, run the following command in your command-line environment:</span></span>

```
npm install -g iothub-explorer@latest
```

<span data-ttu-id="f5b1d-163">Chcete-li získat další informace o tom všechny příkazy iothub-explorer a jejich parametrů můžete následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f5b1d-163">You can use the following command to get additional help about all the iothub-explorer commands and their parameters:</span></span>

```bash
iothub-explorer help
```

### <a name="azure-portal"></a><span data-ttu-id="f5b1d-164">portál Azure</span><span class="sxs-lookup"><span data-stu-id="f5b1d-164">Azure portal</span></span>
<span data-ttu-id="f5b1d-165">Úplné rozhraní příkazového řádku prostředí umožňuje vytvářet a spravovat všechny prostředky Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-165">A full CLI experience helps you create and manage all your Azure resources.</span></span> <span data-ttu-id="f5b1d-166">Můžete také použít [portál Azure](../azure-portal-overview.md) pomoci zřizování, spravovat a ladit vašich prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-166">You might also want to use the [Azure portal](../azure-portal-overview.md) to help provision, manage, and debug your Azure resources.</span></span>

## <a name="azure-storage-issues"></a><span data-ttu-id="f5b1d-167">Problémů s úložištěm Azure</span><span class="sxs-lookup"><span data-stu-id="f5b1d-167">Azure storage issues</span></span>
<span data-ttu-id="f5b1d-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) je samostatná aplikace od společnosti Microsoft, který můžete použít pro práci s daty Azure Storage ve Windows, systému macOS a Linux.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-168">[Microsoft Azure Storage Explorer (preview)](http://storageexplorer.com) is a standalone app from Microsoft that you can use to work with Azure Storage data on Windows, macOS, and Linux.</span></span> <span data-ttu-id="f5b1d-169">Pomocí tohoto nástroje můžete připojit k tabulku a zobrazit data v ní.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-169">By using this tool, you can connect to your table and see the data in it.</span></span> <span data-ttu-id="f5b1d-170">Tento nástroj slouží k řešení potíží vašeho úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f5b1d-170">You can use this tool to troubleshoot your Azure Storage issues.</span></span>
