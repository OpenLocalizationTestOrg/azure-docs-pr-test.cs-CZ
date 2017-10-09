> [!div class="op_single_selector"]
> * [<span data-ttu-id="dfeed-101">Node.js</span><span class="sxs-lookup"><span data-stu-id="dfeed-101">Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [<span data-ttu-id="dfeed-102">C#/Node.js</span><span class="sxs-lookup"><span data-stu-id="dfeed-102">C#/Node.js</span></span>](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [<span data-ttu-id="dfeed-103">C#</span><span class="sxs-lookup"><span data-stu-id="dfeed-103">C#</span></span>](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [<span data-ttu-id="dfeed-104">Java</span><span class="sxs-lookup"><span data-stu-id="dfeed-104">Java</span></span>](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

<span data-ttu-id="dfeed-105">Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky).</span><span class="sxs-lookup"><span data-stu-id="dfeed-105">Device twins are JSON documents that store device state information (metadata, configurations, and conditions).</span></span> <span data-ttu-id="dfeed-106">IoT Hub trvá dvojče zařízení pro každé zařízení, která se připojuje tooit.</span><span class="sxs-lookup"><span data-stu-id="dfeed-106">IoT Hub persists a device twin for each device that connects tooit.</span></span>

<span data-ttu-id="dfeed-107">Použijte dvojčata zařízení na:</span><span class="sxs-lookup"><span data-stu-id="dfeed-107">Use device twins to:</span></span>

* <span data-ttu-id="dfeed-108">Ukládání metadat ze zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="dfeed-108">Store device metadata from your solution back end.</span></span>
* <span data-ttu-id="dfeed-109">Sestava aktuální informace o stavu, jako jsou k dispozici funkce a podmínky (třeba hello připojení metoda se používá) z vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="dfeed-109">Report current state information such as available capabilities and conditions (for example, hello connectivity method used) from your device app.</span></span>
* <span data-ttu-id="dfeed-110">Synchronizujte hello stav pracovních dlouho běžící (například aktualizací firmwaru a konfigurace) mezi aplikací zařízení a back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfeed-110">Synchronize hello state of long-running workflows (such as firmware and configuration updates) between a device app and a back-end app.</span></span>
* <span data-ttu-id="dfeed-111">Dotaz na vaše zařízení metadata, konfigurace nebo stavu.</span><span class="sxs-lookup"><span data-stu-id="dfeed-111">Query your device metadata, configuration, or state.</span></span>

> [!NOTE]
> <span data-ttu-id="dfeed-112">Dvojčata zařízení jsou navrženy pro synchronizaci a pro dotazování konfigurací zařízení a podmínky.</span><span class="sxs-lookup"><span data-stu-id="dfeed-112">Device twins are designed for synchronization and for querying device configurations and conditions.</span></span> <span data-ttu-id="dfeed-113">– Další informace na při toouse dvojčata zařízení naleznete v [pochopit dvojčata zařízení][lnk-twins].</span><span class="sxs-lookup"><span data-stu-id="dfeed-113">More informations on when toouse device twins can be found in [Understand device twins][lnk-twins].</span></span>

<span data-ttu-id="dfeed-114">Dvojčata zařízení se ukládají do služby IoT hub a obsahovat:</span><span class="sxs-lookup"><span data-stu-id="dfeed-114">Device twins are stored in an IoT hub and contain:</span></span>

* <span data-ttu-id="dfeed-115">*značky*, metadat zařízení, které jsou přístupné pouze hello back-end řešení;</span><span class="sxs-lookup"><span data-stu-id="dfeed-115">*tags*, device metadata accessible only by hello solution back end;</span></span>
* <span data-ttu-id="dfeed-116">*požadovaného vlastnosti*, objekty JSON upravitelnými řešením hello back end a lze zobrazit aplikace hello zařízení; a</span><span class="sxs-lookup"><span data-stu-id="dfeed-116">*desired properties*, JSON objects modifiable by hello solution back end and observable by hello device app; and</span></span>
* <span data-ttu-id="dfeed-117">*hlášené vlastnosti*, objekty JSON upravitelnými aplikace hello zařízení a přečíst back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="dfeed-117">*reported properties*, JSON objects modifiable by hello device app and readable by hello solution back end.</span></span> <span data-ttu-id="dfeed-118">Značky a vlastností nesmí obsahovat pole, ale mohou být vnořené objekty.</span><span class="sxs-lookup"><span data-stu-id="dfeed-118">Tags and properties cannot contain arrays, but objects can be nested.</span></span>

![][img-twin]

<span data-ttu-id="dfeed-119">Kromě toho se můžete dotazovat dvojčata zařízení založené na všechny hello výše data back-end hello řešení.</span><span class="sxs-lookup"><span data-stu-id="dfeed-119">Additionally, hello solution back end can query device twins based on all hello above data.</span></span>
<span data-ttu-id="dfeed-120">Odkazovat příliš[pochopit dvojčata zařízení] [ lnk-twins] Další informace o dvojčata zařízení a toohello [IoT Hub dotazovací jazyk] [ lnk-query] odkaz pro zadávání dotazů.</span><span class="sxs-lookup"><span data-stu-id="dfeed-120">Refer too[Understand device twins][lnk-twins] for more information about device twins, and toohello [IoT Hub query language][lnk-query] reference for querying.</span></span>

> [!NOTE]
> <span data-ttu-id="dfeed-121">V tomto okamžiku jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="dfeed-121">At this time, device twins are accessible only from devices that connect tooIoT Hub using hello MQTT protocol.</span></span> <span data-ttu-id="dfeed-122">Naleznete toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.</span><span class="sxs-lookup"><span data-stu-id="dfeed-122">Please refer toohello [MQTT support][lnk-devguide-mqtt] article for instructions on how tooconvert existing device app toouse MQTT.</span></span>

<span data-ttu-id="dfeed-123">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="dfeed-123">This tutorial shows you how to:</span></span>

* <span data-ttu-id="dfeed-124">Vytvoření aplikace back-end, který přidává *značky* tooa dvojče zařízení a aplikaci simulovaného zařízení, která generuje sestavy funkčnost připojení kanálu jako *hlášené vlastnost* na dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="dfeed-124">Create a back-end app that adds *tags* tooa device twin, and a simulated device app that reports its connectivity channel as a *reported property* on hello device twin.</span></span>
* <span data-ttu-id="dfeed-125">Dotaz na zařízení z back-end aplikace pomocí filtrů o vlastnostech dříve vytvořili a hello značky.</span><span class="sxs-lookup"><span data-stu-id="dfeed-125">Query devices from your back-end app using filters on hello tags and properties previously created.</span></span>

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md