---
title: "Pro zařízení Azure IoT SDK pro jazyk C - serializátor | Microsoft Docs"
description: "Jak používat knihovnu serializátor v zařízení Azure IoT SDK pro jazyk C vytvoření aplikace pro zařízení, které komunikují pomocí služby IoT hub."
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
ms.openlocfilehash: aa03c29c54d75538b1fdf987cac5f09d5d344f73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a><span data-ttu-id="37762-103">Pro zařízení Azure IoT SDK pro jazyk C – informace o serializátor</span><span class="sxs-lookup"><span data-stu-id="37762-103">Azure IoT device SDK for C – more about serializer</span></span>
<span data-ttu-id="37762-104">[Nejprve článek](iot-hub-device-sdk-c-intro.md) této série zavedená **zařízení Azure IoT SDK pro jazyk C**. Další článek poskytuje podrobnější popis [ **IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="37762-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. The next article provided a more detailed description of the [**IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="37762-105">Tento článek poskytuje podrobnější popis zbývající součásti dokončí pokrytí sady SDK: **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-105">This article completes coverage of the SDK by providing a more detailed description of the remaining component: the **serializer** library.</span></span>

<span data-ttu-id="37762-106">Úvodní článek popisuje postup použití **serializátor** knihovnu, která se události odesílat a přijímat zprávy ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="37762-106">The introductory article described how to use the **serializer** library to send events to and receive messages from IoT Hub.</span></span> <span data-ttu-id="37762-107">V tomto článku jsme rozšířit tento diskuzi o podrobnější vysvětlení způsobu modelu svá data pomocí **serializátor** makro jazyk.</span><span class="sxs-lookup"><span data-stu-id="37762-107">In this article, we extend that discussion by providing a more complete explanation of how to model your data with the **serializer** macro language.</span></span> <span data-ttu-id="37762-108">Článek také obsahuje další podrobnosti o tom, jak knihovny serializuje zprávy (a v některých případech, jak můžete řídit chování serializace).</span><span class="sxs-lookup"><span data-stu-id="37762-108">The article also includes more detail about how the library serializes messages (and in some cases how you can control the serialization behavior).</span></span> <span data-ttu-id="37762-109">Popíšeme také některé parametry, které můžete upravit určující velikost modely, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="37762-109">We'll also describe some parameters you can modify that determine the size of the models you create.</span></span>

<span data-ttu-id="37762-110">Nakonec článek znovu navštíví některá témata popsaná v předchozích články například zprávu a vlastnosti zpracování.</span><span class="sxs-lookup"><span data-stu-id="37762-110">Finally, the article revisits some topics covered in previous articles such as message and property handling.</span></span> <span data-ttu-id="37762-111">Až budete zjistěte těchto pracovních funkcí ve stejném způsobem pomocí **serializátor** knihovny stejně jako s **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-111">As we'll find out, those features work in the same way using the **serializer** library as they do with the **IoTHubClient** library.</span></span>

<span data-ttu-id="37762-112">Všechno, co popsané v tomto článku je založena na **serializátor** ukázky SDK.</span><span class="sxs-lookup"><span data-stu-id="37762-112">Everything described in this article is based on the **serializer** SDK samples.</span></span> <span data-ttu-id="37762-113">Pokud chcete sledovat, najdete v článku **simplesample\_amqp** a **simplesample\_http** aplikace, které jsou součástí sady SDK zařízení Azure IoT pro C.</span><span class="sxs-lookup"><span data-stu-id="37762-113">If you want to follow along, see the **simplesample\_amqp** and **simplesample\_http** applications included in the Azure IoT device SDK for C.</span></span>

<span data-ttu-id="37762-114">Můžete najít [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o rozhraní API v [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="37762-114">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-modeling-language"></a><span data-ttu-id="37762-115">Představuje Modelovací jazyk</span><span class="sxs-lookup"><span data-stu-id="37762-115">The modeling language</span></span>
<span data-ttu-id="37762-116">[Úvodní článek](iot-hub-device-sdk-c-intro.md) této série zavedená **zařízení Azure IoT SDK pro jazyk C** Modelovací jazyk prostřednictvím v příkladu v **simplesample\_amqp** aplikace:</span><span class="sxs-lookup"><span data-stu-id="37762-116">The [introductory article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C** modeling language through the example provided in the **simplesample\_amqp** application:</span></span>

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

<span data-ttu-id="37762-117">Jak vidíte, který představuje Modelovací jazyk je založena na maker v jazyce C.</span><span class="sxs-lookup"><span data-stu-id="37762-117">As you can see, the modeling language is based on C macros.</span></span> <span data-ttu-id="37762-118">Je třeba nejprve vaší definice s **BEGIN\_obor názvů** a vždy končit **END\_obor názvů**.</span><span class="sxs-lookup"><span data-stu-id="37762-118">You always begin your definition with **BEGIN\_NAMESPACE** and always end with **END\_NAMESPACE**.</span></span> <span data-ttu-id="37762-119">Je běžné název oboru názvů pro vaši společnost nebo, jako v následujícím příkladu, projekt, který právě pracujete.</span><span class="sxs-lookup"><span data-stu-id="37762-119">It's common to name the namespace for your company or, as in this example, the project that you're working on.</span></span>

<span data-ttu-id="37762-120">Co v oboru názvů jsou definice modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-120">What goes inside the namespace are model definitions.</span></span> <span data-ttu-id="37762-121">V takovém případě je jeden model anemometer.</span><span class="sxs-lookup"><span data-stu-id="37762-121">In this case, there is a single model for an anemometer.</span></span> <span data-ttu-id="37762-122">Ještě jednou může být název modelu, nic, ale obvykle to jmenuje pro zařízení nebo typ dat, kterou chcete exchange službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="37762-122">Once again, the model can be named anything, but typically this is named for the device or type of data you want to exchange with IoT Hub.</span></span>  

<span data-ttu-id="37762-123">Modely obsahují definice událostí můžete příjem příchozích dat do služby IoT Hub ( *data*) a také zprávy může přijímat ze služby IoT Hub ( *akce*).</span><span class="sxs-lookup"><span data-stu-id="37762-123">Models contain a definition of the events you can ingress to IoT Hub (the *data*) as well as the messages you can receive from IoT Hub (the *actions*).</span></span> <span data-ttu-id="37762-124">Jak je vidět z příkladu události mít typ a název; název a volitelné parametry (každý s typem) mají akce.</span><span class="sxs-lookup"><span data-stu-id="37762-124">As you can see from the example, events have a type and a name; actions have a name and optional parameters (each with a type).</span></span>

<span data-ttu-id="37762-125">Co není ukázáno v této ukázce jsou další datové typy, které jsou podporované sadou SDK.</span><span class="sxs-lookup"><span data-stu-id="37762-125">What’s not demonstrated in this sample are additional data types that are supported by the SDK.</span></span> <span data-ttu-id="37762-126">Jsme zaměříme tento další.</span><span class="sxs-lookup"><span data-stu-id="37762-126">We'll cover that next.</span></span>

> [!NOTE]
> <span data-ttu-id="37762-127">IoT Hub odkazuje na data, která zařízení odesílá do ní jako *události*, zatímco jazyk modelování odkazuje na jej jako *data* (definovat pomocí **WITH_DATA**).</span><span class="sxs-lookup"><span data-stu-id="37762-127">IoT Hub refers to the data a device sends to it as *events*, while the modeling language refers to it as *data* (defined using **WITH_DATA**).</span></span> <span data-ttu-id="37762-128">Podobně IoT Hub odkazuje na data, která odešlete na zařízení jako *zprávy*, zatímco jazyk modelování odkazuje na jej jako *akce* (definovat pomocí **WITH_ACTION**).</span><span class="sxs-lookup"><span data-stu-id="37762-128">Likewise, IoT Hub refers to the data you send to devices as *messages*, while the modeling language refers to it as *actions* (defined using **WITH_ACTION**).</span></span> <span data-ttu-id="37762-129">Upozorňujeme, že tyto podmínky mohou být použity zcela zaměnitelným významem v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="37762-129">Be aware that these terms may be used interchangeably in this article.</span></span>
> 
> 

### <a name="supported-data-types"></a><span data-ttu-id="37762-130">Podporované datové typy</span><span class="sxs-lookup"><span data-stu-id="37762-130">Supported data types</span></span>
<span data-ttu-id="37762-131">Jsou podporovány následující typy dat v modelů vytvořených pomocí **serializátor** knihovny:</span><span class="sxs-lookup"><span data-stu-id="37762-131">The following data types are supported in models created with the **serializer** library:</span></span>

| <span data-ttu-id="37762-132">Typ</span><span class="sxs-lookup"><span data-stu-id="37762-132">Type</span></span> | <span data-ttu-id="37762-133">Popis</span><span class="sxs-lookup"><span data-stu-id="37762-133">Description</span></span> |
| --- | --- |
| <span data-ttu-id="37762-134">Double</span><span class="sxs-lookup"><span data-stu-id="37762-134">double</span></span> |<span data-ttu-id="37762-135">Dvojitá přesnost číslo s plovoucí desetinnou</span><span class="sxs-lookup"><span data-stu-id="37762-135">double precision floating point number</span></span> |
| <span data-ttu-id="37762-136">celá čísla</span><span class="sxs-lookup"><span data-stu-id="37762-136">int</span></span> |<span data-ttu-id="37762-137">32bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-137">32 bit integer</span></span> |
| <span data-ttu-id="37762-138">Plovoucí desetinná čárka</span><span class="sxs-lookup"><span data-stu-id="37762-138">float</span></span> |<span data-ttu-id="37762-139">číslo s plovoucí desetinnou jednoduchou přesností</span><span class="sxs-lookup"><span data-stu-id="37762-139">single precision floating point number</span></span> |
| <span data-ttu-id="37762-140">dlouhá</span><span class="sxs-lookup"><span data-stu-id="37762-140">long</span></span> |<span data-ttu-id="37762-141">dlouhé celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-141">long integer</span></span> |
| <span data-ttu-id="37762-142">int8\_t</span><span class="sxs-lookup"><span data-stu-id="37762-142">int8\_t</span></span> |<span data-ttu-id="37762-143">8bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-143">8 bit integer</span></span> |
| <span data-ttu-id="37762-144">Int16\_t</span><span class="sxs-lookup"><span data-stu-id="37762-144">int16\_t</span></span> |<span data-ttu-id="37762-145">16bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-145">16 bit integer</span></span> |
| <span data-ttu-id="37762-146">Int32\_t</span><span class="sxs-lookup"><span data-stu-id="37762-146">int32\_t</span></span> |<span data-ttu-id="37762-147">32bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-147">32 bit integer</span></span> |
| <span data-ttu-id="37762-148">Int64\_t</span><span class="sxs-lookup"><span data-stu-id="37762-148">int64\_t</span></span> |<span data-ttu-id="37762-149">64bitové celé číslo</span><span class="sxs-lookup"><span data-stu-id="37762-149">64 bit integer</span></span> |
| <span data-ttu-id="37762-150">BOOL</span><span class="sxs-lookup"><span data-stu-id="37762-150">bool</span></span> |<span data-ttu-id="37762-151">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="37762-151">boolean</span></span> |
| <span data-ttu-id="37762-152">ASCII\_char\_ptr</span><span class="sxs-lookup"><span data-stu-id="37762-152">ascii\_char\_ptr</span></span> |<span data-ttu-id="37762-153">Řetězec ASCII</span><span class="sxs-lookup"><span data-stu-id="37762-153">ASCII string</span></span> |
| <span data-ttu-id="37762-154">EDM\_DATUM\_ČAS\_POSUNUTÍ</span><span class="sxs-lookup"><span data-stu-id="37762-154">EDM\_DATE\_TIME\_OFFSET</span></span> |<span data-ttu-id="37762-155">Datum čas posunutí</span><span class="sxs-lookup"><span data-stu-id="37762-155">date time offset</span></span> |
| <span data-ttu-id="37762-156">EDM\_GUID</span><span class="sxs-lookup"><span data-stu-id="37762-156">EDM\_GUID</span></span> |<span data-ttu-id="37762-157">IDENTIFIKÁTOR GUID</span><span class="sxs-lookup"><span data-stu-id="37762-157">GUID</span></span> |
| <span data-ttu-id="37762-158">EDM\_BINÁRNÍ</span><span class="sxs-lookup"><span data-stu-id="37762-158">EDM\_BINARY</span></span> |<span data-ttu-id="37762-159">Binární</span><span class="sxs-lookup"><span data-stu-id="37762-159">binary</span></span> |
| <span data-ttu-id="37762-160">DEKLAROVAT\_– STRUKTURA</span><span class="sxs-lookup"><span data-stu-id="37762-160">DECLARE\_STRUCT</span></span> |<span data-ttu-id="37762-161">Komplexní datový typ</span><span class="sxs-lookup"><span data-stu-id="37762-161">complex data type</span></span> |

<span data-ttu-id="37762-162">Začněme s posledním datovým typem.</span><span class="sxs-lookup"><span data-stu-id="37762-162">Let’s start with the last data type.</span></span> <span data-ttu-id="37762-163">**DECLARE\_struktura** můžete zadat rozšířené datové typy, které jsou seskupení primitivní typy.</span><span class="sxs-lookup"><span data-stu-id="37762-163">The **DECLARE\_STRUCT** allows you to define complex data types, which are groupings of the other primitive types.</span></span> <span data-ttu-id="37762-164">Tato seskupení povolit nám definovat model, který vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="37762-164">These groupings allow us to define a model that looks like this:</span></span>

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

<span data-ttu-id="37762-165">Naše model obsahuje událostí jednoho datového typu **TestType**.</span><span class="sxs-lookup"><span data-stu-id="37762-165">Our model contains a single data event of type **TestType**.</span></span> <span data-ttu-id="37762-166">**TestType** jedná o komplexní typ, který zahrnuje několik členů, které se souhrnně ukazují primitivní typy podporované systémem **serializátor** Modelovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="37762-166">**TestType** is a complex type that includes several members, which collectively demonstrate the primitive types supported by the **serializer** modeling language.</span></span>

<span data-ttu-id="37762-167">S takového modelu jsme můžete napsat kód pro odesílání dat do služby IoT Hub, který se zobrazí následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="37762-167">With a model like this, we can write code to send data to IoT Hub that appears as follows:</span></span>

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

<span data-ttu-id="37762-168">V podstatě, jsme se přiřazení hodnoty k každý člen **testovací** strukturu a pak volání **SendAsync** k odeslání **testovací** dat událostí do cloudu.</span><span class="sxs-lookup"><span data-stu-id="37762-168">Basically, we’re assigning a value to every member of the **Test** structure and then calling **SendAsync** to send the **Test** data event to the cloud.</span></span> <span data-ttu-id="37762-169">**SendAsync** je pomocné funkce, která odesílá události jednoho datového do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-169">**SendAsync** is a helper function that sends a single data event to IoT Hub:</span></span>

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
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

<span data-ttu-id="37762-170">Tato funkce serializuje daná data události a odesílá do služby IoT Hub pomocí **IoTHubClient\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="37762-170">This function serializes the given data event and sends it to IoT Hub using **IoTHubClient\_SendEventAsync**.</span></span> <span data-ttu-id="37762-171">Toto je stejný kód popsané v předchozí články (**SendAsync** zapouzdří logiku do vhodného funkce).</span><span class="sxs-lookup"><span data-stu-id="37762-171">This is the same code discussed in previous articles (**SendAsync** encapsulates the logic into a convenient function).</span></span>

<span data-ttu-id="37762-172">Jeden další pomocné funkce použitá v předchozí kód je **GetDateTimeOffset**.</span><span class="sxs-lookup"><span data-stu-id="37762-172">One other helper function used in the previous code is **GetDateTimeOffset**.</span></span> <span data-ttu-id="37762-173">Tato funkce transformuje daném čase na hodnotu typu **EDM\_datum\_čas\_posun**:</span><span class="sxs-lookup"><span data-stu-id="37762-173">This function transforms the given time into a value of type **EDM\_DATE\_TIME\_OFFSET**:</span></span>

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

<span data-ttu-id="37762-174">Pokud spustíte tento kód, následující zpráva odeslána do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-174">If you run this code, the following message is sent to IoT Hub:</span></span>

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

<span data-ttu-id="37762-175">Upozorňujeme, že serializaci JSON, který je ve formátu vygenerované pomocí **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-175">Note that the serialization is JSON, which is the format generated by the **serializer** library.</span></span> <span data-ttu-id="37762-176">Všimněte si, že každý člen serializovaný objekt JSON odpovídá členů **TestType** , definovaného v našem modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-176">Also note that each member of the serialized JSON object matches the members of the **TestType** that we defined in our model.</span></span> <span data-ttu-id="37762-177">Hodnoty také přesně shodovat s hodnotami používá v kódu.</span><span class="sxs-lookup"><span data-stu-id="37762-177">The values also exactly match those used in the code.</span></span> <span data-ttu-id="37762-178">Ale Všimněte si, že binární data s kódováním base64, pomocí: "AQID" je Base64, pomocí kódování {0x01, 0x02, 0x03}.</span><span class="sxs-lookup"><span data-stu-id="37762-178">However, note that the binary data is base64-encoded: "AQID" is the base64 encoding of {0x01, 0x02, 0x03}.</span></span>

<span data-ttu-id="37762-179">Tento příklad ukazuje výhodou použití **serializátor** knihovna – umožňuje poslat JSON do cloudu, bez nutnosti explicitně serializace v naší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="37762-179">This example demonstrates the advantage of using the **serializer** library -- it enables us to send JSON to the cloud, without having to explicitly deal with serialization in our application.</span></span> <span data-ttu-id="37762-180">Všechny, které máme si dělat starosti se nastaví hodnoty dat události v našem modelu a poté volání jednoduché rozhraní API k odeslání události do cloudu.</span><span class="sxs-lookup"><span data-stu-id="37762-180">All we have to worry about is setting the values of the data events in our model and then calling simple APIs to send those events to the cloud.</span></span>

<span data-ttu-id="37762-181">Pomocí těchto informací jsme můžete definovat modely, které zahrnují řadu podporované datové typy, včetně komplexní typy (může i zahrnuta komplexních typů v rámci jiné komplexní typy).</span><span class="sxs-lookup"><span data-stu-id="37762-181">With this information, we can define models that include the range of supported data types, including complex types (we could even include complex types within other complex types).</span></span> <span data-ttu-id="37762-182">Však mohl serializovat JSON vygenerovaných v předchozím příkladu se vyvolá bod důležité.</span><span class="sxs-lookup"><span data-stu-id="37762-182">However, he serialized JSON generated by the example above brings up an important point.</span></span> <span data-ttu-id="37762-183">*Jak* odešleme dat pomocí **serializátor** knihovny určuje přesně jak je formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="37762-183">*How* we send data with the **serializer** library determines exactly how the JSON is formed.</span></span> <span data-ttu-id="37762-184">Zda je konkrétní bodu, co jsme zaměříme Další.</span><span class="sxs-lookup"><span data-stu-id="37762-184">That particular point is what we'll cover next.</span></span>

## <a name="more-about-serialization"></a><span data-ttu-id="37762-185">Další informace o serializaci</span><span class="sxs-lookup"><span data-stu-id="37762-185">More about serialization</span></span>
<span data-ttu-id="37762-186">V předchozí části jsou zdůrazněné příklad výstupu generované **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-186">The previous section highlights an example of the output generated by the **serializer** library.</span></span> <span data-ttu-id="37762-187">V této části vám objasníme, jak knihovny serializuje data a jak můžete řídit tohoto chování pomocí serializace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="37762-187">In this section, we'll explain how the library serializes data and how you can control that behavior using the serialization APIs.</span></span>

<span data-ttu-id="37762-188">Chcete-li zálohy diskuse o serializaci, jsme budete pracovat se nový model podle termostatu.</span><span class="sxs-lookup"><span data-stu-id="37762-188">In order to advance the discussion on serialization, we'll work with a new model based on a thermostat.</span></span> <span data-ttu-id="37762-189">Nejprve umožňuje zadat základní na scénáři Pokoušíme se adresa.</span><span class="sxs-lookup"><span data-stu-id="37762-189">First, let's provide some background on the scenario we're trying to address.</span></span>

<span data-ttu-id="37762-190">Chceme modelu termostatu, které měří teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="37762-190">We want to model a thermostat that measures temperature and humidity.</span></span> <span data-ttu-id="37762-191">Každá položka dat je přejdete k odeslání do služby IoT Hub jinak.</span><span class="sxs-lookup"><span data-stu-id="37762-191">Each piece of data is going to be sent to IoT Hub differently.</span></span> <span data-ttu-id="37762-192">Ve výchozím nastavení ingresses termostatu teploty událostí jednou za 2 minut; událost vlhkosti je ingressed jednou za 15 minut.</span><span class="sxs-lookup"><span data-stu-id="37762-192">By default, the thermostat ingresses a temperature event once every 2 minutes; a humidity event is ingressed once every 15 minutes.</span></span> <span data-ttu-id="37762-193">Po ingressed buď událostí musí obsahovat časové razítko, které zobrazuje čas, že se měří odpovídající teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="37762-193">When either event is ingressed, it must include a timestamp that shows the time that the corresponding temperature or humidity was measured.</span></span>

<span data-ttu-id="37762-194">Tento scénář, vám ukážeme dva různé způsoby, jak model dat, a vysvětlíme účinek, modelování měl na serializovaných výstup.</span><span class="sxs-lookup"><span data-stu-id="37762-194">Given this scenario, we'll demonstrate two different ways to model the data, and we'll explain the effect that modeling has on the serialized output.</span></span>

### <a name="model-1"></a><span data-ttu-id="37762-195">Model 1</span><span class="sxs-lookup"><span data-stu-id="37762-195">Model 1</span></span>
<span data-ttu-id="37762-196">Tady je první verze součásti model, který podporuje předchozí situaci:</span><span class="sxs-lookup"><span data-stu-id="37762-196">Here's the first version of a model that supports the previous scenario:</span></span>

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

<span data-ttu-id="37762-197">Všimněte si, že tento model zahrnuje dvě data události: **teploty** a **vlhkosti**.</span><span class="sxs-lookup"><span data-stu-id="37762-197">Note that the model includes two data events: **Temperature** and **Humidity**.</span></span> <span data-ttu-id="37762-198">Na rozdíl od předchozích příkladech typ jednotlivých událostí je struktura definovat pomocí **DECLARE\_struktura**.</span><span class="sxs-lookup"><span data-stu-id="37762-198">Unlike previous examples, the type of each event is a structure defined using **DECLARE\_STRUCT**.</span></span> <span data-ttu-id="37762-199">**TemperatureEvent** zahrnuje měření teploty a časové razítko; **HumidityEvent** obsahuje měření vlhkosti a časové razítko.</span><span class="sxs-lookup"><span data-stu-id="37762-199">**TemperatureEvent** includes a temperature measurement and a timestamp; **HumidityEvent** contains a humidity measurement and a timestamp.</span></span> <span data-ttu-id="37762-200">Tento model nám poskytuje přirozené způsob, jak model data pro scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="37762-200">This model gives us a natural way to model the data for the scenario described above.</span></span> <span data-ttu-id="37762-201">Když jsme odeslání události do cloudu, buď pošleme teploty/časové razítko nebo pár vlhkosti/časové razítko.</span><span class="sxs-lookup"><span data-stu-id="37762-201">When we send an event to the cloud, we'll either send a temperature/timestamp or a humidity/timestamp pair.</span></span>

<span data-ttu-id="37762-202">Vám můžeme poslat teploty událostí v cloudu pomocí kódu, například následující:</span><span class="sxs-lookup"><span data-stu-id="37762-202">We can send a temperature event to the cloud using code such as the following:</span></span>

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

<span data-ttu-id="37762-203">Jsme budete používat pevně definovaných hodnot pro teploty a vlhkosti v ukázkovém kódu, ale Představte si, že jsme se ve skutečnosti načítání tyto hodnoty vzorkováním odpovídající snímače na termostatu.</span><span class="sxs-lookup"><span data-stu-id="37762-203">We'll use hard-coded values for temperature and humidity in the sample code, but imagine that we’re actually retrieving these values by sampling the corresponding sensors on the thermostat.</span></span>

<span data-ttu-id="37762-204">Kód výše používá **GetDateTimeOffset** pomocné rutiny, která byla zavedená dříve.</span><span class="sxs-lookup"><span data-stu-id="37762-204">The code above uses the **GetDateTimeOffset** helper that was introduced previously.</span></span> <span data-ttu-id="37762-205">Z důvodů, které se stanou zrušte novější se tento kód explicitně odděluje úlohu serializaci a odeslání události.</span><span class="sxs-lookup"><span data-stu-id="37762-205">For reasons that will become clear later, this code explicitly separates the task of serializing and sending the event.</span></span> <span data-ttu-id="37762-206">Předchozí kód serializuje teploty událostí do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="37762-206">The previous code serializes the temperature event into a buffer.</span></span> <span data-ttu-id="37762-207">Potom **sendMessage** je pomocné funkce (součástí **simplesample\_amqp**), odešle událost do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-207">Then, **sendMessage** is a helper function (included in **simplesample\_amqp**) that sends the event to IoT Hub:</span></span>

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

<span data-ttu-id="37762-208">Tento kód je podmnožinou **SendAsync** pomocné rutiny popsané v předchozí části, takže jsme nebude projít ho znovu sem.</span><span class="sxs-lookup"><span data-stu-id="37762-208">This code is a subset of the **SendAsync** helper described in the previous section, so we won’t go over it again here.</span></span>

<span data-ttu-id="37762-209">Když jsme spustí předchozí kódu pro odeslání události teploty, tento formulář serializovaných události je odeslána do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-209">When we run the previous code to send the Temperature event, this serialized form of the event is sent to IoT Hub:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="37762-210">Byla odeslána teploty, které je typu **TemperatureEvent** a že struktura obsahuje **teploty** a **čas** člen.</span><span class="sxs-lookup"><span data-stu-id="37762-210">We're sending a temperature which is of type **TemperatureEvent** and that struct contains a **Temperature** and **Time** member.</span></span> <span data-ttu-id="37762-211">To se projeví přímo v serializovaných datech.</span><span class="sxs-lookup"><span data-stu-id="37762-211">This is directly reflected in the serialized data.</span></span>

<span data-ttu-id="37762-212">Podobně vám můžeme poslat vlhkosti událostí s tímto kódem:</span><span class="sxs-lookup"><span data-stu-id="37762-212">Similarly, we can send a humidity event with this code:</span></span>

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="37762-213">Serializovaná formulář, který je odeslán do služby IoT Hub vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="37762-213">The serialized form that’s sent to IoT Hub appears as follows:</span></span>

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="37762-214">To je opět podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="37762-214">Again, this is as expected.</span></span>

<span data-ttu-id="37762-215">V tomto modelu, který chcete mít jak další události lze snadno přidat.</span><span class="sxs-lookup"><span data-stu-id="37762-215">With this model, you can imagine how additional events can easily be added.</span></span> <span data-ttu-id="37762-216">Můžete definovat další struktur pomocí **DECLARE\_struktura**a zahrnovat odpovídající události v modelu pomocí **WITH\_DATA**.</span><span class="sxs-lookup"><span data-stu-id="37762-216">You define more structures using **DECLARE\_STRUCT**, and include the corresponding event in the model using **WITH\_DATA**.</span></span>

<span data-ttu-id="37762-217">Nyní Pojďme upravit modelu tak, že obsahují stejná data, ale s jinou strukturu.</span><span class="sxs-lookup"><span data-stu-id="37762-217">Now, let’s modify the model so that it includes the same data but with a different structure.</span></span>

### <a name="model-2"></a><span data-ttu-id="37762-218">Model 2</span><span class="sxs-lookup"><span data-stu-id="37762-218">Model 2</span></span>
<span data-ttu-id="37762-219">Vezměte v úvahu tento alternativní model tomu výše:</span><span class="sxs-lookup"><span data-stu-id="37762-219">Consider this alternative model to the one above:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="37762-220">V takovém případě jsme vyloučili **DECLARE\_struktura** makra a jsou jednoduše definování datové položky z našich scénář pomocí jednoduché typy z představuje Modelovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="37762-220">In this case we've eliminated the **DECLARE\_STRUCT** macros and are simply defining the data items from our scenario using simple types from the modeling language.</span></span>

<span data-ttu-id="37762-221">Jenom pro tuto chvíli umožňuje ignorovat **čas** událostí.</span><span class="sxs-lookup"><span data-stu-id="37762-221">Just for the moment let’s ignore the **Time** event.</span></span> <span data-ttu-id="37762-222">S vyhraďte, zde je kód pro příjem příchozích dat **teploty**:</span><span class="sxs-lookup"><span data-stu-id="37762-222">With that aside, here’s the code to ingress **Temperature**:</span></span>

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

<span data-ttu-id="37762-223">Tento kód následující událostí serializovaný odešle do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-223">This code sends the following serialized event to IoT Hub:</span></span>

```
{"Temperature":75}
```

<span data-ttu-id="37762-224">A kód pro odesílání událostí vlhkosti vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="37762-224">And the code for sending the Humidity event appears as follows:</span></span>

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="37762-225">Tento kód to odešle do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-225">This code sends this to IoT Hub:</span></span>

```
{"Humidity":45}
```

<span data-ttu-id="37762-226">Pokud existují ještě žádné výskyt nečekaných událostí.</span><span class="sxs-lookup"><span data-stu-id="37762-226">So far there are still no surprises.</span></span> <span data-ttu-id="37762-227">Nyní změníme jak používáme makro SERIALIZACE.</span><span class="sxs-lookup"><span data-stu-id="37762-227">Now let's change how we use the SERIALIZE macro.</span></span>

<span data-ttu-id="37762-228">**SERIALIZACE** makro může trvat několik dat události jako argumenty.</span><span class="sxs-lookup"><span data-stu-id="37762-228">The **SERIALIZE** macro can take multiple data events as arguments.</span></span> <span data-ttu-id="37762-229">To umožňuje nám k serializaci **teploty** a **vlhkosti** událostí společně a odešlete je do služby IoT Hub v jednom volání:</span><span class="sxs-lookup"><span data-stu-id="37762-229">This enables us to serialize the **Temperature** and **Humidity** event together and send them to IoT Hub in one call:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="37762-230">Může uhodnout, že výsledkem tohoto kódu je, že se dvěma datovými události posílají do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-230">You might guess that the result of this code is that two data events are sent to IoT Hub:</span></span>

<span data-ttu-id="37762-231">[</span><span class="sxs-lookup"><span data-stu-id="37762-231">[</span></span>

<span data-ttu-id="37762-232">{"Teploty": 75},</span><span class="sxs-lookup"><span data-stu-id="37762-232">{"Temperature":75},</span></span>

<span data-ttu-id="37762-233">{"Vlhkosti": 45}</span><span class="sxs-lookup"><span data-stu-id="37762-233">{"Humidity":45}</span></span>

<span data-ttu-id="37762-234">]</span><span class="sxs-lookup"><span data-stu-id="37762-234">]</span></span>

<span data-ttu-id="37762-235">Jinými slovy, můžou očekávat, že tento kód je stejný jako odesílání **teploty** a **vlhkosti** samostatně.</span><span class="sxs-lookup"><span data-stu-id="37762-235">In other words, you might expect that this code is the same as sending **Temperature** and **Humidity** separately.</span></span> <span data-ttu-id="37762-236">Je právě v zájmu usnadnění předat obou události **SERIALIZACE** ve stejném volání.</span><span class="sxs-lookup"><span data-stu-id="37762-236">It’s just a convenience to pass both events to **SERIALIZE** in the same call.</span></span> <span data-ttu-id="37762-237">Nicméně, který tak není.</span><span class="sxs-lookup"><span data-stu-id="37762-237">However, that’s not the case.</span></span> <span data-ttu-id="37762-238">Místo toho výše uvedený kód odešle tato událost jednoho datového do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-238">Instead, the code above sends this single data event to IoT Hub:</span></span>

<span data-ttu-id="37762-239">{"Teploty": 75, "vlhkosti": 45}</span><span class="sxs-lookup"><span data-stu-id="37762-239">{"Temperature":75, "Humidity":45}</span></span>

<span data-ttu-id="37762-240">To může zdát neobvyklé, protože naše model definuje **teploty** a **vlhkosti** jako dvě *samostatné* události:</span><span class="sxs-lookup"><span data-stu-id="37762-240">This may seem strange because our model defines **Temperature** and **Humidity** as two *separate* events:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="37762-241">Více bodu jsme nebyl model tyto události kde **teploty** a **vlhkosti** jsou ve stejné struktuře:</span><span class="sxs-lookup"><span data-stu-id="37762-241">More to the point, we didn’t model these events where **Temperature** and **Humidity** are in the same structure:</span></span>

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

<span data-ttu-id="37762-242">Pokud jsme použili tento model, bude možné snadněji pochopit, jak **teploty** a **vlhkosti** se využije stejné serializovaná zpráva.</span><span class="sxs-lookup"><span data-stu-id="37762-242">If we used this model, it would be easier to understand how **Temperature** and **Humidity** would be sent in the same serialized message.</span></span> <span data-ttu-id="37762-243">Ale nemusí být zřejmé, proč funguje tímto způsobem při předání obě data události **SERIALIZACE** pomocí modelu 2.</span><span class="sxs-lookup"><span data-stu-id="37762-243">However it may not be clear why it works that way when you pass both data events to **SERIALIZE** using model 2.</span></span>

<span data-ttu-id="37762-244">Toto chování je jednodušší zjistit, pokud víte, předpokladů že **serializátor** je zajistit knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-244">This behavior is easier to understand if you know the assumptions that the **serializer** library is making.</span></span> <span data-ttu-id="37762-245">Chcete-li mít smysl to přejděte zpět do našeho modelu:</span><span class="sxs-lookup"><span data-stu-id="37762-245">To make sense of this let’s go back to our model:</span></span>

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

<span data-ttu-id="37762-246">Tento model si můžete představit v objektově orientované podmínky.</span><span class="sxs-lookup"><span data-stu-id="37762-246">Think of this model in object-oriented terms.</span></span> <span data-ttu-id="37762-247">V takovém případě jsme se modelování fyzického zařízení (termostatu) a zařízení zahrnuje atributů, například **teploty** a **vlhkosti**.</span><span class="sxs-lookup"><span data-stu-id="37762-247">In this case we’re modeling a physical device (a thermostat) and that device includes attributes like **Temperature** and **Humidity**.</span></span>

<span data-ttu-id="37762-248">Vám můžeme poslat celý stav našeho modelu s kódem například následující:</span><span class="sxs-lookup"><span data-stu-id="37762-248">We can send the entire state of our model with code such as the following:</span></span>

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

<span data-ttu-id="37762-249">Za předpokladu, že hodnoty z teploty, nastavení vlhkosti a času, vidíme by událost takto odeslané do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="37762-249">Assuming the values of Temperature, Humidity and Time are set, we would see an event like this sent to IoT Hub:</span></span>

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="37762-250">Někdy může jenom chcete odeslat *některé* vlastnosti modelu do cloudu (to je obzvláště hodnota true, pokud model obsahuje velké množství dat událostí).</span><span class="sxs-lookup"><span data-stu-id="37762-250">Sometimes you may only want to send *some* properties of the model to the cloud (this is especially true if your model contains a large number of data events).</span></span> <span data-ttu-id="37762-251">Je užitečné k odeslání pouze podmnožinu dat události, například v našem příkladu starší:</span><span class="sxs-lookup"><span data-stu-id="37762-251">It’s useful to send only a subset of data events, such as in our earlier example:</span></span>

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

<span data-ttu-id="37762-252">Tím se vygeneruje přesně stejnou událostí serializovaný jako kdyby měl definovaného **TemperatureEvent** s **teploty** a **čas** člena, stejně jako jsme s modelu 1.</span><span class="sxs-lookup"><span data-stu-id="37762-252">This generates exactly the same serialized event as if we had defined a **TemperatureEvent** with a **Temperature** and **Time** member, just as we did with model 1.</span></span> <span data-ttu-id="37762-253">V takovém případě jsme byli schopni generování přesně stejnou událostí serializovaný pomocí jiného modelu (model 2), protože volali jsme **SERIALIZACE** jiným způsobem.</span><span class="sxs-lookup"><span data-stu-id="37762-253">In this case we were able to generate exactly the same serialized event by using a different model (model 2) because we called **SERIALIZE** in a different way.</span></span>

<span data-ttu-id="37762-254">Důležité je, že pokud předáte více dat událostí do **SERIALIZACE,** pak předpokládá, že každá událost je vlastnost v jeden objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="37762-254">The important point is that if you pass multiple data events to **SERIALIZE,** then it assumes each event is a property in a single JSON object.</span></span>

<span data-ttu-id="37762-255">Nejlepší metodou závisí na a jak si myslíte o modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-255">The best approach depends on you and how you think about your model.</span></span> <span data-ttu-id="37762-256">Pokud odesíláte "události" do cloudu a každé události, obsahuje sada vlastností, prvním přístupem je velké množství smysl.</span><span class="sxs-lookup"><span data-stu-id="37762-256">If you’re sending "events" to the cloud and each event contains a defined set of properties, then the first approach makes a lot of sense.</span></span> <span data-ttu-id="37762-257">V takovém případě byste použili **DECLARE\_struktura** definovat strukturu každé události a zahrnout je do modelu pomocí **WITH\_DATA** makro.</span><span class="sxs-lookup"><span data-stu-id="37762-257">In that case you would use **DECLARE\_STRUCT** to define the structure of each event and then include them in your model with the **WITH\_DATA** macro.</span></span> <span data-ttu-id="37762-258">Pak je odeslat všechny události, jako jsme to udělali v prvním příkladu výše.</span><span class="sxs-lookup"><span data-stu-id="37762-258">Then you send each event as we did in the first example above.</span></span> <span data-ttu-id="37762-259">V tomto přístupu jenom předáte jednoho datového událost pro **SERIALIZÁTOR**.</span><span class="sxs-lookup"><span data-stu-id="37762-259">In this approach you would only pass a single data event to **SERIALIZER**.</span></span>

<span data-ttu-id="37762-260">Pokud si myslíte o modelu způsobem objektově orientované, může vyhovovat druhý postup je.</span><span class="sxs-lookup"><span data-stu-id="37762-260">If you think about your model in an object-oriented fashion, then the second approach may suit you.</span></span> <span data-ttu-id="37762-261">V takovém případě elementy definovat pomocí **WITH\_DATA** jsou "vlastnosti" objektu.</span><span class="sxs-lookup"><span data-stu-id="37762-261">In this case, the elements defined using **WITH\_DATA** are the "properties" of your object.</span></span> <span data-ttu-id="37762-262">Předat ať podmnožinu události **SERIALIZACE** , se vám líbí, podle toho, kolik z vaší "" stav objektu se mají posílat do cloudu.</span><span class="sxs-lookup"><span data-stu-id="37762-262">You pass whatever subset of events to **SERIALIZE** that you like, depending on how much of your "object’s" state you want to send to the cloud.</span></span>

<span data-ttu-id="37762-263">Nether přístup je pravé nebo nesprávné.</span><span class="sxs-lookup"><span data-stu-id="37762-263">Nether approach is right or wrong.</span></span> <span data-ttu-id="37762-264">Právě zajímat, jak **serializátor** funguje knihovny a vybrat přístup modelování, který nejlépe vyhovuje vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="37762-264">Just be aware of how the **serializer** library works, and pick the modeling approach that best fits your needs.</span></span>

## <a name="message-handling"></a><span data-ttu-id="37762-265">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="37762-265">Message handling</span></span>
<span data-ttu-id="37762-266">Tento článek, pokud má pouze popsané odesílání událostí do služby IoT Hub a nebyl řešit přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="37762-266">So far this article has only discussed sending events to IoT Hub, and hasn't addressed receiving messages.</span></span> <span data-ttu-id="37762-267">Z důvodu pro to, co je potřeba vědět o přijímání zpráv má z velké části popsaná v [dříve článek](iot-hub-device-sdk-c-intro.md).</span><span class="sxs-lookup"><span data-stu-id="37762-267">The reason for this is that what we need to know about receiving messages has largely been covered in an [earlier article](iot-hub-device-sdk-c-intro.md).</span></span> <span data-ttu-id="37762-268">Z tohoto článku odvolat, zpracování zpráv tak, že zaregistrujete funkci zpětného volání zpráva:</span><span class="sxs-lookup"><span data-stu-id="37762-268">Recall from that article that you process messages by registering a message callback function:</span></span>

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

<span data-ttu-id="37762-269">Funkce zpětného volání, která je volána, když je obdržena zprávu zapište:</span><span class="sxs-lookup"><span data-stu-id="37762-269">You then write the callback function that’s invoked when a message is received:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
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

<span data-ttu-id="37762-270">Tato implementace **IoTHubMessage** volá konkrétní funkci pro každou akci do modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-270">This implementation of **IoTHubMessage** calls the specific function for each action in your model.</span></span> <span data-ttu-id="37762-271">Například pokud váš model definuje tato akce:</span><span class="sxs-lookup"><span data-stu-id="37762-271">For example, if your model defines this action:</span></span>

```
WITH_ACTION(SetAirResistance, int, Position)
```

<span data-ttu-id="37762-272">Je nutné definovat funkce s Tento podpis:</span><span class="sxs-lookup"><span data-stu-id="37762-272">You must define a function with this signature:</span></span>

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

<span data-ttu-id="37762-273">**SetAirResistance** pak je volána při odeslání zprávy do vašeho zařízení.</span><span class="sxs-lookup"><span data-stu-id="37762-273">**SetAirResistance** is then called when that message is sent to your device.</span></span>

<span data-ttu-id="37762-274">Co jsme nebyly vysvětlené ještě je serializovaná verze zprávy, která bude vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="37762-274">What we haven't explained yet is what the serialized version of message looks like.</span></span> <span data-ttu-id="37762-275">Jinými slovy Pokud chcete odeslat **SetAirResistance** zpráva do zařízení, co typu?</span><span class="sxs-lookup"><span data-stu-id="37762-275">In other words, if you want to send a **SetAirResistance** message to your device, what does that look like?</span></span>

<span data-ttu-id="37762-276">Pokud odesíláte zprávy do zařízení, krok lze provést pomocí sady SDK služby Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="37762-276">If you're sending a message to a device, you would do so through the Azure IoT service SDK.</span></span> <span data-ttu-id="37762-277">Dál potřebujete vědět, jaké řetězec k odeslání do volání určité akce.</span><span class="sxs-lookup"><span data-stu-id="37762-277">You still need to know what string to send to invoke a particular action.</span></span> <span data-ttu-id="37762-278">Obecný formát pro odesílání zprávy vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="37762-278">The general format for sending a message appears as follows:</span></span>

```
{"Name" : "", "Parameters" : "" }
```

<span data-ttu-id="37762-279">Odesíláte serializovaný objekt JSON s dvě vlastnosti: **název** je název akce (zprávy) a **parametry** obsahuje parametry této akce.</span><span class="sxs-lookup"><span data-stu-id="37762-279">You're sending a serialized JSON object with two properties: **Name** is the name of the action (message) and **Parameters** contains the parameters of that action.</span></span>

<span data-ttu-id="37762-280">Například pro vyvolání **SetAirResistance** tuto zprávu můžete odeslat do zařízení:</span><span class="sxs-lookup"><span data-stu-id="37762-280">For example, to invoke **SetAirResistance** you can send this message to a device:</span></span>

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

<span data-ttu-id="37762-281">Název akce musí přesně shodovat akce definované v modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-281">The action name must exactly match an action defined in your model.</span></span> <span data-ttu-id="37762-282">Názvy parametrů musí odpovídat také.</span><span class="sxs-lookup"><span data-stu-id="37762-282">The parameter names must match as well.</span></span> <span data-ttu-id="37762-283">Všimněte si také malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="37762-283">Also note case sensitivity.</span></span> <span data-ttu-id="37762-284">**Název** a **parametry** jsou vždy velká písmena.</span><span class="sxs-lookup"><span data-stu-id="37762-284">**Name** and **Parameters** are always uppercase.</span></span> <span data-ttu-id="37762-285">Ujistěte se, že malá a velká písmena názvu akce a parametrů do modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-285">Make sure to match the case of your action name and parameters in your model.</span></span> <span data-ttu-id="37762-286">V tomto příkladu je název akce "SetAirResistance" a není "setairresistance".</span><span class="sxs-lookup"><span data-stu-id="37762-286">In this example, the action name is "SetAirResistance" and not "setairresistance".</span></span>

<span data-ttu-id="37762-287">Další dvě akce **TurnFanOn** a **TurnFanOff** nelze vyvolat odesláním těchto zpráv do zařízení:</span><span class="sxs-lookup"><span data-stu-id="37762-287">The two other actions **TurnFanOn** and **TurnFanOff** can be invoked by sending these messages to a device:</span></span>

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

<span data-ttu-id="37762-288">Tato část popisuje všechno, co potřebujete vědět, kdy události odesílání a příjem zpráv s **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-288">This section described everything you need to know when sending events and receiving messages with the **serializer** library.</span></span> <span data-ttu-id="37762-289">Než budete pokračovat, můžeme zahrnují některé parametry, které můžete nakonfigurovat, které řídí, jak velké je váš model.</span><span class="sxs-lookup"><span data-stu-id="37762-289">Before moving on, let's cover some parameters you can configure that control how large your model is.</span></span>

## <a name="macro-configuration"></a><span data-ttu-id="37762-290">Konfigurace – makro</span><span class="sxs-lookup"><span data-stu-id="37762-290">Macro configuration</span></span>
<span data-ttu-id="37762-291">Pokud používáte **serializátor** knihovny důležitou součástí sady SDK zajímat se nachází v knihovně azure-c sdílené – nástroj.</span><span class="sxs-lookup"><span data-stu-id="37762-291">If you’re using the **Serializer** library an important part of the SDK to be aware of is found in the azure-c-shared-utility library.</span></span>
<span data-ttu-id="37762-292">Při naklonování úložišti Azure-iot-sdk-c z Githubu pomocí možnosti--rekurzivní zjistíte této sdílené nástroj knihovny:</span><span class="sxs-lookup"><span data-stu-id="37762-292">If you have cloned the Azure-iot-sdk-c repository from GitHub using the --recursive option, then you will find this shared utility library here:</span></span>

```
.\\c-utility
```

<span data-ttu-id="37762-293">Pokud nebyly klonovat knihovny, můžete ji najít [zde](https://github.com/Azure/azure-c-shared-utility).</span><span class="sxs-lookup"><span data-stu-id="37762-293">If you have not cloned the library, you can find it [here](https://github.com/Azure/azure-c-shared-utility).</span></span>

<span data-ttu-id="37762-294">V knihovně Sdílené nástroj najdete následující složku:</span><span class="sxs-lookup"><span data-stu-id="37762-294">Within the shared utility library, you will find the following folder:</span></span>

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

<span data-ttu-id="37762-295">Tato složka obsahuje řešení sady Visual Studio názvem **makro\_utils\_h\_generator.sln**:</span><span class="sxs-lookup"><span data-stu-id="37762-295">This folder contains a Visual Studio solution called **macro\_utils\_h\_generator.sln**:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

<span data-ttu-id="37762-296">Generuje program v tomto řešení **makro\_utils.h** souboru.</span><span class="sxs-lookup"><span data-stu-id="37762-296">The program in this solution generates the **macro\_utils.h** file.</span></span> <span data-ttu-id="37762-297">Je výchozí makro\_utils.h soubor je součástí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="37762-297">There’s a default macro\_utils.h file included with the SDK.</span></span> <span data-ttu-id="37762-298">Toto řešení umožňuje změnit některé parametry a pak znovu vytvořte soubor hlaviček na základě těchto parametrů.</span><span class="sxs-lookup"><span data-stu-id="37762-298">This solution allows you to modify some parameters and then recreate the header file based on these parameters.</span></span>

<span data-ttu-id="37762-299">Jsou dva klíče parametry se to týká s **nArithmetic** a **nMacroParameters** které jsou definované v těchto dvou řádcích najít v makro\_utils.tt:</span><span class="sxs-lookup"><span data-stu-id="37762-299">The two key parameters to be concerned with are **nArithmetic** and **nMacroParameters** which are defined in these two lines found in macro\_utils.tt:</span></span>

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

<span data-ttu-id="37762-300">Tyto hodnoty jsou výchozí parametry součástí sady SDK.</span><span class="sxs-lookup"><span data-stu-id="37762-300">These values are the default parameters included with the SDK.</span></span> <span data-ttu-id="37762-301">Jednotlivé parametry mají následující význam:</span><span class="sxs-lookup"><span data-stu-id="37762-301">Each parameter has the following meaning:</span></span>

* <span data-ttu-id="37762-302">nMacroParameters – Určuje, kolik parametry může mít v jedné DECLARE\_definici makra modelu.</span><span class="sxs-lookup"><span data-stu-id="37762-302">nMacroParameters – Controls how many parameters you can have in one DECLARE\_MODEL macro definition.</span></span>
* <span data-ttu-id="37762-303">nArithmetic – ovládací prvky celkový počet členů v modelu povolené.</span><span class="sxs-lookup"><span data-stu-id="37762-303">nArithmetic – Controls the total number of members allowed in a model.</span></span>

<span data-ttu-id="37762-304">Z důvodu, který tyto parametry jsou důležité je, protože tím i určovat, jak velká může být váš model.</span><span class="sxs-lookup"><span data-stu-id="37762-304">The reason these parameters are important is because they control how large your model can be.</span></span> <span data-ttu-id="37762-305">Představte si třeba tuto definici modelu:</span><span class="sxs-lookup"><span data-stu-id="37762-305">For example, consider this model definition:</span></span>

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

<span data-ttu-id="37762-306">Jak je uvedeno nahoře, **DECLARE\_modelu** je právě makro C.</span><span class="sxs-lookup"><span data-stu-id="37762-306">As mentioned previously, **DECLARE\_MODEL** is just a C macro.</span></span> <span data-ttu-id="37762-307">Názvy modelu a **WITH\_DATA** – příkaz (ještě jiné makro) jsou parametry **DECLARE\_modelu**.</span><span class="sxs-lookup"><span data-stu-id="37762-307">The names of the model and the **WITH\_DATA** statement (yet another macro) are parameters of **DECLARE\_MODEL**.</span></span> <span data-ttu-id="37762-308">**nMacroParameters** definuje, kolik parametry můžou být součástí **DECLARE\_modelu**.</span><span class="sxs-lookup"><span data-stu-id="37762-308">**nMacroParameters** defines how many parameters can be included in **DECLARE\_MODEL**.</span></span> <span data-ttu-id="37762-309">Definuje efektivně, kolik dat událostí a akce deklarace může mít.</span><span class="sxs-lookup"><span data-stu-id="37762-309">Effectively, this defines how many data event and action declarations you can have.</span></span> <span data-ttu-id="37762-310">Jako takový výchozí limit 124 to znamená, že můžete definovat model pomocí kombinace o 60 akce a data události.</span><span class="sxs-lookup"><span data-stu-id="37762-310">As such, with the default limit of 124 this means that you can define a model with a combination of about 60 actions and data events.</span></span> <span data-ttu-id="37762-311">Pokud se pokusíte překročí toto omezení, dostanete chyby kompilátoru, které vypadají podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="37762-311">If you try to exceed this limit, you'll receive compiler errors that look similar to this:</span></span>

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

<span data-ttu-id="37762-312">**NArithmetic** parametr se o vnitřní strukturu a funkci jazyka – makro víc než vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="37762-312">The **nArithmetic** parameter is more about the internal workings of the macro language than your application.</span></span>  <span data-ttu-id="37762-313">Jimi řídí celkový počet členů, může mít v modelu, včetně **DECLARE_STRUCT** makra.</span><span class="sxs-lookup"><span data-stu-id="37762-313">It controls the total number of members you can have in your model, including **DECLARE_STRUCT** macros.</span></span> <span data-ttu-id="37762-314">Pokud spustíte zobrazuje chyby kompilátoru, jako je tato, pak doporučujeme zvýšit **nArithmetic**:</span><span class="sxs-lookup"><span data-stu-id="37762-314">If you start seeing compiler errors such as this, then you should try increasing **nArithmetic**:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

<span data-ttu-id="37762-315">Pokud chcete změnit tyto parametry, upravte hodnoty v makro\_utils.tt souboru, znovu zkompiluje makro\_utils\_h\_generator.sln řešení a spusťte zkompilovaný program.</span><span class="sxs-lookup"><span data-stu-id="37762-315">If you want to change these parameters, modify the values in the macro\_utils.tt file, recompile the macro\_utils\_h\_generator.sln solution, and run the compiled program.</span></span> <span data-ttu-id="37762-316">Když uděláte, nové makro\_utils.h soubor je vygenerována a uložena v umístění.\\ běžné\\inc adresáře.</span><span class="sxs-lookup"><span data-stu-id="37762-316">When you do so, a new macro\_utils.h file is generated and placed in the .\\common\\inc directory.</span></span>

<span data-ttu-id="37762-317">Chcete-li použít novou verzi – makro\_utils.h, odebrat **serializátor** balíček NuGet, řešení a v jeho místní zahrnovat **serializátor** projektu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37762-317">In order to use the new version of macro\_utils.h, remove the **serializer** NuGet package from your solution and in its place include the **serializer** Visual Studio project.</span></span> <span data-ttu-id="37762-318">To umožňuje váš kód mohl zkompilovat proti zdrojový kód knihovny serializátor.</span><span class="sxs-lookup"><span data-stu-id="37762-318">This enables your code to compile against the source code of the serializer library.</span></span> <span data-ttu-id="37762-319">To zahrnuje aktualizované makro\_utils.h.</span><span class="sxs-lookup"><span data-stu-id="37762-319">This includes the updated macro\_utils.h.</span></span> <span data-ttu-id="37762-320">Pokud chcete to udělat pro **simplesample\_amqp**, začněte tím, že balíček NuGet pro knihovnu serializátor odebráním řešení:</span><span class="sxs-lookup"><span data-stu-id="37762-320">If you want to do this for **simplesample\_amqp**, start by removing the NuGet package for the serializer library from the solution:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

<span data-ttu-id="37762-321">Pak přidejte tento projekt do řešení sady Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="37762-321">Then add this project to your Visual Studio solution:</span></span>

> <span data-ttu-id="37762-322">. \\c\\serializátor\\sestavení\\windows\\serializer.vcxproj</span><span class="sxs-lookup"><span data-stu-id="37762-322">.\\c\\serializer\\build\\windows\\serializer.vcxproj</span></span>
> 
> 

<span data-ttu-id="37762-323">Když jste hotovi, řešení by měl vypadat přibližně takto:</span><span class="sxs-lookup"><span data-stu-id="37762-323">When you're done, your solution should look like this:</span></span>

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

<span data-ttu-id="37762-324">Nyní když zkompilujete řešení aktualizované makro\_utils.h je součástí vaší binární.</span><span class="sxs-lookup"><span data-stu-id="37762-324">Now when you compile your solution, the updated macro\_utils.h is included in your binary.</span></span>

<span data-ttu-id="37762-325">Všimněte si, že zvýšení tyto hodnoty dostatečně vysoký může překročit omezení kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="37762-325">Note that increasing these values high enough can exceed compiler limits.</span></span> <span data-ttu-id="37762-326">K tomuto bodu **nMacroParameters** je hlavní parametr, pomocí kterého se to týká.</span><span class="sxs-lookup"><span data-stu-id="37762-326">To this point, the **nMacroParameters** is the main parameter with which to be concerned.</span></span> <span data-ttu-id="37762-327">Specifikace C99 Určuje, že minimálně 127 parametry jsou povoleny v definici makra.</span><span class="sxs-lookup"><span data-stu-id="37762-327">The C99 spec specifies that a minimum of 127 parameters are allowed in a macro definition.</span></span> <span data-ttu-id="37762-328">Kompilátor Microsoft přesně odpovídá o specifikace (a může obsahovat maximálně 127 znaků), takže nebude možné zvýšit **nMacroParameters** nad výchozí.</span><span class="sxs-lookup"><span data-stu-id="37762-328">The Microsoft compiler follows the spec exactly (and has a limit of 127), so you won't be able to increase **nMacroParameters** beyond the default.</span></span> <span data-ttu-id="37762-329">Další kompilátory může umožňují Uděláte to tak (například kompilátoru GNU podporuje vyšší limit).</span><span class="sxs-lookup"><span data-stu-id="37762-329">Other compilers might allow you to do so (for example, the GNU compiler supports a higher limit).</span></span>

<span data-ttu-id="37762-330">Dosavadní téměř všechno, co potřebujete vědět o tom, jak psát kód, který představuje jste zahrnutých jsme **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-330">So far we've covered just about everything you need to know about how to write code with the **serializer** library.</span></span> <span data-ttu-id="37762-331">Před uzavřením, můžeme pokroku některá témata z předchozí články, které asi vás zajímá o.</span><span class="sxs-lookup"><span data-stu-id="37762-331">Before concluding, let's revisit some topics from previous articles that you may be wondering about.</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="37762-332">Rozhraní API nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="37762-332">The lower-level APIs</span></span>
<span data-ttu-id="37762-333">Ukázkovou aplikaci, na kterém se tento článek zaměřuje je **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="37762-333">The sample application on which this article focused is **simplesample\_amqp**.</span></span> <span data-ttu-id="37762-334">Této ukázce se používá vyšší úrovni (jiný-"UDOU") rozhraní API pro události odesílat a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="37762-334">This sample uses the higher-level (the non-"LL") APIs to send events and receive messages.</span></span> <span data-ttu-id="37762-335">Používáte-li tato rozhraní API, spustí vlákna na pozadí, který se stará o odesílání událostí i přijímání zpráv.</span><span class="sxs-lookup"><span data-stu-id="37762-335">If you use these APIs, a background thread runs which takes care of both sending events and receiving messages.</span></span> <span data-ttu-id="37762-336">Můžete však použít rozhraní API nižší úrovni (vše) Pokud chcete odstranit tento vlákna na pozadí a proveďte explicitní kontrolu nad při odesílání událostí nebo příjem zpráv z cloudu.</span><span class="sxs-lookup"><span data-stu-id="37762-336">However, you can use the lower-level (LL) APIs to eliminate this background thread and take explicit control over when you send events or receive messages from the cloud.</span></span>

<span data-ttu-id="37762-337">Jak je popsáno v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md), je sada funkcí, které se skládá z vyšší úrovně rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="37762-337">As described in a [previous article](iot-hub-device-sdk-c-iothubclient.md), there is a set of functions that consists of the higher-level APIs:</span></span>

* <span data-ttu-id="37762-338">IoTHubClient\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="37762-338">IoTHubClient\_CreateFromConnectionString</span></span>
* <span data-ttu-id="37762-339">IoTHubClient\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="37762-339">IoTHubClient\_SendEventAsync</span></span>
* <span data-ttu-id="37762-340">IoTHubClient\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="37762-340">IoTHubClient\_SetMessageCallback</span></span>
* <span data-ttu-id="37762-341">IoTHubClient\_Destroy</span><span class="sxs-lookup"><span data-stu-id="37762-341">IoTHubClient\_Destroy</span></span>

<span data-ttu-id="37762-342">Tato rozhraní API je ukázán v **simplesample\_amqp**.</span><span class="sxs-lookup"><span data-stu-id="37762-342">These APIs are demonstrated in **simplesample\_amqp**.</span></span>

<span data-ttu-id="37762-343">K dispozici je také podobá sadu rozhraní API nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="37762-343">There is also an analogous set of lower-level APIs.</span></span>

* <span data-ttu-id="37762-344">IoTHubClient\_UDOU\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="37762-344">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="37762-345">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="37762-345">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="37762-346">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="37762-346">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="37762-347">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="37762-347">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="37762-348">Některé nižší úrovně rozhraní API funkční stejným způsobem, jak je popsáno v předchozí článcích.</span><span class="sxs-lookup"><span data-stu-id="37762-348">Note that the lower-level APIs work exactly the same way as described in the previous articles.</span></span> <span data-ttu-id="37762-349">První sadu rozhraní API můžete použít, pokud chcete, aby vlákna na pozadí pro zpracování událostí odesílání a příjmu zprávy.</span><span class="sxs-lookup"><span data-stu-id="37762-349">You can use the first set of APIs if you want a background thread to handle sending events and receiving messages.</span></span> <span data-ttu-id="37762-350">Druhá sada rozhraní API použijte, pokud chcete, aby explicitní ovládat, kdy odesílat a přijímat data ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="37762-350">You use the second set of APIs if you want explicit control over when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="37762-351">Buď sadu rozhraní API pracovní stejnou měrou i s **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-351">Either set of APIs work equally well with the **serializer** library.</span></span>

<span data-ttu-id="37762-352">Příklad použití rozhraní API nižší úrovně s **serializátor** knihovny, najdete v článku **simplesample\_http** aplikace.</span><span class="sxs-lookup"><span data-stu-id="37762-352">For an example of how the lower-level APIs are used with the **serializer** library, see the **simplesample\_http** application.</span></span>

## <a name="additional-topics"></a><span data-ttu-id="37762-353">Další témata</span><span class="sxs-lookup"><span data-stu-id="37762-353">Additional topics</span></span>
<span data-ttu-id="37762-354">Několik dalších tématech důležité zmínit, znovu se vlastnost zpracování, pomocí přihlašovacích údajů jiného zařízení a možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="37762-354">A few other topics worth mentioning again are property handling, using alternate device credentials, and configuration options.</span></span> <span data-ttu-id="37762-355">Toto jsou všechna témata popsaná v [předchozí článek](iot-hub-device-sdk-c-iothubclient.md).</span><span class="sxs-lookup"><span data-stu-id="37762-355">These are all topics covered in a [previous article](iot-hub-device-sdk-c-iothubclient.md).</span></span> <span data-ttu-id="37762-356">Hlavní bod je, že všechny tyto funkce fungovat stejně jako s **serializátor** knihovny stejně jako s **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-356">The main point is that all of these features work in the same way with the **serializer** library as they do with the **IoTHubClient** library.</span></span> <span data-ttu-id="37762-357">Například pokud chcete přiřadit vlastnosti události z modelu, použijete **IoTHubMessage\_vlastnosti** a **mapy**\_**AddorUpdate**, stejným způsobem, jak je popsáno dříve:</span><span class="sxs-lookup"><span data-stu-id="37762-357">For example, if you want to attach properties to an event from your model, you use **IoTHubMessage\_Properties** and **Map**\_**AddorUpdate**, the same way as described previously:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="37762-358">Zda událost byla vygenerována z **serializátor** knihovny nebo vytvořit ručně pomocí **IoTHubClient** knihovny nezáleží.</span><span class="sxs-lookup"><span data-stu-id="37762-358">Whether the event was generated from the **serializer** library or created manually using the **IoTHubClient** library does not matter.</span></span>

<span data-ttu-id="37762-359">Pro přihlašovací údaje jiného zařízení, pomocí **IoTHubClient\_UDOU\_vytvořit** funguje stejně dobře jako **IoTHubClient\_CreateFromConnectionString** pro přidělování **IOTHUB\_klienta\_zpracování**.</span><span class="sxs-lookup"><span data-stu-id="37762-359">For the alternate device credentials, using **IoTHubClient\_LL\_Create** works just as well as **IoTHubClient\_CreateFromConnectionString** for allocating an **IOTHUB\_CLIENT\_HANDLE**.</span></span>

<span data-ttu-id="37762-360">Nakonec pokud používáte **serializátor** knihovny, můžete nastavit možnosti konfigurace s **IoTHubClient\_UDOU\_SetOption** stejně jako jste to udělali při použití **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-360">Finally, if you're using the **serializer** library, you can set configuration options with **IoTHubClient\_LL\_SetOption** just as you did when using the **IoTHubClient** library.</span></span>

<span data-ttu-id="37762-361">Funkce, které jsou jedinečné pro **serializátor** knihovny se inicializace rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="37762-361">A feature that is unique to the **serializer** library are the initialization APIs.</span></span> <span data-ttu-id="37762-362">Před zahájením práce s knihovny, musí volat **serializátor\_init**:</span><span class="sxs-lookup"><span data-stu-id="37762-362">Before you can start working with the library, you must call **serializer\_init**:</span></span>

```
serializer_init(NULL);
```

<span data-ttu-id="37762-363">To se provádí těsně před voláním **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="37762-363">This is done just before you call **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="37762-364">Podobně po dokončení práce s knihovnou, posledním volání budete provedete je **serializátor\_deinit**:</span><span class="sxs-lookup"><span data-stu-id="37762-364">Similarly, when you're done working with the library, the last call you’ll make is to **serializer\_deinit**:</span></span>

```
serializer_deinit();
```

<span data-ttu-id="37762-365">Jinak, všechny ostatní funkce výše uvedených fungovat stejně **serializátor** knihovny nebudou **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="37762-365">Otherwise, all of the other features listed above work the same in the **serializer** library as they do in the **IoTHubClient** library.</span></span> <span data-ttu-id="37762-366">Další informace o kterékoli z těchto témat, najdete v článku [předchozí článek](iot-hub-device-sdk-c-iothubclient.md) této série.</span><span class="sxs-lookup"><span data-stu-id="37762-366">For more information about any of these topics, see the [previous article](iot-hub-device-sdk-c-iothubclient.md) in this series.</span></span>

## <a name="next-steps"></a><span data-ttu-id="37762-367">Další kroky</span><span class="sxs-lookup"><span data-stu-id="37762-367">Next steps</span></span>
<span data-ttu-id="37762-368">Tento článek podrobně popisuje jedinečné aspekty **serializátor** knihovny, které jsou součástí **zařízení Azure IoT SDK pro jazyk C**. S informacemi, pokud že byste měli mít dostatečné povědomí o tom, jak používat modely odesílat události a přijímat zprávy ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="37762-368">This article describes in detail the unique aspects of the **serializer** library contained in the **Azure IoT device SDK for C**. With the information provided you should have a good understanding of how to use models to send events and receive messages from IoT Hub.</span></span>

<span data-ttu-id="37762-369">To se taky ukončí řady třemi částmi o tom, jak vyvíjet aplikace s **zařízení Azure IoT SDK pro jazyk C**. To by měl být dostatek informací pro jenom vám pomůžou začít ale získáte důkladné znalosti o fungování rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="37762-369">This also concludes the three-part series on how to develop applications with the **Azure IoT device SDK for C**. This should be enough information to not only get you started but give you a thorough understanding of how the APIs work.</span></span> <span data-ttu-id="37762-370">Další informace nejsou několik ukázky v sadě SDK, které nejsou zahrnuté v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="37762-370">For additional information, there are a few samples in the SDK not covered here.</span></span> <span data-ttu-id="37762-371">Jinak [dokumentaci k sadě SDK](https://github.com/Azure/azure-iot-sdk-c) je dobré prostředku pro další informace.</span><span class="sxs-lookup"><span data-stu-id="37762-371">Otherwise, the [SDK documentation](https://github.com/Azure/azure-iot-sdk-c) is a good resource for additional information.</span></span>

<span data-ttu-id="37762-372">Další informace o vývoji pro IoT Hub, najdete v tématu [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="37762-372">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="37762-373">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="37762-373">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="37762-374">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="37762-374">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
