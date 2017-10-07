---
title: "zařízení Azure IoT aaaThe SDK pro jazyk C | Microsoft Docs"
description: "Začínáme s Azure IoT zařízení hello SDK pro jazyk C a zjistěte, jak toocreate aplikací pro zařízení, komunikují s služby IoT hub."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 9e20742e6ea513c124bfaf28f02f6fba86170daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c"></a>Pro zařízení Azure IoT SDK pro jazyk C

Hello **zařízení Azure IoT SDK** je sada knihoven určené toosimplify hello proces odesílání zpráv tooand příjmu zprávy z hello **Azure IoT Hub** služby. Existují různé varianty hello SDK, každý cílení na konkrétní platformu, ale tento článek popisuje hello **zařízení Azure IoT SDK pro jazyk C**.

zařízení Azure IoT Hello SDK pro jazyk C je napsán v přenositelnost toomaximize ANSI C (C99). Díky této funkci hello knihovny vhodným toooperate na několika platformách a zařízeních, zejména v případě, že minimalizovat disku a spotřeba paměti je prioritu.

Existují širokou škálu platformy, na které hello SDK je testovaná (viz hello [Azure certifikované pro katalog zařízení IoT](https://catalog.azureiotsuite.com/) podrobnosti). I když tento článek obsahuje návody ukázkový kód spuštěný na platformě Windows hello, napříč hello rozsah podporované platformy je stejný jako kód hello popsané v tomto článku.

Tento článek vás seznámí toohello architektura zařízení Azure IoT hello SDK pro C. Ukazuje, jak knihovny zařízení hello tooinitialize, odesílat data tooIoT rozbočovače a přijímat zprávy z něj. Hello informace v tomto článku by měla být dostatek tooget pomocí hello SDK spuštěna, ale také poskytuje ukazatele tooadditional informace o knihovnách hello.

## <a name="sdk-architecture"></a>Architektura sady SDK

Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).

nejnovější verzi knihovny hello Hello lze nalézt v hello **hlavní** větve hello úložiště:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* Hello základní implementaci hello SDK je v hello **iothub\_klienta** složku, která obsahuje implementaci hello hello nejnižší vrstvu rozhraní API v hello SDK: hello **IoTHubClient** knihovny. Hello **IoTHubClient** knihovna obsahuje rozhraní API pro implementace nezpracovaná zasílání zpráv pro odesílání zpráv tooIoT rozbočovače a přijímání zpráv ze služby IoT Hub. Při použití této knihovny, je zodpovědná za implementace zpráva serializaci, ale další podrobnosti o komunikaci se službou IoT Hub jsou zpracovávány pro vás.
* Hello **serializátor** složka obsahuje podpůrné funkce a ukázky, které ukazují, jak hello tooserialize dat před odesláním tooAzure IoT Hub pomocí klientské knihovny. použití Hello hello serializátor není povinné a zajišťuje pro potřeby. toouse hello **serializátor** knihovny, můžete definovat model, který určuje hello data toosend tooIoT rozbočovače hello zprávy a očekáváte, že tooreceive z něj. Po definování hello modelu hello SDK poskytuje s plochy rozhraní API, která vám umožní tooeasily pracovní zařízení cloud a zprávy typu cloud zařízení bez obav o hello podrobnosti serializace. Hello knihovně závisí na jiné opensourcové knihovny, které implementují přenosu pomocí protokolů, například MQTT a AMQP.
* Hello **IoTHubClient** knihovny závisí na jiné opensourcové knihovny:
  * Hello [Azure C sdílené nástroj](https://github.com/Azure/azure-c-shared-utility) knihovny, která poskytuje běžné funkce pro základní úlohy (například řetězce, manipulace se seznamem a vstupně-výstupní operace) potřeby napříč několika souvisejících s Azure C sady SDK.
  * Hello [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) knihovny, která je na straně klienta implementace AMQP optimalizované pro zařízení prostředků omezené.
  * Hello [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) knihovny, která je pro obecné účely knihovna implementaci protokolu MQTT hello a optimalizovaný pro zařízení prostředků omezené.

Použijte tyto knihovny je snazší toounderstand prohlížením ukázkový kód. Hello následující části vás provede procesem řadu hello ukázkové aplikace, které jsou součástí hello SDK. Tento názorný postup měl dát dobrou působí pro hello různé možnosti hello architektury vrstev hello SDK a hello toohow Úvod rozhraní API fungovat.

## <a name="before-you-run-hello-samples"></a>Před spuštěním ukázky hello

Před spuštěním ukázky hello v zařízení Azure IoT hello SDK pro jazyk C, je nutné [vytvořit instanci hello služby IoT Hub](iot-hub-create-through-portal.md) ve vašem předplatném Azure. Dokončete hello následující úlohy:

* Příprava vývojového prostředí
* Získejte přihlašovací údaje zařízení.

### <a name="prepare-your-development-environment"></a>Příprava vývojového prostředí

Balíčky jsou k dispozici pro běžné platformy (například NuGet pro systém Windows nebo apt_get Debian a Ubuntu) a ukázky hello použít tyto balíčky, pokud je k dispozici. V některých případech musíte toocompile hello SDK pro nebo na zařízení. Pokud potřebujete toocompile hello SDK, přečtěte si [Příprava vývojového prostředí](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) v úložišti GitHub hello.

tooobtain hello ukázkový kód aplikace, stáhnout kopii hello SDK z Githubu. Získání vaší kopie hello zdroje z hello **hlavní** větve hello [úložiště GitHub](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-hello-device-credentials"></a>Získat přihlašovací údaje hello zařízení

Teď, když máte hello ukázka zdrojového kódu, je další toodo věc hello tooget sadu přihlašovacích údajů zařízení. Pro zařízení toobe možné tooaccess služby IoT hub je nejprve nutno přidat hello zařízení toohello registru identit služby IoT Hub. Když přidáte zařízení, můžete získat sadu pověření zařízení, které potřebujete pro hello zařízení toobe možné tooconnect toohello IoT hub. ukázkové aplikace Hello popsané v další části hello předpokládají, že tyto přihlašovací údaje ve formě hello **zařízení připojovací řetězec**.

Existuje několik nástrojů toohelp s otevřeným zdrojem můžete spravovat své služby IoT hub.

* Aplikace systému Windows s názvem [explorer zařízení](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).
* Nástroj příkazového řádku node.js napříč platformami názvem [iothub-explorer](https://github.com/azure/iothub-explorer).

Tento kurz používá hello grafické *explorer zařízení* nástroj. Můžete taky hello *iothub-explorer* Pokud dáváte přednost toouse nástroj příkazového řádku.

Nástroj Průzkumník zařízení Hello používá tooperform knihovny služby Azure IoT hello různé funkce na IoT Hub, včetně přidávání zařízení. Jestliže používáte hello zařízení explorer nástroj tooadd zařízení, můžete získat připojovací řetězec pro vaše zařízení. Je nutné tento připojovací řetězec toorun hello ukázkové aplikace.

Pokud si nejste obeznámeni s nástroji Průzkumník hello zařízení, hello následující postup popisuje, jak toouse ho tooadd zařízení a získat připojovací řetězec zařízení.

Nástroj pro zařízení explorer tooinstall hello, najdete v části [jak toouse hello Explorer zařízení pro zařízení IoT Hub](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

Při spuštění programu hello, zobrazí se toto rozhraní:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Zadejte vaše **IoT Hub připojovací řetězec** v hello nejprve pole a klikněte na **aktualizace**. Tento krok nakonfiguruje nástroj hello tak, aby může komunikovat se službou IoT Hub.

Pokud je nakonfigurovaná hello připojovací řetězec služby IoT Hub, klikněte na tlačítko hello **správy** karty:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Tato karta je, kde budete spravovat hello zařízení zaregistrovaná ve službě IoT hub.

Vytvoření zařízení kliknutím hello **vytvořit** tlačítko. Zobrazí se dialogové okno zobrazí sadu předem vyplněná klíče (primární i sekundární). Zadejte **ID zařízení** a pak klikněte na **vytvořit**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Při vytváření hello zařízení hello zařízení seznam aktualizací s všechny hello zaregistrované zařízení, včetně hello ten, který jste právě vytvořili. Pokud kliknete pravým tlačítkem na nové zařízení, zobrazí se v této nabídce:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Pokud se rozhodnete **zkopírujte připojovací řetězec pro vybrané zařízení**, hello zařízení připojovací řetězec je zkopírovaný toohello schránky. Ponechat si kopii hello zařízení připojovací řetězec. Budete ho potřebovat při spuštění ukázkové aplikace hello popsané v následující části hello.

Po dokončení kroků hello výše, jste připravené toostart systémem nějaký kód. Obě ukázky mít konstanta hello horní části hello hlavní zdrojový soubor, který umožňuje tooenter připojovací řetězec. Například hello odpovídající řádek z hello **iothub\_klienta\_ukázka\_mqtt** aplikace se zobrazí následujícím způsobem.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-hello-iothubclient-library"></a>Použití knihovny IoTHubClient hello

V rámci hello **iothub\_klienta** složky v hello [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) úložiště, je **ukázky** složky, která obsahuje aplikaci s názvem **iothub\_klienta\_ukázka\_mqtt**.

verze Windows Hello hello **iothub\_klienta\_ukázka\_mqtt** aplikace obsahuje hello následující řešení sady Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> Pokud tento projekt otevřít v aplikaci Visual Studio 2017, přijměte hello výzvy tooretarget hello projektu toohello nejnovější verzi.

Toto řešení obsahuje jedné aplikace. Existují čtyři balíčky NuGet, které jsou nainstalované v tomto řešení:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Budete vždy potřebovat hello **Microsoft.Azure.C.SharedUtility** balíček při práci s hello SDK. Tato ukázka používá protokol MQTT hello, proto je nutné zahrnout hello **Microsoft.Azure.umqtt** a **Microsoft.Azure.IoTHub.MqttTransport** balíčků (existují zde ekvivalentní balíčky pro AMQP a HTTP ). Protože ukázka hello používá hello **IoTHubClient** knihovny, musíte taky zahrnout hello **Microsoft.Azure.IoTHub.IoTHubClient** balíček ve vašem řešení.

Hello implementace pro ukázkovou aplikaci hello můžete najít v hello **iothub\_klienta\_ukázka\_mqtt.c** zdrojový soubor.

Hello následující kroky pomocí této ukázkové aplikaci toowalk vás jaké jsou požadavky toouse hello **IoTHubClient** knihovny.

### <a name="initialize-hello-library"></a>Inicializace knihovny hello

> [!NOTE]
> Před zahájením práce s knihovnami hello, může být nutné tooperform inicializace některé specifické pro platformu. Například pokud máte v plánu toouse AMQP v systému Linux je nutné inicializovat knihovny OpenSSL hello. Hello ukázky v hello [úložiště GitHub](https://github.com/Azure/azure-iot-sdk-c) volání funkce nástroj hello **platformy\_init** při hello spustí klienta a volejte hello **platformy\_deinit**  funkce před ukončením. Tyto funkce jsou deklarované v hello platform.h hlavičkový soubor. Zkontrolujte hello Definice tyto funkce pro svou cílovou platformu v hello [úložiště](https://github.com/Azure/azure-iot-sdk-c) toodetermine jestli potřebujete tooinclude žádné specifické pro platformu inicializace kódu v vašeho klienta.

toostart práci s knihovnami hello nejprve přidělit popisovač klienta služby IoT Hub:

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

Předáte kopii hello zařízení připojovací řetězec, který jste získali z hello zařízení explorer nástroj toothis funkce. Můžete také určit toouse protokol hello komunikace. Tento příklad používá MQTT, ale AMQP a HTTP jsou také možnosti.

Pokud máte platný **IOTHUB\_klienta\_zpracování**, je možné spustit volání rozhraní API toosend hello a přijímat zprávy tooand ze služby IoT Hub.

### <a name="send-messages"></a>Odesílání zpráv

Ukázková aplikace Hello nastaví centrum IoT k tooyour smyčky toosend zprávy. Hello následující fragment kódu:

- Vytvoří zprávu.
- Přidá zprávu toohello vlastnost.
- Odešle zprávu.

Nejprve vytvořte zprávu:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission tooIoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

Pokaždé, když odešlete zprávu, zadáte odkaz funkce zpětného volání tooa, která je volána při odesílání dat hello. V tomto příkladu je volána funkce zpětného volání hello **SendConfirmationCallback**. Hello následující fragment kódu ukazuje tuto funkci zpětného volání:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Všimněte si volání toohello hello **IoTHubMessage\_Destroy** fungovat, když jste hotovi s uvítací zprávu. Tato funkce uvolní prostředky hello přidělené při vytváření uvítací zprávu.

### <a name="receive-messages"></a>Příjem zpráv

Přijímání zprávy je asynchronní operace. Když hello zařízení přijme zprávu o se nejprve zaregistrovat tooinvoke hello zpětného volání:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

Parametr last Hello je neplatný ukazatel toowhatever, které chcete. V ukázce hello je celé číslo tooan ukazatele, ale může to být tooa ukazatel složitější datové struktury. Tento parametr umožňuje hello zpětného volání funkce toooperate na sdílení stavu s hello volající tuto funkci.

Když hello zařízení obdrží zprávu, hello registrovaná funkce zpětného volání je volána. Tato funkce zpětného volání načte:

* id zprávy Hello a id korelace z uvítací zprávu.
* obsah zprávy Hello.
* Vlastní vlastnosti z uvítací zprávu.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable tooretrieve hello message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive hello work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from hello message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Použití hello **IoTHubMessage\_GetByteArray** funkce tooretrieve uvítací zprávu, která v tomto příkladu je řetězec.

### <a name="uninitialize-hello-library"></a>Uninitialize – hello knihovně

Po dokončení odesílání událostí a přijímání zpráv, můžete inicializaci hello knihovně IoT. toodo tedy vydejte hello následující volání funkce:

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Toto volání uvolní prostředky hello dříve přidělené hello **IoTHubClient\_CreateFromConnectionString** funkce.

Jak můžete vidět, je snadné toosend a přijímat zprávy pomocí hello **IoTHubClient** knihovny. Hello knihovně zpracovává hello podrobnosti o komunikaci se službou IoT Hub, včetně které toouse protocol (z hlediska hello hello vývojáře, je tato možnost jednoduché konfigurace).

Hello **IoTHubClient** knihovna také poskytuje přesnou kontrolu nad jak tooserialize hello data vaše zařízení odesílá tooIoT rozbočovače. V některých případech tato úroveň řízení je několik výhod, ale v jiné je podrobností implementace, které nemají být, že toobe problémem. Pokud tento způsob hello případ, můžete zvážit použití hello **serializátor** knihovny, která je popsaná v další části hello.

## <a name="use-hello-serializer-library"></a>Použití knihovny serializátor hello

Koncepčně hello **serializátor** knihovna je umístěna nad hello **IoTHubClient** knihovny v hello SDK. Používá hello **IoTHubClient** knihovny pro hello základní komunikace se službou IoT Hub, ale přidá modelování možnosti, které musejí hello práci s zpráva serializace odebrání vývojáře hello. Jak funguje tato knihovna je nejvhodnější ukázán na příkladu.

Uvnitř hello **serializátor** složky v hello [úložiště azure-iot-sdk-c](https://github.com/Azure/azure-iot-sdk-c), je **ukázky** složky, která obsahuje aplikaci s názvem **simplesample \_mqtt**. verze systému Windows Hello této ukázky obsahuje hello následující řešení sady Visual Studio:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> Pokud tento projekt otevřít v aplikaci Visual Studio 2017, přijměte hello výzvy tooretarget hello projektu toohello nejnovější verzi.

Stejně jako u předchozí ukázce hello tato zahrnuje několik balíčků NuGet:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

Seznámili jste se většina těchto balíčků v předchozí ukázce hello, ale **Microsoft.Azure.IoTHub.Serializer** je nová. Tento balíček je požadováno, když používáte hello **serializátor** knihovny.

Implementace hello vzorové aplikace hello můžete najít v hello **simplesample\_mqtt.c** souboru.

Hello následující části vás provede procesem hello klíčových částí této ukázce.

### <a name="initialize-hello-library"></a>Inicializace knihovny hello

Práce s hello toostart **serializátor** knihovny, inicializace hello volání rozhraní API:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

Hello volání toohello **serializátor\_init** funkce je jednorázové volání a inicializuje hello základní knihovny. Potom zavolejte hello **IoTHubClient\_UDOU\_CreateFromConnectionString** funkci, která je hello stejné rozhraní API jako hello **IoTHubClient** ukázka. Toto volání nastaví připojovací řetězec zařízení (Toto volání se také kterémkoliv hello protokolu chcete toouse). Tato ukázka používá MQTT jako hello přenosu, ale může použije protokol AMQP nebo HTTP.

Nakonec zavolejte hello **vytvořit\_modelu\_INSTANCE** funkce. **WeatherStation** je obor názvů hello hello modelu a **ContosoAnemometer** je název hello hello modelu. Po vytvoření instance hello modelu, můžete ho toostart odesílání a přijímání zpráv. Je však důležité toounderstand, jaké je model.

### <a name="define-hello-model"></a>Definování hello modelu

Model v hello **serializátor** knihovny definuje hello zprávy, které zařízení může odesílat tooIoT rozbočovače a hello zprávy, označované jako *akce* v hello Modelovací jazyk, který může přijímat. Definovat model pomocí sady maker v jazyce C jako hello **simplesample\_mqtt** ukázkovou aplikaci:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Hello **začít\_obor názvů** a **END\_obor názvů** makra obou trvat hello obor názvů modelu hello jako argument. Očekává se, že nic mezi tyto makra je hello definice modelu nebo modely a hello datové struktury, které používají modely hello.

V tomto příkladu je jednotný model názvem **ContosoAnemometer**. Tento model definuje dva kusy dat, že zařízení může odesílat tooIoT rozbočovače: **DeviceId** a **větru**. Definuje také tři akce (zpráv), které vaše zařízení může získat: **TurnFanOn**, **TurnFanOff**, a **SetAirResistance**. Každý datový prvek má typ, přičemž každá akce má název (a volitelně sadu parametrů).

Hello dat a akce definované v modelu hello definovat plochy rozhraní API, můžete použít toosend zprávy tooIoT rozbočovače a reagovat toomessages odeslané toohello zařízení. Použití tohoto modelu odhalíte nejlépe v příkladu.

### <a name="send-messages"></a>Odesílání zpráv

Hello model definuje hello data, můžete odeslat tooIoT rozbočovače. V tomto příkladu to znamená, jeden z hello dvě položky dat definované pomocí hello **WITH_DATA** makro. Existuje několik kroků požadované toosend **DeviceId** a **větru** hodnoty tooan IoT hub. Hello je nejdřív tooset hello data, která chcete toosend:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Hello modelu definovaného dříve vám umožní tooset hello hodnoty nastavením členy **struktura**. V dalším kroku serializovat uvítací zprávu, že chcete toosend:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed tooserialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Tento kód serializuje hello zařízení cloud tooa vyrovnávací paměti (odkazuje **cílové**). Kód Hello poté vyvolá hello **sendMessage** funkce toosend hello zpráva tooIoT rozbočovače:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable toocreate a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed toohand over hello message tooIoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted hello message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


druhý parametr toolast Hello **IoTHubClient\_UDOU\_SendEventAsync** je funkce zpětného volání tooa referenční dokumentace, která je volána, když hello data je úspěšně odeslána. Tady je funkce zpětného volání hello v ukázce hello:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

druhý parametr Hello je ukazatel toouser kontextu; Hello předán stejné ukazatel příliš**IoTHubClient\_UDOU\_SendEventAsync**. V takovém případě hello kontext je jednoduchý čítač, ale může být, vše, co chcete.

To je všechno je toosending zpráv typu zařízení cloud. je technologie Hello pouze zopakuji toocover jak tooreceive zprávy.

### <a name="receive-messages"></a>Příjem zpráv

Přijímání zprávy funguje podobně toohello způsobem zprávy fungovat v hello **IoTHubClient** knihovny. Nejprve zaregistrovat funkci zpětného volání zpráva:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable tooIoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

Potom napíšete hello funkce zpětného volání, která je volána, když je obdržena zprávu:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Tento kód je často používaný – ho má stejné hello pro řešení. Tato funkce přijímá uvítací zprávu a má na starosti směrování je příliš toohello příslušnou funkci prostřednictvím volání hello**EXECUTE\_příkaz**. Volaná funkce Hello v tomto okamžiku závisí na definici hello hello akcí v daném modelu.

Když definujete akce v modelu, jste požadované tooimplement funkci, která je volána, když zařízení obdrží hello odpovídající zprávu. Například pokud váš model definuje tato akce:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Definice funkce s Tento podpis:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Všimněte si, jak hello název funkce hello odpovídá názvu hello hello akce v modelu hello a aby hello parametry funkce hello odpovídaly hello parametry zadané pro akce hello. první parametr Hello vždy je nutné a obsahuje instanci toohello ukazatel modelu.

Když hello zařízení obdrží zprávu, která odpovídá tento podpis, hello odpovídající funkce je volána. Proto kromě zajištění dostatečného s tooinclude hello často používaný kód z **IoTHubMessage**, přijímání zpráv stačí definice jednoduché funkce pro jednotlivé akce definované v modelu.

### <a name="uninitialize-hello-library"></a>Uninitialize – hello knihovně

Když jste hotovi odesílání dat a přijímání zpráv, můžete inicializaci hello knihovně IoT:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Každá z těchto tří funkcí zarovnaná s hello tři Inicializace funkce popsané. Volání tato rozhraní API zajistí, že můžete uvolnit dříve přidělené prostředky.

## <a name="next-steps"></a>Další kroky

Tento článek zahrnutých hello základy používání knihovny hello v hello **zařízení Azure IoT SDK pro jazyk C**. Je k dispozici dostatek toounderstand informace, co je součástí hello SDK, jeho architektura a způsob spuštění tooget práce s hello ukázky Windows. Další článek Hello pokračuje hello popis hello SDK tím, že vysvětlí [více o hello knihovně IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
