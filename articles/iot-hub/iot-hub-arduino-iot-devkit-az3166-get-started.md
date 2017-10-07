---
title: "aaaIoT DevKit toocloud - připojení DevKit AZ3166 IoT tooAzure IoT Hub | Microsoft Docs"
description: "Zjistěte, jak toosetup a připojte toosend data toohello cloudu Azure platformy IoT DevKit AZ3166 tooAzure IoT Hub pro něj v tomto kurzu."
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
ms.openlocfilehash: fd36abaed1fd9f73028b84312bca9e900fb9c004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-iot-devkit-az3166-tooazure-iot-hub-in-hello-cloud"></a><span data-ttu-id="93831-103">Připojit IoT DevKit AZ3166 tooAzure IoT Hub v cloudu hello</span><span class="sxs-lookup"><span data-stu-id="93831-103">Connect IoT DevKit AZ3166 tooAzure IoT Hub in hello cloud</span></span>

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

<span data-ttu-id="93831-104">Hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) lze použít toodevelop a prototypu využití služby Microsoft Azure řešení Internetu věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="93831-104">hello [MXChip IoT DevKit](https://microsoft.github.io/azure-iot-developer-kit/) can be used toodevelop and prototype Internet of Things (IoT) solutions leveraging Microsoft Azure services.</span></span> <span data-ttu-id="93831-105">Obsahuje kompatibilní panelu s bohatou periferních zařízení a senzorů, balíček Tabule open source a rozšiřujících se Arduino [projekty katalogu](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span><span class="sxs-lookup"><span data-stu-id="93831-105">It includes an Arduino compatible board with rich peripherals and sensors, an open-source board package and a growing [projects catalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="93831-106">Co dělat</span><span class="sxs-lookup"><span data-stu-id="93831-106">What you do</span></span>
<span data-ttu-id="93831-107">Připojit [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub shromažďování hello teploty a vlhkosti data ze senzorů vytvořit a odeslat hello data tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="93831-107">Connect [DevKit](https://microsoft.github.io/azure-iot-developer-kit/) tooan Azure IoT hub that you create, collect hello temperature and humidity data from sensors and send hello data tooIoT hub.</span></span>

<span data-ttu-id="93831-108">Nemáte DevKit ještě?</span><span class="sxs-lookup"><span data-stu-id="93831-108">Don't have a DevKit yet?</span></span> <span data-ttu-id="93831-109">Získat novou [zde](https://aka.ms/iot-devkit-purchase).</span><span class="sxs-lookup"><span data-stu-id="93831-109">Get a new one [here](https://aka.ms/iot-devkit-purchase).</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="93831-110">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="93831-110">What you learn</span></span>

* <span data-ttu-id="93831-111">Jak tooconnect IoT DevKit tooWireless přístup bodu a příprava vývojového prostředí.</span><span class="sxs-lookup"><span data-stu-id="93831-111">How tooconnect IoT DevKit tooWireless access point and prepare your development environment.</span></span>
* <span data-ttu-id="93831-112">Jak toocreate služby IoT hub a registrovat zařízení pro MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="93831-112">How toocreate an IoT hub and register a device for MXChip IoT DevKit.</span></span>
* <span data-ttu-id="93831-113">Jak data snímačů toocollect spuštěním ukázkovou aplikaci na MXChip IoT DevKit.</span><span class="sxs-lookup"><span data-stu-id="93831-113">How toocollect sensor data by running a sample application on MXChip IoT DevKit.</span></span>
* <span data-ttu-id="93831-114">Jak toosend hello senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="93831-114">How toosend hello sensor data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="93831-115">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="93831-115">What you need</span></span>

* <span data-ttu-id="93831-116">MXChip IoT DevKit fórum s malých kabelu USB.</span><span class="sxs-lookup"><span data-stu-id="93831-116">An MXChip IoT DevKit board with a micro USB cable.</span></span> [<span data-ttu-id="93831-117">Získejte nyní</span><span class="sxs-lookup"><span data-stu-id="93831-117">Get it now</span></span>](https://aka.ms/iot-devkit-purchase)
* <span data-ttu-id="93831-118">Počítač se systémem Windows 10 nebo systému macOS 10.10 +</span><span class="sxs-lookup"><span data-stu-id="93831-118">A computer running Windows 10 or macOS 10.10+</span></span>
* <span data-ttu-id="93831-119">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="93831-119">An active Azure subscription</span></span>
  * <span data-ttu-id="93831-120">Aktivovat [Bezplatný zkušební účet Microsoft Azure 30 dnů](https://azureinfo.microsoft.com/us-freetrial.html)</span><span class="sxs-lookup"><span data-stu-id="93831-120">Activate a [free 30-day trial Microsoft Azure account](https://azureinfo.microsoft.com/us-freetrial.html)</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="93831-121">Připravte svůj hardware</span><span class="sxs-lookup"><span data-stu-id="93831-121">Prepare your hardware</span></span>

<span data-ttu-id="93831-122">Zapojení hello hardwaru tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="93831-122">Hook up hello hardware tooyour computer.</span></span>

### <a name="hardware-you-need"></a><span data-ttu-id="93831-123">Hardware, které potřebujete</span><span class="sxs-lookup"><span data-stu-id="93831-123">Hardware you need</span></span>

* <span data-ttu-id="93831-124">DevKit panelu</span><span class="sxs-lookup"><span data-stu-id="93831-124">DevKit board</span></span>
* <span data-ttu-id="93831-125">Malých kabelu USB</span><span class="sxs-lookup"><span data-stu-id="93831-125">Micro USB cable</span></span>

![získávání spuštění – hardware](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/hardware.jpg)

### <a name="connect-devkit-tooyour-computer"></a><span data-ttu-id="93831-127">Připojte počítač tooyour DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-127">Connect DevKit tooyour computer</span></span>

1. <span data-ttu-id="93831-128">Připojení USB end tooyour počítače</span><span class="sxs-lookup"><span data-stu-id="93831-128">Connect USB end tooyour PC</span></span>
2. <span data-ttu-id="93831-129">Připojení Micro USB end toohello DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-129">Connect Micro USB end toohello DevKit</span></span>
3. <span data-ttu-id="93831-130">Další toopower Hello zelená DIODU potvrdí připojení</span><span class="sxs-lookup"><span data-stu-id="93831-130">hello green LED next toopower confirms connection</span></span>

![získávání spuštění – připojení](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect.jpg)

## <a name="configure-wifi"></a><span data-ttu-id="93831-132">Konfigurace sítě Wi-Fi</span><span class="sxs-lookup"><span data-stu-id="93831-132">Configure WiFi</span></span>

<span data-ttu-id="93831-133">Projekty IoT spoléhají na připojení k Internetu.</span><span class="sxs-lookup"><span data-stu-id="93831-133">IoT projects rely on Internet connectivity.</span></span> <span data-ttu-id="93831-134">Použijte následující pokyny tooconfigure hello DevKit tooconnect tooWiFi hello.</span><span class="sxs-lookup"><span data-stu-id="93831-134">Use hello following instructions tooconfigure hello DevKit tooconnect tooWiFi.</span></span>

### <a name="enter-ap-mode"></a><span data-ttu-id="93831-135">Zadejte režim Asie a Tichomoří</span><span class="sxs-lookup"><span data-stu-id="93831-135">Enter AP Mode</span></span>

<span data-ttu-id="93831-136">Podržte tlačítko B, poté resetujte nabízení a verze hello tlačítko a potom na tlačítko pro uvolnění B. Vaše DevKit zadá režimu přístupový bod pro konfiguraci sítě Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="93831-136">Hold down button B, then push and release hello reset button, then release button B. Your DevKit will enter AP Mode for configuring WiFi.</span></span> <span data-ttu-id="93831-137">úvodní obrazovka se zobrazí hello služby nastavit Identifier(SSID) DevKit Dobrý den, jakož i hello konfigurace portálu IP adresa:</span><span class="sxs-lookup"><span data-stu-id="93831-137">hello screen will display hello Service Set Identifier(SSID) of hello DevKit as well as hello configuration portal IP address:</span></span>

![získávání spuštění-Wi-Fi-Asie a Tichomoří](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ap.jpg)

### <a name="connect-toodevkit-ap"></a><span data-ttu-id="93831-139">Asie a Tichomoří tooDevKit připojení</span><span class="sxs-lookup"><span data-stu-id="93831-139">Connect tooDevKit AP</span></span>

<span data-ttu-id="93831-140">Nyní použijte jiný povolit Wi-Fi zařízení (počítač nebo mobilní telefon) tooconnect toohello DevKit SSID (zvýrazněných v výše uvedený snímek obrazovky hello), ponechte prázdné heslo hello.</span><span class="sxs-lookup"><span data-stu-id="93831-140">Now, use another WiFi enabled device (PC or mobile phone) tooconnect toohello DevKit SSID (highlighted in hello screenshot above), leave hello password empty.</span></span>

![získávání spuštění ssid](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/connect-ssid.png)

### <a name="configure-wifi-for-devkit"></a><span data-ttu-id="93831-142">Konfigurace sítě Wi-Fi pro DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-142">Configure WiFi for DevKit</span></span>

<span data-ttu-id="93831-143">Otevřete hello IP adresu na obrazovce DevKit hello v počítači nebo v prohlížeči mobilního telefonu, vyberte hello Wi-Fi sítě, kterou chcete hello DevKit tooconnect na a pak zadejte heslo hello.</span><span class="sxs-lookup"><span data-stu-id="93831-143">Open hello IP address shown on hello DevKit screen on your PC or mobile phone browser, select hello WiFi network you want hello DevKit tooconnect to, then type hello password.</span></span> <span data-ttu-id="93831-144">Klikněte na tlačítko **připojit** toocomplete:</span><span class="sxs-lookup"><span data-stu-id="93831-144">Click **Connect** toocomplete:</span></span>

![získávání spuštění-Wi-Fi portal](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-portal.png)

<span data-ttu-id="93831-146">Jakmile hello připojení úspěšné, hello DevKit restartuje za několik sekund.</span><span class="sxs-lookup"><span data-stu-id="93831-146">Once hello connection succeeds, hello DevKit will reboot in a few seconds.</span></span> <span data-ttu-id="93831-147">Pokud byly úspěšné, zobrazí se název hello Wi-Fi a IP adresu na úvodní obrazovka:</span><span class="sxs-lookup"><span data-stu-id="93831-147">If succeeded, you will see hello WiFi name and IP address on hello screen:</span></span>

![získávání spuštění-Wi-Fi ip](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/wifi-ip.jpg)

> [!NOTE] 
<span data-ttu-id="93831-149">zobrazená v hello fotografií Hello IP adresa nemusí odpovídat skutečné IP hello přiřazené a zobrazené na obrazovce DevKit hello.</span><span class="sxs-lookup"><span data-stu-id="93831-149">hello IP address displayed in hello photo may not match hello actual IP assigned and displayed on hello DevKit screen.</span></span> <span data-ttu-id="93831-150">To je normální jako Wi-Fi používá DHCP toodynamically přiřazovat IP adresy.</span><span class="sxs-lookup"><span data-stu-id="93831-150">This is normal as WiFi uses DHCP toodynamically assign IPs.</span></span>

<span data-ttu-id="93831-151">Po dokončení konfigurace sítě Wi-Fi, bude vaše přihlašovací údaje trvalé hello zařízení pro toto připojení, i v případě, že byl odpojen.</span><span class="sxs-lookup"><span data-stu-id="93831-151">After WiFi is configured, your credentials will be persisted on hello device for that connection, even if unplugged.</span></span> <span data-ttu-id="93831-152">Například pokud jste nakonfigurovali hello DevKit pro Wi-Fi v domácnosti a pak trvalo hello DevKit toohello office, budete potřebovat tooreconfigure Asie režimu (od kroku **zadejte režim Asie**) tooconnect hello DevKit tooyour office Wi-Fi.</span><span class="sxs-lookup"><span data-stu-id="93831-152">For example, if you configured hello DevKit for WiFi in your home and then took hello DevKit toohello office, you will need tooreconfigure AP mode (starting at step **Enter AP Mode**) tooconnect hello DevKit tooyour office WiFi.</span></span> 

## <a name="start-using-devkit"></a><span data-ttu-id="93831-153">Začít používat DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-153">Start using DevKit</span></span>

<span data-ttu-id="93831-154">Hello výchozí aplikaci spuštěnou na DevKit zkontroluje hello nejnovější verzi firmwaru hello a zobrazí některé senzor diagnostická data za vás.</span><span class="sxs-lookup"><span data-stu-id="93831-154">hello default app running on DevKit will check hello latest version of hello firmware and display some sensor diagnosis data for you.</span></span>

### <a name="upgrade-toohello-latest-firmware"></a><span data-ttu-id="93831-155">Upgradovat nejnovější firmware toohello</span><span class="sxs-lookup"><span data-stu-id="93831-155">Upgrade toohello latest firmware</span></span>

<span data-ttu-id="93831-156">Zobrazí se výzva na úvodní obrazovka obě hello firmware aktuální a nejnovější verze, pokud je potřeba upgrade.</span><span class="sxs-lookup"><span data-stu-id="93831-156">You will be prompted on hello screen both hello current and latest firmware version if there is an upgrade needed.</span></span> <span data-ttu-id="93831-157">Postupujte podle [upgradovat firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) Průvodce tooupgrade ho.</span><span class="sxs-lookup"><span data-stu-id="93831-157">Follow [Upgrade firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/upgrading/) guide tooupgrade it.</span></span>

![získávání spuštění firmwaru](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/firmware.jpg)

> [!NOTE] 
<span data-ttu-id="93831-159">Toto je jednorázové úsilí, jakmile začnete vývoji na hello DevKit a nahrát aplikaci, budete mít nejnovější firmware hello pocházet s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="93831-159">This is a one-time effort, once you start developing on hello DevKit and upload your app, you will have hello latest firmware come with your app.</span></span>

### <a name="test-various-sensors"></a><span data-ttu-id="93831-160">Testování různých senzorů</span><span class="sxs-lookup"><span data-stu-id="93831-160">Test various sensors</span></span>

<span data-ttu-id="93831-161">Stiskněte tlačítko B tootest senzorů, pokračujte stisknutím a uvolněním hello B tlačítko toocycle prostřednictvím každý senzor.</span><span class="sxs-lookup"><span data-stu-id="93831-161">Press button B tootest sensors, continue pressing and releasing hello B button toocycle through each sensor.</span></span>

![získávání spuštění senzorů](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/sensors.jpg)

## <a name="prepare-development-environment"></a><span data-ttu-id="93831-163">Příprava vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="93831-163">Prepare development environment</span></span>

<span data-ttu-id="93831-164">Nyní je čas tooset hello vývojového prostředí: nástroje a balíčky pro toobuild omráčení aplikace či aplikace IoT.</span><span class="sxs-lookup"><span data-stu-id="93831-164">Now it's time tooset up hello development environment: tools and packages for you toobuild stunning IoT applications.</span></span> <span data-ttu-id="93831-165">Můžete vybrat Windows nebo verze systému macOS podle tooyour operačního systému.</span><span class="sxs-lookup"><span data-stu-id="93831-165">You can choose Windows or macOS version according tooyour operating system.</span></span>


### <a name="windows"></a><span data-ttu-id="93831-166">Windows</span><span class="sxs-lookup"><span data-stu-id="93831-166">Windows</span></span>

<span data-ttu-id="93831-167">Doporučujeme vám toouse hello instalace balíčku tooprepare hello vývojové prostředí.</span><span class="sxs-lookup"><span data-stu-id="93831-167">We encourage you toouse hello installation package tooprepare hello development environment.</span></span> <span data-ttu-id="93831-168">Pokud narazíte na problémy, můžete podle hello [ruční kroky](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget ho provést.</span><span class="sxs-lookup"><span data-stu-id="93831-168">If you encounter any issues, you can follow hello [manual steps](https://microsoft.github.io/azure-iot-developer-kit/docs/installation/) tooget it done.</span></span>

#### <a name="download-latest-package"></a><span data-ttu-id="93831-169">Stáhněte si nejnovější balíček</span><span class="sxs-lookup"><span data-stu-id="93831-169">Download latest package</span></span>

<span data-ttu-id="93831-170">Hello `.zip` si stáhnout soubor obsahuje všechny nezbytné nástroje a balíčky, které jsou potřebné pro vývoj DevKit.</span><span class="sxs-lookup"><span data-stu-id="93831-170">hello `.zip` file you download contains all necessary tools and packages required for DevKit development.</span></span>

> [!div class="button"]
[<span data-ttu-id="93831-171">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="93831-171">Download</span></span>](https://azureboard.azureedge.net/prod/installpackage/devkit_install_1.0.2.zip)


> [!NOTE] 
> <span data-ttu-id="93831-172">Hello `.zip` soubor obsahuje následující hello nástroje a balíčků.</span><span class="sxs-lookup"><span data-stu-id="93831-172">hello `.zip` file contains hello following tools and packages.</span></span> <span data-ttu-id="93831-173">Pokud již máte některé součásti nainstalované, skript hello rozpozná a je přeskočit.</span><span class="sxs-lookup"><span data-stu-id="93831-173">If you already have some components installed, hello script will detect and skip them.</span></span>
> * <span data-ttu-id="93831-174">Node.js a Yarn: modul Runtime pro hello instalační skript a automatizované úlohy</span><span class="sxs-lookup"><span data-stu-id="93831-174">Node.js and Yarn: Runtime for hello setup script and automated tasks</span></span>
> * <span data-ttu-id="93831-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) -prostředí příkazového řádku a platformy pro správu prostředků Azure, hello MSI obsahuje závislé Python a pip.</span><span class="sxs-lookup"><span data-stu-id="93831-175">[Azure CLI 2.0 MSI](https://docs.microsoft.com//cli/azure/install-azure-cli#windows) - Cross-platform command-line experience for managing Azure resources, hello MSI contains dependent Python and pip.</span></span>
> * <span data-ttu-id="93831-176">[Visual Studio Code](https://code.visualstudio.com/): editor lehký kód pro vývoj DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-176">[Visual Studio Code](https://code.visualstudio.com/): Lightweight code editor for DevKit development</span></span>
> * <span data-ttu-id="93831-177">[Rozšíření sady Visual Studio Code pro Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): vývoj umožňuje Arduino v produktu VS Code</span><span class="sxs-lookup"><span data-stu-id="93831-177">[Visual Studio Code extension for Arduino](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.vscode-arduino): Enables Arduino development in VS Code</span></span>
> * <span data-ttu-id="93831-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello rozšíření pro Arduino spoléhá na tento nástroj</span><span class="sxs-lookup"><span data-stu-id="93831-178">[Arduino IDE](https://www.arduino.cc/en/Main/Software): hello extension for Arduino relies on this tool</span></span>
> * <span data-ttu-id="93831-179">Balíček DevKit Tabule: Nástroj řetězy, knihovny a projekty doplňku hello DevKit</span><span class="sxs-lookup"><span data-stu-id="93831-179">DevKit Board Package: Tool chains, libraries and projects for hello DevKit</span></span>
> * <span data-ttu-id="93831-180">Nástroj ST odkaz: Základní nástroje a ovladače</span><span class="sxs-lookup"><span data-stu-id="93831-180">ST-Link Utility: Essential utilities and drivers</span></span>

#### <a name="run-installation-script"></a><span data-ttu-id="93831-181">Spusťte instalační skript</span><span class="sxs-lookup"><span data-stu-id="93831-181">Run installation script</span></span>

<span data-ttu-id="93831-182">V Průzkumníku souborů Windows, vyhledejte hello `.zip` a rozbalte ho, vyhledejte `install.cmd`, klikněte pravým tlačítkem a vyberte **"Spustit jako správce"** toostart.</span><span class="sxs-lookup"><span data-stu-id="93831-182">In Windows File Explorer, locate hello `.zip` and extract it, find `install.cmd`, right-click and select **"Run as administrator"** toostart.</span></span>

![získávání spuštění spustit správce.](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/run-admin.png)

<span data-ttu-id="93831-184">Během instalace zobrazí se průběh hello každý nástroj nebo balíčku.</span><span class="sxs-lookup"><span data-stu-id="93831-184">During installation, you will see hello progress of each tool or package.</span></span>

![získávání spuštění instalace](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/install.png)

#### <a name="confirm-tooinstall-drivers"></a><span data-ttu-id="93831-186">Potvrďte tooinstall ovladače</span><span class="sxs-lookup"><span data-stu-id="93831-186">Confirm tooinstall drivers</span></span>

<span data-ttu-id="93831-187">Hello VS Code pro rozšíření Arduino spoléhá na hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="93831-187">hello VS Code for Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="93831-188">Pokud je to hello poprvé instalujete hello Arduino IDE, bude výzvami tooinstall příslušné ovladače:</span><span class="sxs-lookup"><span data-stu-id="93831-188">If this is hello first time you are installing hello Arduino IDE, you will be prompted tooinstall relevant drivers:</span></span>

![získávání spuštění – ovladače](media/iot-hub-arduino-devkit-az3166-get-started/getting-started/driver.png)

<span data-ttu-id="93831-190">Má trvat přibližně 10 minut toofinish instalace v závislosti na rychlosti sítě Internet.</span><span class="sxs-lookup"><span data-stu-id="93831-190">It should take around 10 minutes toofinish installation depending on your Internet speed.</span></span> <span data-ttu-id="93831-191">Po dokončení instalace hello byste měli vidět Visual Studio Code a Arduino IDE zástupce na ploše.</span><span class="sxs-lookup"><span data-stu-id="93831-191">Once hello installation is complete, you should see Visual Studio Code and Arduino IDE shortcuts on your desktop.</span></span>

> [!NOTE] 
<span data-ttu-id="93831-192">V některých případech při spuštění VS Code, zobrazí se výzva k chybě, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu.</span><span class="sxs-lookup"><span data-stu-id="93831-192">Occasionally, when you launch VS Code, you will be prompted with an error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="93831-193">toosolve ji zavřít VS Code Arduino IDE spustit jednou a VS kód by měl vyhledejte Arduino IDE cestu správně.</span><span class="sxs-lookup"><span data-stu-id="93831-193">toosolve it, close VS Code, launch Arduino IDE once and VS Code should locate Arduino IDE path correctly.</span></span>


### <a name="macos-preview"></a><span data-ttu-id="93831-194">systému macOS (Preview)</span><span class="sxs-lookup"><span data-stu-id="93831-194">macOS (Preview)</span></span>

<span data-ttu-id="93831-195">Postupujte podle těchto kroků tooprepare vývojového prostředí v systému macOS.</span><span class="sxs-lookup"><span data-stu-id="93831-195">Follow these steps tooprepare development environment on macOS.</span></span>

#### <a name="install-azure-cli-20"></a><span data-ttu-id="93831-196">Instalace Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="93831-196">Install Azure CLI 2.0</span></span>

<span data-ttu-id="93831-197">Postupujte podle hello [oficiální průvodce](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span><span class="sxs-lookup"><span data-stu-id="93831-197">Follow hello [official guide](https://docs.microsoft.com//cli/azure/install-azure-cli) tooinstall Azure CLI 2.0:</span></span>

<span data-ttu-id="93831-198">Nainstalovat Azure CLI 2.0 s jedním `curl` příkaz:</span><span class="sxs-lookup"><span data-stu-id="93831-198">Install Azure CLI 2.0 with one `curl` command:</span></span>

```bash
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="93831-199">A znovu spusťte váš příkazové prostředí pro efekt tootake změny:</span><span class="sxs-lookup"><span data-stu-id="93831-199">And restart your command shell for changes tootake effect:</span></span>

```bash
exec -l $SHELL
```

#### <a name="install-arduino-ide"></a><span data-ttu-id="93831-200">Nainstalujte Arduino IDE</span><span class="sxs-lookup"><span data-stu-id="93831-200">Install Arduino IDE</span></span>

<span data-ttu-id="93831-201">Hello rozšíření Visual Studio Code Arduino spoléhá na hello Arduino IDE.</span><span class="sxs-lookup"><span data-stu-id="93831-201">hello Visual Studio Code Arduino extension relies on hello Arduino IDE.</span></span> <span data-ttu-id="93831-202">Stáhněte a nainstalujte hello [Arduino IDE pro systému macOS](https://www.arduino.cc/en/Main/Software).</span><span class="sxs-lookup"><span data-stu-id="93831-202">Download and install hello [Arduino IDE for macOS](https://www.arduino.cc/en/Main/Software).</span></span>

#### <a name="install-visual-studio-code"></a><span data-ttu-id="93831-203">Nainstalovat Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93831-203">Install Visual Studio Code</span></span>

<span data-ttu-id="93831-204">Stáhněte a nainstalujte [Visual Studio Code pro systému macOS](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="93831-204">Download and install [Visual Studio Code for macOS](https://code.visualstudio.com/).</span></span> <span data-ttu-id="93831-205">Bude jím hello primární vývojový nástroj k vytváření aplikací DevKit IoT.</span><span class="sxs-lookup"><span data-stu-id="93831-205">This will be hello primary development tool for building DevKit IoT applications.</span></span>

####  <a name="download-latest-package"></a><span data-ttu-id="93831-206">Stáhněte si nejnovější balíček</span><span class="sxs-lookup"><span data-stu-id="93831-206">Download latest package</span></span>

1. <span data-ttu-id="93831-207">Nainstalujte Node.js.</span><span class="sxs-lookup"><span data-stu-id="93831-207">Install Node.js.</span></span> <span data-ttu-id="93831-208">Můžete použít Správce balíčků oblíbených systému macOS [Homebrew](https://brew.sh/) nebo [předem vytvořené instalační program](https://nodejs.org/en/download/) tooinstall ho.</span><span class="sxs-lookup"><span data-stu-id="93831-208">You can use popular macOS package manager [Homebrew](https://brew.sh/) or [pre-built installer](https://nodejs.org/en/download/) tooinstall it.</span></span>

2. <span data-ttu-id="93831-209">Stáhněte si `.zip` souboru, který obsahuje skripty úloh, které jsou potřebné pro vývoj DevKit v produktu VS Code.</span><span class="sxs-lookup"><span data-stu-id="93831-209">Download `.zip` file containing task scripts required for DevKit development in VS Code.</span></span>

   > [!div class="button"]
   [<span data-ttu-id="93831-210">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="93831-210">Download</span></span>](https://azureboard.azureedge.net/installpackage/devkit_tasks_1.0.2.zip)

   <span data-ttu-id="93831-211">Vyhledejte hello `.zip` a rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="93831-211">Locate hello `.zip` and extract it.</span></span> <span data-ttu-id="93831-212">Spusťte **Terminálové** aplikace a spusťte hello následující příkazy tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="93831-212">Then launch **Terminal** app and run hello following commands tooconfigure:</span></span>

   <span data-ttu-id="93831-213">Přesunutí složky uživatele systému macOS tooyour extrahovanou složku:</span><span class="sxs-lookup"><span data-stu-id="93831-213">Move extracted folder tooyour macOS user folder:</span></span>
   ```bash
   mv [.zip extracted folder]/azure-board-cli ~/. ; cd ~/azure-board-cli
   ```
  
   <span data-ttu-id="93831-214">Nainstalujte `npm` balíčky:</span><span class="sxs-lookup"><span data-stu-id="93831-214">Install `npm` packages:</span></span>
   ```
   npm install
   ```

#### <a name="install-vs-code-extension-for-arduino"></a><span data-ttu-id="93831-215">Instalace rozšíření VS Code pro Arduino</span><span class="sxs-lookup"><span data-stu-id="93831-215">Install VS Code extension for Arduino</span></span>

<span data-ttu-id="93831-216">Visual Studio Code můžete rozšíření Marketplace tooinstall přímo v nástroji hello, jednoduše klikněte na ikonu rozšíření hello v levé nabídce podokně hello a pak vyhledejte `Arduino` tooinstall:</span><span class="sxs-lookup"><span data-stu-id="93831-216">Visual Studio Code allows you tooinstall Marketplace extensions directly in hello tool, simply click hello extensions icon in hello left menu pane and then search `Arduino` tooinstall:</span></span>

![instalace rozšíření](media/iot-hub-arduino-devkit-az3166-get-started/installation-extensions-mac.png)

#### <a name="install-devkit-board-package"></a><span data-ttu-id="93831-218">Instalovat balíček DevKit panelu</span><span class="sxs-lookup"><span data-stu-id="93831-218">Install DevKit board package</span></span>

<span data-ttu-id="93831-219">Budete potřebovat tooadd hello DevKit panel pomocí hello panelu Správce v kódu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="93831-219">You will need tooadd hello DevKit board using hello Board Manager in Visual Studio Code.</span></span>

1. <span data-ttu-id="93831-220">Použití `Cmd+Shift+P` tooinvoke příkaz palety a typ **Arduino** potom najděte a vyberte **Arduino: Tabule Manager**.</span><span class="sxs-lookup"><span data-stu-id="93831-220">Use `Cmd+Shift+P` tooinvoke command palette and type **Arduino** then find and select **Arduino: Board Manager**.</span></span>

2. <span data-ttu-id="93831-221">Klikněte na tlačítko **další adresy URL** v pravé dolní části hello.</span><span class="sxs-lookup"><span data-stu-id="93831-221">Click **'Additional URLs'** at hello bottom right.</span></span>
   <span data-ttu-id="93831-222">![instalace další adresy URL](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span><span class="sxs-lookup"><span data-stu-id="93831-222">![installation-additional-urls](media/iot-hub-arduino-devkit-az3166-get-started/installation-additional-urls-mac.png)</span></span>

3. <span data-ttu-id="93831-223">V hello `settings.json` soubor, přidejte řádek na konec hello `USER SETTINGS` podokně a uložte.</span><span class="sxs-lookup"><span data-stu-id="93831-223">In hello `settings.json` file, add a line at hello bottom of `USER SETTINGS` pane and save.</span></span>
   ```json
   "arduino.additionalUrls": "https://raw.githubusercontent.com/VSChina/azureiotdevkit_tools/master/package_azureboard_index.json"
   ```
   ![instalace. nastavení json](media/iot-hub-arduino-devkit-az3166-get-started/installation-settings-json-mac.png)

4. <span data-ttu-id="93831-225">Nyní v hello Manager panelu Hledat 'az3166' a nainstalujte nejnovější verzi hello.</span><span class="sxs-lookup"><span data-stu-id="93831-225">Now in hello Board Manager search for 'az3166' and install hello latest version.</span></span>
   <span data-ttu-id="93831-226">![instalace az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span><span class="sxs-lookup"><span data-stu-id="93831-226">![installation-az3166](media/iot-hub-arduino-devkit-az3166-get-started/installation-az3166-mac.png)</span></span>

<span data-ttu-id="93831-227">Nyní máte všechny potřebné nástroje hello a balíčky pro systému macOS nainstalována.</span><span class="sxs-lookup"><span data-stu-id="93831-227">You now have all hello necessary tools and packages installed for macOS.</span></span>


## <a name="open-project-folder"></a><span data-ttu-id="93831-228">Otevřít projekt složky</span><span class="sxs-lookup"><span data-stu-id="93831-228">Open project folder</span></span>

### <a name="launch-vs-code"></a><span data-ttu-id="93831-229">Spusťte VS Code</span><span class="sxs-lookup"><span data-stu-id="93831-229">Launch VS Code</span></span>

<span data-ttu-id="93831-230">Ujistěte se, že vaše DevKit není připojený.</span><span class="sxs-lookup"><span data-stu-id="93831-230">Make sure your DevKit is not connected.</span></span> <span data-ttu-id="93831-231">Nejprve spusťte VS Code a připojte počítač tooyour DevKit hello.</span><span class="sxs-lookup"><span data-stu-id="93831-231">Launch VS Code first and connect hello DevKit tooyour computer.</span></span> <span data-ttu-id="93831-232">VS Code automaticky ji najít a Překryvné úvodní stránka:</span><span class="sxs-lookup"><span data-stu-id="93831-232">VS Code will automatically find it and pop up an introduction page:</span></span>

![Mini solution vscode](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-vscode.png)

> [!NOTE] 
<span data-ttu-id="93831-234">V některých případech při spuštění VS Code, zobrazí se výzva s chybou, který nemůže najít Arduino IDE nebo balíček Příbuzná panelu.</span><span class="sxs-lookup"><span data-stu-id="93831-234">Occasionally, when you launch VS Code, you will be prompted with error that cannot find Arduino IDE or related board package.</span></span> <span data-ttu-id="93831-235">toosolve ji zavřít VS Code, znovu spusťte Arduino IDE a VS kód by měl vyhledejte Arduino IDE cestu správně.</span><span class="sxs-lookup"><span data-stu-id="93831-235">toosolve it, close VS Code, launch Arduino IDE once again and VS Code should locate Arduino IDE path correctly.</span></span>

### <a name="open-arduino-examples-folder"></a><span data-ttu-id="93831-236">Otevřít složku Arduino příklady</span><span class="sxs-lookup"><span data-stu-id="93831-236">Open Arduino Examples folder</span></span>

<span data-ttu-id="93831-237">Přepínač příliš**"Arduino příklady"** kartě, přejděte příliš`Examples for MXCHIP AZ3166 > AzureIoT` a klikněte na `GetStarted`.</span><span class="sxs-lookup"><span data-stu-id="93831-237">Switch too**'Arduino Examples'** tab, navigate too`Examples for MXCHIP AZ3166 > AzureIoT` and click on `GetStarted`.</span></span>

![Mini solution příklady](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution-examples.png)

<span data-ttu-id="93831-239">Pokud jste tooclose hello podokně tooreload ji, použijte `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) tooinvoke příkaz palety a typ **Arduino** toofind a vyberte **Arduino: Příklady**.</span><span class="sxs-lookup"><span data-stu-id="93831-239">If you happen tooclose hello pane, tooreload it, use `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** toofind and select **Arduino: Examples**.</span></span>

## <a name="provision-azure-services"></a><span data-ttu-id="93831-240">Zřídit služby Azure</span><span class="sxs-lookup"><span data-stu-id="93831-240">Provision Azure services</span></span>

<span data-ttu-id="93831-241">V okně řešení hello spusťte úlohu `Ctrl+P` (systému macOS: `Cmd+P`) tak, že zadáte 'úkolů cloudu provision':</span><span class="sxs-lookup"><span data-stu-id="93831-241">In hello solution window, run your task through `Ctrl+P` (macOS: `Cmd+P`) by typing 'task cloud-provision':</span></span>

<span data-ttu-id="93831-242">Hello VS Code terminálu, že interaktivního příkazového řádku vás provede zřizování hello vyžadovat služeb Azure:</span><span class="sxs-lookup"><span data-stu-id="93831-242">In hello VS Code terminal, an interactive command line will guide you through provisioning hello required Azure services:</span></span>

![Mini-solution-cloud-provision](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/cloud-provision.png)

## <a name="build-and-upload-arduino-sketch"></a><span data-ttu-id="93831-244">Vytvoření a nahrání Arduino nákresu</span><span class="sxs-lookup"><span data-stu-id="93831-244">Build and upload Arduino sketch</span></span>

### <a name="install-required-library"></a><span data-ttu-id="93831-245">Nainstalujte požadované knihovny</span><span class="sxs-lookup"><span data-stu-id="93831-245">Install required library</span></span>

1. <span data-ttu-id="93831-246">Stiskněte klávesu `F1` nebo `Ctrl+Shift+P` (systému macOS: `Cmd+Shift+P`) tooinvoke příkaz palety a typ **Arduino** potom najděte a vyberte **Arduino: Správce knihovny**.</span><span class="sxs-lookup"><span data-stu-id="93831-246">Press `F1` or `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) tooinvoke command palette and type **Arduino** then find and select **Arduino: Library Manager**.</span></span>

2. <span data-ttu-id="93831-247">Vyhledejte `ArduinoJson` knihovnu a klikněte na **instalace**</span><span class="sxs-lookup"><span data-stu-id="93831-247">Search for `ArduinoJson` library and click **Install**</span></span>

### <a name="build-and-upload-hello-device-code"></a><span data-ttu-id="93831-248">Vytvoření a nahrání hello zařízení kódu</span><span class="sxs-lookup"><span data-stu-id="93831-248">Build and upload hello device code</span></span>

<span data-ttu-id="93831-249">Použití `Ctrl+P` (systému macOS: `Cmd+P`) toorun 'úloh zařízení – nahrání'.</span><span class="sxs-lookup"><span data-stu-id="93831-249">Use `Ctrl+P` (macOS: `Cmd+P`) toorun 'task device-upload'.</span></span> <span data-ttu-id="93831-250">Hello terminálu vyzve jste tooenter režim konfigurace.</span><span class="sxs-lookup"><span data-stu-id="93831-250">hello terminal will prompt you tooenter configuration mode.</span></span> <span data-ttu-id="93831-251">toodo Ano, podržte tlačítko A potom tlačítko Obnovit hello nabízení a verzi.</span><span class="sxs-lookup"><span data-stu-id="93831-251">toodo so, hold down button A, then push and release hello reset button.</span></span> <span data-ttu-id="93831-252">úvodní obrazovka se zobrazí "Konfigurace".</span><span class="sxs-lookup"><span data-stu-id="93831-252">hello screen will display 'Configuration'.</span></span> <span data-ttu-id="93831-253">Toto je tooset hello připojovací řetězec, který načte z kroku 'úloh cloudu provision'.</span><span class="sxs-lookup"><span data-stu-id="93831-253">This is tooset hello connection string that retrieves from 'task cloud-provision' step.</span></span>

<span data-ttu-id="93831-254">Potom se spustí, ověření a odeslání nákresu Arduino hello:</span><span class="sxs-lookup"><span data-stu-id="93831-254">Then it will start verifying and uploading hello Arduino sketch:</span></span>

![Mini-solution – zařízení – nahrání](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/device-upload.png)

<span data-ttu-id="93831-256">Hello DevKit bude restartujte počítač a spustit hello kódu.</span><span class="sxs-lookup"><span data-stu-id="93831-256">hello DevKit will reboot and start running hello code.</span></span>

## <a name="test-hello-project"></a><span data-ttu-id="93831-257">Test hello projektu</span><span class="sxs-lookup"><span data-stu-id="93831-257">Test hello project</span></span>

<span data-ttu-id="93831-258">V produktu VS Code klepněte na ikonu moduly power hello na hello stavovém řádku tooopen hello sériové monitorování.</span><span class="sxs-lookup"><span data-stu-id="93831-258">In VS Code, click hello power plug icon on hello status bar tooopen hello Serial Monitor.</span></span>

<span data-ttu-id="93831-259">Ukázková aplikace Hello pracuje správně, až uvidíte hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="93831-259">hello sample application is running successfully when you see hello following results:</span></span>

* <span data-ttu-id="93831-260">Dobrý den zobrazí sériové monitorování hello stejné informace jako obsah hello hello snímku obrazovky níže.</span><span class="sxs-lookup"><span data-stu-id="93831-260">hello Serial Monitor displays hello same information as hello content in hello screenshot below.</span></span>
* <span data-ttu-id="93831-261">Hello DIODU na MXChip IoT DevKit bliká.</span><span class="sxs-lookup"><span data-stu-id="93831-261">hello LED on MXChip IoT DevKit is blinking.</span></span>

![Závěrečný výstup v produktu VS Code](media/iot-hub-arduino-devkit-az3166-get-started/mini-solution/connect-iothub/result-serial-output.png)

## <a name="problems-and-feedback"></a><span data-ttu-id="93831-263">Problémy a zpětné vazby</span><span class="sxs-lookup"><span data-stu-id="93831-263">Problems and feedback</span></span>

<span data-ttu-id="93831-264">Můžete najít [nejčastější dotazy k](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) Pokud dojde k potížím nebo oslovení toous z hello kanály níže.</span><span class="sxs-lookup"><span data-stu-id="93831-264">You can find [FAQs](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) if you encounter problems or reach out toous from hello channels below.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93831-265">Další kroky</span><span class="sxs-lookup"><span data-stu-id="93831-265">Next steps</span></span>

<span data-ttu-id="93831-266">Úspěšně jste se připojili MXChip IoT DevKit tooyour IoT Hub a odeslané hello zachytit senzor data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="93831-266">You have successfully connected an MXChip IoT DevKit tooyour IoT Hub, and sent hello captured sensor data tooyour IoT hub.</span></span>

<span data-ttu-id="93831-267">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="93831-267">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

- [<span data-ttu-id="93831-268">Správa zasílání zpráv cloudových zařízení pomocí iothub-exploreru</span><span class="sxs-lookup"><span data-stu-id="93831-268">Manage cloud device messaging with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-explorer-cloud-device-messaging)
- [<span data-ttu-id="93831-269">Uložit IoT Hub zprávy tooAzure úložiště dat</span><span class="sxs-lookup"><span data-stu-id="93831-269">Save IoT Hub messages tooAzure data storage</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-store-data-in-azure-table-storage)
- [<span data-ttu-id="93831-270">Použít data snímačů v reálném čase toovisualize Power BI ze služby Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="93831-270">Use Power BI toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-power-bi)
- [<span data-ttu-id="93831-271">Použití Azure Web Apps toovisualize snímačů v reálném čase dat z Azure IoT Hub</span><span class="sxs-lookup"><span data-stu-id="93831-271">Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub</span></span>](https://docs.microsoft.com//azure/iot-hub/iot-hub-live-data-visualization-in-web-apps)
- [<span data-ttu-id="93831-272">Předpověď počasí pomocí hello senzor dat z centra IoT v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="93831-272">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-weather-forecast-machine-learning)
- [<span data-ttu-id="93831-273">Správa zařízení s využitím iothub-exploreru</span><span class="sxs-lookup"><span data-stu-id="93831-273">Device management with iothub-explorer</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-device-management-iothub-explorer)
- [<span data-ttu-id="93831-274">Vzdálené monitorování a oznámení s využitím Logic Apps</span><span class="sxs-lookup"><span data-stu-id="93831-274">Remote monitoring and notifications with Logic Apps</span></span>](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitoring-notifications-with-azure-logic-apps)