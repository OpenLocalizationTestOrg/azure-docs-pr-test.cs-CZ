## <a name="typical-output"></a>Příklad typického výstupu

Hello následující příklad ukazuje výstup hello podle ukázka programu hello Hello World zapisují toohello souboru protokolu. výstup Hello je formátován pro čitelnost:

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

## <a name="code-snippets"></a>Fragmenty kódu

Tato část popisuje některé klíče oddílů hello kódu hello hello\_world vzorek.

### <a name="iot-edge-gateway-creation"></a>Vytvoření brány IoT Edge

Je nutné implementovat *procesu gateway*. Tento program vytvoří hello interní infrastruktury (Zprostředkovatel hello), načte moduly hello IoT okraj a nakonfiguruje procesu gateway hello. Okraj IoT poskytuje hello **brány\_vytvořit\_z\_JSON** funkce tooenable toobootstrap brány ze souboru JSON. toouse hello **brány\_vytvořit\_z\_JSON** fungovat, předejte ji hello cesta tooa JSON soubor, který určuje hello tooload moduly IoT okraj.

Můžete najít hello kód pro proces hello brány v hello *Hello, World* ukázku v hello [main.c] [ lnk-main-c] souboru. Pro čitelnost hello následující fragment kódu ukazuje, zkrácený verzi kód procesu hello brány. Tento příklad program vytvoří brány a potom počká, než hello uživatele toopress hello **ENTER** klíče předtím, než ho strhne hello brány.

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

soubor nastavení Hello JSON obsahuje seznam tooload moduly IoT okraj a hello propojení mezi hello moduly. Každý modul IoT Edge musíte zadat a:

* **název**: jedinečný název pro modul hello.
* **zavaděč**: zavaděč, že zná, jak tooload hello potřeby modulu. Zavaděče jsou rozšiřovacím bodem pro načítání různých typů modulů. Okraj IoT poskytuje zavaděče pro použití s moduly, které jsou napsané v nativní C, Node.js, Java a rozhraní .NET. Ukázka programu Hello Hello World pouze používá hello nativní C zavaděč, protože všechny moduly hello v této ukázce jsou dynamické knihovny, které jsou napsané v C. Další informace o tom, moduly IoT Edge toouse napsané v různých jazycích, najdete v části hello [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), nebo [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) ukázky.
    * **název**: název hello zavaděči hello používá tooload hello modulu.
    * **EntryPoint**: hello cesta toohello knihovny obsahující hello modulu. V Linuxu je touto knihovnou soubor .so, v systému Windows soubor .dll. Hello vstupní bod je konkrétní toohello typ zavaděč používá. Hello Node.js zavaděč vstupní bod je soubor .js. Hello Java zavaděč vstupní bod je cestu pro třídy a název třídy. Hello .NET zavaděč vstupní bod je název sestavení a název třídy.

* **argumentů**: všechny konfigurační informace hello modul musí.

Následující kód ukazuje hello JSON použít toodeclare všechny Hello hello IoT Edge moduly pro ukázka programu hello Hello World v systému Linux. Jestli modul vyžaduje všechny argumenty závisí na návrh hello hello modulu. V tomto příkladu hello protokolovacího nástroje modul přijímá argument, který je hello cesta toohello výstupní soubor a hello hello\_world modul má žádné argumenty.

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

soubor JSON Hello obsahuje taky odkazy hello mezi hello moduly, které se předávají toohello zprostředkovatele. Propojení má dvě vlastnosti:

* **Zdroj**: název modulu z hello `modules` části nebo `\*`.
* **podřízený**: název modulu z hello `modules` části.

Každé propojení definuje trasu a směr zpráv. Zprávy ze hello **zdroj** modulu se dodávají toohello **podřízený** modulu. Můžete nastavit hello **zdroj** modulu příliš`\*`, což znamená, že hello **podřízený** modul přijímá zprávy od libovolný modul.

Hello následující kód ukazuje hello JSON použít tooconfigure propojení mezi hello moduly používané v hello hello\_world vzorek v systému Linux. Každou zprávu vyprodukované hello `hello_world` modulu je spotřebovávají hello `logger` modulu.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Publikování zpráv modulu hello\_world

Můžete najít hello kódu aplikace hello hello\_zprávy toopublish modulu world v hello ['hello_world.c'] [ lnk-helloworld-c] souboru. Hello následující fragment kódu ukazuje změněný hello kód s poznámky přidané a některé zpracování odebrat pro čitelnost kód chyb:

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

### <a name="helloworld-module-message-processing"></a>Zpracování zpráv modulu hello\_world

Hello hello\_world modul nikdy zpracovává zprávy, že z ostatních modulů IoT Edge publikování toohello zprostředkovatele. Proto hello implementace zpětné volání zprávy hello v hello hello\_world modulu je funkce, žádná operace.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Publikování a zpracování zpráv protokolovacího modulu

Hello protokolovacího nástroje modul přijímá zprávy od zprostředkovatele hello a zapíše tooa souboru. Nikdy publikuje žádné zprávy. Proto hello kód hello protokolovacího nástroje modul nikdy nevolá hello **Broker_Publish** funkce.

Hello **Logger_Receive** funkce v hello [logger.c] [ lnk-logger-c] soubor je hello zpětného volání hello zprostředkovatele vyvolá toodeliver zprávy toohello protokoly modulu. Hello následující fragment kódu ukazuje změněné verze s poznámky přidané a některé zpracování odebrat pro čitelnost kód chyb:

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

## <a name="next-steps"></a>Další kroky

V tomto článku jste spustili jednoduché IoT hraniční bránu, která zapisuje soubor protokolu tooa zprávy. toorun vzorku, který odesílá zprávy tooIoT centru, najdete v části [IoT Edge – odesílání zpráv typu zařízení cloud s simulované zařízení pomocí Linux] [ lnk-gateway-simulated-linux] nebo [IoT Edge – odesílání zpráv typu zařízení cloud s Simulovaná zařízení pomocí Windows][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md