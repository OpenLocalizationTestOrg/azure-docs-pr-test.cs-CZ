---
title: "Pro zařízení Azure IoT SDK pro jazyk C - IoTHubClient | Microsoft Docs"
description: "Jak používat knihovnu IoTHubClient v zařízení Azure IoT SDK pro jazyk C vytvoření aplikace pro zařízení, které komunikují pomocí služby IoT hub."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: 422d89014511f0d08ba57a893570ff7b253b7bc4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="54db3-103">Pro zařízení Azure IoT SDK pro jazyk C – informace o IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="54db3-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="54db3-104">[Nejprve článek](iot-hub-device-sdk-c-intro.md) této série zavedená **zařízení Azure IoT SDK pro jazyk C**. Tento článek vysvětluje, SDK jsou dvě vrstvy architektury.</span><span class="sxs-lookup"><span data-stu-id="54db3-104">The [first article](iot-hub-device-sdk-c-intro.md) in this series introduced the **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="54db3-105">Na bázi je **IoTHubClient** knihovnu, která přímo spravuje komunikace se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-105">At the base is the **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="54db3-106">K dispozici je také **serializátor** knihovny, který sestaví v horní části daného k poskytování služeb serializace.</span><span class="sxs-lookup"><span data-stu-id="54db3-106">There's also the **serializer** library that builds on top of that to provide serialization services.</span></span> <span data-ttu-id="54db3-107">V tomto článku jsme vám poskytují další podrobnosti na **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-107">In this article we'll provide additional detail on the **IoTHubClient** library.</span></span>

<span data-ttu-id="54db3-108">Předchozí článek popisuje postup použití **IoTHubClient** k odesílání událostí do služby IoT Hub a příjem zpráv knihovnu.</span><span class="sxs-lookup"><span data-stu-id="54db3-108">The previous article described how to use the **IoTHubClient** library to send events to IoT Hub and receive messages.</span></span> <span data-ttu-id="54db3-109">Tento článek rozšiřuje této diskuzi o vysvětlením, jak spravovat přesněji *při* odesílat a přijímat data, Představujeme vám **nižší úrovně rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="54db3-109">This article extends that discussion by explaining how to more precisely manage *when* you send and receive data, introducing you to the **lower-level APIs**.</span></span> <span data-ttu-id="54db3-110">Dále vysvětlíme postup připojení vlastnosti k události (a je načtou ze zprávy) pomocí vlastnosti zpracování funkce **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-110">We'll also explain how to attach properties to events (and retrieve them from messages) using the property handling features in the **IoTHubClient** library.</span></span> <span data-ttu-id="54db3-111">Nakonec poskytujeme další vysvětlení různých způsobů zpracování zprávy přijaté ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-111">Finally, we'll provide additional explanation of different ways to handle messages received from IoT Hub.</span></span>

<span data-ttu-id="54db3-112">Článek se ukončí pokrývajících několik ostatní témata, včetně více o pověření zařízení a jak změnit chování **IoTHubClient** prostřednictvím možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="54db3-112">The article concludes by covering a couple of miscellaneous topics, including more about device credentials and how to change the behavior of the **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="54db3-113">Použijeme **IoTHubClient** ukázky SDK na tato témata popisují.</span><span class="sxs-lookup"><span data-stu-id="54db3-113">We'll use the **IoTHubClient** SDK samples to explain these topics.</span></span> <span data-ttu-id="54db3-114">Pokud chcete sledovat, najdete v článku **iothub\_klienta\_ukázka\_http** a **iothub\_klienta\_ukázka\_amqp**aplikace, které jsou součástí sady SDK zařízení Azure IoT pro C. všechno popsané v následujících částech je znázorněn v tyto ukázky.</span><span class="sxs-lookup"><span data-stu-id="54db3-114">If you want to follow along, see the **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in the Azure IoT device SDK for C. Everything described in the following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="54db3-115">Můžete najít [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o rozhraní API v [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="54db3-115">You can find the [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of the API in the [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="the-lower-level-apis"></a><span data-ttu-id="54db3-116">Rozhraní API nižší úrovně</span><span class="sxs-lookup"><span data-stu-id="54db3-116">The lower-level APIs</span></span>
<span data-ttu-id="54db3-117">Základní operace popsané v předchozím článku **IotHubClient** v kontextu **iothub\_klienta\_ukázka\_amqp** aplikace.</span><span class="sxs-lookup"><span data-stu-id="54db3-117">The previous article described the basic operation of the **IotHubClient** within the context of the **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="54db3-118">Například vysvětleny postupy k chybě při inicializaci knihovny pomocí tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="54db3-118">For example, it explained how to initialize the library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="54db3-119">Popisuje také postup odesílání událostí pomocí volání této funkce.</span><span class="sxs-lookup"><span data-stu-id="54db3-119">It also described how to send events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="54db3-120">Článek také popisuje jak přijmout zprávy tak, že zaregistrujete funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="54db3-120">The article also described how to receive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="54db3-121">V článku také vám ukázal, jak uvolnit prostředky, jako je například následující kód.</span><span class="sxs-lookup"><span data-stu-id="54db3-121">The article also showed how to free resources using code such as the following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="54db3-122">Jsou ale doprovodné funkce pro každé z těchto rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="54db3-122">However there are companion functions to each of these APIs:</span></span>

