## <a name="typical-output"></a><span data-ttu-id="6d5ef-101">Příklad typického výstupu</span><span class="sxs-lookup"><span data-stu-id="6d5ef-101">Typical output</span></span>

<span data-ttu-id="6d5ef-102">Hello následující příklad ukazuje výstup hello podle ukázka programu hello Hello World zapisují toohello souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-102">hello following example shows hello output written toohello log file by hello Hello World sample.</span></span> <span data-ttu-id="6d5ef-103">výstup Hello je formátován pro čitelnost:</span><span class="sxs-lookup"><span data-stu-id="6d5ef-103">hello output is formatted for legibility:</span></span>

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a><span data-ttu-id="6d5ef-104">Fragmenty kódu</span><span class="sxs-lookup"><span data-stu-id="6d5ef-104">Code snippets</span></span>

<span data-ttu-id="6d5ef-105">Tato část popisuje některé klíče oddílů hello kódu hello hello\_world vzorek.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-105">This section discusses some key sections of hello code in hello hello\_world sample.</span></span>

### <a name="iot-edge-gateway-creation"></a><span data-ttu-id="6d5ef-106">Vytvoření brány IoT Edge</span><span class="sxs-lookup"><span data-stu-id="6d5ef-106">IoT Edge gateway creation</span></span>

<span data-ttu-id="6d5ef-107">Je nutné implementovat *procesu gateway*.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-107">You must implement a *gateway process*.</span></span> <span data-ttu-id="6d5ef-108">Tento program vytvoří hello interní infrastruktury (Zprostředkovatel hello), načte moduly hello IoT okraj a nakonfiguruje procesu gateway hello.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-108">This program creates hello internal infrastructure (hello broker), loads hello IoT Edge modules, and configures hello gateway process.</span></span> <span data-ttu-id="6d5ef-109">Okraj IoT poskytuje hello **brány\_vytvořit\_z\_JSON** funkce tooenable toobootstrap brány ze souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-109">IoT Edge provides hello **Gateway\_Create\_From\_JSON** function tooenable you toobootstrap a gateway from a JSON file.</span></span> <span data-ttu-id="6d5ef-110">toouse hello **brány\_vytvořit\_z\_JSON** fungovat, předejte ji hello cesta tooa JSON soubor, který určuje hello tooload moduly IoT okraj.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-110">toouse hello **Gateway\_Create\_From\_JSON** function, pass it hello path tooa JSON file that specifies hello IoT Edge modules tooload.</span></span>

<span data-ttu-id="6d5ef-111">Můžete najít hello kód pro proces hello brány v hello *Hello, World* ukázku v hello [main.c] [ lnk-main-c] souboru.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-111">You can find hello code for hello gateway process in hello *Hello World* sample in hello [main.c][lnk-main-c] file.</span></span> <span data-ttu-id="6d5ef-112">Pro čitelnost hello následující fragment kódu ukazuje, zkrácený verzi kód procesu hello brány.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-112">For legibility, hello following snippet shows an abbreviated version of hello gateway process code.</span></span> <span data-ttu-id="6d5ef-113">Tento příklad program vytvoří brány a potom počká, než hello uživatele toopress hello **ENTER** klíče předtím, než ho strhne hello brány.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-113">This example program creates a gateway and then waits for hello user toopress hello **ENTER** key before it tears down hello gateway.</span></span>

