## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="dd974-101">Příprava vašeho Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="dd974-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="dd974-102">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="dd974-102">Install Raspbian</span></span>

<span data-ttu-id="dd974-103">Pokud používáte vaše platformy malin poprvé, musíte nainstalovat operační systém Raspbian pomocí NOOBS na kartu SD. součástí sady.</span><span class="sxs-lookup"><span data-stu-id="dd974-103">If this is the first time you are using your Raspberry Pi, you need to install the Raspbian operating system using NOOBS on the SD card included in the kit.</span></span> <span data-ttu-id="dd974-104">[Malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje postup instalace operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-104">The [Raspberry Pi Software Guide][lnk-install-raspbian] describes how to install an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="dd974-105">Tento kurz předpokládá, že jste nainstalovali Raspbian operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-105">This tutorial assumes you have installed the Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="dd974-106">Používání SD karet součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již má nainstalované NOOBS.</span><span class="sxs-lookup"><span data-stu-id="dd974-106">The SD card included in the [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="dd974-107">Můžete spustit pí malin z této karty a zvolit instalaci operačního systému Raspbian.</span><span class="sxs-lookup"><span data-stu-id="dd974-107">You can boot the Raspberry Pi from this card and choose to install the Raspbian OS.</span></span>

### <a name="set-up-the-hardware"></a><span data-ttu-id="dd974-108">Nastavení hardwaru</span><span class="sxs-lookup"><span data-stu-id="dd974-108">Set up the hardware</span></span>

<span data-ttu-id="dd974-109">Tento kurz používá BME280 senzoru součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] ke generování telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="dd974-109">This tutorial uses the BME280 sensor included in the [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] to generate telemetry data.</span></span> <span data-ttu-id="dd974-110">Používá DIODU označíte, když pí malin zpracovává volání metody na řídicím panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="dd974-110">It uses an LED to indicate when the Raspberry Pi processes a method invocation from the solution dashboard.</span></span>

<span data-ttu-id="dd974-111">Součásti na panelu chléb jsou:</span><span class="sxs-lookup"><span data-stu-id="dd974-111">The components on the bread board are:</span></span>

- <span data-ttu-id="dd974-112">Red DIODU</span><span class="sxs-lookup"><span data-stu-id="dd974-112">Red LED</span></span>
- <span data-ttu-id="dd974-113">220 Ohm odpor (červená, red menších.)</span><span class="sxs-lookup"><span data-stu-id="dd974-113">220-Ohm resistor (red, red, brown)</span></span>
- <span data-ttu-id="dd974-114">Senzor BME280</span><span class="sxs-lookup"><span data-stu-id="dd974-114">BME280 sensor</span></span>

<span data-ttu-id="dd974-115">Následující diagram ukazuje, jak připojit hardwaru:</span><span class="sxs-lookup"><span data-stu-id="dd974-115">The following diagram shows how to connect your hardware:</span></span>

![Nastavení hardwaru pro malin platformy][img-connection-diagram]

<span data-ttu-id="dd974-117">Následující tabulka shrnuje připojení z platformy malin součástí na breadboard:</span><span class="sxs-lookup"><span data-stu-id="dd974-117">The following table summarizes the connections from the Raspberry Pi to the components on the breadboard:</span></span>

| <span data-ttu-id="dd974-118">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="dd974-118">Raspberry Pi</span></span>            | <span data-ttu-id="dd974-119">Breadboard</span><span class="sxs-lookup"><span data-stu-id="dd974-119">Breadboard</span></span>             |<span data-ttu-id="dd974-120">Barva</span><span class="sxs-lookup"><span data-stu-id="dd974-120">Color</span></span>         |
| ----------------------- | ---------------------- | ------------- |
| <span data-ttu-id="dd974-121">ZEM (Pin 14)</span><span class="sxs-lookup"><span data-stu-id="dd974-121">GND (Pin 14)</span></span>            | <span data-ttu-id="dd974-122">Indikátor - sunout pin (18A)</span><span class="sxs-lookup"><span data-stu-id="dd974-122">LED -ve pin (18A)</span></span>      | <span data-ttu-id="dd974-123">Fialová</span><span class="sxs-lookup"><span data-stu-id="dd974-123">Purple</span></span>          |
| <span data-ttu-id="dd974-124">GPCLK0 (Pin 7)</span><span class="sxs-lookup"><span data-stu-id="dd974-124">GPCLK0 (Pin 7)</span></span>          | <span data-ttu-id="dd974-125">Odpor (25A)</span><span class="sxs-lookup"><span data-stu-id="dd974-125">Resistor (25A)</span></span>         | <span data-ttu-id="dd974-126">Orange</span><span class="sxs-lookup"><span data-stu-id="dd974-126">Orange</span></span>          |
| <span data-ttu-id="dd974-127">SPI_CE0 (Pin 24)</span><span class="sxs-lookup"><span data-stu-id="dd974-127">SPI_CE0 (Pin 24)</span></span>        | <span data-ttu-id="dd974-128">CS (39A)</span><span class="sxs-lookup"><span data-stu-id="dd974-128">CS (39A)</span></span>               | <span data-ttu-id="dd974-129">Modrá</span><span class="sxs-lookup"><span data-stu-id="dd974-129">Blue</span></span>          |
| <span data-ttu-id="dd974-130">SPI_SCLK (Pin 23)</span><span class="sxs-lookup"><span data-stu-id="dd974-130">SPI_SCLK (Pin 23)</span></span>       | <span data-ttu-id="dd974-131">SCK (36A)</span><span class="sxs-lookup"><span data-stu-id="dd974-131">SCK (36A)</span></span>              | <span data-ttu-id="dd974-132">Žlutá</span><span class="sxs-lookup"><span data-stu-id="dd974-132">Yellow</span></span>        |
| <span data-ttu-id="dd974-133">SPI_MISO (Pin 21)</span><span class="sxs-lookup"><span data-stu-id="dd974-133">SPI_MISO (Pin 21)</span></span>       | <span data-ttu-id="dd974-134">SDO (37A)</span><span class="sxs-lookup"><span data-stu-id="dd974-134">SDO (37A)</span></span>              | <span data-ttu-id="dd974-135">Bílá</span><span class="sxs-lookup"><span data-stu-id="dd974-135">White</span></span>         |
| <span data-ttu-id="dd974-136">SPI_MOSI (Pin 19)</span><span class="sxs-lookup"><span data-stu-id="dd974-136">SPI_MOSI (Pin 19)</span></span>       | <span data-ttu-id="dd974-137">SDI (38A)</span><span class="sxs-lookup"><span data-stu-id="dd974-137">SDI (38A)</span></span>              | <span data-ttu-id="dd974-138">Zelená</span><span class="sxs-lookup"><span data-stu-id="dd974-138">Green</span></span>         |
| <span data-ttu-id="dd974-139">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="dd974-139">GND (Pin 6)</span></span>             | <span data-ttu-id="dd974-140">ZEM (35A)</span><span class="sxs-lookup"><span data-stu-id="dd974-140">GND (35A)</span></span>              | <span data-ttu-id="dd974-141">Černá</span><span class="sxs-lookup"><span data-stu-id="dd974-141">Black</span></span>         |
| <span data-ttu-id="dd974-142">3.3 V (Pin 1)</span><span class="sxs-lookup"><span data-stu-id="dd974-142">3.3 V (Pin 1)</span></span>           | <span data-ttu-id="dd974-143">3Vo (34A)</span><span class="sxs-lookup"><span data-stu-id="dd974-143">3Vo (34A)</span></span>              | <span data-ttu-id="dd974-144">Červená</span><span class="sxs-lookup"><span data-stu-id="dd974-144">Red</span></span>           |

<span data-ttu-id="dd974-145">Chcete-li dokončit nastavení hardwaru, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="dd974-145">To complete the hardware setup, you need to:</span></span>

- <span data-ttu-id="dd974-146">Připojte vaše platformy malin k napájení součástí sady.</span><span class="sxs-lookup"><span data-stu-id="dd974-146">Connect your Raspberry Pi to the power supply included in the kit.</span></span>
- <span data-ttu-id="dd974-147">Vaše platformy malin připojte k síti pomocí kabelu Ethernet, který je součástí vaší sady.</span><span class="sxs-lookup"><span data-stu-id="dd974-147">Connect your Raspberry Pi to your network using the Ethernet cable included in your kit.</span></span> <span data-ttu-id="dd974-148">Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-148">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="dd974-149">Teď jste dokončili nastavení hardwaru vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-149">You have now completed the hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="dd974-150">Přihlaste se a přístup k terminálu</span><span class="sxs-lookup"><span data-stu-id="dd974-150">Sign in and access the terminal</span></span>

<span data-ttu-id="dd974-151">Máte dvě možnosti pro přístup k Terminálové prostředí na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="dd974-151">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="dd974-152">Pokud máte klávesnici a monitorování, které jsou připojené k vaší malin platformy, můžete použít Raspbian grafického uživatelského rozhraní pro přístup k okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="dd974-152">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

- <span data-ttu-id="dd974-153">Přístup na příkazovém řádku vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="dd974-153">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="dd974-154">Použijte okno terminálu v grafickém uživatelském rozhraní</span><span class="sxs-lookup"><span data-stu-id="dd974-154">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="dd974-155">Výchozí pověření pro Raspbian jsou uživatelské jméno **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="dd974-155">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="dd974-156">Na hlavním panelu v grafickém uživatelském rozhraní, můžete spustit **Terminálové** nástroj pomocí ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="dd974-156">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="dd974-157">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="dd974-157">Sign in with SSH</span></span>

<span data-ttu-id="dd974-158">SSH můžete použít pro příkazového řádku přístup k vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-158">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="dd974-159">Článek [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje postup konfigurace SSH na vaše malin platformy a jak se připojit z [Windows] [ lnk-ssh-windows] nebo [ Linux & Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="dd974-159">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="dd974-160">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="dd974-160">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="dd974-161">Volitelné: Sdílené složky na vaše malin platformy</span><span class="sxs-lookup"><span data-stu-id="dd974-161">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="dd974-162">Volitelně můžete sdílet složky na vaše malin pí s prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="dd974-162">Optionally, you may want to share a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="dd974-163">Sdílení složky vám umožní použít upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) Chcete-li upravit soubory na vaše malin platformy místo použití `nano` nebo `vi`.</span><span class="sxs-lookup"><span data-stu-id="dd974-163">Sharing a folder enables you to use your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) to edit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="dd974-164">Sdílení složky s Windows, konfigurace serveru Samba na malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-164">To share a folder with Windows, configure a Samba server on the Raspberry Pi.</span></span> <span data-ttu-id="dd974-165">Můžete taky použít integrované [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.</span><span class="sxs-lookup"><span data-stu-id="dd974-165">Alternatively, use the built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

### <a name="enable-spi"></a><span data-ttu-id="dd974-166">Povolit SPI</span><span class="sxs-lookup"><span data-stu-id="dd974-166">Enable SPI</span></span>

<span data-ttu-id="dd974-167">Než spustíte ukázkovou aplikaci, je nutné povolit sběrnici sériové periferní rozhraní (SPI) na malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-167">Before you can run the sample application, you must enable the Serial Peripheral Interface (SPI) bus on the Raspberry Pi.</span></span> <span data-ttu-id="dd974-168">Malin platformy zařízení senzor BME280 komunikuje přes sběrnici SPI.</span><span class="sxs-lookup"><span data-stu-id="dd974-168">The Raspberry Pi communicates with the BME280 sensor device over the SPI bus.</span></span> <span data-ttu-id="dd974-169">Chcete-li upravit konfigurační soubor, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dd974-169">Use the following command to edit the configuration file:</span></span>

```sh
sudo nano /boot/config.txt
```

<span data-ttu-id="dd974-170">Vyhledejte řádek:</span><span class="sxs-lookup"><span data-stu-id="dd974-170">Find the line:</span></span>

`#dtparam=spi=on`

- <span data-ttu-id="dd974-171">Chcete-li Odkomentujte řádek, odstraňte `#` na začátku.</span><span class="sxs-lookup"><span data-stu-id="dd974-171">To uncomment the line, delete the `#` at the start.</span></span>
- <span data-ttu-id="dd974-172">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="dd974-172">Save your changes (**Ctrl-O**, **Enter**) and exit the editor (**Ctrl-X**).</span></span>
- <span data-ttu-id="dd974-173">Pokud chcete povolit SPI, restartování malin pí.</span><span class="sxs-lookup"><span data-stu-id="dd974-173">To enable SPI, reboot the Raspberry Pi.</span></span> <span data-ttu-id="dd974-174">Restartování odpojí terminálu, musíte se přihlásit znovu, pokud restartování malin platformy:</span><span class="sxs-lookup"><span data-stu-id="dd974-174">Rebooting disconnects the terminal, you need to sign in again when the Raspberry Pi restarts:</span></span>

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/