* <span data-ttu-id="54db3-123">IoTHubClient\_UDOU\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="54db3-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="54db3-124">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="54db3-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="54db3-125">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="54db3-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="54db3-126">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="54db3-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="54db3-127">Všechny tyto funkce zahrnout "Vše" název rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="54db3-127">These functions all include “LL” in the API name.</span></span> <span data-ttu-id="54db3-128">Než tu, která jsou shodné s jejich protějšky v jiné všechny parametry každou z těchto funkcí.</span><span class="sxs-lookup"><span data-stu-id="54db3-128">Other than that, the parameters of each of these functions are identical to their non-LL counterparts.</span></span> <span data-ttu-id="54db3-129">Chování těchto funkcí je však jinou důležitá jedním způsobem.</span><span class="sxs-lookup"><span data-stu-id="54db3-129">However, the behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="54db3-130">Při volání **IoTHubClient\_CreateFromConnectionString**, základní knihovny vytvořit nové vlákno, které běží na pozadí.</span><span class="sxs-lookup"><span data-stu-id="54db3-130">When you call **IoTHubClient\_CreateFromConnectionString**, the underlying libraries create a new thread that runs in the background.</span></span> <span data-ttu-id="54db3-131">Tento přístup z více vláken odesílá události do a ze služby IoT Hub přijímá zprávy.</span><span class="sxs-lookup"><span data-stu-id="54db3-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="54db3-132">Žádný takový vlákno se vytvoří při práci s "Vše" rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="54db3-132">No such thread is created when working with the "LL" APIs.</span></span> <span data-ttu-id="54db3-133">Vytvoření vlákně na pozadí je pro vaše pohodlí pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="54db3-133">The creation of the background thread is a convenience to the developer.</span></span> <span data-ttu-id="54db3-134">Nemusíte si dělat starosti o explicitně události odesílání a přijímání zpráv ze služby IoT Hub – dojde automaticky na pozadí.</span><span class="sxs-lookup"><span data-stu-id="54db3-134">You don’t have to worry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in the background.</span></span> <span data-ttu-id="54db3-135">Naproti tomu "Vše" rozhraní API umožňují explicitní kontrolu nad komunikace se službou IoT Hub, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="54db3-135">In contrast, the "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="54db3-136">To lépe pochopit, podíváme se na příklad:</span><span class="sxs-lookup"><span data-stu-id="54db3-136">To understand this better, let’s look at an example:</span></span>

<span data-ttu-id="54db3-137">Při volání **IoTHubClient\_SendEventAsync**, jaké ve skutečnosti úlohy je uvedení události do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="54db3-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting the event in a buffer.</span></span> <span data-ttu-id="54db3-138">Vlákně na pozadí vytvořen při volání **IoTHubClient\_CreateFromConnectionString** neustále monitoruje této vyrovnávací paměti a odešle všechna data, která obsahuje do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-138">The background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains to IoT Hub.</span></span> <span data-ttu-id="54db3-139">K tomu dojde na pozadí ve stejnou dobu, hlavní vlákno provádí jinou práci.</span><span class="sxs-lookup"><span data-stu-id="54db3-139">This happens in the background at the same time that the main thread is performing other work.</span></span>

<span data-ttu-id="54db3-140">Podobně když se zaregistrujete funkce zpětného volání pro zprávy pomocí **IoTHubClient\_SetMessageCallback**, že instruující SDK tak, aby měl vlákně na pozadí vyvolat funkce zpětného volání, když zprávu přijme, nezávisle na hlavní vlákno.</span><span class="sxs-lookup"><span data-stu-id="54db3-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing the SDK to have the background thread invoke the callback function when a message is received, independent of the main thread.</span></span>

<span data-ttu-id="54db3-141">Rozhraní API "vše" nevytvářejte vlákna na pozadí.</span><span class="sxs-lookup"><span data-stu-id="54db3-141">The "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="54db3-142">Místo toho musí být voláno nové rozhraní API explicitně odesílat a přijímat data ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-142">Instead, a new API must be called to explicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="54db3-143">Tento postup je znázorněn v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="54db3-143">This is demonstrated in the following example.</span></span>

