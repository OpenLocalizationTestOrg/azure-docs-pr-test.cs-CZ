## <a name="prerequisites"></a><span data-ttu-id="b8a55-101">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8a55-101">Prerequisites</span></span>

<span data-ttu-id="b8a55-102">toocomplete tohoto kurzu potřebujete aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="b8a55-102">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="b8a55-103">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="b8a55-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b8a55-104">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="b8a55-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="b8a55-105">Požadovaný software</span><span class="sxs-lookup"><span data-stu-id="b8a55-105">Required software</span></span>

<span data-ttu-id="b8a55-106">Musíte klient SSH na vaše tooenable stolní počítače můžete tooremotely hello se příkaz access řádek na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="b8a55-106">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="b8a55-107">Windows nezahrnuje klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="b8a55-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="b8a55-108">Doporučujeme používat [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="b8a55-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="b8a55-109">Většina Linuxových distribucích a Mac OS, obsahují hello nástroj příkazového řádku SSH.</span><span class="sxs-lookup"><span data-stu-id="b8a55-109">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="b8a55-110">Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="b8a55-110">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="b8a55-111">Požadovaný hardware</span><span class="sxs-lookup"><span data-stu-id="b8a55-111">Required hardware</span></span>

<span data-ttu-id="b8a55-112">Stolní počítač tooenable tooconnect můžete vzdáleně toohello příkazového řádku na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="b8a55-112">A desktop computer tooenable you tooconnect remotely toohello command line on hello Raspberry Pi.</span></span>

<span data-ttu-id="b8a55-113">[Microsoft IoT Starter Kit malin pí 3] [ lnk-starter-kits] nebo ekvivalentní součásti.</span><span class="sxs-lookup"><span data-stu-id="b8a55-113">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="b8a55-114">Tento kurz používá hello následujících položek z hello kit:</span><span class="sxs-lookup"><span data-stu-id="b8a55-114">This tutorial uses hello following items from hello kit:</span></span>

- <span data-ttu-id="b8a55-115">Malinová pí 3</span><span class="sxs-lookup"><span data-stu-id="b8a55-115">Raspberry Pi 3</span></span>
- <span data-ttu-id="b8a55-116">Karta MicroSD (s NOOBS)</span><span class="sxs-lookup"><span data-stu-id="b8a55-116">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="b8a55-117">Kabelu USB malé</span><span class="sxs-lookup"><span data-stu-id="b8a55-117">A USB Mini cable</span></span>
- <span data-ttu-id="b8a55-118">Kabel Ethernet</span><span class="sxs-lookup"><span data-stu-id="b8a55-118">An Ethernet cable</span></span>
- <span data-ttu-id="b8a55-119">Senzor BME280</span><span class="sxs-lookup"><span data-stu-id="b8a55-119">BME280 sensor</span></span>
- <span data-ttu-id="b8a55-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="b8a55-120">Breadboard</span></span>
- <span data-ttu-id="b8a55-121">Můstek vodičům</span><span class="sxs-lookup"><span data-stu-id="b8a55-121">Jumper wires</span></span>
- <span data-ttu-id="b8a55-122">Odpory</span><span class="sxs-lookup"><span data-stu-id="b8a55-122">Resistors</span></span>
- <span data-ttu-id="b8a55-123">LED</span><span class="sxs-lookup"><span data-stu-id="b8a55-123">LEDs</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/