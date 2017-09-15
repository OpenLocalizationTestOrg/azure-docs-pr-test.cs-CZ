## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="9947a-101">Příprava vašeho Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="9947a-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="9947a-102">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="9947a-102">Install Raspbian</span></span>

<span data-ttu-id="9947a-103">Pokud používáte vaše platformy malin poprvé, musíte nainstalovat operační systém Raspbian pomocí NOOBS na kartu SD. součástí sady.</span><span class="sxs-lookup"><span data-stu-id="9947a-103">If this is the first time you are using your Raspberry Pi, you need to install the Raspbian operating system using NOOBS on the SD card included in the kit.</span></span> <span data-ttu-id="9947a-104">[Malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje postup instalace operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-104">The [Raspberry Pi Software Guide][lnk-install-raspbian] describes how to install an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="9947a-105">Tento kurz předpokládá, že jste nainstalovali Raspbian operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-105">This tutorial assumes you have installed the Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="9947a-106">Používání SD karet součástí [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již má nainstalované NOOBS.</span><span class="sxs-lookup"><span data-stu-id="9947a-106">The SD card included in the [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="9947a-107">Můžete spustit pí malin z této karty a zvolit instalaci operačního systému Raspbian.</span><span class="sxs-lookup"><span data-stu-id="9947a-107">You can boot the Raspberry Pi from this card and choose to install the Raspbian OS.</span></span>

<span data-ttu-id="9947a-108">Chcete-li dokončit nastavení hardwaru, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="9947a-108">To complete the hardware setup, you need to:</span></span>

- <span data-ttu-id="9947a-109">Připojte vaše platformy malin k napájení součástí sady.</span><span class="sxs-lookup"><span data-stu-id="9947a-109">Connect your Raspberry Pi to the power supply included in the kit.</span></span>
- <span data-ttu-id="9947a-110">Vaše platformy malin připojte k síti pomocí kabelu Ethernet, který je součástí vaší sady.</span><span class="sxs-lookup"><span data-stu-id="9947a-110">Connect your Raspberry Pi to your network using the Ethernet cable included in your kit.</span></span> <span data-ttu-id="9947a-111">Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="9947a-112">Teď jste dokončili nastavení hardwaru vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-112">You have now completed the hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="9947a-113">Přihlaste se a přístup k terminálu</span><span class="sxs-lookup"><span data-stu-id="9947a-113">Sign in and access the terminal</span></span>

<span data-ttu-id="9947a-114">Máte dvě možnosti pro přístup k Terminálové prostředí na vaše malin platformy:</span><span class="sxs-lookup"><span data-stu-id="9947a-114">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="9947a-115">Pokud máte klávesnici a monitorování, které jsou připojené k vaší malin platformy, můžete použít Raspbian grafického uživatelského rozhraní pro přístup k okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="9947a-115">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

- <span data-ttu-id="9947a-116">Přístup na příkazovém řádku vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="9947a-116">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="9947a-117">Použijte okno terminálu v grafickém uživatelském rozhraní</span><span class="sxs-lookup"><span data-stu-id="9947a-117">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="9947a-118">Výchozí pověření pro Raspbian jsou uživatelské jméno **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="9947a-118">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="9947a-119">Na hlavním panelu v grafickém uživatelském rozhraní, můžete spustit **Terminálové** nástroj pomocí ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="9947a-119">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="9947a-120">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="9947a-120">Sign in with SSH</span></span>

<span data-ttu-id="9947a-121">SSH můžete použít pro příkazového řádku přístup k vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-121">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="9947a-122">Článek [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje postup konfigurace SSH na vaše malin platformy a jak se připojit z [Windows] [ lnk-ssh-windows] nebo [ Linux & Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="9947a-122">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="9947a-123">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="9947a-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="9947a-124">Volitelné: Sdílené složky na vaše malin platformy</span><span class="sxs-lookup"><span data-stu-id="9947a-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="9947a-125">Volitelně můžete sdílet složky na vaše malin pí s prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="9947a-125">Optionally, you may want to share a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="9947a-126">Sdílení složky vám umožní použít upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) Chcete-li upravit soubory na vaše malin platformy místo použití `nano` nebo `vi`.</span><span class="sxs-lookup"><span data-stu-id="9947a-126">Sharing a folder enables you to use your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) to edit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="9947a-127">Sdílení složky s Windows, konfigurace serveru Samba na malin pí.</span><span class="sxs-lookup"><span data-stu-id="9947a-127">To share a folder with Windows, configure a Samba server on the Raspberry Pi.</span></span> <span data-ttu-id="9947a-128">Můžete taky použít integrované [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.</span><span class="sxs-lookup"><span data-stu-id="9947a-128">Alternatively, use the built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/