<span data-ttu-id="54db3-144">**Iothub\_klienta\_ukázka\_http** aplikace, která je součástí sady SDK ukazuje nižší úrovně rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="54db3-144">The **iothub\_client\_sample\_http** application that’s included in the SDK demonstrates the lower-level APIs.</span></span> <span data-ttu-id="54db3-145">V této ukázce jsme odesílat události do služby IoT Hub s kódem například následující:</span><span class="sxs-lookup"><span data-stu-id="54db3-145">In that sample, we send events to IoT Hub with code such as the following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="54db3-146">První tři řádky vytvořit zprávu a poslední řádek odesílá události.</span><span class="sxs-lookup"><span data-stu-id="54db3-146">The first three lines create the message, and the last line sends the event.</span></span> <span data-ttu-id="54db3-147">Ale jak je uvedeno nahoře, "odesílání" událost znamená, že se data jednoduše umístí do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="54db3-147">However, as mentioned previously, "sending" the event means that the data is simply placed in a buffer.</span></span> <span data-ttu-id="54db3-148">Nic se přenášejí v síti, když říkáme **IoTHubClient\_UDOU\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="54db3-148">Nothing is transmitted on the network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="54db3-149">V pořadí, které se ve skutečnosti příchozí přenos dat do služby IoT Hub, musí volat **IoTHubClient\_UDOU\_DoWork**, protože v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="54db3-149">In order to actually ingress the data to IoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="54db3-150">Tento kód (z **iothub\_klienta\_ukázka\_http** aplikace) opakovaného volá **IoTHubClient\_UDOU\_DoWork**.</span><span class="sxs-lookup"><span data-stu-id="54db3-150">This code (from the **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="54db3-151">Pokaždé, když **IoTHubClient\_UDOU\_DoWork** je volána, odešle některé události z vyrovnávací paměti do služby IoT Hub a načte zprávu ve frontě se odešle do zařízení.</span><span class="sxs-lookup"><span data-stu-id="54db3-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from the buffer to IoT Hub and it retrieves a queued message being sent to the device.</span></span> <span data-ttu-id="54db3-152">Druhém případě znamená, že pokud jsme registrovaná funkce zpětného volání pro zprávy, pak zpětné volání je vyvolán (za předpokladu, že všechny zprávy jsou zařazeny do fronty).</span><span class="sxs-lookup"><span data-stu-id="54db3-152">The latter case means that if we registered a callback function for messages, then the callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="54db3-153">By zaregistrovali funkci zpětného volání s kódem například následující:</span><span class="sxs-lookup"><span data-stu-id="54db3-153">We would have registered such a callback function with code such as the following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="54db3-154">Z důvodu, **IoTHubClient\_UDOU\_DoWork** se často říká smyčku je, že pokaždé, když je volána, odešle *některé* uložená do vyrovnávací paměti události IoT Hub a načte *Další* zprávy do fronty pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="54db3-154">The reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events to IoT Hub and retrieves *the next* message queued up for the device.</span></span> <span data-ttu-id="54db3-155">Každé volání není zaručena odesílat všechny uložené ve vyrovnávací paměti události nebo k načtení všech zpráv zařazených do fronty.</span><span class="sxs-lookup"><span data-stu-id="54db3-155">Each call isn’t guaranteed to send all buffered events or to retrieve all queued messages.</span></span> <span data-ttu-id="54db3-156">Pokud chcete odeslat všechny události ve vyrovnávací paměti a pokračujte na další zpracování můžete tento smyčky nahradit kód například následující:</span><span class="sxs-lookup"><span data-stu-id="54db3-156">If you want to send all events in the buffer and then continue on with other processing you can replace this loop with code such as the following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="54db3-157">Tento kód zavolá **IoTHubClient\_UDOU\_DoWork** dokud všechny události ve vyrovnávací paměti byly odeslány do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in the buffer have been sent to IoT Hub.</span></span> <span data-ttu-id="54db3-158">Všimněte si, že to také neznamená, že všechny zprávy ve frontě byly přijaty.</span><span class="sxs-lookup"><span data-stu-id="54db3-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="54db3-159">Součást důvodem je, že kontrola "vše" zpráv není deterministický jako akce.</span><span class="sxs-lookup"><span data-stu-id="54db3-159">Part of the reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="54db3-160">Co se stane, když je načíst "všechny" zprávy, ale jinou se pak odešlou do zařízení hned po?</span><span class="sxs-lookup"><span data-stu-id="54db3-160">What happens if you retrieve "all" of the messages, but then another one is sent to the device immediately after?</span></span> <span data-ttu-id="54db3-161">Lepší způsob, jak řešit, který je s naprogramovaných vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="54db3-161">A better way to deal with that is with a programmed timeout.</span></span> <span data-ttu-id="54db3-162">Funkce zpětného volání zpráva může například obnovit časovač pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="54db3-162">For example, the message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="54db3-163">Poté můžete napsat logiku pokračovat zpracování, pokud například nebyly přijaty žádné zprávy za posledních *X* sekund.</span><span class="sxs-lookup"><span data-stu-id="54db3-163">You can then write logic to continue processing if, for example, no messages have been received in the last *X* seconds.</span></span>

<span data-ttu-id="54db3-164">Pokud jste dokončení ingressing události a přijímání zpráv, nezapomeňte volat funkci odpovídající vyčištění prostředků.</span><span class="sxs-lookup"><span data-stu-id="54db3-164">When you’re finished ingressing events and receiving messages, be sure to call the corresponding function to clean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="54db3-165">Je v podstatě jen jednu sadu rozhraní API pro odesílat a přijímat data z vlákna na pozadí a další sadu rozhraní API, která má stejnou funkci bez vlákně na pozadí.</span><span class="sxs-lookup"><span data-stu-id="54db3-165">Basically there’s only one set of APIs to send and receive data with a background thread and another set of APIs that does the same thing without the background thread.</span></span> <span data-ttu-id="54db3-166">Mnoho vývojáři můžou upřednostňovat bez API UDOU-, ale nižší úrovně rozhraní API je užitečný v případě vývojář chce explicitní kontrolu nad síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="54db3-166">A lot of developers may prefer the non-LL APIs, but the lower-level APIs are useful when the developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="54db3-167">Například některá zařízení shromažďování dat přes čas a události příchozího pouze v určitých intervalech (například jednou za hodinu nebo jednou za den).</span><span class="sxs-lookup"><span data-stu-id="54db3-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="54db3-168">Nižší úrovně rozhraní API nabízejí možnost explicitně řídit při odesílání a příjem dat ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-168">The lower-level APIs give you the ability to explicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="54db3-169">Ostatní jednoduše bude upřednostňovat jednoduchost, které poskytují rozhraní API nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="54db3-169">Others will simply prefer the simplicity that the lower-level APIs provide.</span></span> <span data-ttu-id="54db3-170">Všechno, co se stane na hlavního vlákna než některé situaci práce na pozadí.</span><span class="sxs-lookup"><span data-stu-id="54db3-170">Everything happens on the main thread rather than some work happening in the background.</span></span>

<span data-ttu-id="54db3-171">Podle toho, která modelu si zvolíte, ujistěte se konzistentní které rozhraní API používáte.</span><span class="sxs-lookup"><span data-stu-id="54db3-171">Whichever model you choose, be sure to be consistent in which APIs you use.</span></span> <span data-ttu-id="54db3-172">Pokud spustíte voláním **IoTHubClient\_UDOU\_CreateFromConnectionString**, ujistěte se, jenom pomocí odpovídající nižší úrovně rozhraní API pro všechny následné pracovní:</span><span class="sxs-lookup"><span data-stu-id="54db3-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use the corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="54db3-173">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="54db3-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="54db3-174">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="54db3-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="54db3-175">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="54db3-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="54db3-176">IoTHubClient\_UDOU\_DoWork</span><span class="sxs-lookup"><span data-stu-id="54db3-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="54db3-177">Naopak je také hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="54db3-177">The opposite is true as well.</span></span> <span data-ttu-id="54db3-178">Pokud začnete se **IoTHubClient\_CreateFromConnectionString**, pak použít jiný API UDOU - pro žádné další zpracování.</span><span class="sxs-lookup"><span data-stu-id="54db3-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use the non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="54db3-179">V zařízení Azure IoT SDK C, najdete v článku **iothub\_klienta\_ukázka\_http** aplikace kompletní příklad rozhraní API nižší úrovně.</span><span class="sxs-lookup"><span data-stu-id="54db3-179">In the Azure IoT device SDK for C, see the **iothub\_client\_sample\_http** application for a complete example of the lower-level APIs.</span></span> <span data-ttu-id="54db3-180">**Iothub\_klienta\_ukázka\_amqp** aplikace může být odkazováno kompletní příklad z jiných - UDOU rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="54db3-180">The **iothub\_client\_sample\_amqp** application can be referenced for a full example of the non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="54db3-181">Vlastnost zpracování</span><span class="sxs-lookup"><span data-stu-id="54db3-181">Property handling</span></span>
<span data-ttu-id="54db3-182">Pokud při Popsali jsme odesílání dat, jsme jste byla odkazující na tělo zprávy.</span><span class="sxs-lookup"><span data-stu-id="54db3-182">So far when we've described sending data, we've been referring to the body of the message.</span></span> <span data-ttu-id="54db3-183">Představte si třeba tento kód:</span><span class="sxs-lookup"><span data-stu-id="54db3-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="54db3-184">Tento příklad odešle zprávu do služby IoT Hub s textem "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="54db3-184">This example sends a message to IoT Hub with the text "Hello World."</span></span> <span data-ttu-id="54db3-185">IoT Hub však umožňuje také vlastnosti, které chcete připojit ke každé zprávě.</span><span class="sxs-lookup"><span data-stu-id="54db3-185">However, IoT Hub also allows properties to be attached to each message.</span></span> <span data-ttu-id="54db3-186">Vlastnosti jsou páry název/hodnota, které je možné připojit ke zprávě.</span><span class="sxs-lookup"><span data-stu-id="54db3-186">Properties are name/value pairs that can be attached to the message.</span></span> <span data-ttu-id="54db3-187">Například můžeme můžete upravit předchozí kód k připojení ke zprávě vlastnost:</span><span class="sxs-lookup"><span data-stu-id="54db3-187">For example, we can modify the previous code to attach a property to the message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="54db3-188">Začneme voláním **IoTHubMessage\_vlastnosti** a předání popisovač našem zpráv.</span><span class="sxs-lookup"><span data-stu-id="54db3-188">We start by calling **IoTHubMessage\_Properties** and passing it the handle of our message.</span></span> <span data-ttu-id="54db3-189">Je nám získat zpět **MAPY\_zpracování** odkaz, který umožňuje nám chcete začít přidávat vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54db3-189">What we get back is a **MAP\_HANDLE** reference that enables us to start adding properties.</span></span> <span data-ttu-id="54db3-190">Lze provést voláním **mapy\_AddOrUpdate**, což trvá odkaz na MAPU\_POPISOVAČ, název vlastnosti a hodnotu vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54db3-190">The latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference to a MAP\_HANDLE, the property name, and the property value.</span></span> <span data-ttu-id="54db3-191">S tímto rozhraním API přidáme jako mnoho vlastností, jako jsme jako.</span><span class="sxs-lookup"><span data-stu-id="54db3-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="54db3-192">Pokud je událost pro čtení z **Event Hubs**, příjemce, můžete vytvořit výčet vlastností a načíst jejich příslušné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="54db3-192">When the event is read from **Event Hubs**, the receiver can enumerate the properties and retrieve their corresponding values.</span></span> <span data-ttu-id="54db3-193">Například v rozhraní .NET to by se udělat přístup [vlastnosti kolekce u objektu EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="54db3-193">For example, in .NET this would be accomplished by accessing the [Properties collection on the EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="54db3-194">V předchozím příkladu jsme se připojuje k událost, která nám odeslat do služby IoT Hub vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54db3-194">In the previous example, we’re attaching properties to an event that we send to IoT Hub.</span></span> <span data-ttu-id="54db3-195">Vlastnosti lze také připojit k zprávy přijaté ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-195">Properties can also be attached to messages received from IoT Hub.</span></span> <span data-ttu-id="54db3-196">Pokud nám chcete načíst vlastnosti ze zprávy, můžeme použít například následující kód v našem zpráv funkce zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="54db3-196">If we want to retrieve properties from a message, we can use code such as the following in our message callback function:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

<span data-ttu-id="54db3-197">Volání **IoTHubMessage\_vlastnosti** vrátí **MAPY\_zpracování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="54db3-197">The call to **IoTHubMessage\_Properties** returns the **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="54db3-198">Jsme pak předejte tento odkaz na **mapy\_GetInternals** získat odkaz na pole dvojice název hodnota (stejně jako počet vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="54db3-198">We then pass that reference to **Map\_GetInternals** to obtain a reference to an array of the name/value pairs (as well as a count of the properties).</span></span> <span data-ttu-id="54db3-199">V tomto okamžiku je jednoduché výčtu vlastností, abyste se dostali na hodnoty, které má být.</span><span class="sxs-lookup"><span data-stu-id="54db3-199">At that point it's a simple matter of enumerating the properties to get to the values we want.</span></span>

<span data-ttu-id="54db3-200">Nemusíte používat vlastnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="54db3-200">You don't have to use properties in your application.</span></span> <span data-ttu-id="54db3-201">Ale pokud budete muset jejich nastavení u události nebo je načtou ze zprávy, **IoTHubClient** knihovny lze snadno.</span><span class="sxs-lookup"><span data-stu-id="54db3-201">However, if you need to set them on events or retrieve them from messages, the **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="54db3-202">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="54db3-202">Message handling</span></span>
<span data-ttu-id="54db3-203">Jak jsme uvedli dříve, při doručování zpráv ze služby IoT Hub **IoTHubClient** knihovny reaguje vyvoláním registrovaná funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="54db3-203">As stated previously, when messages arrive from IoT Hub the **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="54db3-204">Návratový parametr této funkce, které si zaslouží další vysvětlení není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="54db3-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="54db3-205">Tady je výňatek funkce zpětného volání v **iothub\_klienta\_ukázka\_http** ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="54db3-205">Here’s an excerpt of the callback function in the **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="54db3-206">Všimněte si, že je návratový typ **IOTHUBMESSAGE\_DISPOZICE\_výsledek** a v tomto případě vrátíme **IOTHUBMESSAGE\_platných**.</span><span class="sxs-lookup"><span data-stu-id="54db3-206">Note that the return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="54db3-207">Existují jiné hodnoty vrátíme z této funkce, které změnit jak **IoTHubClient** knihovny reaguje na zpětné volání zprávy.</span><span class="sxs-lookup"><span data-stu-id="54db3-207">There are other values we can return from this function that change how the **IoTHubClient** library reacts to the message callback.</span></span> <span data-ttu-id="54db3-208">Tady jsou uvedené možnosti.</span><span class="sxs-lookup"><span data-stu-id="54db3-208">Here are the options.</span></span>

* <span data-ttu-id="54db3-209">**IOTHUBMESSAGE\_platných** – zpráva byla úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="54db3-209">**IOTHUBMESSAGE\_ACCEPTED** – The message has been processed successfully.</span></span> <span data-ttu-id="54db3-210">**IoTHubClient** knihovny nebude vyvolání funkce zpětného volání znovu s stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="54db3-210">The **IoTHubClient** library will not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="54db3-211">**IOTHUBMESSAGE\_ZAMÍTNUTÝ** – zpráva nebyla zpracována, a neexistuje žádný pádem si chtít v budoucnu učinit.</span><span class="sxs-lookup"><span data-stu-id="54db3-211">**IOTHUBMESSAGE\_REJECTED** – The message was not processed and there is no desire to do so in the future.</span></span> <span data-ttu-id="54db3-212">**IoTHubClient** knihovny nesmí vyvolání funkce zpětného volání znovu s stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="54db3-212">The **IoTHubClient** library should not invoke the callback function again with the same message.</span></span>
* <span data-ttu-id="54db3-213">**IOTHUBMESSAGE\_ABANDONED** – zpráva nebyla zpracována úspěšně, ale **IoTHubClient** knihovny by měla vyvolat funkce zpětného volání znovu s stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="54db3-213">**IOTHUBMESSAGE\_ABANDONED** – The message was not processed successfully, but the **IoTHubClient** library should invoke the callback function again with the same message.</span></span>

<span data-ttu-id="54db3-214">Pro první dvě návratové kódy, **IoTHubClient** knihovny odešle zprávu do služby IoT Hub indikující, že zpráva by měla odstranění z fronty zařízení a nejsou doručeny znovu.</span><span class="sxs-lookup"><span data-stu-id="54db3-214">For the first two return codes, the **IoTHubClient** library sends a message to IoT Hub indicating that the message should be deleted from the device queue and not delivered again.</span></span> <span data-ttu-id="54db3-215">Projeví je stejný (odstraní zprávu z fronty zařízení), ale je stále zaznamenat zda byla zpráva přijaty nebo odmítnuty.</span><span class="sxs-lookup"><span data-stu-id="54db3-215">The net effect is the same (the message is deleted from the device queue), but whether the message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="54db3-216">Záznam tento rozdíl je užitečné odesílatelé zprávy, kteří může sledovat zpětnou vazbu a zjistit, pokud má zařízení přijímat nebo konkrétní zprávu odmítl.</span><span class="sxs-lookup"><span data-stu-id="54db3-216">Recording this distinction is useful to senders of the message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="54db3-217">V případě, že poslední zprávu se odesílá i do služby IoT Hub, ale znamená by měl vysláním zprávy.</span><span class="sxs-lookup"><span data-stu-id="54db3-217">In the last case a message is also sent to IoT Hub, but it indicates that the message should be redelivered.</span></span> <span data-ttu-id="54db3-218">Pokud dojde k nějaké chybě, ale chcete si vyzkoušet znovu zpracovat zprávu, obvykle budete abandon zprávu.</span><span class="sxs-lookup"><span data-stu-id="54db3-218">Typically you’ll abandon a message if you encounter some error but want to try to process the message again.</span></span> <span data-ttu-id="54db3-219">Spočívá v tom, odmítat zprávu odpovídající když dojde k neodstranitelné chybě (nebo jednoduše se rozhodnete, že nechcete, aby ke zpracování zprávy).</span><span class="sxs-lookup"><span data-stu-id="54db3-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want to process the message).</span></span>

<span data-ttu-id="54db3-220">V každém případě mějte na paměti různých návratových kódů tak, aby dokáže vyvolat chování, které chcete z **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-220">In any case, be aware of the different return codes so that you can elicit the behavior you want from the **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="54db3-221">Přihlašovací údaje jiného zařízení</span><span class="sxs-lookup"><span data-stu-id="54db3-221">Alternate device credentials</span></span>
<span data-ttu-id="54db3-222">Jak je popsáno dříve, prvním krokem při práci s **IoTHubClient** knihovna je použít k získání **IOTHUB\_klienta\_zpracování** pomocí volání například následující:</span><span class="sxs-lookup"><span data-stu-id="54db3-222">As explained previously, the first thing to do when working with the **IoTHubClient** library is to obtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as the following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="54db3-223">Argumenty, které mají **IoTHubClient\_CreateFromConnectionString** jsou zařízení připojovací řetězec a parametr, který určuje protokol, využijeme ke komunikaci se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-223">The arguments to **IoTHubClient\_CreateFromConnectionString** are the device connection string and a parameter that indicates the protocol we use to communicate with IoT Hub.</span></span> <span data-ttu-id="54db3-224">Připojovací řetězec zařízení má formát, který se zobrazí takto:</span><span class="sxs-lookup"><span data-stu-id="54db3-224">The device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="54db3-225">Existují čtyři údaje v tento řetězec: název služby IoT Hub, příponu IoT Hub, ID zařízení a sdílený přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="54db3-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="54db3-226">Při vytváření IoT hub instance v portálu Azure získáte ze služby IoT hub plně kvalifikovaný název domény (FQDN) – to vám dává název centra IoT (první část plně kvalifikovaný název domény) a příponou centra IoT (zbytek plně kvalifikovaný název domény).</span><span class="sxs-lookup"><span data-stu-id="54db3-226">You obtain the fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in the Azure portal — this gives you the IoT hub name (the first part of the FQDN) and the IoT hub suffix (the rest of the FQDN).</span></span> <span data-ttu-id="54db3-227">Získat ID zařízení a sdílený přístupový klíč při registraci zařízení s centrem IoT (jak je popsáno v [předchozí článek](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="54db3-227">You get the device ID and the shared access key when you register your device with IoT Hub (as described in the [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="54db3-228">**IoTHubClient\_CreateFromConnectionString** nabízí jeden způsob, jak provést inicializaci knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-228">**IoTHubClient\_CreateFromConnectionString** gives you one way to initialize the library.</span></span> <span data-ttu-id="54db3-229">Pokud dáváte přednost, můžete vytvořit nový **IOTHUB\_klienta\_zpracování** pomocí těchto jednotlivých parametrů místo připojovací řetězec zařízení.</span><span class="sxs-lookup"><span data-stu-id="54db3-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than the device connection string.</span></span> <span data-ttu-id="54db3-230">Můžete toho dosáhnout s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="54db3-230">This is achieved with the following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="54db3-231">To provede stejnou akci jako **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="54db3-231">This accomplishes the same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="54db3-232">To nemusí připadat zřejmé, že chcete použít **IoTHubClient\_CreateFromConnectionString** místo Tato podrobnější metoda inicializace.</span><span class="sxs-lookup"><span data-stu-id="54db3-232">It may seem obvious that you would want to use **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="54db3-233">Mějte na paměti, ale, že při registraci zařízení do služby IoT Hub můžete získat je ID zařízení a klíč zařízení (ne připojovací řetězec).</span><span class="sxs-lookup"><span data-stu-id="54db3-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="54db3-234">*Explorer zařízení* počínaje nástroj sady SDK [předchozí článek](iot-hub-device-sdk-c-intro.md) používá knihovny v **sady SDK služby Azure IoT** vytvoření připojovacího řetězce zařízení z ID zařízení , klíč zařízení a název hostitele služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="54db3-234">The *device explorer* SDK tool introduced in the [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in the **Azure IoT service SDK** to create the device connection string from the device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="54db3-235">Proto volání **IoTHubClient\_UDOU\_vytvořit** může být vhodnější, protože uloží krok generování připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="54db3-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you the step of generating a connection string.</span></span> <span data-ttu-id="54db3-236">Použijte kteroukoli metodou je vhodné.</span><span class="sxs-lookup"><span data-stu-id="54db3-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="54db3-237">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="54db3-237">Configuration options</span></span>
<span data-ttu-id="54db3-238">Pokud vše popsané o způsobu, jakým **IoTHubClient** knihovny funguje odráží jeho výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="54db3-238">So far everything described about the way the **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="54db3-239">Existují však několik možností, které můžete nastavit, aby změny fungování knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-239">However, there are a few options that you can set to change how the library works.</span></span> <span data-ttu-id="54db3-240">Toho dosahuje využitím **IoTHubClient\_UDOU\_SetOption** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="54db3-240">This is accomplished by leveraging the **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="54db3-241">Vezměte v úvahu v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="54db3-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="54db3-242">Existuje několik možností, které se běžně používají:</span><span class="sxs-lookup"><span data-stu-id="54db3-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="54db3-243">**SetBatching** (logická hodnota) – Pokud **true**, data zasílaná do služby IoT Hub je odeslaný v dávkách.</span><span class="sxs-lookup"><span data-stu-id="54db3-243">**SetBatching** (bool) – If **true**, then data sent to IoT Hub is sent in batches.</span></span> <span data-ttu-id="54db3-244">Pokud **false**, pak jsou zprávy odesílány jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="54db3-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="54db3-245">Výchozí hodnota je **false**.</span><span class="sxs-lookup"><span data-stu-id="54db3-245">The default is **false**.</span></span> <span data-ttu-id="54db3-246">Všimněte si, že **SetBatching** možnost se vztahuje pouze na protokol HTTP, ne na protokoly MQTT nebo AMQP.</span><span class="sxs-lookup"><span data-stu-id="54db3-246">Note that the **SetBatching** option only applies to the HTTP protocol and not to the MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="54db3-247">**Časový limit** (bez znaménka int) – Tato hodnota je reprezentována v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="54db3-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="54db3-248">Pokud odesílání požadavku protokolu HTTP nebo přijímání odpověď trvá déle, než tuto chvíli a pak časový limit připojení.</span><span class="sxs-lookup"><span data-stu-id="54db3-248">If sending an HTTP request or receiving a response takes longer than this time, then the connection times out.</span></span>

<span data-ttu-id="54db3-249">Dávkování možnost je důležité.</span><span class="sxs-lookup"><span data-stu-id="54db3-249">The batching option is important.</span></span> <span data-ttu-id="54db3-250">Ve výchozím nastavení jsou události ingresses knihovny jednotlivě (jedna událost je to bez ohledu na předáte **IoTHubClient\_UDOU\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="54db3-250">By default, the library ingresses events individually (a single event is whatever you pass to **IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="54db3-251">Pokud je možnost dávkování **true**, knihovny shromažďuje tolik události, jak je možné z vyrovnávací paměti (až maximální velikost zprávy, bude přijímat IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="54db3-251">If the batching option is **true**, the library collects as many events as it can from the buffer (up to the maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="54db3-252">Batch událostí se odešlou do služby IoT Hub v jednom volání protokolu HTTP (jednotlivé události jsou seskupeny do pole JSON).</span><span class="sxs-lookup"><span data-stu-id="54db3-252">The event batch is sent to IoT Hub in a single HTTP call (the individual events are bundled into a JSON array).</span></span> <span data-ttu-id="54db3-253">Povolení dávkování obvykle za následek zvýšení výkonu velkých vzhledem k tomu, že se snižuje odezvy sítě.</span><span class="sxs-lookup"><span data-stu-id="54db3-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="54db3-254">Také významně snižuje šířky pásma, protože při odesílání jednu sadu hlaviček protokolu HTTP s batch událostí a nikoli sadu hlavičky pro každé jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="54db3-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="54db3-255">Pokud nemáte konkrétní důvod, proč neurčí jinak, obvykle budete chtít povolit dávkování.</span><span class="sxs-lookup"><span data-stu-id="54db3-255">Unless you have a specific reason to do otherwise, typically you’ll want to enable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54db3-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54db3-256">Next steps</span></span>
<span data-ttu-id="54db3-257">Tento článek podrobně popisuje chování **IoTHubClient** knihovna nalezena v **zařízení Azure IoT SDK pro jazyk C**. Pomocí těchto informací byste měli mít dostatečné povědomí o možnostech **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-257">This article describes in detail the behavior of the **IoTHubClient** library found in the **Azure IoT device SDK for C**. With this information, you should have a good understanding of the capabilities of the **IoTHubClient** library.</span></span> <span data-ttu-id="54db3-258">[Následující článek](iot-hub-device-sdk-c-serializer.md) poskytuje podobné podrobností na **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="54db3-258">The [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on the **serializer** library.</span></span>

<span data-ttu-id="54db3-259">Další informace o vývoji pro IoT Hub, najdete v tématu [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="54db3-259">To learn more about developing for IoT Hub, see the [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="54db3-260">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="54db3-260">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="54db3-261">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="54db3-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
