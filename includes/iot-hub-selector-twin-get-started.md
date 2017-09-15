> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9fca-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="d9fca-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [<span data-ttu-id="d9fca-102">C#/Node.js</span><span class="sxs-lookup"><span data-stu-id="d9fca-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [<span data-ttu-id="d9fca-103">C#</span><span class="sxs-lookup"><span data-stu-id="d9fca-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [<span data-ttu-id="d9fca-104">Java</span><span class="sxs-lookup"><span data-stu-id="d9fca-104">Java</span></span>](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

<span data-ttu-id="d9fca-105">Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="d9fca-105">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="d9fca-106">IoT Hub trvá dvojče zařízení pro každé zařízení, která se k němu připojuje.</span><span class="sxs-lookup"><span data-stu-id="d9fca-106">IoT Hub persists a device twin for each device that connects to it.</span></span>

<span data-ttu-id="d9fca-107">Použijte dvojčata zařízení na:</span><span class="sxs-lookup"><span data-stu-id="d9fca-107">Use device twins to:</span></span>

* <span data-ttu-id="d9fca-108">Ukládání metadat ze zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="d9fca-108">Store device metadata from your solution back end.</span></span>
* <span data-ttu-id="d9fca-109">Sestava aktuální informace o stavu, jako jsou k dispozici funkce a podmínky (třeba připojení metoda se používá) z vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="d9fca-109">Report current state information such as available capabilities and conditions (for example, the connectivity method used) from your device app.</span></span>
* <span data-ttu-id="d9fca-110">Synchronizujte stav pracovních dlouho běžící (například aktualizací firmwaru a konfigurace) mezi aplikací zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="d9fca-110">Synchronize the state of long-running workflows (such as firmware and configuration updates) between a device app and a back-end app.</span></span>
* <span data-ttu-id="d9fca-111">Dotaz na vaše zařízení metadata, konfigurace nebo stavu.</span><span class="sxs-lookup"><span data-stu-id="d9fca-111">Query your device metadata, configuration, or state.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fca-112">Dvojčata zařízení jsou navrženy pro synchronizaci a pro dotazování konfigurací zařízení a podmínky.</span><span class="sxs-lookup"><span data-stu-id="d9fca-112">Device twins are designed for synchronization and for querying device configurations and conditions.</span></span> <span data-ttu-id="d9fca-113">– Další informace týkající se použití dvojčata zařízení naleznete v [pochopit dvojčata zařízení][lnk-twins].</span><span class="sxs-lookup"><span data-stu-id="d9fca-113">More informations on when to use device twins can be found in [Understand device twins][lnk-twins].</span></span>

<span data-ttu-id="d9fca-114">Dvojčata zařízení se ukládají do služby IoT hub a obsahovat:</span><span class="sxs-lookup"><span data-stu-id="d9fca-114">Device twins are stored in an IoT hub and contain:</span></span>

* <span data-ttu-id="d9fca-115">*značky*, metadat zařízení přístupná jenom pro back-end řešení;</span><span class="sxs-lookup"><span data-stu-id="d9fca-115">*tags*, device metadata accessible only by the solution back end;</span></span>
* <span data-ttu-id="d9fca-116">*požadovaného vlastnosti*, objekty JSON upravitelnými řešení back end a lze zobrazit aplikace zařízení; a</span><span class="sxs-lookup"><span data-stu-id="d9fca-116">*desired properties*, JSON objects modifiable by the solution back end and observable by the device app; and</span></span>
* <span data-ttu-id="d9fca-117">*hlášené vlastnosti*, objekty JSON upravitelnými aplikace zařízení a přečíst back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="d9fca-117">*reported properties*, JSON objects modifiable by the device app and readable by the solution back end.</span></span> <span data-ttu-id="d9fca-118">Značky a vlastností nesmí obsahovat pole, ale mohou být vnořené objekty.</span><span class="sxs-lookup"><span data-stu-id="d9fca-118">Tags and properties cannot contain arrays, but objects can be nested.</span></span>

![][img-twin]

<span data-ttu-id="d9fca-119">Kromě toho se můžete dotazovat dvojčata zařízení na základě výše uvedené dat back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="d9fca-119">Additionally, the solution back end can query device twins based on all the above data.</span></span>
<span data-ttu-id="d9fca-120">Odkazovat na [pochopit dvojčata zařízení] [ lnk-twins] Další informace o dvojčata zařízení a [IoT Hub dotazovací jazyk] [ lnk-query] odkaz pro dotazování.</span><span class="sxs-lookup"><span data-stu-id="d9fca-120">Refer to [Understand device twins][lnk-twins] for more information about device twins, and to the [IoT Hub query language][lnk-query] reference for querying.</span></span>

> [!NOTE]
> <span data-ttu-id="d9fca-121">V tomto okamžiku jsou přístupné pouze ze zařízení, která se připojují ke službě IoT Hub pomocí protokolu MQTT dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="d9fca-121">At this time, device twins are accessible only from devices that connect to IoT Hub using the MQTT protocol.</span></span> <span data-ttu-id="d9fca-122">Naleznete [MQTT podporu] [ lnk-devguide-mqtt] článek pokyny o tom, jak převést stávající aplikace zařízení používat MQTT.</span><span class="sxs-lookup"><span data-stu-id="d9fca-122">Please refer to the [MQTT support][lnk-devguide-mqtt] article for instructions on how to convert existing device app to use MQTT.</span></span>

<span data-ttu-id="d9fca-123">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="d9fca-123">This tutorial shows you how to:</span></span>

* <span data-ttu-id="d9fca-124">Vytvoření aplikace back-end, který přidává *značky* dvojče zařízení a aplikaci simulovaného zařízení, která generuje sestavy jeho připojení kanálu jako *hlášené vlastnost* na dvojče zařízení.</span><span class="sxs-lookup"><span data-stu-id="d9fca-124">Create a back-end app that adds *tags* to a device twin, and a simulated device app that reports its connectivity channel as a *reported property* on the device twin.</span></span>
* <span data-ttu-id="d9fca-125">Dotaz na zařízení z back-end aplikace pomocí filtrů o vlastnostech dříve vytvořili a značky.</span><span class="sxs-lookup"><span data-stu-id="d9fca-125">Query devices from your back-end app using filters on the tags and properties previously created.</span></span>

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md