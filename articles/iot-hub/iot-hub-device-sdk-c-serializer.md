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
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="cbf42-103">Pro zařízení Azure IoT SDK pro jazyk C – informace o serializátor</span><span class="sxs-lookup"><span data-stu-id="cbf42-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="cbf42-104">Hello [nejprve článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C**. hello následující článek poskytuje podrobnější popis hello [ **IoTHubClient** ](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="cbf42-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. hello next article provided a more detailed description of hello [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="cbf42-105">Tento článek poskytuje podrobnější popis hello zbývající součásti dokončí pokrytí hello SDK: hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-105">This article completes coverage of hello SDK by providing a more detailed description of hello remaining component: hello **serializer** library.</span></span>

<span data-ttu-id="cbf42-106">Hello úvodní článek popisuje, jak toouse hello **serializátor** knihovny toosend události tooand přijímat zprávy ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cbf42-106">hello introductory article described how toouse hello **serializer** library toosend events tooand receive messages from IoT Hub.</span></span> <span data-ttu-id="cbf42-107">V tomto článku jsme rozšířit tento diskuzi o podrobnější vysvětlení způsobu toomodel svá data pomocí hello **serializátor** makro jazyk.</span><span class="sxs-lookup"><span data-stu-id="cbf42-107">In this article, we extend that discussion by providing a more complete explanation of how toomodel your data with hello **serializer** macro language.</span></span> <span data-ttu-id="cbf42-108">článek Hello také obsahuje další podrobnosti o tom, jak hello knihovně serializuje zprávy (a v některých případech jak můžete řídit chování serializace hello).</span><span class="sxs-lookup"><span data-stu-id="cbf42-108">hello article also includes more detail about how hello library serializes messages (and in some cases how you can control hello serialization behavior).</span></span> <span data-ttu-id="cbf42-109">Popíšeme také některé parametry, které můžete upravit určující velikost hello hello modelů, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="cbf42-109">We'll also describe some parameters you can modify that determine hello size of hello models you create.</span></span>

<span data-ttu-id="cbf42-110">Nakonec hello článku znovu navštíví některá témata popsaná v předchozích články například zprávu a vlastnosti zpracování.</span><span class="sxs-lookup"><span data-stu-id="cbf42-110">Finally, hello article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="cbf42-111">Jak jsme najdete na těchto pracovních funkcí v hello stejným způsobem pomocí hello **serializátor** knihovny stejně jako s hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-111">As we'll find out, those features work in hello same way using hello **serializer** library as they do with hello **IoTHubClient** library.</span></span>

<span data-ttu-id="cbf42-112">Všechno, co popsané v tomto článku je založena na hello **serializátor** ukázky SDK.</span><span class="sxs-lookup"><span data-stu-id="cbf42-112">Everything described in this article is based on hello **serializer** SDK samples.</span></span> <span data-ttu-id="cbf42-113">Pokud chcete toofollow společně, najdete v části hello **simplesample\_amqp** a **simplesample\_http** aplikace, které jsou součástí zařízení Azure IoT hello SDK pro C.</span><span class="sxs-lookup"><span data-stu-id="cbf42-113">If you want toofollow along, see hello **simplesample\_amqp** and **simplesample\_http** applications included in hello Azure IoT device SDK for C.</span></span>

<span data-ttu-id="cbf42-114">Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="cbf42-114">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-modeling-language"></a><span data-ttu-id="cbf42-115">představuje Modelovací jazyk Hello</span><span class="sxs-lookup"><span data-stu-id="cbf42-115">hello modeling language</span></span>
<span data-ttu-id="cbf42-116">Hello [úvodní článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C** Modelovací jazyk prostřednictvím hello příkladu v hello **simplesample\_amqp**  aplikace:</span><span class="sxs-lookup"><span data-stu-id="cbf42-116">hello [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C** modeling language through hello example provided in hello **simplesample\_amqp** application:</span></span>

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

<span data-ttu-id="cbf42-117">Jak je vidět, hello Modelovací jazyk je založena na maker v jazyce C.</span><span class="sxs-lookup"><span data-stu-id="cbf42-117">As you can see, hello modeling language is based on C macros.</span></span> <span data-ttu-id="cbf42-118">Je třeba nejprve vaší definice s **BEGIN\_obor názvů** a vždy končit **END\_obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="cbf42-119">Je běžné tooname hello obor názvů pro vaši společnost nebo, jako v následujícím příkladě hello projekt, který právě pracujete.</span><span class="sxs-lookup"><span data-stu-id="cbf42-119">It's common tooname hello namespace for your company or, as in this example, hello project that you're working on.</span></span>

<span data-ttu-id="cbf42-120">Co v oboru názvů hello jsou definice modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-120">What goes inside hello namespace are model definitions.</span></span> <span data-ttu-id="cbf42-121">V takovém případě je jeden model anemometer.</span><span class="sxs-lookup"><span data-stu-id="cbf42-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="cbf42-122">Ještě jednou hello modelu může být název nic, ale to má obvykle název pro hello zařízení nebo typ dat chcete tooexchange službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cbf42-122">Once again, hello model can be named anything, but typically this is named for hello device or type of data you want tooexchange with IoT Hub.</span></span>  

<span data-ttu-id="cbf42-123">Modely obsahují definice hello událostí můžete příjem příchozích dat tooIoT centra (hello *data*) a také můžete přijímat ze služby IoT Hub zprávy hello (hello *akce*).</span><span class="sxs-lookup"><span data-stu-id="cbf42-123">Models contain a definition of hello events you can ingress tooIoT Hub (hello *data*) as well as hello messages you can receive from IoT Hub (hello *actions*).</span></span> <span data-ttu-id="cbf42-124">Jak je vidět z příkladu hello události mít typ a název; název a volitelné parametry (každý s typem) mají akce.</span><span class="sxs-lookup"><span data-stu-id="cbf42-124">As you can see from hello example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="cbf42-125">Co není ukázáno v této ukázce jsou další datové typy, které jsou podporovány hello SDK.</span><span class="sxs-lookup"><span data-stu-id="cbf42-125">What’s not demonstrated in this sample are additional data types that are supported by hello SDK.</span></span> <span data-ttu-id="cbf42-126">Jsme zaměříme tento další.</span><span class="sxs-lookup"><span data-stu-id="cbf42-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="cbf42-127">IoT Hub odkazuje toohello data zařízení odesílá tooit jako *události*, zatímco hello Modelovací jazyk odkazuje tooit jako *data* (definovat pomocí **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="cbf42-127">IoT Hub refers toohello data a device sends tooit as *events*, while hello modeling language refers tooit as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="cbf42-128">Podobně IoT Hub odkazuje toohello data, která odešlete toodevices jako *zprávy*, zatímco hello Modelovací jazyk odkazuje tooit jako *akce* (definovat pomocí **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="cbf42-128">Likewise, IoT Hub refers toohello data you send toodevices as *messages*, while hello modeling language refers tooit as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="cbf42-129">Upozorňujeme, že tyto podmínky mohou být použity zcela zaměnitelným významem v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="cbf42-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="cbf42-130">Podporované datové typy</span><span class="sxs-lookup"><span data-stu-id="cbf42-130">Supported data types</span></span>
<span data-ttu-id="cbf42-131">v modelů vytvořených pomocí hello jsou podporovány následující typy dat Hello **serializátor** knihovny:</span><span class="sxs-lookup"><span data-stu-id="cbf42-131">hello following data types are supported in models created with hello **serializer** library:</span></span>

| <span data-ttu-id="cbf42-132">Typ</span><span class="sxs-lookup"><span data-stu-id="cbf42-132">Type</span></span> | <span data-ttu-id="cbf42-133">Popis</span><span class="sxs-lookup"><span data-stu-id="cbf42-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="cbf42-134">Double</span><span class="sxs-lookup"><span data-stu-id="cbf42-134">double</span></span> |<span data-ttu-id="cbf42-135">Dvojitá přesnost číslo s plovoucí desetinnou</span><span class="sxs-lookup"><span data-stu-id="cbf42-135">double precision floating point number</span></span> |
| <span data-ttu-id="cbf42-136">celá čísla</span><span class="sxs-lookup"><span data-stu-id="cbf42-136">int</span></span> |<span data-ttu-id="cbf42-137">32bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-137">32 bit integer</span></span> |
| <span data-ttu-id="cbf42-138">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="cbf42-138">float</span></span> |<span data-ttu-id="cbf42-139">číslo s plovoucí desetinnou jednoduchou přesností</span><span class="sxs-lookup"><span data-stu-id="cbf42-139">single precision floating point number</span></span> |
| <span data-ttu-id="cbf42-140">dlouhá</span><span class="sxs-lookup"><span data-stu-id="cbf42-140">long</span></span> |<span data-ttu-id="cbf42-141">dlouhé celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-141">long integer</span></span> |
| <span data-ttu-id="cbf42-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="cbf42-142">int8\_t</span></span> |<span data-ttu-id="cbf42-143">8bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-143">8 bit integer</span></span> |
| <span data-ttu-id="cbf42-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="cbf42-144">int16\_t</span></span> |<span data-ttu-id="cbf42-145">16bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-145">16 bit integer</span></span> |
| <span data-ttu-id="cbf42-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="cbf42-146">int32\_t</span></span> |<span data-ttu-id="cbf42-147">32bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-147">32 bit integer</span></span> |
| <span data-ttu-id="cbf42-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="cbf42-148">int64\_t</span></span> |<span data-ttu-id="cbf42-149">64bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="cbf42-149">64 bit integer</span></span> |
| <span data-ttu-id="cbf42-150">BOOL</span><span class="sxs-lookup"><span data-stu-id="cbf42-150">bool</span></span> |<span data-ttu-id="cbf42-151">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="cbf42-151">boolean</span></span> |
| <span data-ttu-id="cbf42-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="cbf42-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="cbf42-153">Řetězec ASCII</span><span class="sxs-lookup"><span data-stu-id="cbf42-153">ASCII string</span></span> |
| <span data-ttu-id="cbf42-154">EDM\_DATUM\_ČAS\_POSUNUTÍ</span><span class="sxs-lookup"><span data-stu-id="cbf42-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="cbf42-155">Datum čas posunutí</span><span class="sxs-lookup"><span data-stu-id="cbf42-155">date time offset</span></span> |
| <span data-ttu-id="cbf42-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="cbf42-156">EDM\_GUID</span></span> |<span data-ttu-id="cbf42-157">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="cbf42-157">GUID</span></span> |
| <span data-ttu-id="cbf42-158">EDM\_BINÁRNÍ</span><span class="sxs-lookup"><span data-stu-id="cbf42-158">EDM\_BINARY</span></span> |<span data-ttu-id="cbf42-159">Binární</span><span class="sxs-lookup"><span data-stu-id="cbf42-159">binary</span></span> |
| <span data-ttu-id="cbf42-160">DEKLAROVAT\_– STRUKTURA</span><span class="sxs-lookup"><span data-stu-id="cbf42-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="cbf42-161">Komplexní datový typ</span><span class="sxs-lookup"><span data-stu-id="cbf42-161">complex data type</span></span> |

<span data-ttu-id="cbf42-162">Začněme s datovým typem poslední hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-162">Let’s start with hello last data type.</span></span> <span data-ttu-id="cbf42-163">Hello **DECLARE\_struktura** můžete toodefine komplexní datové typy, které jsou seskupení hello jiné primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="cbf42-163">hello **DECLARE\_STRUCT** allows you toodefine complex data types, which are groupings of hello other primitive types.</span></span> <span data-ttu-id="cbf42-164">Tato seskupení nám umožňují toodefine model, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cbf42-164">These groupings allow us toodefine a model that looks like this:</span></span>

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

<span data-ttu-id="cbf42-165">Naše model obsahuje událostí jednoho datového typu **TestType**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="cbf42-166">**TestType** jedná o komplexní typ, který zahrnuje několik členů, které se souhrnně ukazují hello primitivní typy podporované systémem hello **serializátor** Modelovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="cbf42-166">**TestType** is a complex type that includes several members, which collectively demonstrate hello primitive types supported by hello **serializer** modeling language.</span></span>

<span data-ttu-id="cbf42-167">S modelem takhle jsme zápisu kódu toosend data tooIoT rozbočovače, který se zobrazí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cbf42-167">With a model like this, we can write code toosend data tooIoT Hub that appears as follows:</span></span>

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

<span data-ttu-id="cbf42-168">V podstatě, jsme se přiřazení členem hodnota tooevery hello **Test** strukturu a pak volání **SendAsync** toosend hello **Test** toohello dat události v cloudu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-168">Basically, we’re assigning a value tooevery member of hello **Test** structure and then calling **SendAsync** toosend hello **Test** data event toohello cloud.</span></span> <span data-ttu-id="cbf42-169">**SendAsync** je pomocné funkce, která odesílá události tooIoT jednoho datového centra:</span><span class="sxs-lookup"><span data-stu-id="cbf42-169">**SendAsync** is a helper function that sends a single data event tooIoT Hub:</span></span>

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

<span data-ttu-id="cbf42-170">Tato funkce serializuje hello určitá událost data a odešle ji tooIoT centra pomocí **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-170">This function serializes hello given data event and sends it tooIoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="cbf42-171">Toto je stejný kód popsané v předchozí články hello (**SendAsync** zapouzdří hello logiku do vhodného funkce).</span><span class="sxs-lookup"><span data-stu-id="cbf42-171">This is hello same code discussed in previous articles (**SendAsync** encapsulates hello logic into a convenient function).</span></span>

<span data-ttu-id="cbf42-172">Jeden další pomocné funkce použitá v předchozí kód hello je **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-172">One other helper function used in hello previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="cbf42-173">Tato funkce transformuje zadaný čas do hodnoty typu hello **EDM\_datum\_čas\_posun**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-173">This function transforms hello given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

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

<span data-ttu-id="cbf42-174">Pokud spustíte tento kód, je odeslána hello následující zprávou tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="cbf42-174">If you run this code, hello following message is sent tooIoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="cbf42-175">Upozorňujeme, že hello serializace JSON, který je formát hello generované hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-175">Note that hello serialization is JSON, which is hello format generated by hello **serializer** library.</span></span> <span data-ttu-id="cbf42-176">Také Všimněte si, že každý člen hello serializovat objekt JSON odpovídající hello členy hello **TestType** , definovaného v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-176">Also note that each member of hello serialized JSON object matches hello members of hello **TestType** that we defined in our model.</span></span> <span data-ttu-id="cbf42-177">hodnoty Hello také přesně odpovídat používaných v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-177">hello values also exactly match those used in hello code.</span></span> <span data-ttu-id="cbf42-178">Ale Všimněte si, že binární data hello kódováním base64: "AQID" je text hello, kódování base64 {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="cbf42-178">However, note that hello binary data is base64-encoded: "AQID" is hello base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="cbf42-179">Tento příklad ukazuje hello výhodou použití hello **serializátor** knihovna – umožňuje nám toosend JSON toohello cloudu, bez nutnosti tooexplicitly řešit serializace v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cbf42-179">This example demonstrates hello advantage of using hello **serializer** library -- it enables us toosend JSON toohello cloud, without having tooexplicitly deal with serialization in our application.</span></span> <span data-ttu-id="cbf42-180">Všechny, které máme tooworry o je nastavení hello hodnot hello dat události v našich modelu a poté volání jednoduché rozhraní API toosend tyto události toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-180">All we have tooworry about is setting hello values of hello data events in our model and then calling simple APIs toosend those events toohello cloud.</span></span>

<span data-ttu-id="cbf42-181">Pomocí těchto informací jsme můžete definovat modelů, které obsahují hello rozsah podporované datové typy, včetně komplexní typy (může i zahrnuta komplexních typů v rámci jiné komplexní typy).</span><span class="sxs-lookup"><span data-stu-id="cbf42-181">With this information, we can define models that include hello range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="cbf42-182">Však mohl serializovat JSON vygenerované pomocí hello příkladu výše, na které se vyvolá bod důležité.</span><span class="sxs-lookup"><span data-stu-id="cbf42-182">However, he serialized JSON generated by hello example above brings up an important point.</span></span> <span data-ttu-id="cbf42-183">*Jak* jsme odesílat data s hello **serializátor** knihovny určuje přesně jak je vytvořen hello JSON.</span><span class="sxs-lookup"><span data-stu-id="cbf42-183">*How* we send data with hello **serializer** library determines exactly how hello JSON is formed.</span></span> <span data-ttu-id="cbf42-184">Zda je konkrétní bodu, co jsme zaměříme Další.</span><span class="sxs-lookup"><span data-stu-id="cbf42-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="cbf42-185">Další informace o serializaci</span><span class="sxs-lookup"><span data-stu-id="cbf42-185">More about serialization</span></span>
<span data-ttu-id="cbf42-186">Příklad výstupu hello generované hello označuje předchozí části Hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-186">hello previous section highlights an example of hello output generated by hello **serializer** library.</span></span> <span data-ttu-id="cbf42-187">V této části vám objasníme, jak hello knihovně serializuje data a jak můžete řídit tohoto chování pomocí hello serializace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbf42-187">In this section, we'll explain how hello library serializes data and how you can control that behavior using hello serialization APIs.</span></span>

<span data-ttu-id="cbf42-188">V pořadí tooadvance hello zabývat serializace jsme budete pracovat s nový model podle termostatu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-188">In order tooadvance hello discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="cbf42-189">Nejprve umožňuje zadat základní na scénáři hello pokoušíme tooaddress.</span><span class="sxs-lookup"><span data-stu-id="cbf42-189">First, let's provide some background on hello scenario we're trying tooaddress.</span></span>

<span data-ttu-id="cbf42-190">Chceme toomodel termostatu, které měří teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="cbf42-190">We want toomodel a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="cbf42-191">Každá položka dat bude toobe jinak odeslané tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="cbf42-191">Each piece of data is going toobe sent tooIoT Hub differently.</span></span> <span data-ttu-id="cbf42-192">Ve výchozím nastavení hello termostatu ingresses teploty událostí jednou za 2 minut; událost vlhkosti je ingressed jednou za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="cbf42-192">By default, hello thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="cbf42-193">Pokud se buď událostí ingressed, musí obsahovat časové razítko, který zobrazuje čas hello této odpovídající teplotě hello nebo vlhkosti se měří.</span><span class="sxs-lookup"><span data-stu-id="cbf42-193">When either event is ingressed, it must include a timestamp that shows hello time that hello corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="cbf42-194">Zadané v tomto scénáři, budete ukážeme dva různé způsoby toomodel hello dat a vysvětlíme hello vliv, modelování má na hello serializovat výstup.</span><span class="sxs-lookup"><span data-stu-id="cbf42-194">Given this scenario, we'll demonstrate two different ways toomodel hello data, and we'll explain hello effect that modeling has on hello serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="cbf42-195">Model 1</span><span class="sxs-lookup"><span data-stu-id="cbf42-195">Model 1</span></span>
<span data-ttu-id="cbf42-196">Tady je hello první verzi modelu, podporuje hello předchozí situaci:</span><span class="sxs-lookup"><span data-stu-id="cbf42-196">Here's hello first version of a model that supports hello previous scenario:</span></span>

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

<span data-ttu-id="cbf42-197">Všimněte si, že hello modelu zahrnuje dvě data události: **teploty** a **vlhkosti**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-197">Note that hello model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="cbf42-198">Na rozdíl od předchozích příkladech hello typu jednotlivých událostí je struktura definovat pomocí **DECLARE\_struktura**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-198">Unlike previous examples, hello type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="cbf42-199">**TemperatureEvent** zahrnuje měření teploty a časové razítko; **HumidityEvent** obsahuje měření vlhkosti a časové razítko.</span><span class="sxs-lookup"><span data-stu-id="cbf42-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="cbf42-200">Tento model nám dává dat přirozené způsob toomodel hello hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="cbf42-200">This model gives us a natural way toomodel hello data for hello scenario described above.</span></span> <span data-ttu-id="cbf42-201">Když jsme poslat svůj cloud toohello událostí, buď pošleme teploty/časové razítko nebo pár vlhkosti/časové razítko.</span><span class="sxs-lookup"><span data-stu-id="cbf42-201">When we send an event toohello cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="cbf42-202">Vám můžeme poslat cloudu toohello událostí teploty pomocí kódu třeba hello následující:</span><span class="sxs-lookup"><span data-stu-id="cbf42-202">We can send a temperature event toohello cloud using code such as hello following:</span></span>

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

<span data-ttu-id="cbf42-203">Jsme budete používat pevně definovaných hodnot pro teploty a vlhkosti v hello ukázkový kód, ale Představte si, že jsme se ve skutečnosti načítání tyto hodnoty vzorkováním hello odpovídající senzorů na termostatu hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-203">We'll use hard-coded values for temperature and humidity in hello sample code, but imagine that we’re actually retrieving these values by sampling hello corresponding sensors on hello thermostat.</span></span>

<span data-ttu-id="cbf42-204">výše uvedený kód Hello používá hello **GetDateTimeOffset** pomocné rutiny, která byla zavedená dříve.</span><span class="sxs-lookup"><span data-stu-id="cbf42-204">hello code above uses hello **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="cbf42-205">Z důvodů, které se stanou zrušte novější se tento kód explicitně odděluje hello úlohy serializaci a odesílat události hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-205">For reasons that will become clear later, this code explicitly separates hello task of serializing and sending hello event.</span></span> <span data-ttu-id="cbf42-206">Předchozí kód Hello serializuje hello teploty událostí do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="cbf42-206">hello previous code serializes hello temperature event into a buffer.</span></span> <span data-ttu-id="cbf42-207">Potom **sendMessage** je pomocné funkce (součástí **simplesample\_amqp**), odešle hello event tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="cbf42-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends hello event tooIoT Hub:</span></span>

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

<span data-ttu-id="cbf42-208">Tento kód je podmnožinou hello **SendAsync** pomocné rutiny popsané v předchozí části hello, takže jsme nebude projít ho znovu sem.</span><span class="sxs-lookup"><span data-stu-id="cbf42-208">This code is a subset of hello **SendAsync** helper described in hello previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="cbf42-209">Při spuštění jsme hello předchozí kód toosend hello teploty událostí, tento serializovaných formulář hello událostí je odeslána tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="cbf42-209">When we run hello previous code toosend hello Temperature event, this serialized form of hello event is sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="cbf42-210">Byla odeslána teploty, které je typu **TemperatureEvent** a že struktura obsahuje **teploty** a **čas** člen.</span><span class="sxs-lookup"><span data-stu-id="cbf42-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="cbf42-211">To se projeví přímo v datech hello serializovat.</span><span class="sxs-lookup"><span data-stu-id="cbf42-211">This is directly reflected in hello serialized data.</span></span>

<span data-ttu-id="cbf42-212">Podobně vám můžeme poslat vlhkosti událostí s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="cbf42-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="cbf42-213">Hello serializovat formulář, který odeslal tooIoT rozbočovače vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cbf42-213">hello serialized form that’s sent tooIoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="cbf42-214">To je opět podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="cbf42-214">Again, this is as expected.</span></span>

<span data-ttu-id="cbf42-215">V tomto modelu, který chcete mít jak další události lze snadno přidat.</span><span class="sxs-lookup"><span data-stu-id="cbf42-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="cbf42-216">Můžete definovat další struktur pomocí **DECLARE\_struktura**a zahrnovat hello odpovídající události v hello pomocí modelu **WITH\_DATA**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-216">You define more structures using **DECLARE\_STRUCT**, and include hello corresponding event in hello model using **WITH\_DATA**.</span></span>

<span data-ttu-id="cbf42-217">Nyní Pojďme upravit hello modelu tak, že obsahují hello stejná data, ale s jinou strukturu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-217">Now, let’s modify hello model so that it includes hello same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="cbf42-218">Model 2</span><span class="sxs-lookup"><span data-stu-id="cbf42-218">Model 2</span></span>
<span data-ttu-id="cbf42-219">Vezměte v úvahu tato alternativní modelu toohello jeden výše:</span><span class="sxs-lookup"><span data-stu-id="cbf42-219">Consider this alternative model toohello one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="cbf42-220">V takovém případě jsme vyloučili hello **DECLARE\_struktura** makra a jsou jednoduše definování hello datové položky z našich scénář pomocí jednoduché typy z hello Modelovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="cbf42-220">In this case we've eliminated hello **DECLARE\_STRUCT** macros and are simply defining hello data items from our scenario using simple types from hello modeling language.</span></span>

<span data-ttu-id="cbf42-221">Jenom pro hello chvíli umožňuje ignorovat hello **čas** událostí.</span><span class="sxs-lookup"><span data-stu-id="cbf42-221">Just for hello moment let’s ignore hello **Time** event.</span></span> <span data-ttu-id="cbf42-222">S vyhraďte, zde je kód tooingress hello **teploty**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-222">With that aside, here’s hello code tooingress **Temperature**:</span></span>

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

<span data-ttu-id="cbf42-223">Tento kód odešle hello následující serializovat event tooIoT Hub:</span><span class="sxs-lookup"><span data-stu-id="cbf42-223">This code sends hello following serialized event tooIoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="cbf42-224">A hello kód pro odesílání událostí vlhkosti hello vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="cbf42-224">And hello code for sending hello Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="cbf42-225">Tento kód odešle tento tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="cbf42-225">This code sends this tooIoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="cbf42-226">Pokud existují ještě žádné výskyt nečekaných událostí.</span><span class="sxs-lookup"><span data-stu-id="cbf42-226">So far there are still no surprises.</span></span> <span data-ttu-id="cbf42-227">Nyní změníme jak používáme makro hello SERIALIZACE.</span><span class="sxs-lookup"><span data-stu-id="cbf42-227">Now let's change how we use hello SERIALIZE macro.</span></span>

<span data-ttu-id="cbf42-228">Hello **SERIALIZACE** makro může trvat několik dat události jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="cbf42-228">hello **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="cbf42-229">To umožňuje nám tooserialize hello **teploty** a **vlhkosti** událostí společně a jejich odeslání tooIoT rozbočovače jedno volání:</span><span class="sxs-lookup"><span data-stu-id="cbf42-229">This enables us tooserialize hello **Temperature** and **Humidity** event together and send them tooIoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="cbf42-230">Může uhodnout, že hello výsledek tohoto kódu je, že dvě data události se posílají tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="cbf42-230">You might guess that hello result of this code is that two data events are sent tooIoT Hub:</span></span>

<span data-ttu-id="cbf42-231">[</span><span class="sxs-lookup"><span data-stu-id="cbf42-231">[</span></span>

<span data-ttu-id="cbf42-232">{"Teploty": 75},</span><span class="sxs-lookup"><span data-stu-id="cbf42-232">{"Temperature":75},</span></span>

<span data-ttu-id="cbf42-233">{"Vlhkosti": 45}</span><span class="sxs-lookup"><span data-stu-id="cbf42-233">{"Humidity":45}</span></span>

<span data-ttu-id="cbf42-234">]</span><span class="sxs-lookup"><span data-stu-id="cbf42-234">]</span></span>

<span data-ttu-id="cbf42-235">Jinými slovy, můžou očekávat, že tento kód je text hello stejné jako odesílání **teploty** a **vlhkosti** samostatně.</span><span class="sxs-lookup"><span data-stu-id="cbf42-235">In other words, you might expect that this code is hello same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="cbf42-236">Je právě usnadnění práce toopass obou události příliš**SERIALIZACE** v hello stejné volat.</span><span class="sxs-lookup"><span data-stu-id="cbf42-236">It’s just a convenience toopass both events too**SERIALIZE** in hello same call.</span></span> <span data-ttu-id="cbf42-237">Ale to není hello případ.</span><span class="sxs-lookup"><span data-stu-id="cbf42-237">However, that’s not hello case.</span></span> <span data-ttu-id="cbf42-238">Místo toho výše uvedený kód hello odešle tento tooIoT událostí jednoho datového centra:</span><span class="sxs-lookup"><span data-stu-id="cbf42-238">Instead, hello code above sends this single data event tooIoT Hub:</span></span>

<span data-ttu-id="cbf42-239">{"Teploty": 75, "vlhkosti": 45}</span><span class="sxs-lookup"><span data-stu-id="cbf42-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="cbf42-240">To může zdát neobvyklé, protože naše model definuje **teploty** a **vlhkosti** jako dvě *samostatné* události:</span><span class="sxs-lookup"><span data-stu-id="cbf42-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="cbf42-241">Další toohello bodu jsme nebyl model tyto události kde **teploty** a **vlhkosti** jsou v hello stejnou strukturu:</span><span class="sxs-lookup"><span data-stu-id="cbf42-241">More toohello point, we didn’t model these events where **Temperature** and **Humidity** are in hello same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="cbf42-242">Pokud jsme použili tento model, by bylo snazší toounderstand jak **teploty** a **vlhkosti** mají být odeslány v hello stejné serializovat zprávy.</span><span class="sxs-lookup"><span data-stu-id="cbf42-242">If we used this model, it would be easier toounderstand how **Temperature** and **Humidity** would be sent in hello same serialized message.</span></span> <span data-ttu-id="cbf42-243">Ale nemusí být zřejmé, proč funguje tímto způsobem při předání obou událostí data příliš**SERIALIZACE** pomocí modelu 2.</span><span class="sxs-lookup"><span data-stu-id="cbf42-243">However it may not be clear why it works that way when you pass both data events too**SERIALIZE** using model 2.</span></span>

<span data-ttu-id="cbf42-244">Toto chování je snazší toounderstand Pokud znáte hello předpoklady tohoto hello **serializátor** je zajistit knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-244">This behavior is easier toounderstand if you know hello assumptions that hello **serializer** library is making.</span></span> <span data-ttu-id="cbf42-245">smyslu toomake Vraťme back tooour modelu:</span><span class="sxs-lookup"><span data-stu-id="cbf42-245">toomake sense of this let’s go back tooour model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="cbf42-246">Tento model si můžete představit v objektově orientované podmínky.</span><span class="sxs-lookup"><span data-stu-id="cbf42-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="cbf42-247">V takovém případě jsme se modelování fyzického zařízení (termostatu) a zařízení zahrnuje atributů, například **teploty** a **vlhkosti**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="cbf42-248">Vám můžeme poslat hello celý stav našeho modelu s kódem například hello následující:</span><span class="sxs-lookup"><span data-stu-id="cbf42-248">We can send hello entire state of our model with code such as hello following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="cbf42-249">Za předpokladu, že jsou nastavené hodnoty hello z teploty a vlhkosti, čas, jsme zobrazen událost podobně jako tento odeslané tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="cbf42-249">Assuming hello values of Temperature, Humidity and Time are set, we would see an event like this sent tooIoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="cbf42-250">V některých případech může být pouze vhodné toosend *některé* vlastností hello modelu toohello cloudu (to je obzvláště hodnota true, pokud model obsahuje velké množství dat událostí).</span><span class="sxs-lookup"><span data-stu-id="cbf42-250">Sometimes you may only want toosend *some* properties of hello model toohello cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="cbf42-251">Je užitečné toosend pouze podmnožinu dat události, například v našem příkladu starší:</span><span class="sxs-lookup"><span data-stu-id="cbf42-251">It’s useful toosend only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="cbf42-252">Tím se vygeneruje přesně hello stejné serializovat události, jako kdyby měl definovaného **TemperatureEvent** s **teploty** a **čas** člena, stejně jako jsme s modelu 1.</span><span class="sxs-lookup"><span data-stu-id="cbf42-252">This generates exactly hello same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="cbf42-253">V tomto případě nebylo možné toogenerate přesně hello stejné serializovat událostí pomocí jiného modelu (model 2), protože volali jsme **SERIALIZACE** jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="cbf42-253">In this case we were able toogenerate exactly hello same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="cbf42-254">Hello bodu důležité je, že pokud předáte více dat událostí příliš**SERIALIZACE,** pak předpokládá, že každá událost je vlastnost v jeden objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="cbf42-254">hello important point is that if you pass multiple data events too**SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="cbf42-255">Nejlepším postupem Hello závisí na a jak si myslíte o modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-255">hello best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="cbf42-256">Pokud odesíláte "události" toohello cloudu a každé události, obsahuje sada vlastností, hello prvním přístupem je velké množství smysl.</span><span class="sxs-lookup"><span data-stu-id="cbf42-256">If you’re sending "events" toohello cloud and each event contains a defined set of properties, then hello first approach makes a lot of sense.</span></span> <span data-ttu-id="cbf42-257">V takovém případě byste použili **DECLARE\_struktura** toodefine hello struktura jednotlivých událostí a zahrnout je do modelu pomocí hello **WITH\_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="cbf42-257">In that case you would use **DECLARE\_STRUCT** toodefine hello structure of each event and then include them in your model with hello **WITH\_DATA** macro.</span></span> <span data-ttu-id="cbf42-258">Každá událost se pak odeslat jako jsme to udělali v prvním příkladu hello výše.</span><span class="sxs-lookup"><span data-stu-id="cbf42-258">Then you send each event as we did in hello first example above.</span></span> <span data-ttu-id="cbf42-259">V tomto přístupu by pouze předáte jednoho datového událostí příliš**SERIALIZÁTOR**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-259">In this approach you would only pass a single data event too**SERIALIZER**.</span></span>

<span data-ttu-id="cbf42-260">Pokud si myslíte o modelu způsobem objektově orientované, může vyhovovat hello druhý postup je.</span><span class="sxs-lookup"><span data-stu-id="cbf42-260">If you think about your model in an object-oriented fashion, then hello second approach may suit you.</span></span> <span data-ttu-id="cbf42-261">V takovém případě hello elementy definovat pomocí **WITH\_DATA** jsou hello "vlastnosti" objektu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-261">In this case, hello elements defined using **WITH\_DATA** are hello "properties" of your object.</span></span> <span data-ttu-id="cbf42-262">Ať podmnožinu události předáte příliš**SERIALIZACE** , že se vám líbí, podle toho, kolik z vaší "" stav objektu chcete toosend toohello cloudu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-262">You pass whatever subset of events too**SERIALIZE** that you like, depending on how much of your "object’s" state you want toosend toohello cloud.</span></span>

<span data-ttu-id="cbf42-263">Nether přístup je pravé nebo nesprávné.</span><span class="sxs-lookup"><span data-stu-id="cbf42-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="cbf42-264">Právě zajímat, jak hello **serializátor** funguje knihovny a vybrat hello modelování přístup, který nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="cbf42-264">Just be aware of how hello **serializer** library works, and pick hello modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="cbf42-265">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="cbf42-265">Message handling</span></span>
<span data-ttu-id="cbf42-266">Tento článek, pokud má pouze popsané odesílání událostí tooIoT rozbočovače a nebyl řešit přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="cbf42-266">So far this article has only discussed sending events tooIoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="cbf42-267">Hello důvod pro to, co potřebujeme tooknow o přijímání zpráv má z velké části popsaná v [dříve článek](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="cbf42-267">hello reason for this is that what we need tooknow about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="cbf42-268">Z tohoto článku odvolat, zpracování zpráv tak, že zaregistrujete funkci zpětného volání zpráva:</span><span class="sxs-lookup"><span data-stu-id="cbf42-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="cbf42-269">Potom napíšete hello funkce zpětného volání, která je volána, když je obdržena zprávu:</span><span class="sxs-lookup"><span data-stu-id="cbf42-269">You then write hello callback function that’s invoked when a message is received:</span></span>

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

<span data-ttu-id="cbf42-270">Tato implementace **IoTHubMessage** volání hello konkrétní funkce pro každou akci do modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-270">This implementation of **IoTHubMessage** calls hello specific function for each action in your model.</span></span> <span data-ttu-id="cbf42-271">Například pokud váš model definuje tato akce:</span><span class="sxs-lookup"><span data-stu-id="cbf42-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="cbf42-272">Je nutné definovat funkce s Tento podpis:</span><span class="sxs-lookup"><span data-stu-id="cbf42-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position too%d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="cbf42-273">**SetAirResistance** pak je volána při odeslání zprávy tooyour zařízení.</span><span class="sxs-lookup"><span data-stu-id="cbf42-273">**SetAirResistance** is then called when that message is sent tooyour device.</span></span>

<span data-ttu-id="cbf42-274">Co jsme nebyly vysvětlené je ještě Jaká zpráva vypadá jako verze hello serializovat.</span><span class="sxs-lookup"><span data-stu-id="cbf42-274">What we haven't explained yet is what hello serialized version of message looks like.</span></span> <span data-ttu-id="cbf42-275">Jinými slovy Pokud chcete, aby toosend **SetAirResistance** zpráva tooyour zařízení, co typu?</span><span class="sxs-lookup"><span data-stu-id="cbf42-275">In other words, if you want toosend a **SetAirResistance** message tooyour device, what does that look like?</span></span>

<span data-ttu-id="cbf42-276">Pokud chcete odeslat zprávu tooa zařízení, krok lze provést pomocí sady SDK služby Azure IoT hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-276">If you're sending a message tooa device, you would do so through hello Azure IoT service SDK.</span></span> <span data-ttu-id="cbf42-277">Je stále nutné tooknow řetězec toosend tooinvoke určité akce.</span><span class="sxs-lookup"><span data-stu-id="cbf42-277">You still need tooknow what string toosend tooinvoke a particular action.</span></span> <span data-ttu-id="cbf42-278">Hello obecný formát pro odesílání zprávy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="cbf42-278">hello general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="cbf42-279">Odesíláte serializovaný objekt JSON s dvě vlastnosti: **název** je hello název akce hello (zprávy) a **parametry** obsahuje parametry hello této akce.</span><span class="sxs-lookup"><span data-stu-id="cbf42-279">You're sending a serialized JSON object with two properties: **Name** is hello name of hello action (message) and **Parameters** contains hello parameters of that action.</span></span>

<span data-ttu-id="cbf42-280">Například tooinvoke **SetAirResistance** můžete odeslat tuto zprávu tooa zařízení:</span><span class="sxs-lookup"><span data-stu-id="cbf42-280">For example, tooinvoke **SetAirResistance** you can send this message tooa device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="cbf42-281">Název akce Hello musí přesně shodovat akce definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-281">hello action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="cbf42-282">Hello názvy parametrů musí odpovídat také.</span><span class="sxs-lookup"><span data-stu-id="cbf42-282">hello parameter names must match as well.</span></span> <span data-ttu-id="cbf42-283">Všimněte si také malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="cbf42-283">Also note case sensitivity.</span></span> <span data-ttu-id="cbf42-284">**Název** a **parametry** jsou vždy velká písmena.</span><span class="sxs-lookup"><span data-stu-id="cbf42-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="cbf42-285">Ujistěte se, že případ hello toomatch názvu akce a parametrů do modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-285">Make sure toomatch hello case of your action name and parameters in your model.</span></span> <span data-ttu-id="cbf42-286">V tomto příkladu je název akce hello "SetAirResistance" a není "setairresistance".</span><span class="sxs-lookup"><span data-stu-id="cbf42-286">In this example, hello action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="cbf42-287">Hello dva další akce **TurnFanOn** a **TurnFanOff** nelze vyvolat odesláním těchto zpráv tooa zařízení:</span><span class="sxs-lookup"><span data-stu-id="cbf42-287">hello two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages tooa device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="cbf42-288">Tato část popisuje všechno, co potřebujete tooknow při odesílání událostí a přijímání zpráv s hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-288">This section described everything you need tooknow when sending events and receiving messages with hello **serializer** library.</span></span> <span data-ttu-id="cbf42-289">Než budete pokračovat, můžeme zahrnují některé parametry, které můžete nakonfigurovat, které řídí, jak velké je váš model.</span><span class="sxs-lookup"><span data-stu-id="cbf42-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="cbf42-290">Konfigurace – makro</span><span class="sxs-lookup"><span data-stu-id="cbf42-290">Macro configuration</span></span>
<span data-ttu-id="cbf42-291">Pokud používáte hello **serializátor** knihovny důležitou součástí toobe SDK hello vědět, se nachází v hello azure-c sdílené – knihovna nástrojů.</span><span class="sxs-lookup"><span data-stu-id="cbf42-291">If you’re using hello **Serializer** library an important part of hello SDK toobe aware of is found in hello azure-c-shared-utility library.</span></span>
<span data-ttu-id="cbf42-292">Při naklonování úložiště hello Azure-iot-sdk-c z Githubu pomocí možnosti--rekurzivní hello zjistíte této sdílené nástroj knihovny:</span><span class="sxs-lookup"><span data-stu-id="cbf42-292">If you have cloned hello Azure-iot-sdk-c repository from GitHub using hello --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="cbf42-293">Pokud nebyly klonovat hello knihovně, můžete ji najít [zde](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="cbf42-293">If you have not cloned hello library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="cbf42-294">V rámci hello nástroj sdílené knihovny zjistíte hello následující složku:</span><span class="sxs-lookup"><span data-stu-id="cbf42-294">Within hello shared utility library, you will find hello following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="cbf42-295">Tato složka obsahuje řešení sady Visual Studio názvem **makro\_utils\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="cbf42-296">Hello program v tomto řešení generuje hello **makro\_utils.h** souboru.</span><span class="sxs-lookup"><span data-stu-id="cbf42-296">hello program in this solution generates hello **macro\_utils.h** file.</span></span> <span data-ttu-id="cbf42-297">Je výchozí makro\_utils.h souboru součástí hello SDK.</span><span class="sxs-lookup"><span data-stu-id="cbf42-297">There’s a default macro\_utils.h file included with hello SDK.</span></span> <span data-ttu-id="cbf42-298">Toto řešení vám umožní toomodify některé parametry a pak znovu vytvořte hello hlavičkový soubor na základě těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="cbf42-298">This solution allows you toomodify some parameters and then recreate hello header file based on these parameters.</span></span>

<span data-ttu-id="cbf42-299">Hello dva klíče parametry toobe problémem jsou **nArithmetic** a **nMacroParameters** které jsou definované v těchto dvou řádcích najít v makro\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="cbf42-299">hello two key parameters toobe concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="cbf42-300">Tyto hodnoty jsou součástí hello SDK hello výchozí parametry.</span><span class="sxs-lookup"><span data-stu-id="cbf42-300">These values are hello default parameters included with hello SDK.</span></span> <span data-ttu-id="cbf42-301">Každý parametr má hello následující význam:</span><span class="sxs-lookup"><span data-stu-id="cbf42-301">Each parameter has hello following meaning:</span></span>

* <span data-ttu-id="cbf42-302">nMacroParameters – Určuje, kolik parametry může mít v jedné DECLARE\_definici makra modelu.</span><span class="sxs-lookup"><span data-stu-id="cbf42-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="cbf42-303">nArithmetic – ovládací prvky hello celkový počet členů v modelu povolené.</span><span class="sxs-lookup"><span data-stu-id="cbf42-303">nArithmetic – Controls hello total number of members allowed in a model.</span></span>

<span data-ttu-id="cbf42-304">Důvod Hello, které tyto parametry jsou důležité je, protože tím i určovat, jak velká může být váš model.</span><span class="sxs-lookup"><span data-stu-id="cbf42-304">hello reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="cbf42-305">Představte si třeba tuto definici modelu:</span><span class="sxs-lookup"><span data-stu-id="cbf42-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="cbf42-306">Jak je uvedeno nahoře, **DECLARE\_modelu** je právě makro C.</span><span class="sxs-lookup"><span data-stu-id="cbf42-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="cbf42-307">Hello názvy hello modelu a hello **WITH\_DATA** – příkaz (ještě jiné makro) jsou parametry **DECLARE\_modelu**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-307">hello names of hello model and hello **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="cbf42-308">**nMacroParameters** definuje, kolik parametry můžou být součástí **DECLARE\_modelu**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="cbf42-309">Definuje efektivně, kolik dat událostí a akce deklarace může mít.</span><span class="sxs-lookup"><span data-stu-id="cbf42-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="cbf42-310">Jako takový hello výchozí limit 124 to znamená, že můžete definovat model pomocí kombinace o 60 akce a data události.</span><span class="sxs-lookup"><span data-stu-id="cbf42-310">As such, with hello default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="cbf42-311">Pokud se pokusíte tooexceed tento limit, dostanete chyby kompilátoru, které vypadají podobně jako toothis:</span><span class="sxs-lookup"><span data-stu-id="cbf42-311">If you try tooexceed this limit, you'll receive compiler errors that look similar toothis:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="cbf42-312">Hello **nArithmetic** parametr je více informací o interní chodu hello hello makro jazyka než vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="cbf42-312">hello **nArithmetic** parameter is more about hello internal workings of hello macro language than your application.</span></span>  <span data-ttu-id="cbf42-313">Ovládá hello celkový počet členů, může mít v modelu, včetně **DECLARE_STRUCT** makra.</span><span class="sxs-lookup"><span data-stu-id="cbf42-313">It controls hello total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="cbf42-314">Pokud spustíte zobrazuje chyby kompilátoru, jako je tato, pak doporučujeme zvýšit **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="cbf42-315">Pokud chcete tyto parametry toochange, upravte hodnoty hello v hello makro\_utils.tt soubor, pak ji znovu zkompilovat hello makro\_utils\_h\_generator.sln řešení a spuštění hello zkompilovaný program.</span><span class="sxs-lookup"><span data-stu-id="cbf42-315">If you want toochange these parameters, modify hello values in hello macro\_utils.tt file, recompile hello macro\_utils\_h\_generator.sln solution, and run hello compiled program.</span></span> <span data-ttu-id="cbf42-316">Když uděláte, nové makro\_utils.h souboru je generována a umístit do hello.\\ běžné\\inc adresáře.</span><span class="sxs-lookup"><span data-stu-id="cbf42-316">When you do so, a new macro\_utils.h file is generated and placed in hello .\\common\\inc directory.</span></span>

<span data-ttu-id="cbf42-317">V pořadí toouse hello novou verzi makro\_utils.h, odeberte hello **serializátor** balíček NuGet, řešení a v jeho místní zahrnovat hello **serializátor** projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbf42-317">In order toouse hello new version of macro\_utils.h, remove hello **serializer** NuGet package from your solution and in its place include hello **serializer** Visual Studio project.</span></span> <span data-ttu-id="cbf42-318">To umožňuje váš kód toocompile proti hello zdrojového kódu hello serializátor knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-318">This enables your code toocompile against hello source code of hello serializer library.</span></span> <span data-ttu-id="cbf42-319">To zahrnuje hello aktualizovat makro\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="cbf42-319">This includes hello updated macro\_utils.h.</span></span> <span data-ttu-id="cbf42-320">Pokud chcete, aby toodo to pro **simplesample\_amqp**, začněte tím, že balíček NuGet hello hello serializátor knihovny odebráním hello řešení:</span><span class="sxs-lookup"><span data-stu-id="cbf42-320">If you want toodo this for **simplesample\_amqp**, start by removing hello NuGet package for hello serializer library from hello solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="cbf42-321">Pak přidejte tento projekt tooyour řešení sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cbf42-321">Then add this project tooyour Visual Studio solution:</span></span>

> <span data-ttu-id="cbf42-322">. \\c\\serializátor\\sestavení\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="cbf42-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="cbf42-323">Když jste hotovi, řešení by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="cbf42-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="cbf42-324">Teď když zkompilujete řešení, hello aktualizovat makro\_utils.h je součástí vaší binární.</span><span class="sxs-lookup"><span data-stu-id="cbf42-324">Now when you compile your solution, hello updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="cbf42-325">Všimněte si, že zvýšení tyto hodnoty dostatečně vysoký může překročit omezení kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="cbf42-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="cbf42-326">toothis bod, hello **nMacroParameters** je hlavní parametr hello s které toobe obavy.</span><span class="sxs-lookup"><span data-stu-id="cbf42-326">toothis point, hello **nMacroParameters** is hello main parameter with which toobe concerned.</span></span> <span data-ttu-id="cbf42-327">Specifikace C99 Hello Určuje, že jsou definice makra povolen minimálně 127 parametry.</span><span class="sxs-lookup"><span data-stu-id="cbf42-327">hello C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="cbf42-328">Hello kompilátoru Microsoft přesně dodržuje specifikaci hello (a může obsahovat maximálně 127 znaků), takže nebudete moct tooincrease **nMacroParameters** nad výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-328">hello Microsoft compiler follows hello spec exactly (and has a limit of 127), so you won't be able tooincrease **nMacroParameters** beyond hello default.</span></span> <span data-ttu-id="cbf42-329">Další kompilátory může umožňují toodo tak (například hello GNU kompilátoru podporuje vyšší limit).</span><span class="sxs-lookup"><span data-stu-id="cbf42-329">Other compilers might allow you toodo so (for example, hello GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="cbf42-330">Dosavadní téměř všechno, co potřebujete tooknow o tom, jak toowrite kód s hello jste zahrnutých jsme **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-330">So far we've covered just about everything you need tooknow about how toowrite code with hello **serializer** library.</span></span> <span data-ttu-id="cbf42-331">Před uzavřením, můžeme pokroku některá témata z předchozí články, které asi vás zajímá o.</span><span class="sxs-lookup"><span data-stu-id="cbf42-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="cbf42-332">Hello nižší úrovně rozhraní API</span><span class="sxs-lookup"><span data-stu-id="cbf42-332">hello lower-level APIs</span></span>
<span data-ttu-id="cbf42-333">je Hello ukázkovou aplikaci, na kterém se tento článek zaměřuje **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-333">hello sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="cbf42-334">Tato ukázka používá hello vyšší úrovně (hello bez – "vše") rozhraní API toosend události a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="cbf42-334">This sample uses hello higher-level (hello non-"LL") APIs toosend events and receive messages.</span></span> <span data-ttu-id="cbf42-335">Používáte-li tato rozhraní API, spustí vlákna na pozadí, který se stará o odesílání událostí i přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="cbf42-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="cbf42-336">Můžete však použít hello nižší úrovni (vše) rozhraní API tooeliminate tento vlákna na pozadí a trvat explicitní kontrolu nad při odesílání událostí nebo příjem zpráv z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-336">However, you can use hello lower-level (LL) APIs tooeliminate this background thread and take explicit control over when you send events or receive messages from hello cloud.</span></span>

<span data-ttu-id="cbf42-337">Jak je popsáno v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md), je sada funkcí, které se skládá z hello vyšší úrovně rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="cbf42-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of hello higher-level APIs:</span></span>

* <span data-ttu-id="cbf42-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="cbf42-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="cbf42-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="cbf42-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="cbf42-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="cbf42-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="cbf42-341">IoTHubClient\_Destroy</span><span class="sxs-lookup"><span data-stu-id="cbf42-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="cbf42-342">Tato rozhraní API je ukázán v **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="cbf42-343">K dispozici je také podobá sadu rozhraní API nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="cbf42-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="cbf42-344">IoTHubClient\_UDOU\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="cbf42-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="cbf42-345">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="cbf42-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="cbf42-346">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="cbf42-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="cbf42-347">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="cbf42-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="cbf42-348">Všimněte si, že hello nižší úrovně rozhraní API pracovní přesně hello stejným způsobem, jak popsané v předchozí článcích hello.</span><span class="sxs-lookup"><span data-stu-id="cbf42-348">Note that hello lower-level APIs work exactly hello same way as described in hello previous articles.</span></span> <span data-ttu-id="cbf42-349">Hello první sadu rozhraní API můžete použít, pokud chcete, aby vlákno toohandle pozadí, události odesílání a přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="cbf42-349">You can use hello first set of APIs if you want a background thread toohandle sending events and receiving messages.</span></span> <span data-ttu-id="cbf42-350">Pokud chcete, aby explicitní ovládat, kdy odesílat a přijímat data ze služby IoT Hub použijete hello druhá sada rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbf42-350">You use hello second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="cbf42-351">Buď sadu rozhraní API pro práci s hello stejně dobře **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-351">Either set of APIs work equally well with hello **serializer** library.</span></span>

<span data-ttu-id="cbf42-352">Pro příklad hello nižší úrovně používají rozhraní API s hello **serializátor** knihovny, najdete v části hello **simplesample\_http** aplikace.</span><span class="sxs-lookup"><span data-stu-id="cbf42-352">For an example of how hello lower-level APIs are used with hello **serializer** library, see hello **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="cbf42-353">Další témata</span><span class="sxs-lookup"><span data-stu-id="cbf42-353">Additional topics</span></span>
<span data-ttu-id="cbf42-354">Několik dalších tématech důležité zmínit, znovu se vlastnost zpracování, pomocí přihlašovacích údajů jiného zařízení a možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="cbf42-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="cbf42-355">Toto jsou všechna témata popsaná v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="cbf42-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="cbf42-356">Hello hlavní bod je, že všechny tyto funkce pracovat v hello stejný způsob s hello **serializátor** knihovny stejně jako s hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-356">hello main point is that all of these features work in hello same way with hello **serializer** library as they do with hello **IoTHubClient** library.</span></span> <span data-ttu-id="cbf42-357">Pokud chcete tooattach vlastnosti tooan událostí z modelu, můžete použít například **IoTHubMessage\_vlastnosti** a **mapy**\_**AddorUpdate**, stejným způsobem, jak jsme je už popsali hello:</span><span class="sxs-lookup"><span data-stu-id="cbf42-357">For example, if you want tooattach properties tooan event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, hello same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="cbf42-358">Jestli se vygeneroval hello událostí ze hello **serializátor** knihovny nebo vytvořit ručně pomocí hello **IoTHubClient** knihovny nezáleží.</span><span class="sxs-lookup"><span data-stu-id="cbf42-358">Whether hello event was generated from hello **serializer** library or created manually using hello **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="cbf42-359">Pro hello alternativní přihlašovací údaje zařízení, pomocí **IoTHubClient\_UDOU\_vytvořit** funguje stejně dobře jako **IoTHubClient\_CreateFromConnectionString** pro přidělování **IOTHUB\_klienta\_zpracování**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-359">For hello alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="cbf42-360">Nakonec pokud používáte hello **serializátor** knihovny, můžete nastavit možnosti konfigurace s **IoTHubClient\_UDOU\_SetOption** stejně jako jste to udělali při použití hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-360">Finally, if you're using hello **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using hello **IoTHubClient** library.</span></span>

<span data-ttu-id="cbf42-361">Funkce, která je jedinečný toohello **serializátor** knihovny jsou hello inicializace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbf42-361">A feature that is unique toohello **serializer** library are hello initialization APIs.</span></span> <span data-ttu-id="cbf42-362">Před zahájením práce s hello knihovně, musí volat **serializátor\_init**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-362">Before you can start working with hello library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="cbf42-363">To se provádí těsně před voláním **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="cbf42-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="cbf42-364">Podobně po dokončení práce s hello knihovně, budete provedete posledním volání hello je příliš**serializátor\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="cbf42-364">Similarly, when you're done working with hello library, hello last call you’ll make is too**serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="cbf42-365">Jinak, všechny další funkce uvedené výše hello pracovní stejné hello v hello **serializátor** knihovny stejně jako v hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="cbf42-365">Otherwise, all of hello other features listed above work hello same in hello **serializer** library as they do in hello **IoTHubClient** library.</span></span> <span data-ttu-id="cbf42-366">Další informace o všech těchto tématech najdete v tématu hello [předchozí článek](iot-hub-device-sdk-c-iothubclient.md) této série.</span><span class="sxs-lookup"><span data-stu-id="cbf42-366">For more information about any of these topics, see hello [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbf42-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cbf42-367">Next steps</span></span>
<span data-ttu-id="cbf42-368">Tento článek popisuje, v podrobností hello jedinečné aspekty hello **serializátor** obsažené v hello knihovně **zařízení Azure IoT SDK pro jazyk C**. S hello informací by měl mít dostatečné povědomí o tom, jak toouse modelů toosend události a přijímat zprávy ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="cbf42-368">This article describes in detail hello unique aspects of hello **serializer** library contained in hello **Azure IoT device SDK for C**. With hello information provided you should have a good understanding of how toouse models toosend events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="cbf42-369">To se taky ukončí hello třemi částmi řady na jak hello toodevelop aplikace s **zařízení Azure IoT SDK pro jazyk C**. To by měl být dostatek informace toonot pouze get, kterou jste zahájili ale získáte důkladné znalosti o fungování hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="cbf42-369">This also concludes hello three-part series on how toodevelop applications with hello **Azure IoT device SDK for C**. This should be enough information toonot only get you started but give you a thorough understanding of how hello APIs work.</span></span> <span data-ttu-id="cbf42-370">Další informace naleznete několik ukázky v nejsou hello SDK nejsou zahrnuté v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="cbf42-370">For additional information, there are a few samples in hello SDK not covered here.</span></span> <span data-ttu-id="cbf42-371">V opačném hello [dokumentaci k sadě SDK](https://github.com/Azure/azure-iot-sdk-c) je dobré prostředku pro další informace.</span><span class="sxs-lookup"><span data-stu-id="cbf42-371">Otherwise, hello [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="cbf42-372">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="cbf42-372">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="cbf42-373">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="cbf42-373">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="cbf42-374">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="cbf42-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