```c
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed toocreate hello gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

<span data-ttu-id="6d5ef-114">soubor nastavení Hello JSON obsahuje seznam tooload moduly IoT okraj a hello propojení mezi hello moduly.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-114">hello JSON settings file contains a list of IoT Edge modules tooload and hello links between hello modules.</span></span> <span data-ttu-id="6d5ef-115">Každý modul IoT Edge musíte zadat a:</span><span class="sxs-lookup"><span data-stu-id="6d5ef-115">Each IoT Edge module must specify a:</span></span>

* <span data-ttu-id="6d5ef-116">**název**: jedinečný název pro modul hello.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-116">**name**: a unique name for hello module.</span></span>
* <span data-ttu-id="6d5ef-117">**zavaděč**: zavaděč, že zná, jak tooload hello potřeby modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-117">**loader**: a loader that knows how tooload hello desired module.</span></span> <span data-ttu-id="6d5ef-118">Zavaděče jsou rozšiřovacím bodem pro načítání různých typů modulů.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-118">Loaders are an extension point for loading different types of modules.</span></span> <span data-ttu-id="6d5ef-119">Okraj IoT poskytuje zavaděče pro použití s moduly, které jsou napsané v nativní C, Node.js, Java a rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-119">IoT Edge provides loaders for use with modules written in native C, Node.js, Java, and .NET.</span></span> <span data-ttu-id="6d5ef-120">Ukázka programu Hello Hello World pouze používá hello nativní C zavaděč, protože všechny moduly hello v této ukázce jsou dynamické knihovny, které jsou napsané v C. Další informace o tom, moduly IoT Edge toouse napsané v různých jazycích, najdete v části hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), nebo [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) ukázky.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-120">hello Hello World sample only uses hello native C loader because all hello modules in this sample are dynamic libraries written in C. For more information about how toouse IoT Edge modules written in different languages, see hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), or [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) samples.</span></span>
    * <span data-ttu-id="6d5ef-121">**název**: název hello zavaděči hello používá tooload hello modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-121">**name**: hello name of hello loader used tooload hello module.</span></span>
    * <span data-ttu-id="6d5ef-122">**EntryPoint**: hello cesta toohello knihovny obsahující hello modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-122">**entrypoint**: hello path toohello library containing hello module.</span></span> <span data-ttu-id="6d5ef-123">V Linuxu je touto knihovnou soubor .so, v systému Windows soubor .dll.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-123">On Linux this library is a .so file, on Windows this library is a .dll file.</span></span> <span data-ttu-id="6d5ef-124">Hello vstupní bod je konkrétní toohello typ zavaděč používá.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-124">hello entry point is specific toohello type of loader being used.</span></span> <span data-ttu-id="6d5ef-125">Hello Node.js zavaděč vstupní bod je soubor .js.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-125">hello Node.js loader entry point is a .js file.</span></span> <span data-ttu-id="6d5ef-126">Hello Java zavaděč vstupní bod je cestu pro třídy a název třídy.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-126">hello Java loader entry point is a classpath and a class name.</span></span> <span data-ttu-id="6d5ef-127">Hello .NET zavaděč vstupní bod je název sestavení a název třídy.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-127">hello .NET loader entry point is an assembly name and a class name.</span></span>

* <span data-ttu-id="6d5ef-128">**argumentů**: všechny konfigurační informace hello modul musí.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-128">**args**: any configuration information hello module needs.</span></span>

<span data-ttu-id="6d5ef-129">Následující kód ukazuje hello JSON použít toodeclare všechny Hello hello IoT Edge moduly pro ukázka programu hello Hello World v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-129">hello following code shows hello JSON used toodeclare all hello IoT Edge modules for hello Hello World sample on Linux.</span></span> <span data-ttu-id="6d5ef-130">Jestli modul vyžaduje všechny argumenty závisí na návrh hello hello modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-130">Whether a module requires any arguments depends on hello design of hello module.</span></span> <span data-ttu-id="6d5ef-131">V tomto příkladu hello protokolovacího nástroje modul přijímá argument, který je hello cesta toohello výstupní soubor a hello hello\_world modul má žádné argumenty.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-131">In this example, hello logger module takes an argument that is hello path toohello output file and hello hello\_world module has no arguments.</span></span>

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

<span data-ttu-id="6d5ef-132">soubor JSON Hello obsahuje taky odkazy hello mezi hello moduly, které se předávají toohello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-132">hello JSON file also contains hello links between hello modules that are passed toohello broker.</span></span> <span data-ttu-id="6d5ef-133">Propojení má dvě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="6d5ef-133">A link has two properties:</span></span>

* <span data-ttu-id="6d5ef-134">**Zdroj**: název modulu z hello `modules` části nebo `\*`.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-134">**source**: a module name from hello `modules` section, or `\*`.</span></span>
* <span data-ttu-id="6d5ef-135">**podřízený**: název modulu z hello `modules` části.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-135">**sink**: a module name from hello `modules` section.</span></span>

<span data-ttu-id="6d5ef-136">Každé propojení definuje trasu a směr zpráv.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-136">Each link defines a message route and direction.</span></span> <span data-ttu-id="6d5ef-137">Zprávy ze hello **zdroj** modulu se dodávají toohello **podřízený** modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-137">Messages from hello **source** module are delivered toohello **sink** module.</span></span> <span data-ttu-id="6d5ef-138">Můžete nastavit hello **zdroj** modulu příliš`\*`, což znamená, že hello **podřízený** modul přijímá zprávy od libovolný modul.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-138">You can set hello **source** module too`\*`, which indicates that hello **sink** module receives messages from any module.</span></span>

<span data-ttu-id="6d5ef-139">Hello následující kód ukazuje hello JSON použít tooconfigure propojení mezi hello moduly používané v hello hello\_world vzorek v systému Linux.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-139">hello following code shows hello JSON used tooconfigure links between hello modules used in hello hello\_world sample on Linux.</span></span> <span data-ttu-id="6d5ef-140">Každou zprávu vyprodukované hello `hello_world` modulu je spotřebovávají hello `logger` modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-140">Every message produced by hello `hello_world` module is consumed by hello `logger` module.</span></span>

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a><span data-ttu-id="6d5ef-141">Publikování zpráv modulu hello\_world</span><span class="sxs-lookup"><span data-stu-id="6d5ef-141">Hello\_world module message publishing</span></span>

<span data-ttu-id="6d5ef-142">Můžete najít hello kódu aplikace hello hello\_zprávy toopublish modulu world v hello ['hello_world.c'] [ lnk-helloworld-c] souboru.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-142">You can find hello code used by hello hello\_world module toopublish messages in hello ['hello_world.c'][lnk-helloworld-c] file.</span></span> <span data-ttu-id="6d5ef-143">Hello následující fragment kódu ukazuje změněný hello kód s poznámky přidané a některé zpracování odebrat pro čitelnost kód chyb:</span><span class="sxs-lookup"><span data-stu-id="6d5ef-143">hello following snippet shows an amended version of hello code with comments added and some error handling code removed for legibility:</span></span>

```c
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" tooa set of message properties that
    // will be appended toohello message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set hello content for hello message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set hello properties for hello message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on hello msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of hello thread*/
        }
        else
        {
            // publish hello message toohello broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

### <a name="helloworld-module-message-processing"></a><span data-ttu-id="6d5ef-144">Zpracování zpráv modulu hello\_world</span><span class="sxs-lookup"><span data-stu-id="6d5ef-144">Hello\_world module message processing</span></span>

<span data-ttu-id="6d5ef-145">Hello hello\_world modul nikdy zpracovává zprávy, že z ostatních modulů IoT Edge publikování toohello zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-145">hello hello\_world module never processes messages that other IoT Edge modules publish toohello broker.</span></span> <span data-ttu-id="6d5ef-146">Proto hello implementace zpětné volání zprávy hello v hello hello\_world modulu je funkce, žádná operace.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-146">Therefore, hello implementation of hello message callback in hello hello\_world module is a no-op function.</span></span>

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a><span data-ttu-id="6d5ef-147">Publikování a zpracování zpráv protokolovacího modulu</span><span class="sxs-lookup"><span data-stu-id="6d5ef-147">Logger module message publishing and processing</span></span>

<span data-ttu-id="6d5ef-148">Hello protokolovacího nástroje modul přijímá zprávy od zprostředkovatele hello a zapíše tooa souboru.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-148">hello logger module receives messages from hello broker and writes them tooa file.</span></span> <span data-ttu-id="6d5ef-149">Nikdy publikuje žádné zprávy.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-149">It never publishes any messages.</span></span> <span data-ttu-id="6d5ef-150">Proto hello kód hello protokolovacího nástroje modul nikdy nevolá hello **Broker_Publish** funkce.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-150">Therefore, hello code of hello logger module never calls hello **Broker_Publish** function.</span></span>

<span data-ttu-id="6d5ef-151">Hello **Logger_Receive** funkce v hello [logger.c] [ lnk-logger-c] soubor je hello zpětného volání hello zprostředkovatele vyvolá toodeliver zprávy toohello protokoly modulu.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-151">hello **Logger_Receive** function in hello [logger.c][lnk-logger-c] file is hello callback hello broker invokes toodeliver messages toohello logger module.</span></span> <span data-ttu-id="6d5ef-152">Hello následující fragment kódu ukazuje změněné verze s poznámky přidané a některé zpracování odebrat pro čitelnost kód chyb:</span><span class="sxs-lookup"><span data-stu-id="6d5ef-152">hello following snippet shows an amended version with comments added and some error handling code removed for legibility:</span></span>

```c
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get hello message properties from hello message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert hello collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode hello message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start hello construction of hello final string toobe logged by adding
    // hello timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add hello message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add hello content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write hello formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a><span data-ttu-id="6d5ef-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6d5ef-153">Next steps</span></span>

<span data-ttu-id="6d5ef-154">V tomto článku jste spustili jednoduché IoT hraniční bránu, která zapisuje soubor protokolu tooa zprávy.</span><span class="sxs-lookup"><span data-stu-id="6d5ef-154">In this article, you ran a simple IoT Edge gateway that writes messages tooa log file.</span></span> <span data-ttu-id="6d5ef-155">toorun vzorku, který odesílá zprávy tooIoT centru, najdete v části [IoT Edge – odesílání zpráv typu zařízení cloud s simulované zařízení pomocí Linux] [ lnk-gateway-simulated-linux] nebo [IoT Edge – odesílání zpráv typu zařízení cloud s Simulovaná zařízení pomocí Windows][lnk-gateway-simulated-windows].</span><span class="sxs-lookup"><span data-stu-id="6d5ef-155">toorun a sample that sends messages tooIoT Hub, see [IoT Edge – send device-to-cloud messages with a simulated device using Linux][lnk-gateway-simulated-linux] or [IoT Edge – send device-to-cloud messages with a simulated device using Windows][lnk-gateway-simulated-windows].</span></span>


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md