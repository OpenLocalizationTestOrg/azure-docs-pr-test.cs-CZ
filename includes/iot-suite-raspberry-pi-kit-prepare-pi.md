## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="ae905-101">Příprava vašeho Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="ae905-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="ae905-102">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="ae905-102">Install Raspbian</span></span>

<span data-ttu-id="ae905-103">Pokud je to hello poprvé použijete vaše malin platformy, je nutné tooinstall hello Raspbian operačního systému pomocí NOOBS na kartě SD hello součástí hello kit.</span><span class="sxs-lookup"><span data-stu-id="ae905-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="ae905-104">Hello [malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje, jak tooinstall operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="ae905-105">Tento kurz předpokládá, že jste nainstalovali hello Raspbian operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="ae905-106">Hello SD karty součástí hello [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již NOOBS nainstalována.</span><span class="sxs-lookup"><span data-stu-id="ae905-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="ae905-107">Můžete spustit hello malin pí z této karty a zvolte tooinstall hello Raspbian operačního systému.</span><span class="sxs-lookup"><span data-stu-id="ae905-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

### <a name="set-up-hello-hardware"></a><span data-ttu-id="ae905-108">Nastavení hardwaru hello</span><span class="sxs-lookup"><span data-stu-id="ae905-108">Set up hello hardware</span></span>

<span data-ttu-id="ae905-109">Tento kurz používá hello BME280 senzor součástí hello [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] toogenerate telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="ae905-109">This tutorial uses hello BME280 sensor included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] toogenerate telemetry data.</span></span> <span data-ttu-id="ae905-110">Když hello malin pí zpracovává volání metody z řídicí panel řešení hello používá tooindicate Indikátor.</span><span class="sxs-lookup"><span data-stu-id="ae905-110">It uses an LED tooindicate when hello Raspberry Pi processes a method invocation from hello solution dashboard.</span></span>

<span data-ttu-id="ae905-111">součásti Hello na panelu chléb hello jsou:</span><span class="sxs-lookup"><span data-stu-id="ae905-111">hello components on hello bread board are:</span></span>

- <span data-ttu-id="ae905-112">Red DIODU</span><span class="sxs-lookup"><span data-stu-id="ae905-112">Red LED</span></span>
- <span data-ttu-id="ae905-113">220 Ohm odpor (červená, red menších.)</span><span class="sxs-lookup"><span data-stu-id="ae905-113">220-Ohm resistor (red, red, brown)</span></span>
- <span data-ttu-id="ae905-114">Senzor BME280</span><span class="sxs-lookup"><span data-stu-id="ae905-114">BME280 sensor</span></span>

<span data-ttu-id="ae905-115">Hello následující obrázek ukazuje, jak tooconnect hardwaru:</span><span class="sxs-lookup"><span data-stu-id="ae905-115">hello following diagram shows how tooconnect your hardware:</span></span>

![Nastavení hardwaru pro malin platformy][img-connection-diagram]

<span data-ttu-id="ae905-117">Hello následující tabulka shrnuje hello připojení z hello malin pí toohello součásti na hello breadboard:</span><span class="sxs-lookup"><span data-stu-id="ae905-117">hello following table summarizes hello connections from hello Raspberry Pi toohello components on hello breadboard:</span></span>

