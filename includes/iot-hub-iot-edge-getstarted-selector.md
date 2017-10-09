> [!div class="op_single_selector"]
> * [<span data-ttu-id="a4eae-101">Linux</span><span class="sxs-lookup"><span data-stu-id="a4eae-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [<span data-ttu-id="a4eae-102">Windows</span><span class="sxs-lookup"><span data-stu-id="a4eae-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

<span data-ttu-id="a4eae-103">Tento článek obsahuje podrobný návod k hello [Hello, World ukázkový kód] [ lnk-helloworld-sample] tooillustrate hello základní součástí hello [Azure IoT Edge] [ lnk-iot-edge] architektura.</span><span class="sxs-lookup"><span data-stu-id="a4eae-103">This article provides a detailed walkthrough of hello [Hello World sample code][lnk-helloworld-sample] tooillustrate hello fundamental components of hello [Azure IoT Edge][lnk-iot-edge] architecture.</span></span> <span data-ttu-id="a4eae-104">Hello ukázce se používá toobuild hello Azure IoT hraniční bránu jednoduché protokoly každých pět sekund soubor tooa zprávu "hello, world".</span><span class="sxs-lookup"><span data-stu-id="a4eae-104">hello sample uses hello Azure IoT Edge toobuild a simple gateway that logs a "hello world" message tooa file every five seconds.</span></span>

<span data-ttu-id="a4eae-105">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="a4eae-105">This walkthrough covers:</span></span>

* <span data-ttu-id="a4eae-106">**Hello World ukázková architektura**: Popisuje, jak [architektury koncepty Azure IoT Edge] [ lnk-edge-concepts] použít toohello Hello, World vzorek a jak hello součásti zapadají.</span><span class="sxs-lookup"><span data-stu-id="a4eae-106">**Hello World sample architecture**: Describes how [Azure IoT Edge architectural concepts][lnk-edge-concepts] apply toohello Hello World sample and how hello components fit together.</span></span>
* <span data-ttu-id="a4eae-107">**Jak toobuild hello ukázka**: hello kroky požadované toobuild hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="a4eae-107">**How toobuild hello sample**: hello steps required toobuild hello sample.</span></span>
* <span data-ttu-id="a4eae-108">**Jak toorun hello ukázka**: hello kroky požadované toorun hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="a4eae-108">**How toorun hello sample**: hello steps required toorun hello sample.</span></span> 
* <span data-ttu-id="a4eae-109">**Typické výstup**: Příklad hello výstup tooexpect při spuštění ukázkové hello.</span><span class="sxs-lookup"><span data-stu-id="a4eae-109">**Typical output**: An example of hello output tooexpect when you run hello sample.</span></span>
* <span data-ttu-id="a4eae-110">**Fragmenty kódu**: kolekce tooshow fragmenty kódu jak ukázka programu hello Hello World implementuje klíče IoT hraniční brány součásti.</span><span class="sxs-lookup"><span data-stu-id="a4eae-110">**Code snippets**: A collection of code snippets tooshow how hello Hello World sample implements key IoT Edge gateway components.</span></span>


## <a name="hello-world-sample-architecture"></a><span data-ttu-id="a4eae-111">Architektura ukázky Hello World</span><span class="sxs-lookup"><span data-stu-id="a4eae-111">Hello World sample architecture</span></span>
<span data-ttu-id="a4eae-112">Ukázka programu Hello Hello World znázorňuje hello konceptů popsaných v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="a4eae-112">hello Hello World sample illustrates hello concepts described in hello previous section.</span></span> <span data-ttu-id="a4eae-113">Ukázka programu Hello Hello World implementuje IoT hraniční bránu, která se skládá ze dvou IoT Edge moduly kanál:</span><span class="sxs-lookup"><span data-stu-id="a4eae-113">hello Hello World sample implements a IoT Edge gateway that has a pipeline made up of two IoT Edge modules:</span></span>

* <span data-ttu-id="a4eae-114">Hello *hello, world* modulu vytvoří zprávu každých pět sekund a předává je modul toohello protokolovacího nástroje.</span><span class="sxs-lookup"><span data-stu-id="a4eae-114">hello *hello world* module creates a message every five seconds and passes it toohello logger module.</span></span>
* <span data-ttu-id="a4eae-115">Hello *protokolovač* zprávy hello zápisy modulu obdrží tooa souboru.</span><span class="sxs-lookup"><span data-stu-id="a4eae-115">hello *logger* module writes hello messages it receives tooa file.</span></span>

![Architektura ukázky Hello World vytvořené s použitím služby Azure IoT Edge][4]

<span data-ttu-id="a4eae-117">Jak je popsáno v předchozí části hello, hello World Hello modulu nepředává zprávy přímo toohello protokoly modulu každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="a4eae-117">As described in hello previous section, hello Hello World module does not pass messages directly toohello logger module every five seconds.</span></span> <span data-ttu-id="a4eae-118">Místo toho se publikuje zprostředkovatel toohello zpráva každých pět sekund.</span><span class="sxs-lookup"><span data-stu-id="a4eae-118">Instead, it publishes a message toohello broker every five seconds.</span></span>

<span data-ttu-id="a4eae-119">Hello protokolovacího nástroje modul přijímá uvítací zprávu z hello zprostředkovatele a spustí se při, zápis hello obsah souboru tooa zpráva hello.</span><span class="sxs-lookup"><span data-stu-id="a4eae-119">hello logger module receives hello message from hello broker and acts upon it, writing hello contents of hello message tooa file.</span></span>

<span data-ttu-id="a4eae-120">Hello protokoly modulu využívá pouze zprávy z hello zprostředkovatele, se nikdy publikuje nový zprostředkovatel toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="a4eae-120">hello logger module only consumes messages from hello broker, it never publishes new messages toohello broker.</span></span>

![Jak hello zprostředkovatele směrování zpráv mezi moduly v Azure IoT Edge][5]

<span data-ttu-id="a4eae-122">Hello výše uvedené schéma ukazuje architekturu hello ukázka programu hello Hello World a relativní cesty hello toohello zdrojové soubory, které implementují různé části hello ukázka v hello [úložiště][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="a4eae-122">hello figure above shows hello architecture of hello Hello World sample and hello relative paths toohello source files that implement different portions of hello sample in hello [repository][lnk-iot-edge].</span></span> <span data-ttu-id="a4eae-123">Prozkoumejte hello kód vlastní, nebo hello fragmenty kódu níže, použijte jako vodítko.</span><span class="sxs-lookup"><span data-stu-id="a4eae-123">Explore hello code on your own, or use hello code snippets below as a guide.</span></span>

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md