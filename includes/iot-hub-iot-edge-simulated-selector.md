> [!div class="op_single_selector"]
> * [<span data-ttu-id="1c55a-101">Linux</span><span class="sxs-lookup"><span data-stu-id="1c55a-101">Linux</span></span>](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [<span data-ttu-id="1c55a-102">Windows</span><span class="sxs-lookup"><span data-stu-id="1c55a-102">Windows</span></span>](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

<span data-ttu-id="1c55a-103">Tento návod hello [simulované zařízení cloudu nahrát ukázková] ukazuje, jak toouse [Azure IoT Edge] [ lnk-sdk] toosend telemetrie zařízení cloud tooIoT rozbočovače ze simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-103">This walkthrough of hello [Simulated Device Cloud Upload sample] shows you how toouse [Azure IoT Edge][lnk-sdk] toosend device-to-cloud telemetry tooIoT Hub from simulated devices.</span></span>

<span data-ttu-id="1c55a-104">Tento návod ilustruje:</span><span class="sxs-lookup"><span data-stu-id="1c55a-104">This walkthrough covers:</span></span>

* <span data-ttu-id="1c55a-105">**Architektura**: architektury informace o hello [simulované zařízení cloudu nahrát ukázková].</span><span class="sxs-lookup"><span data-stu-id="1c55a-105">**Architecture**: architectural information about hello [Simulated Device Cloud Upload sample].</span></span>
* <span data-ttu-id="1c55a-106">**Sestavení a spuštění**: požadované toobuild hello kroky a spusťte hello ukázková.</span><span class="sxs-lookup"><span data-stu-id="1c55a-106">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="1c55a-107">Architektura</span><span class="sxs-lookup"><span data-stu-id="1c55a-107">Architecture</span></span>

<span data-ttu-id="1c55a-108">Hello [simulované zařízení cloudu nahrát ukázková] ukazuje, jak toocreate bránu, která odesílá telemetrii z simulované zařízení tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-108">hello [Simulated Device Cloud Upload sample] shows how toocreate a gateway that sends telemetry from simulated devices tooan IoT hub.</span></span> <span data-ttu-id="1c55a-109">Zařízení nemusí být možné tooconnect přímo tooIoT rozbočovače protože hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="1c55a-109">A device may not be able tooconnect directly tooIoT Hub because hello device:</span></span>

* <span data-ttu-id="1c55a-110">Nepoužívá komunikační protokol podporovaných službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-110">Does not use a communications protocol understood by IoT Hub.</span></span>
* <span data-ttu-id="1c55a-111">Není dostatečně inteligentní tooremember hello identity přiřazené tooit službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-111">Is not smart enough tooremember hello identity assigned tooit by IoT Hub.</span></span>

<span data-ttu-id="1c55a-112">IoT vstupní brána může vyřešit tyto problémy v hello následující způsoby:</span><span class="sxs-lookup"><span data-stu-id="1c55a-112">An IoT Edge gateway can solve these problems in hello following ways:</span></span>

* <span data-ttu-id="1c55a-113">Hello brány rozumí hello protokol používaný zařízením hello získává telemetrická data zařízení cloud ze zařízení hello a předává tyto zprávy tooIoT použití protokolu podporovaných službou hello IoT hub rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="1c55a-113">hello gateway understands hello protocol used by hello device, receives device-to-cloud telemetry from hello device, and forwards those messages tooIoT Hub using a protocol understood by hello IoT hub.</span></span>

* <span data-ttu-id="1c55a-114">Brána Hello mapuje toodevices identit služby IoT Hub a funguje jako proxy server, když zařízení odesílá zprávy tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="1c55a-114">hello gateway maps IoT Hub identities toodevices and acts as a proxy when a device sends messages tooIoT Hub.</span></span>

<span data-ttu-id="1c55a-115">Hello následující diagram znázorňuje hlavní komponenty hello ukázkový text hello, včetně hello moduly IoT Edge:</span><span class="sxs-lookup"><span data-stu-id="1c55a-115">hello following diagram shows hello main components of hello sample, including hello IoT Edge modules:</span></span>

![][1]

<span data-ttu-id="1c55a-116">moduly Hello nepředávejte zprávy přímo tooeach jiné.</span><span class="sxs-lookup"><span data-stu-id="1c55a-116">hello modules do not pass messages directly tooeach other.</span></span> <span data-ttu-id="1c55a-117">moduly Hello publikovat tooan zprávy interní se zprostředkovatel, který doručí toohello hello zprávy z ostatních modulů pomocí mechanismu předplatného.</span><span class="sxs-lookup"><span data-stu-id="1c55a-117">hello modules publish messages tooan internal broker that delivers hello messages toohello other modules using a subscription mechanism.</span></span> <span data-ttu-id="1c55a-118">Další informace najdete v tématu [Začínáme s Azure IoT Edge][lnk-gw-getstarted].</span><span class="sxs-lookup"><span data-stu-id="1c55a-118">For more information, see [Get started with Azure IoT Edge][lnk-gw-getstarted].</span></span>

### <a name="protocol-ingestion-module"></a><span data-ttu-id="1c55a-119">Modul ingestování protokolu</span><span class="sxs-lookup"><span data-stu-id="1c55a-119">Protocol ingestion module</span></span>

<span data-ttu-id="1c55a-120">Tento modul je hello výchozí bod pro příjem dat ze zařízení, prostřednictvím brány hello a do cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1c55a-120">This module is hello starting point for receiving data from devices, through hello gateway, and into hello cloud.</span></span> <span data-ttu-id="1c55a-121">V ukázce hello hello modul:</span><span class="sxs-lookup"><span data-stu-id="1c55a-121">In hello sample, hello module:</span></span>

1. <span data-ttu-id="1c55a-122">Vytvoří simulované teplotní data.</span><span class="sxs-lookup"><span data-stu-id="1c55a-122">Creates simulated temperature data.</span></span> <span data-ttu-id="1c55a-123">Pokud používáte fyzické zařízení, hello modul čte data z těchto fyzických zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-123">If you use physical devices, hello module reads data from those physical devices.</span></span>
1. <span data-ttu-id="1c55a-124">Vytvoří zprávu.</span><span class="sxs-lookup"><span data-stu-id="1c55a-124">Creates a message.</span></span>
1. <span data-ttu-id="1c55a-125">Hello simulované teplotní data umístí do hello obsah zprávy.</span><span class="sxs-lookup"><span data-stu-id="1c55a-125">Places hello simulated temperature data into hello message content.</span></span>
1. <span data-ttu-id="1c55a-126">Přidá vlastnost s zprávu toohello falešné adresy MAC.</span><span class="sxs-lookup"><span data-stu-id="1c55a-126">Adds a property with a fake MAC address toohello message.</span></span>
1. <span data-ttu-id="1c55a-127">V řetězci hello umožňuje hello zprávy k dispozici toohello další modul.</span><span class="sxs-lookup"><span data-stu-id="1c55a-127">Makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="1c55a-128">Hello modul s názvem **protokol X přijímání** v hello předchozí diagram nazývá **simulované zařízení** ve zdrojovém kódu hello.</span><span class="sxs-lookup"><span data-stu-id="1c55a-128">hello module called **Protocol X ingestion** in hello previous diagram is called **Simulated device** in hello source code.</span></span>

### <a name="mac-lt-gt-iot-hub-id-module"></a><span data-ttu-id="1c55a-129">Adresa MAC &lt; - &gt; Modul ID služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1c55a-129">MAC &lt;-&gt; IoT Hub ID module</span></span>

<span data-ttu-id="1c55a-130">Tento modul slouží k vyhledání zprávy, které mají vlastnost adresu Mac.</span><span class="sxs-lookup"><span data-stu-id="1c55a-130">This module scans for messages that have a Mac address property.</span></span> <span data-ttu-id="1c55a-131">V ukázce hello hello protokol přijímání modul přidává vlastnost adresy MAC hello.</span><span class="sxs-lookup"><span data-stu-id="1c55a-131">In hello sample, hello protocol ingestion module adds hello MAC address property.</span></span> <span data-ttu-id="1c55a-132">Pokud modul hello najde taková vlastnost, přidá jinou vlastnost s zprávou klíče toohello zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-132">If hello module finds such a property, it adds another property with an IoT Hub device key toohello message.</span></span> <span data-ttu-id="1c55a-133">modul Hello vytváří v řetězu hello hello zprávy k dispozici toohello další modul.</span><span class="sxs-lookup"><span data-stu-id="1c55a-133">hello module then makes hello message available toohello next module in hello chain.</span></span>

<span data-ttu-id="1c55a-134">Hello vývojáře nastaví mapování mezi adresy MAC a IoT Hub identity tooassociate hello simulované zařízení pomocí identity zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-134">hello developer sets up a mapping between MAC addresses and IoT Hub identities tooassociate hello simulated devices with IoT Hub device identities.</span></span> <span data-ttu-id="1c55a-135">mapování hello Hello vývojáře ručně přidá jako součást konfigurace modulu hello.</span><span class="sxs-lookup"><span data-stu-id="1c55a-135">hello developer adds hello mapping manually as part of hello module configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="1c55a-136">Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-136">This sample uses a MAC address as a unique device identifier and correlates it with an IoT Hub device identity.</span></span> <span data-ttu-id="1c55a-137">Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor.</span><span class="sxs-lookup"><span data-stu-id="1c55a-137">However, you can write your own module that uses a different unique identifier.</span></span> <span data-ttu-id="1c55a-138">Například zařízení může mít jedinečné sériová čísla nebo hello telemetrická data může zahrnovat název jedinečný vložená zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-138">For example, your devices may have unique serial numbers or hello telemetry data may include a unique embedded device name.</span></span>

### <a name="iot-hub-communication-module"></a><span data-ttu-id="1c55a-139">Komunikační modul služby IoT Hub</span><span class="sxs-lookup"><span data-stu-id="1c55a-139">IoT Hub communication module</span></span>

<span data-ttu-id="1c55a-140">Tento modul přijímá zprávy pomocí služby IoT Hub zařízení klíčovou vlastnost, kterému byla přiřazena předchozí modulem hello.</span><span class="sxs-lookup"><span data-stu-id="1c55a-140">This module takes messages with an IoT Hub device key property that was assigned by hello previous module.</span></span> <span data-ttu-id="1c55a-141">modul Hello pošle uvítací zprávu pomocí centra obsahu tooIoT hello protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c55a-141">hello module sends hello message content tooIoT Hub using hello HTTP protocol.</span></span> <span data-ttu-id="1c55a-142">HTTP je jedním z hello tří protokolů rozumí službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-142">HTTP is one of hello three protocols understood by IoT Hub.</span></span>

<span data-ttu-id="1c55a-143">Tento modul místo otevřením připojení pro každé simulované zařízení, otevře jednoho připojení protokolu HTTP z hello brány toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1c55a-143">Instead of opening a connection for each simulated device, this module opens a single HTTP connection from hello gateway toohello IoT hub.</span></span> <span data-ttu-id="1c55a-144">modul Hello pak multiplexes připojení ze všech hello simulované zařízení přes toto připojení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-144">hello module then multiplexes connections from all hello simulated devices over that connection.</span></span> <span data-ttu-id="1c55a-145">Tento přístup umožňuje velké tooconnect mnoho další zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-145">This approach enables a single gateway tooconnect many more devices.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="1c55a-146">Než začnete</span><span class="sxs-lookup"><span data-stu-id="1c55a-146">Before you get started</span></span>

<span data-ttu-id="1c55a-147">Než začnete, musíte provést tyto akce:</span><span class="sxs-lookup"><span data-stu-id="1c55a-147">Before you get started, you must:</span></span>

* <span data-ttu-id="1c55a-148">[Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, potřebujete hello název vašeho centra toocomplete tento návod.</span><span class="sxs-lookup"><span data-stu-id="1c55a-148">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="1c55a-149">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="1c55a-149">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="1c55a-150">Přidejte dva zařízení tooyour IoT hub a poznamenejte si jeho ID a klíče zařízení.</span><span class="sxs-lookup"><span data-stu-id="1c55a-150">Add two devices tooyour IoT hub and make a note of their ids and device keys.</span></span> <span data-ttu-id="1c55a-151">Můžete použít hello [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroj tooadd zařízení IoT hub toohello jste vytvořili v hello předchozí krok a načíst jejich klíče.</span><span class="sxs-lookup"><span data-stu-id="1c55a-151">You can use hello [device explorer][lnk-device-explorer] or [iothub-explorer][lnk-iothub-explorer] tool tooadd your devices toohello IoT hub you created in hello previous step and retrieve their keys.</span></span>

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[simulované zařízení cloudu nahrát ukázková]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md