| <span data-ttu-id="ae905-118">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="ae905-118">Raspberry Pi</span></span>            | <span data-ttu-id="ae905-119">Breadboard</span><span class="sxs-lookup"><span data-stu-id="ae905-119">Breadboard</span></span>             |<span data-ttu-id="ae905-120">Barva</span><span class="sxs-lookup"><span data-stu-id="ae905-120">Color</span></span>         |
| ----------------------- | ---------------------- | ------------- |
| <span data-ttu-id="ae905-121">ZEM (Pin 14)</span><span class="sxs-lookup"><span data-stu-id="ae905-121">GND (Pin 14)</span></span>            | <span data-ttu-id="ae905-122">Indikátor - sunout pin (18A)</span><span class="sxs-lookup"><span data-stu-id="ae905-122">LED -ve pin (18A)</span></span>      | <span data-ttu-id="ae905-123">Fialová</span><span class="sxs-lookup"><span data-stu-id="ae905-123">Purple</span></span>          |
| <span data-ttu-id="ae905-124">GPCLK0 (Pin 7)</span><span class="sxs-lookup"><span data-stu-id="ae905-124">GPCLK0 (Pin 7)</span></span>          | <span data-ttu-id="ae905-125">Odpor (25A)</span><span class="sxs-lookup"><span data-stu-id="ae905-125">Resistor (25A)</span></span>         | <span data-ttu-id="ae905-126">Orange</span><span class="sxs-lookup"><span data-stu-id="ae905-126">Orange</span></span>          |
| <span data-ttu-id="ae905-127">SPI_CE0 (Pin 24)</span><span class="sxs-lookup"><span data-stu-id="ae905-127">SPI_CE0 (Pin 24)</span></span>        | <span data-ttu-id="ae905-128">CS (39A)</span><span class="sxs-lookup"><span data-stu-id="ae905-128">CS (39A)</span></span>               | <span data-ttu-id="ae905-129">Modrá</span><span class="sxs-lookup"><span data-stu-id="ae905-129">Blue</span></span>          |
| <span data-ttu-id="ae905-130">SPI_SCLK (Pin 23)</span><span class="sxs-lookup"><span data-stu-id="ae905-130">SPI_SCLK (Pin 23)</span></span>       | <span data-ttu-id="ae905-131">SCK (36A)</span><span class="sxs-lookup"><span data-stu-id="ae905-131">SCK (36A)</span></span>              | <span data-ttu-id="ae905-132">Žlutá</span><span class="sxs-lookup"><span data-stu-id="ae905-132">Yellow</span></span>        |
| <span data-ttu-id="ae905-133">SPI_MISO (Pin 21)</span><span class="sxs-lookup"><span data-stu-id="ae905-133">SPI_MISO (Pin 21)</span></span>       | <span data-ttu-id="ae905-134">SDO (37A)</span><span class="sxs-lookup"><span data-stu-id="ae905-134">SDO (37A)</span></span>              | <span data-ttu-id="ae905-135">Bílá</span><span class="sxs-lookup"><span data-stu-id="ae905-135">White</span></span>         |
| <span data-ttu-id="ae905-136">SPI_MOSI (Pin 19)</span><span class="sxs-lookup"><span data-stu-id="ae905-136">SPI_MOSI (Pin 19)</span></span>       | <span data-ttu-id="ae905-137">SDI (38A)</span><span class="sxs-lookup"><span data-stu-id="ae905-137">SDI (38A)</span></span>              | <span data-ttu-id="ae905-138">Zelená</span><span class="sxs-lookup"><span data-stu-id="ae905-138">Green</span></span>         |
| <span data-ttu-id="ae905-139">ZEM (Pin 6)</span><span class="sxs-lookup"><span data-stu-id="ae905-139">GND (Pin 6)</span></span>             | <span data-ttu-id="ae905-140">ZEM (35A)</span><span class="sxs-lookup"><span data-stu-id="ae905-140">GND (35A)</span></span>              | <span data-ttu-id="ae905-141">Černá</span><span class="sxs-lookup"><span data-stu-id="ae905-141">Black</span></span>         |
| <span data-ttu-id="ae905-142">3.3 V (Pin 1)</span><span class="sxs-lookup"><span data-stu-id="ae905-142">3.3 V (Pin 1)</span></span>           | <span data-ttu-id="ae905-143">3Vo (34A)</span><span class="sxs-lookup"><span data-stu-id="ae905-143">3Vo (34A)</span></span>              | <span data-ttu-id="ae905-144">Červená</span><span class="sxs-lookup"><span data-stu-id="ae905-144">Red</span></span>           |

<span data-ttu-id="ae905-145">toocomplete hello hardwaru Instalační program, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ae905-145">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="ae905-146">Připojte vaše malin pí toohello zdroj napájení součástí hello kit.</span><span class="sxs-lookup"><span data-stu-id="ae905-146">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="ae905-147">Propojení vaší sítě tooyour malin pí pomocí kabelu Ethernet hello součástí vaší sady.</span><span class="sxs-lookup"><span data-stu-id="ae905-147">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="ae905-148">Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-148">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="ae905-149">Teď jste dokončili nastavení hardwaru hello vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-149">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="ae905-150">Přihlaste se a přístup k Terminálové hello</span><span class="sxs-lookup"><span data-stu-id="ae905-150">Sign in and access hello terminal</span></span>

