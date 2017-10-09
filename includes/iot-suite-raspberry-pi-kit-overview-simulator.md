## <a name="overview"></a><span data-ttu-id="2e6b4-101">Přehled</span><span class="sxs-lookup"><span data-stu-id="2e6b4-101">Overview</span></span>

<span data-ttu-id="2e6b4-102">V tomto kurzu dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="2e6b4-102">In this tutorial, you complete hello following steps:</span></span>

- <span data-ttu-id="2e6b4-103">Nasaďte instanci hello vzdálené monitorování předkonfigurované řešení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-103">Deploy an instance of hello remote monitoring preconfigured solution tooyour Azure subscription.</span></span> <span data-ttu-id="2e6b4-104">Tento krok automaticky nasadí a nakonfiguruje více služeb Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-104">This step automatically deploys and configures multiple Azure services.</span></span>
- <span data-ttu-id="2e6b4-105">Nastavte vaše zařízení toocommunicate s počítačem a hello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-105">Set up your device toocommunicate with your computer and hello remote monitoring solution.</span></span>
- <span data-ttu-id="2e6b4-106">Aktualizujte hello ukázka zařízení kódu tooconnect toohello řešení vzdáleného monitorování a odeslat simulovanou telemetrii, která můžete zobrazit na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-106">Update hello sample device code tooconnect toohello remote monitoring solution, and send simulated telemetry that you can view on hello solution dashboard.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e6b4-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e6b4-107">Prerequisites</span></span>

<span data-ttu-id="2e6b4-108">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-108">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="2e6b4-109">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-109">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2e6b4-110">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="2e6b4-110">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="2e6b4-111">Požadovaný software</span><span class="sxs-lookup"><span data-stu-id="2e6b4-111">Required software</span></span>

<span data-ttu-id="2e6b4-112">Musíte klient SSH na vaše tooenable stolní počítače můžete tooremotely hello se příkaz access řádek na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-112">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="2e6b4-113">Windows nezahrnuje klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-113">Windows does not include an SSH client.</span></span> <span data-ttu-id="2e6b4-114">Doporučujeme používat [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="2e6b4-114">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="2e6b4-115">Většina Linuxových distribucích a Mac OS, obsahují hello nástroj příkazového řádku SSH.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-115">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="2e6b4-116">Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="2e6b4-116">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="2e6b4-117">Požadovaný hardware</span><span class="sxs-lookup"><span data-stu-id="2e6b4-117">Required hardware</span></span>

<span data-ttu-id="2e6b4-118">Stolní počítač tooenable tooconnect můžete vzdáleně toohello příkazového řádku na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-118">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="2e6b4-119">[Microsoft IoT Starter Kit malin pí 3] [ lnk-starter-kits] nebo ekvivalentní součásti.</span><span class="sxs-lookup"><span data-stu-id="2e6b4-119">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="2e6b4-120">Tento kurz používá hello následujících položek z hello kit:</span><span class="sxs-lookup"><span data-stu-id="2e6b4-120">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="2e6b4-121">Malinová pí 3</span><span class="sxs-lookup"><span data-stu-id="2e6b4-121">Raspberry Pi 3</span></span>
- <span data-ttu-id="2e6b4-122">Karta MicroSD (s NOOBS)</span><span class="sxs-lookup"><span data-stu-id="2e6b4-122">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="2e6b4-123">Kabelu USB malé</span><span class="sxs-lookup"><span data-stu-id="2e6b4-123">A USB Mini cable</span></span>
- <span data-ttu-id="2e6b4-124">Kabel Ethernet</span><span class="sxs-lookup"><span data-stu-id="2e6b4-124">An Ethernet cable</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/