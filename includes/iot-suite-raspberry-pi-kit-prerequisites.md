## <a name="prerequisites"></a><span data-ttu-id="4d1db-101">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4d1db-101">Prerequisites</span></span>

<span data-ttu-id="4d1db-102">K dokončení tohoto kurzu potřebujete mít aktivní předplatné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="4d1db-102">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4d1db-103">Pokud nemáte účet, můžete si během několika minut vytvořit bezplatný účet zkušební.</span><span class="sxs-lookup"><span data-stu-id="4d1db-103">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4d1db-104">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="4d1db-104">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

### <a name="required-software"></a><span data-ttu-id="4d1db-105">Požadovaný software</span><span class="sxs-lookup"><span data-stu-id="4d1db-105">Required software</span></span>

<span data-ttu-id="4d1db-106">Musíte klient SSH na umožňují vzdálený přístup na příkazovém řádku pí malin stolního počítače.</span><span class="sxs-lookup"><span data-stu-id="4d1db-106">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="4d1db-107">Windows nezahrnuje klientem SSH.</span><span class="sxs-lookup"><span data-stu-id="4d1db-107">Windows does not include an SSH client.</span></span> <span data-ttu-id="4d1db-108">Doporučujeme používat [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="4d1db-108">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="4d1db-109">Většina Linuxových distribucích a Mac OS, obsahují nástroj příkazového řádku SSH.</span><span class="sxs-lookup"><span data-stu-id="4d1db-109">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="4d1db-110">Další informace najdete v tématu [SSH pomocí Linux nebo Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="4d1db-110">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

### <a name="required-hardware"></a><span data-ttu-id="4d1db-111">Požadovaný hardware</span><span class="sxs-lookup"><span data-stu-id="4d1db-111">Required hardware</span></span>

<span data-ttu-id="4d1db-112">Stolní počítač, abyste mohli vzdáleně připojit k na příkazovém řádku malin pí.</span><span class="sxs-lookup"><span data-stu-id="4d1db-112">A desktop computer to enable you to connect remotely to the command line on the Raspberry Pi.</span></span>

<span data-ttu-id="4d1db-113">[Microsoft IoT Starter Kit malin pí 3] [ lnk-starter-kits] nebo ekvivalentní součásti.</span><span class="sxs-lookup"><span data-stu-id="4d1db-113">[Microsoft IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] or equivalent components.</span></span> <span data-ttu-id="4d1db-114">V tomto kurzu používá následující položky ze sady kit:</span><span class="sxs-lookup"><span data-stu-id="4d1db-114">This tutorial uses the following items from the kit:</span></span>

- <span data-ttu-id="4d1db-115">Malinová pí 3</span><span class="sxs-lookup"><span data-stu-id="4d1db-115">Raspberry Pi 3</span></span>
- <span data-ttu-id="4d1db-116">Karta MicroSD (s NOOBS)</span><span class="sxs-lookup"><span data-stu-id="4d1db-116">MicroSD Card (with NOOBS)</span></span>
- <span data-ttu-id="4d1db-117">Kabelu USB malé</span><span class="sxs-lookup"><span data-stu-id="4d1db-117">A USB Mini cable</span></span>
- <span data-ttu-id="4d1db-118">Kabel Ethernet</span><span class="sxs-lookup"><span data-stu-id="4d1db-118">An Ethernet cable</span></span>
- <span data-ttu-id="4d1db-119">Senzor BME280</span><span class="sxs-lookup"><span data-stu-id="4d1db-119">BME280 sensor</span></span>
- <span data-ttu-id="4d1db-120">Breadboard</span><span class="sxs-lookup"><span data-stu-id="4d1db-120">Breadboard</span></span>
- <span data-ttu-id="4d1db-121">Můstek vodičům</span><span class="sxs-lookup"><span data-stu-id="4d1db-121">Jumper wires</span></span>
- <span data-ttu-id="4d1db-122">Odpory</span><span class="sxs-lookup"><span data-stu-id="4d1db-122">Resistors</span></span>
- <span data-ttu-id="4d1db-123">LED</span><span class="sxs-lookup"><span data-stu-id="4d1db-123">LEDs</span></span>

[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/