> [!div class="op_single_selector"]
> * [<span data-ttu-id="40a48-101">Linux</span><span class="sxs-lookup"><span data-stu-id="40a48-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [<span data-ttu-id="40a48-102">Windows</span><span class="sxs-lookup"><span data-stu-id="40a48-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

<span data-ttu-id="40a48-103">Tento návod [simulované zařízení cloudu nahrát ukázková] ukazuje, jak používat [Azure IoT Edge] [ lnk-sdk] k odesílání telemetrie zařízení cloud do služby IoT Hub ze simulovaného zařízení .</span><span class="sxs-lookup"><span data-stu-id="40a48-103">This walkthrough of the [Simulated Device Cloud Upload sample] shows you how to use [Azure IoT Edge][lnk-sdk] to send device-to-cloud telemetry to IoT Hub from simulated devices.</span></span>

<span data-ttu-id="40a48-104">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="40a48-104">This walkthrough covers:</span></span>

* <span data-ttu-id="40a48-105">**Architektura**: architektury informace o [simulované zařízení cloudu nahrát ukázková].</span><span class="sxs-lookup"><span data-stu-id="40a48-105">**Architecture**: architectural information about the [Simulated Device Cloud Upload sample].</span></span>
* <span data-ttu-id="40a48-106">**Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="40a48-106">**Build and run**: the steps required to build and run the sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="40a48-107">Architektura</span><span class="sxs-lookup"><span data-stu-id="40a48-107">Architecture</span></span>

<span data-ttu-id="40a48-108">[simulované zařízení cloudu nahrát ukázková] ukazuje, jak vytvořit bránu, která odesílá telemetrii z Simulovaná zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-108">The [Simulated Device Cloud Upload sample] shows how to create a gateway that sends telemetry from simulated devices to an IoT hub.</span></span> <span data-ttu-id="40a48-109">Zařízení nemusí být schopni připojit přímo do služby IoT Hub, protože zařízení:</span><span class="sxs-lookup"><span data-stu-id="40a48-109">A device may not be able to connect directly to IoT Hub because the device:</span></span>

* <span data-ttu-id="40a48-110">Nepoužívá komunikační protokol podporovaných službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-110">Does not use a communications protocol understood by IoT Hub.</span></span>
* <span data-ttu-id="40a48-111">Není dostatečně inteligentní pamatovat identity přiřazené službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-111">Is not smart enough to remember the identity assigned to it by IoT Hub.</span></span>

<span data-ttu-id="40a48-112">IoT vstupní brána může vyřešit tyto problémy následujícími způsoby:</span><span class="sxs-lookup"><span data-stu-id="40a48-112">An IoT Edge gateway can solve these problems in the following ways:</span></span>

* <span data-ttu-id="40a48-113">Brána rozumí protokol zařízení, použije získává telemetrická data zařízení cloud ze zařízení a předá tyto zprávy do služby IoT Hub použití protokolu podporovaných službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-113">The gateway understands the protocol used by the device, receives device-to-cloud telemetry from the device, and forwards those messages to IoT Hub using a protocol understood by the IoT hub.</span></span>

* <span data-ttu-id="40a48-114">Brána mapuje identit služby IoT Hub na zařízení a funguje jako proxy server, když zařízení odesílá zprávy do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-114">The gateway maps IoT Hub identities to devices and acts as a proxy when a device sends messages to IoT Hub.</span></span>

<span data-ttu-id="40a48-115">Následující diagram znázorňuje hlavní komponenty ukázku, včetně modulů hraniční IoT:</span><span class="sxs-lookup"><span data-stu-id="40a48-115">The following diagram shows the main components of the sample, including the IoT Edge modules:</span></span>

![][1]

<span data-ttu-id="40a48-116">Moduly si mezi sebou nepředávají zprávy přímo.</span><span class="sxs-lookup"><span data-stu-id="40a48-116">The modules do not pass messages directly to each other.</span></span> <span data-ttu-id="40a48-117">Moduly publikování zpráv interní zprostředkovatele, který doručí zpráv na jiné moduly pomocí mechanismu předplatného.</span><span class="sxs-lookup"><span data-stu-id="40a48-117">The modules publish messages to an internal broker that delivers the messages to the other modules using a subscription mechanism.</span></span> <span data-ttu-id="40a48-118">Další informace najdete v tématu [Začínáme s Azure IoT Edge][lnk-gw-getstarted].</span><span class="sxs-lookup"><span data-stu-id="40a48-118">For more information, see [Get started with Azure IoT Edge][lnk-gw-getstarted].</span></span>

### <a name="protocol-ingestion-module"></a><span data-ttu-id="40a48-119">Modul ingestování protokolu</span><span class="sxs-lookup"><span data-stu-id="40a48-119">Protocol ingestion module</span></span>

<span data-ttu-id="40a48-120">Tento modul je výchozím bodem pro příjem dat ze zařízení, prostřednictvím brány a do cloudu.</span><span class="sxs-lookup"><span data-stu-id="40a48-120">This module is the starting point for receiving data from devices, through the gateway, and into the cloud.</span></span> <span data-ttu-id="40a48-121">V ukázce modul:</span><span class="sxs-lookup"><span data-stu-id="40a48-121">In the sample, the module:</span></span>

1. <span data-ttu-id="40a48-122">Vytvoří simulované teplotní data.</span><span class="sxs-lookup"><span data-stu-id="40a48-122">Creates simulated temperature data.</span></span> <span data-ttu-id="40a48-123">Pokud používáte fyzické zařízení, modul čte data z těchto fyzických zařízení.</span><span class="sxs-lookup"><span data-stu-id="40a48-123">If you use physical devices, the module reads data from those physical devices.</span></span>
1. <span data-ttu-id="40a48-124">Vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="40a48-124">Creates a message.</span></span>
1. <span data-ttu-id="40a48-125">Simulované teplotní data umístí do obsahu zprávy.</span><span class="sxs-lookup"><span data-stu-id="40a48-125">Places the simulated temperature data into the message content.</span></span>
1. <span data-ttu-id="40a48-126">Přidá vlastnost s falešných adresu MAC na zprávu.</span><span class="sxs-lookup"><span data-stu-id="40a48-126">Adds a property with a fake MAC address to the message.</span></span>
1. <span data-ttu-id="40a48-127">Zpráva zpřístupňuje další modul v řetězu.</span><span class="sxs-lookup"><span data-stu-id="40a48-127">Makes the message available to the next module in the chain.</span></span>

<span data-ttu-id="40a48-128">Názvem modulu **protokol X přijímání** na předchozím obrázku se nazývá **simulované zařízení** ve zdrojovém kódu.</span><span class="sxs-lookup"><span data-stu-id="40a48-128">The module called **Protocol X ingestion** in the previous diagram is called **Simulated device** in the source code.</span></span>

### <a name="mac-lt-gt-iot-hub-id-module"></a><span data-ttu-id="40a48-129">Adresa MAC &lt; - &gt; Modul ID služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="40a48-129">MAC &lt;-&gt; IoT Hub ID module</span></span>

<span data-ttu-id="40a48-130">Tento modul slouží k vyhledání zprávy, které mají vlastnost adresu Mac.</span><span class="sxs-lookup"><span data-stu-id="40a48-130">This module scans for messages that have a Mac address property.</span></span> <span data-ttu-id="40a48-131">V ukázce modul přijímání protokol přidá vlastnost adresy MAC.</span><span class="sxs-lookup"><span data-stu-id="40a48-131">In the sample, the protocol ingestion module adds the MAC address property.</span></span> <span data-ttu-id="40a48-132">Pokud modul najde taková vlastnost, přidá jinou vlastnost s klíčem zařízení IoT Hub ke zprávě.</span><span class="sxs-lookup"><span data-stu-id="40a48-132">If the module finds such a property, it adds another property with an IoT Hub device key to the message.</span></span> <span data-ttu-id="40a48-133">Modul poté zpřístupní zprávu další modul v řetězu.</span><span class="sxs-lookup"><span data-stu-id="40a48-133">The module then makes the message available to the next module in the chain.</span></span>

<span data-ttu-id="40a48-134">Vývojář nastaví mapování mezi adresy MAC i identit služby IoT Hub tak, aby Simulovaná zařízení přidružit identit zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-134">The developer sets up a mapping between MAC addresses and IoT Hub identities to associate the simulated devices with IoT Hub device identities.</span></span> <span data-ttu-id="40a48-135">Vývojář přidá mapování ručně jako součást konfigurace modulu.</span><span class="sxs-lookup"><span data-stu-id="40a48-135">The developer adds the mapping manually as part of the module configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="40a48-136">Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-136">This sample uses a MAC address as a unique device identifier and correlates it with an IoT Hub device identity.</span></span> <span data-ttu-id="40a48-137">Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="40a48-137">However, you can write your own module that uses a different unique identifier.</span></span> <span data-ttu-id="40a48-138">Například zařízení může mít jedinečné sériová čísla nebo telemetrická data může zahrnovat název jedinečný vložená zařízení.</span><span class="sxs-lookup"><span data-stu-id="40a48-138">For example, your devices may have unique serial numbers or the telemetry data may include a unique embedded device name.</span></span>

### <a name="iot-hub-communication-module"></a><span data-ttu-id="40a48-139">Komunikační modul služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="40a48-139">IoT Hub communication module</span></span>

<span data-ttu-id="40a48-140">Tento modul přijímá zprávy pomocí služby IoT Hub zařízení klíčovou vlastnost, kterému byla přiřazena předchozí modulem.</span><span class="sxs-lookup"><span data-stu-id="40a48-140">This module takes messages with an IoT Hub device key property that was assigned by the previous module.</span></span> <span data-ttu-id="40a48-141">Modul odesílá obsah zprávy do služby IoT Hub pomocí protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="40a48-141">The module sends the message content to IoT Hub using the HTTP protocol.</span></span> <span data-ttu-id="40a48-142">HTTP je jedním ze tří protokolů, kterým služba IoT Hub rozumí.</span><span class="sxs-lookup"><span data-stu-id="40a48-142">HTTP is one of the three protocols understood by IoT Hub.</span></span>

<span data-ttu-id="40a48-143">Tento modul místo otevřením připojení pro každé simulované zařízení, otevře jednoho připojení protokolu HTTP z brány do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="40a48-143">Instead of opening a connection for each simulated device, this module opens a single HTTP connection from the gateway to the IoT hub.</span></span> <span data-ttu-id="40a48-144">Modul multiplexes připojení ze simulovaného zařízení pak přes toto připojení.</span><span class="sxs-lookup"><span data-stu-id="40a48-144">The module then multiplexes connections from all the simulated devices over that connection.</span></span> <span data-ttu-id="40a48-145">Tento přístup umožňuje jednu bránu pro připojení mnoho další zařízení.</span><span class="sxs-lookup"><span data-stu-id="40a48-145">This approach enables a single gateway to connect many more devices.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="40a48-146">Než začnete</span><span class="sxs-lookup"><span data-stu-id="40a48-146">Before you get started</span></span>

<span data-ttu-id="40a48-147">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="40a48-147">Before you get started, you must:</span></span>

* <span data-ttu-id="40a48-148">[Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, je třeba název centra pro dokončení tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="40a48-148">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need the name of your hub to complete this walkthrough.</span></span> <span data-ttu-id="40a48-149">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="40a48-149">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="40a48-150">Přidejte dva zařízení do služby IoT hub a poznamenejte si jeho ID a klíče zařízení.</span><span class="sxs-lookup"><span data-stu-id="40a48-150">Add two devices to your IoT hub and make a note of their ids and device keys.</span></span> <span data-ttu-id="40a48-151">Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje pro přidání zařízení ke službě IoT hub, který jste vytvořili v předchozím kroku a Načtěte jejich klíče.</span><span class="sxs-lookup"><span data-stu-id="40a48-151">You can use the [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool to add your devices to the IoT hub you created in the previous step and retrieve their keys.</span></span>

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
<span data-ttu-id="40a48-152">[simulované zařízení cloudu nahrát ukázková]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md</span><span class="sxs-lookup"><span data-stu-id="40a48-152">[Simulated Device Cloud Upload sample]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md</span></span>
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md