<span data-ttu-id="ae905-151">Dvě možnosti tooaccess máte na vaše platformy malin terminálu prostředí:</span><span class="sxs-lookup"><span data-stu-id="ae905-151">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="ae905-152">Pokud máte klávesnici a sledování připojených tooyour malin platformy, můžete použít grafické uživatelské rozhraní Raspbian tooaccess hello okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="ae905-152">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="ae905-153">Přístup hello příkazového řádku na vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="ae905-153">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="ae905-154">Použijte okno terminálu v hello grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="ae905-154">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="ae905-155">uživatelské jméno jsou Hello výchozí pověření pro Raspbian **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="ae905-155">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="ae905-156">Hello hlavním panelu v hello grafického uživatelského rozhraní, můžete spustit hello **Terminálové** nástroj pomocí hello ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="ae905-156">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="ae905-157">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="ae905-157">Sign in with SSH</span></span>

<span data-ttu-id="ae905-158">SSH můžete použít pro přístup přes příkazový řádek tooyour malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-158">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="ae905-159">článek Hello [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje, jak tooconfigure SSH na vaše malin platformy a jak tooconnect z [Windows] [ lnk-ssh-windows] nebo [Operačního systému Linux & Mac][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="ae905-159">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="ae905-160">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="ae905-160">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="ae905-161">Volitelné: Sdílené složky na vaše malin platformy</span><span class="sxs-lookup"><span data-stu-id="ae905-161">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="ae905-162">Volitelně můžete tooshare do složky na vaše malin pí s prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="ae905-162">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="ae905-163">Sdílení složky umožňuje vám toouse upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) tooedit soubory na vaše malin platformy místo použití `nano` nebo `vi`.</span><span class="sxs-lookup"><span data-stu-id="ae905-163">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="ae905-164">tooshare složka s Windows, konfigurace serveru Samba hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-164">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="ae905-165">Můžete taky použít integrované hello [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.</span><span class="sxs-lookup"><span data-stu-id="ae905-165">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

### <a name="enable-spi"></a><span data-ttu-id="ae905-166">Povolit SPI</span><span class="sxs-lookup"><span data-stu-id="ae905-166">Enable SPI</span></span>

<span data-ttu-id="ae905-167">Před spuštěním hello ukázkovou aplikaci, je nutné povolit hello sériové periferní rozhraní (SPI) sběrnice na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-167">Before you can run hello sample application, you must enable hello Serial Peripheral Interface (SPI) bus on hello Raspberry Pi.</span></span> <span data-ttu-id="ae905-168">Hello malin platformy hello BME280 senzor zařízení komunikuje přes hello SPI sběrnice.</span><span class="sxs-lookup"><span data-stu-id="ae905-168">hello Raspberry Pi communicates with hello BME280 sensor device over hello SPI bus.</span></span> <span data-ttu-id="ae905-169">Použijte následující příkaz tooedit hello konfigurační soubor hello:</span><span class="sxs-lookup"><span data-stu-id="ae905-169">Use hello following command tooedit hello configuration file:</span></span>

```sh
sudo nano /boot/config.txt
```

<span data-ttu-id="ae905-170">Najděte hello řádek:</span><span class="sxs-lookup"><span data-stu-id="ae905-170">Find hello line:</span></span>

`#dtparam=spi=on`

- <span data-ttu-id="ae905-171">toouncomment hello řádku odstranění hello `#` při spuštění hello.</span><span class="sxs-lookup"><span data-stu-id="ae905-171">toouncomment hello line, delete hello `#` at hello start.</span></span>
- <span data-ttu-id="ae905-172">Uložte změny (**Ctrl-O**, **Enter**) a ukončete editor hello (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="ae905-172">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>
- <span data-ttu-id="ae905-173">tooenable SPI, restartování hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="ae905-173">tooenable SPI, reboot hello Raspberry Pi.</span></span> <span data-ttu-id="ae905-174">Restartování odpojí hello terminálu, musíte toosign v znovu, pokud restartování hello malin platformy:</span><span class="sxs-lookup"><span data-stu-id="ae905-174">Rebooting disconnects hello terminal, you need toosign in again when hello Raspberry Pi restarts:</span></span>

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