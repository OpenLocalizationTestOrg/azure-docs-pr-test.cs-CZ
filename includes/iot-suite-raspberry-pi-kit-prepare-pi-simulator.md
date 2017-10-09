## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="c8ea4-101">Příprava vašeho Malinová platformy</span><span class="sxs-lookup"><span data-stu-id="c8ea4-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="c8ea4-102">Nainstalujte Raspbian</span><span class="sxs-lookup"><span data-stu-id="c8ea4-102">Install Raspbian</span></span>

<span data-ttu-id="c8ea4-103">Pokud je to hello poprvé použijete vaše malin platformy, je nutné tooinstall hello Raspbian operačního systému pomocí NOOBS na kartě SD hello součástí hello kit.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="c8ea4-104">Hello [malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje, jak tooinstall operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="c8ea4-105">Tento kurz předpokládá, že jste nainstalovali hello Raspbian operačního systému na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="c8ea4-106">Hello SD karty součástí hello [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již NOOBS nainstalována.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="c8ea4-107">Můžete spustit hello malin pí z této karty a zvolte tooinstall hello Raspbian operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

<span data-ttu-id="c8ea4-108">toocomplete hello hardwaru Instalační program, potřebujete:</span><span class="sxs-lookup"><span data-stu-id="c8ea4-108">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="c8ea4-109">Připojte vaše malin pí toohello zdroj napájení součástí hello kit.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-109">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="c8ea4-110">Propojení vaší sítě tooyour malin pí pomocí kabelu Ethernet hello součástí vaší sady.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-110">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="c8ea4-111">Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-111">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="c8ea4-112">Teď jste dokončili nastavení hardwaru hello vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-112">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="c8ea4-113">Přihlaste se a přístup k Terminálové hello</span><span class="sxs-lookup"><span data-stu-id="c8ea4-113">Sign in and access hello terminal</span></span>

<span data-ttu-id="c8ea4-114">Dvě možnosti tooaccess máte na vaše platformy malin terminálu prostředí:</span><span class="sxs-lookup"><span data-stu-id="c8ea4-114">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="c8ea4-115">Pokud máte klávesnici a sledování připojených tooyour malin platformy, můžete použít grafické uživatelské rozhraní Raspbian tooaccess hello okno terminálu.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-115">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="c8ea4-116">Přístup hello příkazového řádku na vaší malin pí pomocí protokolu SSH ze stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-116">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="c8ea4-117">Použijte okno terminálu v hello grafického uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="c8ea4-117">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="c8ea4-118">uživatelské jméno jsou Hello výchozí pověření pro Raspbian **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-118">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="c8ea4-119">Hello hlavním panelu v hello grafického uživatelského rozhraní, můžete spustit hello **Terminálové** nástroj pomocí hello ikonu, která vypadá jako monitorování.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-119">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="c8ea4-120">Přihlaste se pomocí protokolu SSH</span><span class="sxs-lookup"><span data-stu-id="c8ea4-120">Sign in with SSH</span></span>

<span data-ttu-id="c8ea4-121">SSH můžete použít pro přístup přes příkazový řádek tooyour malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-121">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="c8ea4-122">článek Hello [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje, jak tooconfigure SSH na vaše malin platformy a jak tooconnect z [Windows] [ lnk-ssh-windows] nebo [Operačního systému Linux & Mac][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="c8ea4-122">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="c8ea4-123">Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-123">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="c8ea4-124">Volitelné: Sdílené složky na vaše malin platformy</span><span class="sxs-lookup"><span data-stu-id="c8ea4-124">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="c8ea4-125">Volitelně můžete tooshare do složky na vaše malin pí s prostředí plochy.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-125">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="c8ea4-126">Sdílení složky umožňuje vám toouse upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) tooedit soubory na vaše malin platformy místo použití `nano` nebo `vi`.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-126">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="c8ea4-127">tooshare složka s Windows, konfigurace serveru Samba hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-127">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="c8ea4-128">Můžete taky použít integrované hello [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.</span><span class="sxs-lookup"><span data-stu-id="c8ea4-128">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/