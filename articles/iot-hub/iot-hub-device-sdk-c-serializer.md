---
title: "zařízení IoT aaaAzure SDK pro jazyk C - serializátor | Microsoft Docs"
description: "Jak toouse hello serializátor knihovny zařízení Azure IoT hello SDK pro aplikace C toocreate zařízení, které komunikují pomocí služby IoT hub."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: defbed34-de73-429c-8592-cd863a38e4dd
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: c5776e9b50ffea71df96cb2d342ea2fc045f5a0b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Pro zařízení Azure IoT SDK pro jazyk C – informace o serializátor
Hello [nejprve článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C**. hello následující článek poskytuje podrobnější popis hello [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md). Tento článek poskytuje podrobnější popis hello zbývající součásti dokončí pokrytí hello SDK: hello **serializátor** knihovny.

Hello úvodní článek popisuje, jak toouse hello **serializátor** knihovny toosend události tooand přijímat zprávy ze služby IoT Hub. V tomto článku jsme rozšířit tento diskuzi o podrobnější vysvětlení způsobu toomodel svá data pomocí hello **serializátor** makro jazyk. článek Hello také obsahuje další podrobnosti o tom, jak hello knihovně serializuje zprávy (a v některých případech jak můžete řídit chování serializace hello). Popíšeme také některé parametry, které můžete upravit určující velikost hello hello modelů, které vytvoříte.

Nakonec hello článku znovu navštíví některá témata popsaná v předchozích články například zprávu a vlastnosti zpracování. Jak jsme najdete na těchto pracovních funkcí v hello stejným způsobem pomocí hello **serializátor** knihovny stejně jako s hello **IoTHubClient** knihovny.

Všechno, co popsané v tomto článku je založena na hello **serializátor** ukázky SDK. Pokud chcete toofollow společně, najdete v části hello **simplesample\_amqp** a **simplesample\_http** aplikace, které jsou součástí zařízení Azure IoT hello SDK pro C.

Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="hello-modeling-language"></a>představuje Modelovací jazyk Hello
Hello [úvodní článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C** Modelovací jazyk prostřednictvím hello příkladu v hello **simplesample\_amqp**  aplikace:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Jak je vidět, hello Modelovací jazyk je založena na maker v jazyce C. Je třeba nejprve vaší definice s **BEGIN\_obor názvů** a vždy končit **END\_obor názvů**. Je běžné tooname hello obor názvů pro vaši společnost nebo, jako v následujícím příkladě hello projekt, který právě pracujete.

Co v oboru názvů hello jsou definice modelu. V takovém případě je jeden model anemometer. Ještě jednou hello modelu může být název nic, ale to má obvykle název pro hello zařízení nebo typ dat chcete tooexchange službou IoT Hub.  

Modely obsahují definice hello událostí můžete příjem příchozích dat tooIoT centra (hello *data*) a také můžete přijímat ze služby IoT Hub zprávy hello (hello *akce*). Jak je vidět z příkladu hello události mít typ a název; název a volitelné parametry (každý s typem) mají akce.

Co není ukázáno v této ukázce jsou další datové typy, které jsou podporovány hello SDK. Jsme zaměříme tento další.

> [!NOTE]
> IoT Hub odkazuje toohello data zařízení odesílá tooit jako *události*, zatímco hello Modelovací jazyk odkazuje tooit jako *data* (definovat pomocí **WITH_DATA**). Podobně IoT Hub odkazuje toohello data, která odešlete toodevices jako *zprávy*, zatímco hello Modelovací jazyk odkazuje tooit jako *akce* (definovat pomocí **WITH_ACTION**). Upozorňujeme, že tyto podmínky mohou být použity zcela zaměnitelným významem v tomto článku.
> 
> 

### <a name="supported-data-types"></a>Podporované datové typy
v modelů vytvořených pomocí hello jsou podporovány následující typy dat Hello **serializátor** knihovny:

| Typ | Popis |
| --- | --- |
| Double |Dvojitá přesnost číslo s plovoucí desetinnou |
| celá čísla |32bitové celé číslo |
| Plovoucí desetinná čárka |číslo s plovoucí desetinnou jednoduchou přesností |
| dlouhá |dlouhé celé číslo |
| int8\_t |8bitové celé číslo |
| Int16\_t |16bitové celé číslo |
| Int32\_t |32bitové celé číslo |
| Int64\_t |64bitové celé číslo |
| BOOL |Logická hodnota |
| ASCII\_char\_ptr |Řetězec ASCII |
| EDM\_DATUM\_ČAS\_POSUNUTÍ |Datum čas posunutí |
| EDM\_GUID |IDENTIFIKÁTOR GUID |
| EDM\_BINÁRNÍ |Binární |
| DEKLAROVAT\_– STRUKTURA |Komplexní datový typ |

Začněme s datovým typem poslední hello. Hello **DECLARE\_struktura** můžete toodefine komplexní datové typy, které jsou seskupení hello jiné primitivní typy. Tato seskupení nám umožňují toodefine model, který vypadá takto:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Naše model obsahuje událostí jednoho datového typu **TestType**. **TestType** jedná o komplexní typ, který zahrnuje několik členů, které se souhrnně ukazují hello primitivní typy podporované systémem hello **serializátor** Modelovací jazyk.

S modelem takhle jsme zápisu kódu toosend data tooIoT rozbočovače, který se zobrazí následujícím způsobem:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

V podstatě, jsme se přiřazení členem hodnota tooevery hello **Test** strukturu a pak volání **SendAsync** toosend hello **Test** toohello dat události v cloudu. **SendAsync** je pomocné funkce, která odesílá události tooIoT jednoho datového centra:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate hello string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Tato funkce serializuje hello určitá událost data a odešle ji tooIoT centra pomocí **IoTHubClient\_SendEventAsync**. Toto je stejný kód popsané v předchozí články hello (**SendAsync** zapouzdří hello logiku do vhodného funkce).

Jeden další pomocné funkce použitá v předchozí kód hello je **GetDateTimeOffset**. Tato funkce transformuje zadaný čas do hodnoty typu hello **EDM\_datum\_čas\_posun**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Pokud spustíte tento kód, je odeslána hello následující zprávou tooIoT rozbočovače:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Upozorňujeme, že hello serializace JSON, který je formát hello generované hello **serializátor** knihovny. Také Všimněte si, že každý člen hello serializovat objekt JSON odpovídající hello členy hello **TestType** , definovaného v našem modelu. hodnoty Hello také přesně odpovídat používaných v kódu hello. Ale Všimněte si, že binární data hello kódováním base64: "AQID" je text hello, kódování base64 {0x01, 0x02, 0x03}.

Tento příklad ukazuje hello výhodou použití hello **serializátor** knihovna – umožňuje nám toosend JSON toohello cloudu, bez nutnosti tooexplicitly řešit serializace v naší aplikaci. Všechny, které máme tooworry o je nastavení hello hodnot hello dat události v našich modelu a poté volání jednoduché rozhraní API toosend tyto události toohello cloudu.

Pomocí těchto informací jsme můžete definovat modelů, které obsahují hello rozsah podporované datové typy, včetně komplexní typy (může i zahrnuta komplexních typů v rámci jiné komplexní typy). Však mohl serializovat JSON vygenerované pomocí hello příkladu výše, na které se vyvolá bod důležité. *Jak* jsme odesílat data s hello **serializátor** knihovny určuje přesně jak je vytvořen hello JSON. Zda je konkrétní bodu, co jsme zaměříme Další.

## <a name="more-about-serialization"></a>Další informace o serializaci
Příklad výstupu hello generované hello označuje předchozí části Hello **serializátor** knihovny. V této části vám objasníme, jak hello knihovně serializuje data a jak můžete řídit tohoto chování pomocí hello serializace rozhraní API.

V pořadí tooadvance hello zabývat serializace jsme budete pracovat s nový model podle termostatu. Nejprve umožňuje zadat základní na scénáři hello pokoušíme tooaddress.

Chceme toomodel termostatu, které měří teploty a vlhkosti. Každá položka dat bude toobe jinak odeslané tooIoT rozbočovače. Ve výchozím nastavení hello termostatu ingresses teploty událostí jednou za 2 minut; událost vlhkosti je ingressed jednou za 15 minut. Pokud se buď událostí ingressed, musí obsahovat časové razítko, který zobrazuje čas hello této odpovídající teplotě hello nebo vlhkosti se měří.

Zadané v tomto scénáři, budete ukážeme dva různé způsoby toomodel hello dat a vysvětlíme hello vliv, modelování má na hello serializovat výstup.

### <a name="model-1"></a>Model 1
Tady je hello první verzi modelu, podporuje hello předchozí situaci:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Všimněte si, že hello modelu zahrnuje dvě data události: **teploty** a **vlhkosti**. Na rozdíl od předchozích příkladech hello typu jednotlivých událostí je struktura definovat pomocí **DECLARE\_struktura**. **TemperatureEvent** zahrnuje měření teploty a časové razítko; **HumidityEvent** obsahuje měření vlhkosti a časové razítko. Tento model nám dává dat přirozené způsob toomodel hello hello scénář popsaný výše. Když jsme poslat svůj cloud toohello událostí, buď pošleme teploty/časové razítko nebo pár vlhkosti/časové razítko.

Vám můžeme poslat cloudu toohello událostí teploty pomocí kódu třeba hello následující:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Jsme budete používat pevně definovaných hodnot pro teploty a vlhkosti v hello ukázkový kód, ale Představte si, že jsme se ve skutečnosti načítání tyto hodnoty vzorkováním hello odpovídající senzorů na termostatu hello.

výše uvedený kód Hello používá hello **GetDateTimeOffset** pomocné rutiny, která byla zavedená dříve. Z důvodů, které se stanou zrušte novější se tento kód explicitně odděluje hello úlohy serializaci a odesílat události hello. Předchozí kód Hello serializuje hello teploty událostí do vyrovnávací paměti. Potom **sendMessage** je pomocné funkce (součástí **simplesample\_amqp**), odešle hello event tooIoT Hub:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Tento kód je podmnožinou hello **SendAsync** pomocné rutiny popsané v předchozí části hello, takže jsme nebude projít ho znovu sem.

Při spuštění jsme hello předchozí kód toosend hello teploty událostí, tento serializovaných formulář hello událostí je odeslána tooIoT rozbočovače:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Byla odeslána teploty, které je typu **TemperatureEvent** a že struktura obsahuje **teploty** a **čas** člen. To se projeví přímo v datech hello serializovat.

Podobně vám můžeme poslat vlhkosti událostí s tímto kódem:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Hello serializovat formulář, který odeslal tooIoT rozbočovače vypadá takto:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

To je opět podle očekávání.

V tomto modelu, který chcete mít jak další události lze snadno přidat. Můžete definovat další struktur pomocí **DECLARE\_struktura**a zahrnovat hello odpovídající události v hello pomocí modelu **WITH\_DATA**.

Nyní Pojďme upravit hello modelu tak, že obsahují hello stejná data, ale s jinou strukturu.

### <a name="model-2"></a>Model 2
Vezměte v úvahu tato alternativní modelu toohello jeden výše:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

V takovém případě jsme vyloučili hello **DECLARE\_struktura** makra a jsou jednoduše definování hello datové položky z našich scénář pomocí jednoduché typy z hello Modelovací jazyk.

Jenom pro hello chvíli umožňuje ignorovat hello **čas** událostí. S vyhraďte, zde je kód tooingress hello **teploty**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tento kód odešle hello následující serializovat event tooIoT Hub:

```
{"Temperature":75}
```

A hello kód pro odesílání událostí vlhkosti hello vypadat takto:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Tento kód odešle tento tooIoT rozbočovače:

```
{"Humidity":45}
```

Pokud existují ještě žádné výskyt nečekaných událostí. Nyní změníme jak používáme makro hello SERIALIZACE.

Hello **SERIALIZACE** makro může trvat několik dat události jako argumenty. To umožňuje nám tooserialize hello **teploty** a **vlhkosti** událostí společně a jejich odeslání tooIoT rozbočovače jedno volání:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Může uhodnout, že hello výsledek tohoto kódu je, že dvě data události se posílají tooIoT rozbočovače:

[

{"Teploty": 75},

{"Vlhkosti": 45}

]

Jinými slovy, můžou očekávat, že tento kód je text hello stejné jako odesílání **teploty** a **vlhkosti** samostatně. Je právě usnadnění práce toopass obou události příliš**SERIALIZACE** v hello stejné volat. Ale to není hello případ. Místo toho výše uvedený kód hello odešle tento tooIoT událostí jednoho datového centra:

{"Teploty": 75, "vlhkosti": 45}

To může zdát neobvyklé, protože naše model definuje **teploty** a **vlhkosti** jako dvě *samostatné* události:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Další toohello bodu jsme nebyl model tyto události kde **teploty** a **vlhkosti** jsou v hello stejnou strukturu:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Pokud jsme použili tento model, by bylo snazší toounderstand jak **teploty** a **vlhkosti** mají být odeslány v hello stejné serializovat zprávy. Ale nemusí být zřejmé, proč funguje tímto způsobem při předání obou událostí data příliš**SERIALIZACE** pomocí modelu 2.

Toto chování je snazší toounderstand Pokud znáte hello předpoklady tohoto hello **serializátor** je zajistit knihovny. smyslu toomake Vraťme back tooour modelu:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Tento model si můžete představit v objektově orientované podmínky. V takovém případě jsme se modelování fyzického zařízení (termostatu) a zařízení zahrnuje atributů, například **teploty** a **vlhkosti**.

Vám můžeme poslat hello celý stav našeho modelu s kódem například hello následující:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Za předpokladu, že jsou nastavené hodnoty hello z teploty a vlhkosti, čas, jsme zobrazen událost podobně jako tento odeslané tooIoT rozbočovače:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

V některých případech může být pouze vhodné toosend *některé* vlastností hello modelu toohello cloudu (to je obzvláště hodnota true, pokud model obsahuje velké množství dat událostí). Je užitečné toosend pouze podmnožinu dat události, například v našem příkladu starší:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Tím se vygeneruje přesně hello stejné serializovat události, jako kdyby měl definovaného **TemperatureEvent** s **teploty** a **čas** člena, stejně jako jsme s modelu 1. V tomto případě nebylo možné toogenerate přesně hello stejné serializovat událostí pomocí jiného modelu (model 2), protože volali jsme **SERIALIZACE** jiným způsobem.

Hello bodu důležité je, že pokud předáte více dat událostí příliš**SERIALIZACE,** pak předpokládá, že každá událost je vlastnost v jeden objekt JSON.

Nejlepším postupem Hello závisí na a jak si myslíte o modelu. Pokud odesíláte "události" toohello cloudu a každé události, obsahuje sada vlastností, hello prvním přístupem je velké množství smysl. V takovém případě byste použili **DECLARE\_struktura** toodefine hello struktura jednotlivých událostí a zahrnout je do modelu pomocí hello **WITH\_DATA** makro. Každá událost se pak odeslat jako jsme to udělali v prvním příkladu hello výše. V tomto přístupu by pouze předáte jednoho datového událostí příliš**SERIALIZÁTOR**.

Pokud si myslíte o modelu způsobem objektově orientované, může vyhovovat hello druhý postup je. V takovém případě hello elementy definovat pomocí **WITH\_DATA** jsou hello "vlastnosti" objektu. Ať podmnožinu události předáte příliš**SERIALIZACE** , že se vám líbí, podle toho, kolik z vaší "" stav objektu chcete toosend toohello cloudu.

Nether přístup je pravé nebo nesprávné. Právě zajímat, jak hello **serializátor** funguje knihovny a vybrat hello modelování přístup, který nejlépe vyhovuje vašim potřebám.

## <a name="message-handling"></a>Zpracování zpráv
Tento článek, pokud má pouze popsané odesílání událostí tooIoT rozbočovače a nebyl řešit přijímání zpráv. Hello důvod pro to, co potřebujeme tooknow o přijímání zpráv má z velké části popsaná v [dříve článek](iot-hub-device-sdk-c-intro.md). Z tohoto článku odvolat, zpracování zpráv tak, že zaregistrujete funkci zpětného volání zpráva:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Potom napíšete hello funkce zpětného volání, která je volána, když je obdržena zprávu:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable tooIoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed toomalloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
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

Tato implementace **IoTHubMessage** volání hello konkrétní funkce pro každou akci do modelu. Například pokud váš model definuje tato akce:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Je nutné definovat funkce s Tento podpis:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** pak je volána při odeslání zprávy tooyour zařízení.

Co jsme nebyly vysvětlené je ještě Jaká zpráva vypadá jako verze hello serializovat. Jinými slovy Pokud chcete, aby toosend **SetAirResistance** zpráva tooyour zařízení, co typu?

Pokud chcete odeslat zprávu tooa zařízení, krok lze provést pomocí sady SDK služby Azure IoT hello. Je stále nutné tooknow řetězec toosend tooinvoke určité akce. Hello obecný formát pro odesílání zprávy vypadá takto:

```
{"Name" : "", "Parameters" : "" }
```

Odesíláte serializovaný objekt JSON s dvě vlastnosti: **název** je hello název akce hello (zprávy) a **parametry** obsahuje parametry hello této akce.

Například tooinvoke **SetAirResistance** můžete odeslat tuto zprávu tooa zařízení:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Název akce Hello musí přesně shodovat akce definované v modelu. Hello názvy parametrů musí odpovídat také. Všimněte si také malá a velká písmena. **Název** a **parametry** jsou vždy velká písmena. Ujistěte se, že případ hello toomatch názvu akce a parametrů do modelu. V tomto příkladu je název akce hello "SetAirResistance" a není "setairresistance".

Hello dva další akce **TurnFanOn** a **TurnFanOff** nelze vyvolat odesláním těchto zpráv tooa zařízení:

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Tato část popisuje všechno, co potřebujete tooknow při odesílání událostí a přijímání zpráv s hello **serializátor** knihovny. Než budete pokračovat, můžeme zahrnují některé parametry, které můžete nakonfigurovat, které řídí, jak velké je váš model.

## <a name="macro-configuration"></a>Konfigurace – makro
Pokud používáte hello **serializátor** knihovny důležitou součástí toobe SDK hello vědět, se nachází v hello azure-c sdílené – knihovna nástrojů.
Při naklonování úložiště hello Azure-iot-sdk-c z Githubu pomocí možnosti--rekurzivní hello zjistíte této sdílené nástroj knihovny:

```
.\\c-utility
```

Pokud nebyly klonovat hello knihovně, můžete ji najít [zde](https://github.com/Azure/azure-c-shared-utility).

V rámci hello nástroj sdílené knihovny zjistíte hello následující složku:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Tato složka obsahuje řešení sady Visual Studio názvem **makro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Hello program v tomto řešení generuje hello **makro\_utils.h** souboru. Je výchozí makro\_utils.h souboru součástí hello SDK. Toto řešení vám umožní toomodify některé parametry a pak znovu vytvořte hello hlavičkový soubor na základě těchto parametrů.

Hello dva klíče parametry toobe problémem jsou **nArithmetic** a **nMacroParameters** které jsou definované v těchto dvou řádcích najít v makro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Tyto hodnoty jsou součástí hello SDK hello výchozí parametry. Každý parametr má hello následující význam:

* nMacroParameters – Určuje, kolik parametry může mít v jedné DECLARE\_definici makra modelu.
* nArithmetic – ovládací prvky hello celkový počet členů v modelu povolené.

Důvod Hello, které tyto parametry jsou důležité je, protože tím i určovat, jak velká může být váš model. Představte si třeba tuto definici modelu:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Jak je uvedeno nahoře, **DECLARE\_modelu** je právě makro C. Hello názvy hello modelu a hello **WITH\_DATA** – příkaz (ještě jiné makro) jsou parametry **DECLARE\_modelu**. **nMacroParameters** definuje, kolik parametry můžou být součástí **DECLARE\_modelu**. Definuje efektivně, kolik dat událostí a akce deklarace může mít. Jako takový hello výchozí limit 124 to znamená, že můžete definovat model pomocí kombinace o 60 akce a data události. Pokud se pokusíte tooexceed tento limit, dostanete chyby kompilátoru, které vypadají podobně jako toothis:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

Hello **nArithmetic** parametr je více informací o interní chodu hello hello makro jazyka než vaší aplikace.  Ovládá hello celkový počet členů, může mít v modelu, včetně **DECLARE_STRUCT** makra. Pokud spustíte zobrazuje chyby kompilátoru, jako je tato, pak doporučujeme zvýšit **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Pokud chcete tyto parametry toochange, upravte hodnoty hello v hello makro\_utils.tt soubor, pak ji znovu zkompilovat hello makro\_utils\_h\_generator.sln řešení a spuštění hello zkompilovaný program. Když uděláte, nové makro\_utils.h souboru je generována a umístit do hello.\\ běžné\\inc adresáře.

V pořadí toouse hello novou verzi makro\_utils.h, odeberte hello **serializátor** balíček NuGet, řešení a v jeho místní zahrnovat hello **serializátor** projektu sady Visual Studio. To umožňuje váš kód toocompile proti hello zdrojového kódu hello serializátor knihovny. To zahrnuje hello aktualizovat makro\_utils.h. Pokud chcete, aby toodo to pro **simplesample\_amqp**, začněte tím, že balíček NuGet hello hello serializátor knihovny odebráním hello řešení:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Pak přidejte tento projekt tooyour řešení sady Visual Studio:

> . \\c\\serializátor\\sestavení\\windows\\serializer.vcxproj
> 
> 

Když jste hotovi, řešení by měl vypadat přibližně takto:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Teď když zkompilujete řešení, hello aktualizovat makro\_utils.h je součástí vaší binární.

Všimněte si, že zvýšení tyto hodnoty dostatečně vysoký může překročit omezení kompilátoru. toothis bod, hello **nMacroParameters** je hlavní parametr hello s které toobe obavy. Specifikace C99 Hello Určuje, že jsou definice makra povolen minimálně 127 parametry. Hello kompilátoru Microsoft přesně dodržuje specifikaci hello (a může obsahovat maximálně 127 znaků), takže nebudete moct tooincrease **nMacroParameters** nad výchozí hello. Další kompilátory může umožňují toodo tak (například hello GNU kompilátoru podporuje vyšší limit).

Dosavadní téměř všechno, co potřebujete tooknow o tom, jak toowrite kód s hello jste zahrnutých jsme **serializátor** knihovny. Před uzavřením, můžeme pokroku některá témata z předchozí články, které asi vás zajímá o.

## <a name="hello-lower-level-apis"></a>Hello nižší úrovně rozhraní API
je Hello ukázkovou aplikaci, na kterém se tento článek zaměřuje **simplesample\_amqp**. Tato ukázka používá hello vyšší úrovně (hello bez – "vše") rozhraní API toosend události a přijímat zprávy. Používáte-li tato rozhraní API, spustí vlákna na pozadí, který se stará o odesílání událostí i přijímání zpráv. Můžete však použít hello nižší úrovni (vše) rozhraní API tooeliminate tento vlákna na pozadí a trvat explicitní kontrolu nad při odesílání událostí nebo příjem zpráv z cloudu hello.

Jak je popsáno v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md), je sada funkcí, které se skládá z hello vyšší úrovně rozhraní API:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_Destroy

Tato rozhraní API je ukázán v **simplesample\_amqp**.

K dispozici je také podobá sadu rozhraní API nižší úrovně.

* IoTHubClient\_UDOU\_CreateFromConnectionString
* IoTHubClient\_UDOU\_SendEventAsync
* IoTHubClient\_UDOU\_SetMessageCallback
* IoTHubClient\_UDOU\_Destroy

Všimněte si, že hello nižší úrovně rozhraní API pracovní přesně hello stejným způsobem, jak popsané v předchozí článcích hello. Hello první sadu rozhraní API můžete použít, pokud chcete, aby vlákno toohandle pozadí, události odesílání a přijímání zpráv. Pokud chcete, aby explicitní ovládat, kdy odesílat a přijímat data ze služby IoT Hub použijete hello druhá sada rozhraní API. Buď sadu rozhraní API pro práci s hello stejně dobře **serializátor** knihovny.

Pro příklad hello nižší úrovně používají rozhraní API s hello **serializátor** knihovny, najdete v části hello **simplesample\_http** aplikace.

## <a name="additional-topics"></a>Další témata
Několik dalších tématech důležité zmínit, znovu se vlastnost zpracování, pomocí přihlašovacích údajů jiného zařízení a možnosti konfigurace. Toto jsou všechna témata popsaná v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md). Hello hlavní bod je, že všechny tyto funkce pracovat v hello stejný způsob s hello **serializátor** knihovny stejně jako s hello **IoTHubClient** knihovny. Pokud chcete tooattach vlastnosti tooan událostí z modelu, můžete použít například **IoTHubMessage\_vlastnosti** a **mapy**\_**AddorUpdate**, stejným způsobem, jak jsme je už popsali hello:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Jestli se vygeneroval hello událostí ze hello **serializátor** knihovny nebo vytvořit ručně pomocí hello **IoTHubClient** knihovny nezáleží.

Pro hello alternativní přihlašovací údaje zařízení, pomocí **IoTHubClient\_UDOU\_vytvořit** funguje stejně dobře jako **IoTHubClient\_CreateFromConnectionString** pro přidělování **IOTHUB\_klienta\_zpracování**.

Nakonec pokud používáte hello **serializátor** knihovny, můžete nastavit možnosti konfigurace s **IoTHubClient\_UDOU\_SetOption** stejně jako jste to udělali při použití hello **IoTHubClient** knihovny.

Funkce, která je jedinečný toohello **serializátor** knihovny jsou hello inicializace rozhraní API. Před zahájením práce s hello knihovně, musí volat **serializátor\_init**:

```
serializer_init(NULL);
```

To se provádí těsně před voláním **IoTHubClient\_CreateFromConnectionString**.

Podobně po dokončení práce s hello knihovně, budete provedete posledním volání hello je příliš**serializátor\_deinit**:

```
serializer_deinit();
```

Jinak, všechny další funkce uvedené výše hello pracovní stejné hello v hello **serializátor** knihovny stejně jako v hello **IoTHubClient** knihovny. Další informace o všech těchto tématech najdete v tématu hello [předchozí článek](iot-hub-device-sdk-c-iothubclient.md) této série.

## <a name="next-steps"></a>Další kroky
Tento článek popisuje, v podrobností hello jedinečné aspekty hello **serializátor** obsažené v hello knihovně **zařízení Azure IoT SDK pro jazyk C**. S hello informací by měl mít dostatečné povědomí o tom, jak toouse modelů toosend události a přijímat zprávy ze služby IoT Hub.

To se taky ukončí hello třemi částmi řady na jak hello toodevelop aplikace s **zařízení Azure IoT SDK pro jazyk C**. To by měl být dostatek informace toonot pouze get, kterou jste zahájili ale získáte důkladné znalosti o fungování hello rozhraní API. Další informace naleznete několik ukázky v nejsou hello SDK nejsou zahrnuté v tomto poli. V opačném hello [dokumentaci k sadě SDK](https://github.com/Azure/azure-iot-sdk-c) je dobré prostředku pro další informace.

toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s Azure IoT Edge][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
