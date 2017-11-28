> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c369-101">Linux</span><span class="sxs-lookup"><span data-stu-id="6c369-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [<span data-ttu-id="6c369-102">Windows</span><span class="sxs-lookup"><span data-stu-id="6c369-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

<span data-ttu-id="6c369-103">Tento článek poskytuje podrobný návod k [ukázkovému kódu Hello World][lnk-helloworld-sample] pro ilustraci základních komponent architektury [Azure IoT Edge][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="6c369-103">This article provides a detailed walkthrough of the [Hello World sample code][lnk-helloworld-sample] to illustrate the fundamental components of the [Azure IoT Edge][lnk-iot-edge] architecture.</span></span> <span data-ttu-id="6c369-104">Ukázka používá Azure IoT Edge k vytvoření jednoduché brány, která každých pět sekund zaznamená do souboru zprávu „hello world“.</span><span class="sxs-lookup"><span data-stu-id="6c369-104">The sample uses the Azure IoT Edge to build a simple gateway that logs a "hello world" message to a file every five seconds.</span></span>

<span data-ttu-id="6c369-105">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="6c369-105">This walkthrough covers:</span></span>

* <span data-ttu-id="6c369-106">**Hello World ukázková architektura**: Popisuje, jak [architektury koncepty Azure IoT Edge] [ lnk-edge-concepts] týkají Hello, World vzorek a jak součásti zapadají.</span><span class="sxs-lookup"><span data-stu-id="6c369-106">**Hello World sample architecture**: Describes how [Azure IoT Edge architectural concepts][lnk-edge-concepts] apply to the Hello World sample and how the components fit together.</span></span>
* <span data-ttu-id="6c369-107">**Postup k vytvoření ukázky**: kroky potřebné k vytvoření ukázkového kódu.</span><span class="sxs-lookup"><span data-stu-id="6c369-107">**How to build the sample**: The steps required to build the sample.</span></span>
* <span data-ttu-id="6c369-108">**Postup spuštění ukázky**: kroky potřebné ke spuštění ukázkového kódu.</span><span class="sxs-lookup"><span data-stu-id="6c369-108">**How to run the sample**: The steps required to run the sample.</span></span> 
* <span data-ttu-id="6c369-109">**Příklad typického výstupu**: příklad výstupu, jaký můžete očekávat při spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="6c369-109">**Typical output**: An example of the output to expect when you run the sample.</span></span>
* <span data-ttu-id="6c369-110">**Fragmenty kódu**: kolekce fragmenty kódu zobrazit, jak Hello, World vzorek implementuje klíčové komponenty IoT hraniční brány.</span><span class="sxs-lookup"><span data-stu-id="6c369-110">**Code snippets**: A collection of code snippets to show how the Hello World sample implements key IoT Edge gateway components.</span></span>


## <a name="hello-world-sample-architecture"></a><span data-ttu-id="6c369-111">Architektura ukázky Hello World</span><span class="sxs-lookup"><span data-stu-id="6c369-111">Hello World sample architecture</span></span>
<span data-ttu-id="6c369-112">Ukázka Hello World ilustruje koncepty popsané v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="6c369-112">The Hello World sample illustrates the concepts described in the previous section.</span></span> <span data-ttu-id="6c369-113">Hello World vzorek implementuje IoT hraniční bránu, která se skládá ze dvou IoT Edge moduly kanál:</span><span class="sxs-lookup"><span data-stu-id="6c369-113">The Hello World sample implements a IoT Edge gateway that has a pipeline made up of two IoT Edge modules:</span></span>

* <span data-ttu-id="6c369-114">Modul *hello world* vytvoří každých pět sekund zprávu a předá ji do modulu logger.</span><span class="sxs-lookup"><span data-stu-id="6c369-114">The *hello world* module creates a message every five seconds and passes it to the logger module.</span></span>
* <span data-ttu-id="6c369-115">Modul *logger* zapíše přijatou zprávu do souboru.</span><span class="sxs-lookup"><span data-stu-id="6c369-115">The *logger* module writes the messages it receives to a file.</span></span>

![Architektura ukázky Hello World vytvořené s použitím služby Azure IoT Edge][4]

<span data-ttu-id="6c369-117">Jak je popsáno v předchozí části, modul Hello World nepředává každých pět sekund zprávu přímo do modulu logger.</span><span class="sxs-lookup"><span data-stu-id="6c369-117">As described in the previous section, the Hello World module does not pass messages directly to the logger module every five seconds.</span></span> <span data-ttu-id="6c369-118">Místo toho ji každých pět sekund publikuje do zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="6c369-118">Instead, it publishes a message to the broker every five seconds.</span></span>

<span data-ttu-id="6c369-119">Protokolovací modul obdrží od zprostředkovatele zprávu a postupuje podle ní, zatímco zapisuje obsah zprávy do souboru.</span><span class="sxs-lookup"><span data-stu-id="6c369-119">The logger module receives the message from the broker and acts upon it, writing the contents of the message to a file.</span></span>

<span data-ttu-id="6c369-120">Protokolovací modul od zprostředkovatele zprávy pouze přijímá, nikdy žádné sám nepublikuje.</span><span class="sxs-lookup"><span data-stu-id="6c369-120">The logger module only consumes messages from the broker, it never publishes new messages to the broker.</span></span>

![Způsob, jakým zprostředkovatel provádí směrování zpráv mezi moduly ve službě Azure IoT Edge][5]

<span data-ttu-id="6c369-122">Obrázek nahoře ukazuje architekturu ukázky Hello World a relativní cesty ke zdrojovým souborům, které implementují jednotlivé části, v [úložišti][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="6c369-122">The figure above shows the architecture of the Hello World sample and the relative paths to the source files that implement different portions of the sample in the [repository][lnk-iot-edge].</span></span> <span data-ttu-id="6c369-123">Prozkoumejte kód sami, nebo pro orientaci použijte fragmenty kódu uvedené dole.</span><span class="sxs-lookup"><span data-stu-id="6c369-123">Explore the code on your own, or use the code snippets below as a guide.</span></span>

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md