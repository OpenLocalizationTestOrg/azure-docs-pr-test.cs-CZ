---
title: "IoT DevKit do cloudu - připojit ke službě Azure IoT Hub IoT DevKit AZ3166 | Microsoft Docs"
description: "Zjistěte, jak nastavit a IoT DevKit AZ3166 se připojit ke službě Azure IoT Hub pro něj k odesílání dat do Azure Cloudová platforma v tomto kurzu."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: 
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: xshi
ms.openlocfilehash: 122fac584ac5b954ef7b33a3121bee2c502ebc63
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="connect-iot-devkit-az3166-to-azure-iot-hub-in-the-cloud"></a><span data-ttu-id="5faf2-103">IoT DevKit AZ3166 se připojit ke službě Azure IoT Hub v cloudu</span><span class="sxs-lookup"><span data-stu-id="5faf2-103">Connect IoT DevKit AZ3166 to Azure IoT Hub in the cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="5faf2-104">[MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lze použít k vývoji a řešení Internetu věcí (IoT) prototypu využití služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="5faf2-104">The [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) can be used to develop and prototype Internet of Things (IoT) solutions leveraging Microsoft Azure services.</span></span> <span data-ttu-id="5faf2-105">Obsahuje kompatibilní panelu s bohatou periferních zařízení a senzorů, balíček Tabule open source a rozšiřujících se Arduino [projekty katalogu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span><span class="sxs-lookup"><span data-stu-id="5faf2-105">It includes an Arduino compatible board with rich peripherals and sensors, an open-source board package and a growing [projects catalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="5faf2-106">Co dělat</span><span class="sxs-lookup"><span data-stu-id="5faf2-106">What you do</span></span>
<span data-ttu-id="5faf2-107">Připojit [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) do služby Azure IoT hub, který vytvoříte, shromažďování dat teploty a vlhkosti ze senzorů a odesílání dat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5faf2-107">Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) to an Azure IoT hub that you create, collect the temperature and humidity data from sensors and send the data to IoT hub.</span></span>

<span data-ttu-id="5faf2-108">Nemáte DevKit ještě?</span><span class="sxs-lookup"><span data-stu-id="5faf2-108">Don't have a DevKit yet?</span></span> <span data-ttu-id="5faf2-109">Získat novou [zde](https://aka.ms/iot-devkit-purchase).</span><span class="sxs-lookup"><span data-stu-id="5faf2-109">Get a new one [here](https://aka.ms/iot-devkit-purchase).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="5faf2-110">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="5faf2-110">What you learn</span></span>

* <span data-ttu-id="5faf2-111">Postup připojení IoT DevKit na bezdrátový přístupový bod a příprava vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="5faf2-111">How to connect IoT DevKit to Wireless access point and prepare your development environment.</span></span>
* <span data-ttu-id="5faf2-112">Postup vytvoření služby IoT hub a registrovat zařízení pro MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-112">How to create an IoT hub and register a device for MXChip IoT DevKit.</span></span>
* <span data-ttu-id="5faf2-113">Postup shromažďování dat snímačů spuštěním ukázkovou aplikaci na MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-113">How to collect sensor data by running a sample application on MXChip IoT DevKit.</span></span>
* <span data-ttu-id="5faf2-114">Jak odesílat data snímačů do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5faf2-114">How to send the sensor data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5faf2-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="5faf2-115">What you need</span></span>

* <span data-ttu-id="5faf2-116">MXChip IoT DevKit fórum s malých kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="5faf2-116">An MXChip IoT DevKit board with a micro USB cable.</span></span> [<span data-ttu-id="5faf2-117">Získejte nyní</span><span class="sxs-lookup"><span data-stu-id="5faf2-117">Get it now</span></span>](https://aka.ms/iot-devkit-purchase)
* <span data-ttu-id="5faf2-118">Počítač se systémem Windows 10 nebo systému macOS 10.10 +</span><span class="sxs-lookup"><span data-stu-id="5faf2-118">A computer running Windows 10 or macOS 10.10+</span></span>
* <span data-ttu-id="5faf2-119">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="5faf2-119">An active Azure subscription</span></span>
  * <span data-ttu-id="5faf2-120">Aktivovat [Bezplatný zkušební účet Microsoft Azure 30 dnů](https://azureinfo.microsoft.com/us-freetrial.html)</span><span class="sxs-lookup"><span data-stu-id="5faf2-120">Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html)</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="5faf2-121">Připravte svůj hardware</span><span class="sxs-lookup"><span data-stu-id="5faf2-121">Prepare your hardware</span></span>

<span data-ttu-id="5faf2-122">Propojte hardwaru do vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="5faf2-122">Hook up the hardware to your computer.</span></span>

### <a name="hardware-you-need"></a><span data-ttu-id="5faf2-123">Hardware, které potřebujete</span><span class="sxs-lookup"><span data-stu-id="5faf2-123">Hardware you need</span></span>

* <span data-ttu-id="5faf2-124">DevKit panelu</span><span class="sxs-lookup"><span data-stu-id="5faf2-124">DevKit board</span></span>
* <span data-ttu-id="5faf2-125">Malých kabelu USB</span><span class="sxs-lookup"><span data-stu-id="5faf2-125">Micro USB cable</span></span>

![získávání spuštění – hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-to-your-computer"></a><span data-ttu-id="5faf2-127">Připojte k počítači DevKit</span><span class="sxs-lookup"><span data-stu-id="5faf2-127">Connect DevKit to your computer</span></span>

1. <span data-ttu-id="5faf2-128">Připojte USB end k počítači</span><span class="sxs-lookup"><span data-stu-id="5faf2-128">Connect USB end to your PC</span></span>
2. <span data-ttu-id="5faf2-129">Připojení k DevKit end Micro USB</span><span class="sxs-lookup"><span data-stu-id="5faf2-129">Connect Micro USB end to the DevKit</span></span>
3. <span data-ttu-id="5faf2-130">Zelená DIODU vedle power potvrdí připojení</span><span class="sxs-lookup"><span data-stu-id="5faf2-130">The green LED next to power confirms connection</span></span>

![získávání spuštění – připojení](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a><span data-ttu-id="5faf2-132">Konfigurace sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="5faf2-132">Configure WiFi</span></span>

<span data-ttu-id="5faf2-133">Projekty IoT spoléhají na připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="5faf2-133">IoT projects rely on Internet connectivity.</span></span> <span data-ttu-id="5faf2-134">Použijte následující pokyny ke konfiguraci DevKit pro připojení k Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="5faf2-134">Use the following instructions to configure the DevKit to connect to WiFi.</span></span>

### <a name="enter-ap-mode"></a><span data-ttu-id="5faf2-135">Zadejte režim Asie a Tichomoří</span><span class="sxs-lookup"><span data-stu-id="5faf2-135">Enter AP Mode</span></span>

<span data-ttu-id="5faf2-136">Podržte tlačítko B, pak push a verze na tlačítko obnovit, a uvolněte tlačítko B. Vaše DevKit zadá režimu přístupový bod pro konfiguraci sítě Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="5faf2-136">Hold down button B, then push and release the reset button, then release button B. Your DevKit will enter AP Mode for configuring WiFi.</span></span> <span data-ttu-id="5faf2-137">Na obrazovce zobrazí Identifier(SSID) nastavení služby DevKit, jakož i Konfigurace portálu IP adresa:</span><span class="sxs-lookup"><span data-stu-id="5faf2-137">The screen will display the Service Set Identifier(SSID) of the DevKit as well as the configuration portal IP address:</span></span>

![získávání spuštění-Wi-Fi-Asie a Tichomoří](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-to-devkit-ap"></a><span data-ttu-id="5faf2-139">Připojení k DevKit Asie a Tichomoří</span><span class="sxs-lookup"><span data-stu-id="5faf2-139">Connect to DevKit AP</span></span>

<span data-ttu-id="5faf2-140">Nyní použijte jiné zařízení Wi-Fi povoleno (počítač nebo mobilní telefon), se připojit k DevKit SSID (zvýrazněných v výše uvedený snímek obrazovky), ponechte prázdné heslo.</span><span class="sxs-lookup"><span data-stu-id="5faf2-140">Now, use another WiFi enabled device (PC or mobile phone) to connect to the DevKit SSID (highlighted in the screenshot above), leave the password empty.</span></span>

![získávání spuštění ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a><span data-ttu-id="5faf2-142">Konfigurace sítě Wi-Fi pro DevKit</span><span class="sxs-lookup"><span data-stu-id="5faf2-142">Configure WiFi for DevKit</span></span>

<span data-ttu-id="5faf2-143">Otevřete adresu IP uvedené na obrazovce DevKit v prohlížeči počítače nebo mobilní telefon, vyberte chcete DevKit pro připojení k síti Wi-Fi a pak zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="5faf2-143">Open the IP address shown on the DevKit screen on your PC or mobile phone browser, select the WiFi network you want the DevKit to connect to, then type the password.</span></span> <span data-ttu-id="5faf2-144">Klikněte na tlačítko **Connect** k dokončení:</span><span class="sxs-lookup"><span data-stu-id="5faf2-144">Click **Connect** to complete:</span></span>

![získávání spuštění-Wi-Fi portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

<span data-ttu-id="5faf2-146">Jakmile je připojení úspěšné, DevKit restartuje za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="5faf2-146">Once the connection succeeds, the DevKit will reboot in a few seconds.</span></span> <span data-ttu-id="5faf2-147">Pokud byly úspěšné, zobrazí se název sítě Wi-Fi a IP adresu na obrazovce:</span><span class="sxs-lookup"><span data-stu-id="5faf2-147">If succeeded, you will see the WiFi name and IP address on the screen:</span></span>

![získávání spuštění-Wi-Fi ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
<span data-ttu-id="5faf2-149">IP adresa, zobrazí v fotografie nemusí odpovídat skutečné IP přiřazeny a zobrazují na obrazovce DevKit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-149">The IP address displayed in the photo may not match the actual IP assigned and displayed on the DevKit screen.</span></span> <span data-ttu-id="5faf2-150">To je normální, protože Wi-Fi používá protokol DHCP k dynamickému přidělení IP adresy.</span><span class="sxs-lookup"><span data-stu-id="5faf2-150">This is normal as WiFi uses DHCP to dynamically assign IPs.</span></span>

<span data-ttu-id="5faf2-151">Po dokončení konfigurace sítě Wi-Fi, bude vaše přihlašovací údaje trvalé na zařízení pro toto připojení, i v případě, že byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="5faf2-151">After WiFi is configured, your credentials will be persisted on the device for that connection, even if unplugged.</span></span> <span data-ttu-id="5faf2-152">Například pokud jste nakonfigurovali DevKit pro Wi-Fi v domácnosti a DevKit trvalo office, budete muset překonfigurovat Asie režimu (od kroku **zadejte režim Asie**) pro připojení DevKit pobočku Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="5faf2-152">For example, if you configured the DevKit for WiFi in your home and then took the DevKit to the office, you will need to reconfigure AP mode (starting at step **Enter AP Mode**) to connect the DevKit to your office WiFi.</span></span> 

## <a name="start-using-devkit"></a><span data-ttu-id="5faf2-153">Začít používat DevKit</span><span class="sxs-lookup"><span data-stu-id="5faf2-153">Start using DevKit</span></span>

<span data-ttu-id="5faf2-154">Výchozí aplikace spuštěné v DevKit zkontroluje nejnovější verzi firmwaru a zobrazí některé senzor diagnostická data za vás.</span><span class="sxs-lookup"><span data-stu-id="5faf2-154">The default app running on DevKit will check the latest version of the firmware and display some sensor diagnosis data for you.</span></span>

### <a name="upgrade-to-the-latest-firmware"></a><span data-ttu-id="5faf2-155">Upgrade na nejnovější firmware</span><span class="sxs-lookup"><span data-stu-id="5faf2-155">Upgrade to the latest firmware</span></span>

<span data-ttu-id="5faf2-156">Zobrazí se výzva na obrazovce potřeby obou aktuální a nejnovější verzi firmwaru Pokud dojde upgradu.</span><span class="sxs-lookup"><span data-stu-id="5faf2-156">You will be prompted on the screen both the current and latest firmware version if there is an upgrade needed.</span></span> <span data-ttu-id="5faf2-157">Postupujte podle [upgradovat firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) průvodce ho upgradovat.</span><span class="sxs-lookup"><span data-stu-id="5faf2-157">Follow [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide to upgrade it.</span></span>

![získávání spuštění firmwaru](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
<span data-ttu-id="5faf2-159">Toto je jednorázové úsilí, jakmile začnete vývoji na DevKit a nahrát aplikaci, budete mít nejnovější firmware pocházet s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="5faf2-159">This is a one-time effort, once you start developing on the DevKit and upload your app, you will have the latest firmware come with your app.</span></span>

### <a name="test-various-sensors"></a><span data-ttu-id="5faf2-160">Testování různých senzorů</span><span class="sxs-lookup"><span data-stu-id="5faf2-160">Test various sensors</span></span>

<span data-ttu-id="5faf2-161">Stisknutím tlačítka B test senzorů, pokračujte stisknutím a uvolněním tlačítka B cyklicky každý senzor.</span><span class="sxs-lookup"><span data-stu-id="5faf2-161">Press button B to test sensors, continue pressing and releasing the B button to cycle through each sensor.</span></span>

![získávání spuštění senzorů](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a><span data-ttu-id="5faf2-163">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="5faf2-163">Prepare development environment</span></span>

<span data-ttu-id="5faf2-164">Nyní je čas k nastavení vývojového prostředí: nástroje a balíčky pro sestavit poutavých aplikace či aplikace IoT.</span><span class="sxs-lookup"><span data-stu-id="5faf2-164">Now it's time to set up the development environment: tools and packages for you to build stunning IoT applications.</span></span> <span data-ttu-id="5faf2-165">Můžete podle operačního systému Windows nebo systému macOS verze.</span><span class="sxs-lookup"><span data-stu-id="5faf2-165">You can choose Windows or macOS version according to your operating system.</span></span>


### <a name="windows"></a><span data-ttu-id="5faf2-166">Windows</span><span class="sxs-lookup"><span data-stu-id="5faf2-166">Windows</span></span>

<span data-ttu-id="5faf2-167">Doporučujeme použít instalační balíček Příprava vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="5faf2-167">We encourage you to use the installation package to prepare the development environment.</span></span> <span data-ttu-id="5faf2-168">Pokud narazíte na problémy, můžete podle [ruční kroky](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) ho provést.</span><span class="sxs-lookup"><span data-stu-id="5faf2-168">If you encounter any issues, you can follow the [manual steps](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) to get it done.</span></span>

#### <a name="download-latest-package"></a><span data-ttu-id="5faf2-169">Stáhněte si nejnovější balíček</span><span class="sxs-lookup"><span data-stu-id="5faf2-169">Download latest package</span></span>

<span data-ttu-id="5faf2-170">`.zip` Si stáhnout soubor obsahuje všechny nezbytné nástroje a balíčky, které jsou potřebné pro vývoj DevKit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-170">The `.zip` file you download contains all necessary tools and packages required for DevKit development.</span></span>

> [!div class="button"]
[<span data-ttu-id="5faf2-171">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="5faf2-171">Download</span></span>](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> <span data-ttu-id="5faf2-172">`.zip` Soubor obsahuje následující nástroje a balíčků.</span><span class="sxs-lookup"><span data-stu-id="5faf2-172">The `.zip` file contains the following tools and packages.</span></span> <span data-ttu-id="5faf2-173">Pokud již máte některé součásti nainstalované, skript rozpozná a jejich přeskočit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-173">If you already have some components installed, the script will detect and skip them.</span></span>
> * <span data-ttu-id="5faf2-174">Node.js a Yarn: modul Runtime pro instalační skript a automatizované úlohy</span><span class="sxs-lookup"><span data-stu-id="5faf2-174">Node.js and Yarn: Runtime for the setup script and automated tasks</span></span>
> * <span data-ttu-id="5faf2-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) – možnosti příkazového řádku a platformy pro správu prostředků Azure, soubor MSI obsahuje závislé Python a pip.</span><span class="sxs-lookup"><span data-stu-id="5faf2-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) - Cross-platform command-line experience for managing Azure resources, the MSI contains dependent Python and pip.</span></span>
> * <span data-ttu-id="5faf2-176">[Visual Studio Code](https://code.visualstudio.com/): editor lehký kód pro vývoj DevKit</span><span class="sxs-lookup"><span data-stu-id="5faf2-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight code editor for DevKit development</span></span>
> * <span data-ttu-id="5faf2-177">[Rozšíření sady Visual Studio Code pro Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): vývoj umožňuje Arduino v produktu VS Code</span><span class="sxs-lookup"><span data-stu-id="5faf2-177">[Visual Studio Code extension for Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Enables Arduino development in VS Code</span></span>
> * <span data-ttu-id="5faf2-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): rozšíření pro Arduino spoléhá na tento nástroj</span><span class="sxs-lookup"><span data-stu-id="5faf2-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): The extension for Arduino relies on this tool</span></span>
> * <span data-ttu-id="5faf2-179">Balíček DevKit Tabule: Nástroj řetězy, knihovny a projekty doplňku DevKit</span><span class="sxs-lookup"><span data-stu-id="5faf2-179">DevKit Board Package: Tool chains, libraries and projects for the DevKit</span></span>
> * <span data-ttu-id="5faf2-180">Nástroj ST odkaz: Základní nástroje a ovladače</span><span class="sxs-lookup"><span data-stu-id="5faf2-180">ST-Link Utility: Essential utilities and drivers</span></span>

#### <a name="run-installation-script"></a><span data-ttu-id="5faf2-181">Spusťte instalační skript</span><span class="sxs-lookup"><span data-stu-id="5faf2-181">Run installation script</span></span>

<span data-ttu-id="5faf2-182">V Průzkumníku souborů Windows, vyhledejte `.zip` a rozbalte ho, vyhledejte `install.cmd`, klikněte pravým tlačítkem a vyberte **"Spustit jako správce"** spustit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-182">In Windows File Explorer, locate the `.zip` and extract it, find `install.cmd`, right-click and select **"Run as administrator"** to start.</span></span>

![získávání spuštění spustit správce.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

<span data-ttu-id="5faf2-184">Během instalace zobrazí se průběh každé nástroj nebo balíčku.</span><span class="sxs-lookup"><span data-stu-id="5faf2-184">During installation, you will see the progress of each tool or package.</span></span>

![získávání spuštění instalace](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-to-install-drivers"></a><span data-ttu-id="5faf2-186">Potvrzení instalace ovladačů</span><span class="sxs-lookup"><span data-stu-id="5faf2-186">Confirm to install drivers</span></span>

<span data-ttu-id="5faf2-187">Kód VS pro rozšíření Arduino spoléhá na Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="5faf2-187">The VS Code for Arduino extension relies on the Arduino IDE.</span></span> <span data-ttu-id="5faf2-188">Pokud instalujete Arduino IDE poprvé, budete vyzváni k instalaci příslušné ovladače:</span><span class="sxs-lookup"><span data-stu-id="5faf2-188">If this is the first time you are installing the Arduino IDE, you will be prompted to install relevant drivers:</span></span>

![získávání spuštění – ovladače](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

<span data-ttu-id="5faf2-190">Má trvat přibližně 10 minut na dokončení instalace v závislosti na rychlosti sítě Internet.</span><span class="sxs-lookup"><span data-stu-id="5faf2-190">It should take around 10 minutes to finish installation depending on your Internet speed.</span></span> <span data-ttu-id="5faf2-191">Po dokončení instalace byste měli vidět Visual Studio Code a Arduino IDE zástupce na ploše.</span><span class="sxs-lookup"><span data-stu-id="5faf2-191">Once the installation is complete, you should see Visual Studio Code and Arduino IDE shortcuts on your desktop.</span></span>

> [!NOTE] 
<span data-ttu-id="5faf2-192">V některých případech při spuštění VS Code, zobrazí se výzva k chybě, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu.</span><span class="sxs-lookup"><span data-stu-id="5faf2-192">Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="5faf2-193">K jeho řešení, zavřete VS Code, spusťte jednou Arduino IDE a VS Code by měl vyhledejte Arduino IDE cestu správně.</span><span class="sxs-lookup"><span data-stu-id="5faf2-193">To solve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.</span></span>


### <a name="macos-preview"></a><span data-ttu-id="5faf2-194">systému macOS (Preview)</span><span class="sxs-lookup"><span data-stu-id="5faf2-194">macOS (Preview)</span></span>

<span data-ttu-id="5faf2-195">Postupujte podle následujících kroků připravíte vývojového prostředí v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="5faf2-195">Follow these steps to prepare development environment on macOS.</span></span>

#### <a name="install-azure-cli-20"></a><span data-ttu-id="5faf2-196">Instalace Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5faf2-196">Install Azure CLI 2.0</span></span>

<span data-ttu-id="5faf2-197">Postupujte podle [oficiální průvodce](https://docs.microsoft.com//cli/azure/install-azure-cli) nainstalovat Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="5faf2-197">Follow the [official guide](https://docs.microsoft.com//cli/azure/install-azure-cli) to install Azure CLI 2.0:</span></span>

<span data-ttu-id="5faf2-198">Nainstalovat Azure CLI 2.0 s jedním `curl` příkaz:</span><span class="sxs-lookup"><span data-stu-id="5faf2-198">Install Azure CLI 2.0 with one `curl` command:</span></span>

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="5faf2-199">A znovu spusťte váš příkazové prostředí pro změny se projeví:</span><span class="sxs-lookup"><span data-stu-id="5faf2-199">And restart your command shell for changes to take effect:</span></span>

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a><span data-ttu-id="5faf2-200">Nainstalujte Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="5faf2-200">Install Arduino IDE</span></span>

<span data-ttu-id="5faf2-201">Rozšíření Visual Studio Code Arduino spoléhá na Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="5faf2-201">The Visual Studio Code Arduino extension relies on the Arduino IDE.</span></span> <span data-ttu-id="5faf2-202">Stáhněte a nainstalujte [Arduino IDE pro systému macOS](https://www.arduino.cc/en/Main/Software).</span><span class="sxs-lookup"><span data-stu-id="5faf2-202">Download and install the [Arduino IDE for macOS](https://www.arduino.cc/en/Main/Software).</span></span>

#### <a name="install-visual-studio-code"></a><span data-ttu-id="5faf2-203">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5faf2-203">Install Visual Studio Code</span></span>

<span data-ttu-id="5faf2-204">Stáhněte a nainstalujte [Visual Studio Code pro systému macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="5faf2-204">Download and install [Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span> <span data-ttu-id="5faf2-205">Bude primární vývojový nástroj pro tvorbu DevKit IoT aplikací.</span><span class="sxs-lookup"><span data-stu-id="5faf2-205">This will be the primary development tool for building DevKit IoT applications.</span></span>

####  <a name="download-latest-package"></a><span data-ttu-id="5faf2-206">Stáhněte si nejnovější balíček</span><span class="sxs-lookup"><span data-stu-id="5faf2-206">Download latest package</span></span>

1. <span data-ttu-id="5faf2-207">Nainstalujte Node.js.</span><span class="sxs-lookup"><span data-stu-id="5faf2-207">Install Node.js.</span></span> <span data-ttu-id="5faf2-208">Můžete použít Správce balíčků oblíbených systému macOS [Homebrew](https://brew.sh/) nebo [předem vytvořené instalační program](https://nodejs.org/en/download/) k její instalaci.</span><span class="sxs-lookup"><span data-stu-id="5faf2-208">You can use popular macOS package manager [Homebrew](https://brew.sh/) or [pre-built installer](https://nodejs.org/en/download/) to install it.</span></span>

2. <span data-ttu-id="5faf2-209">Stáhněte si `.zip` souboru, který obsahuje skripty úloh, které jsou potřebné pro vývoj DevKit v produktu VS Code.</span><span class="sxs-lookup"><span data-stu-id="5faf2-209">Download `.zip` file containing task scripts required for DevKit development in VS Code.</span></span>

   > [!div class="button"]
   [<span data-ttu-id="5faf2-210">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="5faf2-210">Download</span></span>](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   <span data-ttu-id="5faf2-211">Vyhledejte `.zip` a rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="5faf2-211">Locate the `.zip` and extract it.</span></span> <span data-ttu-id="5faf2-212">Spusťte **Terminálové** aplikace a spusťte následující příkazy ke konfiguraci:</span><span class="sxs-lookup"><span data-stu-id="5faf2-212">Then launch **Terminal** app and run the following commands to configure:</span></span>

   <span data-ttu-id="5faf2-213">Extrahovanou složku přesunete do složky systému macOS uživatele:</span><span class="sxs-lookup"><span data-stu-id="5faf2-213">Move extracted folder to your macOS user folder:</span></span>
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   <span data-ttu-id="5faf2-214">Nainstalujte `npm` balíčky:</span><span class="sxs-lookup"><span data-stu-id="5faf2-214">Install `npm` packages:</span></span>
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a><span data-ttu-id="5faf2-215">Instalace rozšíření VS Code pro Arduino</span><span class="sxs-lookup"><span data-stu-id="5faf2-215">Install VS Code extension for Arduino</span></span>

<span data-ttu-id="5faf2-216">Visual Studio Code umožňuje instalaci rozšíření Marketplace přímo v nástroji, jednoduše klikněte na ikonu rozšíření v levé nabídce podokně a pak vyhledejte `Arduino` k instalaci:</span><span class="sxs-lookup"><span data-stu-id="5faf2-216">Visual Studio Code allows you to install Marketplace extensions directly in the tool, simply click the extensions icon in the left menu pane and then search `Arduino` to install:</span></span>

![instalace rozšíření](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a><span data-ttu-id="5faf2-218">Instalovat balíček DevKit panelu</span><span class="sxs-lookup"><span data-stu-id="5faf2-218">Install DevKit board package</span></span>

<span data-ttu-id="5faf2-219">Musíte přidat pomocí panelu Správce ve Visual Studio Code DevKit panelu.</span><span class="sxs-lookup"><span data-stu-id="5faf2-219">You will need to add the DevKit board using the Board Manager in Visual Studio Code.</span></span>

1. <span data-ttu-id="5faf2-220">Použití `Cmd+Shift+P` k vyvolání příkazu palety a typ **Arduino** potom najděte a vyberte **Arduino: Tabule Manager**.</span><span class="sxs-lookup"><span data-stu-id="5faf2-220">Use `Cmd+Shift+P` to invoke command palette and type **Arduino** then find and select **Arduino: Board Manager**.</span></span>

2. <span data-ttu-id="5faf2-221">Klikněte na tlačítko **další adresy URL** v pravé dolní části.</span><span class="sxs-lookup"><span data-stu-id="5faf2-221">Click **'Additional URLs'** at the bottom right.</span></span>
   <span data-ttu-id="5faf2-222">![instalace další adresy URL](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span><span class="sxs-lookup"><span data-stu-id="5faf2-222">![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span></span>

3. <span data-ttu-id="5faf2-223">V `settings.json` soubor, přidejte řádek v dolní části `USER SETTINGS` podokně a uložte.</span><span class="sxs-lookup"><span data-stu-id="5faf2-223">In the `settings.json` file, add a line at the bottom of `USER SETTINGS` pane and save.</span></span>
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![instalace. nastavení json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. <span data-ttu-id="5faf2-225">Nyní ve Správci panelu Hledat 'az3166' a nainstalujte nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="5faf2-225">Now in the Board Manager search for 'az3166' and install the latest version.</span></span>
   <span data-ttu-id="5faf2-226">![instalace az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span><span class="sxs-lookup"><span data-stu-id="5faf2-226">![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span></span>

<span data-ttu-id="5faf2-227">Nyní máte všechny potřebné nástroje a balíčky pro systému macOS nainstalována.</span><span class="sxs-lookup"><span data-stu-id="5faf2-227">You now have all the necessary tools and packages installed for macOS.</span></span>


## <a name="open-project-folder"></a><span data-ttu-id="5faf2-228">Otevřít projekt složky</span><span class="sxs-lookup"><span data-stu-id="5faf2-228">Open project folder</span></span>

### <a name="launch-vs-code"></a><span data-ttu-id="5faf2-229">Spusťte VS Code</span><span class="sxs-lookup"><span data-stu-id="5faf2-229">Launch VS Code</span></span>

<span data-ttu-id="5faf2-230">Ujistěte se, že vaše DevKit není připojený.</span><span class="sxs-lookup"><span data-stu-id="5faf2-230">Make sure your DevKit is not connected.</span></span> <span data-ttu-id="5faf2-231">Nejprve spusťte VS Code a připojte DevKit k počítači.</span><span class="sxs-lookup"><span data-stu-id="5faf2-231">Launch VS Code first and connect the DevKit to your computer.</span></span> <span data-ttu-id="5faf2-232">VS Code automaticky ji najít a Překryvné úvodní stránka:</span><span class="sxs-lookup"><span data-stu-id="5faf2-232">VS Code will automatically find it and pop up an introduction page:</span></span>

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
<span data-ttu-id="5faf2-234">V některých případech při spuštění VS Code, zobrazí se výzva s chybou, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu.</span><span class="sxs-lookup"><span data-stu-id="5faf2-234">Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="5faf2-235">K jeho řešení, zavřete VS Code, znovu spusťte Arduino IDE a VS kód by měl vyhledejte Arduino IDE cestu správně.</span><span class="sxs-lookup"><span data-stu-id="5faf2-235">To solve it, close VS Code, launch Arduino IDE once again and VS Code should locate Arduino IDE path correctly.</span></span>

### <a name="open-arduino-examples-folder"></a><span data-ttu-id="5faf2-236">Otevřít složku Arduino příklady</span><span class="sxs-lookup"><span data-stu-id="5faf2-236">Open Arduino Examples folder</span></span>

<span data-ttu-id="5faf2-237">Přepnout na **"Arduino příklady"** kartě, přejděte na `Examples for MXCHIP AZ3166 > AzureIoT` a klikněte na `GetStarted`.</span><span class="sxs-lookup"><span data-stu-id="5faf2-237">Switch to **'Arduino Examples'** tab, navigate to `Examples for MXCHIP AZ3166 > AzureIoT` and click on `GetStarted`.</span></span>

![Mini solution příklady](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

<span data-ttu-id="5faf2-239">Pokud stát zavřete podokno znovu načíst, použijte `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) k vyvolání příkazu palety a typ **Arduino** a vyberte **Arduino: Příklady**.</span><span class="sxs-lookup"><span data-stu-id="5faf2-239">If you happen to close the pane, to reload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to invoke command palette and type **Arduino** to find and select **Arduino: Examples**.</span></span>

## <a name="provision-azure-services"></a><span data-ttu-id="5faf2-240">Zřídit služby Azure</span><span class="sxs-lookup"><span data-stu-id="5faf2-240">Provision Azure services</span></span>

<span data-ttu-id="5faf2-241">V okně řešení spuštění úkolu prostřednictvím `Ctrl+P` (systému macOS: `Cmd+P`) tak, že zadáte 'úkolů cloudu provision':</span><span class="sxs-lookup"><span data-stu-id="5faf2-241">In the solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':</span></span>

<span data-ttu-id="5faf2-242">V produktu VS Code terminálu interaktivního příkazového řádku vás provede zřizování požadované služby Azure:</span><span class="sxs-lookup"><span data-stu-id="5faf2-242">In the VS Code terminal, an interactive command line will guide you through provisioning the required Azure services:</span></span>

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a><span data-ttu-id="5faf2-244">Vytvoření a nahrání Arduino nákresu</span><span class="sxs-lookup"><span data-stu-id="5faf2-244">Build and upload Arduino sketch</span></span>

### <a name="install-required-library"></a><span data-ttu-id="5faf2-245">Nainstalujte požadované knihovny</span><span class="sxs-lookup"><span data-stu-id="5faf2-245">Install required library</span></span>

1. <span data-ttu-id="5faf2-246">Stiskněte klávesu `F1` nebo `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) k vyvolání příkazu palety a typ **Arduino** potom najděte a vyberte **Arduino: Správce knihovny**.</span><span class="sxs-lookup"><span data-stu-id="5faf2-246">Press `F1` or `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) to invoke command palette and type **Arduino** then find and select **Arduino: Library Manager**.</span></span>

2. <span data-ttu-id="5faf2-247">Vyhledejte `ArduinoJson` knihovnu a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="5faf2-247">Search for `ArduinoJson` library and click **Install**</span></span>

### <a name="build-and-upload-the-device-code"></a><span data-ttu-id="5faf2-248">Vytvořit a odeslat kód zařízení</span><span class="sxs-lookup"><span data-stu-id="5faf2-248">Build and upload the device code</span></span>

<span data-ttu-id="5faf2-249">Použití `Ctrl+P` (systému macOS: `Cmd+P`) ke spuštění 'úloha zařízení – nahrání'.</span><span class="sxs-lookup"><span data-stu-id="5faf2-249">Use `Ctrl+P` (macOS: `Cmd+P`) to run 'task device-upload'.</span></span> <span data-ttu-id="5faf2-250">Terminálu zobrazí výzvu k zadání režimu konfigurace.</span><span class="sxs-lookup"><span data-stu-id="5faf2-250">The terminal will prompt you to enter configuration mode.</span></span> <span data-ttu-id="5faf2-251">Uděláte to tak, podržte tlačítko A pak push a verzí na tlačítko Obnovit.</span><span class="sxs-lookup"><span data-stu-id="5faf2-251">To do so, hold down button A, then push and release the reset button.</span></span> <span data-ttu-id="5faf2-252">Na obrazovce zobrazí "Konfigurace".</span><span class="sxs-lookup"><span data-stu-id="5faf2-252">The screen will display 'Configuration'.</span></span> <span data-ttu-id="5faf2-253">To je nastavit připojovací řetězec, který načte z kroku 'úloh cloudu provision'.</span><span class="sxs-lookup"><span data-stu-id="5faf2-253">This is to set the connection string that retrieves from 'task cloud-provision' step.</span></span>

<span data-ttu-id="5faf2-254">Potom se spustí, ověření a odeslání nákresu Arduino:</span><span class="sxs-lookup"><span data-stu-id="5faf2-254">Then it will start verifying and uploading the Arduino sketch:</span></span>

![Mini-solution – zařízení – nahrání](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

<span data-ttu-id="5faf2-256">DevKit bude restartujte počítač a spustit kód.</span><span class="sxs-lookup"><span data-stu-id="5faf2-256">The DevKit will reboot and start running the code.</span></span>

## <a name="test-the-project"></a><span data-ttu-id="5faf2-257">Testování projektu</span><span class="sxs-lookup"><span data-stu-id="5faf2-257">Test the project</span></span>

<span data-ttu-id="5faf2-258">V produktu VS Code klikněte na ikonu power moduly na stavovém řádku otevřete sériové monitorování.</span><span class="sxs-lookup"><span data-stu-id="5faf2-258">In VS Code, click the power plug icon on the status bar to open the Serial Monitor.</span></span>

<span data-ttu-id="5faf2-259">Ukázkové aplikace pracuje správně, pokud jste se zobrazit následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="5faf2-259">The sample application is running successfully when you see the following results:</span></span>

* <span data-ttu-id="5faf2-260">Sériové monitorování zobrazuje stejné informace jako obsah na tomto snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="5faf2-260">The Serial Monitor displays the same information as the content in the screenshot below.</span></span>
* <span data-ttu-id="5faf2-261">Indikátor LED na MXChip IoT DevKit bliká.</span><span class="sxs-lookup"><span data-stu-id="5faf2-261">The LED on MXChip IoT DevKit is blinking.</span></span>

![Závěrečný výstup v produktu VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a><span data-ttu-id="5faf2-263">Problémy a zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="5faf2-263">Problems and feedback</span></span>

<span data-ttu-id="5faf2-264">Můžete najít [nejčastější dotazy k](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Pokud dojde k potížím nebo oslovení nám níže kanály.</span><span class="sxs-lookup"><span data-stu-id="5faf2-264">You can find [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) if you encounter problems or reach out to us from the channels below.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5faf2-265">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5faf2-265">Next steps</span></span>

<span data-ttu-id="5faf2-266">Máte úspěšně připojen IoT DevKit MXChip do služby IoT Hub a data zaznamenaná senzor odeslané do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5faf2-266">You have successfully connected an MXChip IoT DevKit to your IoT Hub, and sent the captured sensor data to your IoT hub.</span></span>

<span data-ttu-id="5faf2-267">Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:</span><span class="sxs-lookup"><span data-stu-id="5faf2-267">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

- [<span data-ttu-id="5faf2-268">Správa zasílání zpráv cloudových zařízení pomocí iothub-exploreru</span><span class="sxs-lookup"><span data-stu-id="5faf2-268">Manage cloud device messaging with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [<span data-ttu-id="5faf2-269">Uložení zpráv IoT Hub do úložiště dat Azure</span><span class="sxs-lookup"><span data-stu-id="5faf2-269">Save IoT Hub messages to Azure data storage</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [<span data-ttu-id="5faf2-270">Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="5faf2-270">Use Power BI to visualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [<span data-ttu-id="5faf2-271">Použít Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="5faf2-271">Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [<span data-ttu-id="5faf2-272">Počasí prognózy používající senzor data ze služby IoT hub v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5faf2-272">Weather forecast using the sensor data from your IoT hub in Azure Machine Learning</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [<span data-ttu-id="5faf2-273">Správa zařízení s využitím iothub-exploreru</span><span class="sxs-lookup"><span data-stu-id="5faf2-273">Device management with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [<span data-ttu-id="5faf2-274">Vzdálené monitorování a oznámení s využitím Logic Apps</span><span class="sxs-lookup"><span data-stu-id="5faf2-274">Remote monitoring and notifications with Logic Apps</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)