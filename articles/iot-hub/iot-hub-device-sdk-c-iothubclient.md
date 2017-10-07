---
title: "zařízení IoT aaaAzure SDK pro jazyk C - IoTHubClient | Microsoft Docs"
description: "Jak toouse hello IoTHubClient knihovny zařízení Azure IoT hello SDK pro aplikace C toocreate zařízení, které komunikují pomocí služby IoT hub."
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
ms.openlocfilehash: d1ece79e9ba6d1e5fd45cabb8fca393b24052e07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a><span data-ttu-id="2471b-103">Pro zařízení Azure IoT SDK pro jazyk C – informace o IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="2471b-103">Azure IoT device SDK for C – more about IoTHubClient</span></span>
<span data-ttu-id="2471b-104">Hello [nejprve článek](iot-hub-device-sdk-c-intro.md) v této řady zavedená hello **zařízení Azure IoT SDK pro jazyk C**. Tento článek vysvětluje, SDK jsou dvě vrstvy architektury.</span><span class="sxs-lookup"><span data-stu-id="2471b-104">hello [first article](iot-hub-device-sdk-c-intro.md) in this series introduced hello **Azure IoT device SDK for C**. That article explained that there are two architectural layers in SDK.</span></span> <span data-ttu-id="2471b-105">Na základní hello je hello **IoTHubClient** knihovnu, která přímo spravuje komunikace se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-105">At hello base is hello **IoTHubClient** library which directly manages communication with IoT Hub.</span></span> <span data-ttu-id="2471b-106">Je také hello **serializátor** knihovny, který sestaví v horní části daného tooprovide služby serializace.</span><span class="sxs-lookup"><span data-stu-id="2471b-106">There's also hello **serializer** library that builds on top of that tooprovide serialization services.</span></span> <span data-ttu-id="2471b-107">V tomto článku jsme vám poskytují další podrobnosti hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2471b-107">In this article we'll provide additional detail on hello **IoTHubClient** library.</span></span>

<span data-ttu-id="2471b-108">Hello předchozí článek popisuje, jak toouse hello **IoTHubClient** knihovny toosend události tooIoT rozbočovače a přijímat zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-108">hello previous article described how toouse hello **IoTHubClient** library toosend events tooIoT Hub and receive messages.</span></span> <span data-ttu-id="2471b-109">Tento článek rozšiřuje této diskuzi o vysvětlením, jak přesně toomore spravovat *při* odesílat a přijímat data, Představujeme vám toohello **nižší úrovně rozhraní API**.</span><span class="sxs-lookup"><span data-stu-id="2471b-109">This article extends that discussion by explaining how toomore precisely manage *when* you send and receive data, introducing you toohello **lower-level APIs**.</span></span> <span data-ttu-id="2471b-110">Dále vysvětlíme jak tooattach vlastnosti tooevents (a je načtou ze zprávy) pomocí vlastnosti hello zpracování funkcí v hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2471b-110">We'll also explain how tooattach properties tooevents (and retrieve them from messages) using hello property handling features in hello **IoTHubClient** library.</span></span> <span data-ttu-id="2471b-111">Nakonec poskytujeme další vysvětlení různé způsoby toohandle zprávy přijaté ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-111">Finally, we'll provide additional explanation of different ways toohandle messages received from IoT Hub.</span></span>

<span data-ttu-id="2471b-112">Hello článku se ukončí pokrývajících několik ostatní témata, včetně více informací o zařízení a jak toochange hello chování hello **IoTHubClient** prostřednictvím možnosti konfigurace.</span><span class="sxs-lookup"><span data-stu-id="2471b-112">hello article concludes by covering a couple of miscellaneous topics, including more about device credentials and how toochange hello behavior of hello **IoTHubClient** through configuration options.</span></span>

<span data-ttu-id="2471b-113">Použijeme hello **IoTHubClient** SDK ukázky tooexplain tato témata.</span><span class="sxs-lookup"><span data-stu-id="2471b-113">We'll use hello **IoTHubClient** SDK samples tooexplain these topics.</span></span> <span data-ttu-id="2471b-114">Pokud chcete toofollow společně, najdete v části hello **iothub\_klienta\_ukázka\_http** a **iothub\_klienta\_ukázka\_amqp** aplikace, které jsou součástí zařízení Azure IoT hello SDK pro C. všechno popsané v následující části hello ukazují tyto ukázky.</span><span class="sxs-lookup"><span data-stu-id="2471b-114">If you want toofollow along, see hello **iothub\_client\_sample\_http** and **iothub\_client\_sample\_amqp** applications that are included in hello Azure IoT device SDK for C. Everything described in hello following sections is demonstrated in these samples.</span></span>

<span data-ttu-id="2471b-115">Můžete najít hello [ **zařízení Azure IoT SDK pro jazyk C** ](https://github.com/Azure/azure-iot-sdk-c) Githubu úložiště a zobrazte podrobnosti o hello rozhraní API v hello [referenční dokumentace rozhraní API jazyka C](https://azure.github.io/azure-iot-sdk-c/index.html).</span><span class="sxs-lookup"><span data-stu-id="2471b-115">You can find hello [**Azure IoT device SDK for C**](https://github.com/Azure/azure-iot-sdk-c) GitHub repository and view details of hello API in hello [C API reference](https://azure.github.io/azure-iot-sdk-c/index.html).</span></span>

## <a name="hello-lower-level-apis"></a><span data-ttu-id="2471b-116">Hello nižší úrovně rozhraní API</span><span class="sxs-lookup"><span data-stu-id="2471b-116">hello lower-level APIs</span></span>
<span data-ttu-id="2471b-117">Hello předchozí článek popisuje základní operace hello hello **IotHubClient** v rámci kontextu hello hello **iothub\_klienta\_ukázka\_amqp** aplikace.</span><span class="sxs-lookup"><span data-stu-id="2471b-117">hello previous article described hello basic operation of hello **IotHubClient** within hello context of hello **iothub\_client\_sample\_amqp** application.</span></span> <span data-ttu-id="2471b-118">Například vysvětlení, jak tooinitialize hello knihovny pomocí tohoto kódu.</span><span class="sxs-lookup"><span data-stu-id="2471b-118">For example, it explained how tooinitialize hello library using this code.</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="2471b-119">Také popisuje, jak pomocí této události toosend volání funkce.</span><span class="sxs-lookup"><span data-stu-id="2471b-119">It also described how toosend events using this function call.</span></span>

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

<span data-ttu-id="2471b-120">Hello článku také popisuje, jak tooreceive zpráv tak, že zaregistrujete funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="2471b-120">hello article also described how tooreceive messages by registering a callback function.</span></span>

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

<span data-ttu-id="2471b-121">Hello článku také ukázal, jak toofree prostředků pomocí kódu, například následující hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-121">hello article also showed how toofree resources using code such as hello following.</span></span>

```
IoTHubClient_Destroy(iotHubClientHandle);
```

<span data-ttu-id="2471b-122">Jsou ale doprovodné funkce tooeach těchto rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="2471b-122">However there are companion functions tooeach of these APIs:</span></span>

* <span data-ttu-id="2471b-123">IoTHubClient\_UDOU\_CreateFromConnectionString</span><span class="sxs-lookup"><span data-stu-id="2471b-123">IoTHubClient\_LL\_CreateFromConnectionString</span></span>
* <span data-ttu-id="2471b-124">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="2471b-124">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="2471b-125">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="2471b-125">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="2471b-126">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="2471b-126">IoTHubClient\_LL\_Destroy</span></span>

<span data-ttu-id="2471b-127">Všechny tyto funkce zahrnout "Vše" v názvu hello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-127">These functions all include “LL” in hello API name.</span></span> <span data-ttu-id="2471b-128">Než tu, která hello parametry každou z těchto funkcí se svými protějšky bez UDOU identické tootheir.</span><span class="sxs-lookup"><span data-stu-id="2471b-128">Other than that, hello parameters of each of these functions are identical tootheir non-LL counterparts.</span></span> <span data-ttu-id="2471b-129">Hello chování těchto funkcí je však jinou důležitá jedním způsobem.</span><span class="sxs-lookup"><span data-stu-id="2471b-129">However, hello behavior of these functions is different in one important way.</span></span>

<span data-ttu-id="2471b-130">Při volání **IoTHubClient\_CreateFromConnectionString**, základní knihovny hello vytvořit nové vlákno, který je spuštěn v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-130">When you call **IoTHubClient\_CreateFromConnectionString**, hello underlying libraries create a new thread that runs in hello background.</span></span> <span data-ttu-id="2471b-131">Tento přístup z více vláken odesílá události do a ze služby IoT Hub přijímá zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-131">This thread sends events to, and receives messages from, IoT Hub.</span></span> <span data-ttu-id="2471b-132">Žádný takový vlákno se vytvoří při práci s hello "Vše" rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-132">No such thread is created when working with hello "LL" APIs.</span></span> <span data-ttu-id="2471b-133">Vytvoření Hello vlákna na pozadí hello je vývojář toohello pohodlí.</span><span class="sxs-lookup"><span data-stu-id="2471b-133">hello creation of hello background thread is a convenience toohello developer.</span></span> <span data-ttu-id="2471b-134">Nemáte tooworry o explicitně události odesílání a přijímání zpráv ze služby IoT Hub – dojde automaticky hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="2471b-134">You don’t have tooworry about explicitly sending events and receiving messages from IoT Hub -- it happens automatically in hello background.</span></span> <span data-ttu-id="2471b-135">Naproti tomu hello "Vše" rozhraní API umožňují explicitní kontrolu nad komunikace se službou IoT Hub, pokud je to nutné.</span><span class="sxs-lookup"><span data-stu-id="2471b-135">In contrast, hello "LL" APIs give you explicit control over communication with IoT Hub, if you need it.</span></span>

<span data-ttu-id="2471b-136">toounderstand tento lepší Podíváme se na příklad:</span><span class="sxs-lookup"><span data-stu-id="2471b-136">toounderstand this better, let’s look at an example:</span></span>

<span data-ttu-id="2471b-137">Při volání **IoTHubClient\_SendEventAsync**, jaké ve skutečnosti úlohy je uvedení hello událostí do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="2471b-137">When you call **IoTHubClient\_SendEventAsync**, what you're actually doing is putting hello event in a buffer.</span></span> <span data-ttu-id="2471b-138">Hello vlákna na pozadí vytvořen při volání **IoTHubClient\_CreateFromConnectionString** neustále monitoruje této vyrovnávací paměti a pošle nějaká data datovým obsahuje tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2471b-138">hello background thread created when you call **IoTHubClient\_CreateFromConnectionString** continually monitors this buffer and sends any data that it contains tooIoT Hub.</span></span> <span data-ttu-id="2471b-139">To se stane, hello pozadí na hello stejný čas této hello hlavního vlákna právě provádí jinou práci.</span><span class="sxs-lookup"><span data-stu-id="2471b-139">This happens in hello background at hello same time that hello main thread is performing other work.</span></span>

<span data-ttu-id="2471b-140">Podobně když se zaregistrujete funkce zpětného volání pro zprávy pomocí **IoTHubClient\_SetMessageCallback**, že instruující hello SDK toohave hello pozadí vlákno vyvolat hello funkce zpětného volání, když zprávu přijme, nezávisle na hello hlavního vlákna.</span><span class="sxs-lookup"><span data-stu-id="2471b-140">Similarly, when you register a callback function for messages using **IoTHubClient\_SetMessageCallback**, you're instructing hello SDK toohave hello background thread invoke hello callback function when a message is received, independent of hello main thread.</span></span>

<span data-ttu-id="2471b-141">Hello "Vše" rozhraní API nevytvářejte vlákna na pozadí.</span><span class="sxs-lookup"><span data-stu-id="2471b-141">hello "LL" APIs don’t create a background thread.</span></span> <span data-ttu-id="2471b-142">Místo toho nové rozhraní API musí být voláno tooexplicitly odesílat a přijímat data ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-142">Instead, a new API must be called tooexplicitly send and receive data from IoT Hub.</span></span> <span data-ttu-id="2471b-143">Ukazují to následující ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-143">This is demonstrated in hello following example.</span></span>

<span data-ttu-id="2471b-144">Hello **iothub\_klienta\_ukázka\_http** aplikace, která je součástí hello SDK ukazuje hello nižší úrovně rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-144">hello **iothub\_client\_sample\_http** application that’s included in hello SDK demonstrates hello lower-level APIs.</span></span> <span data-ttu-id="2471b-145">V této ukázce jsme odesílat události tooIoT rozbočovače s kódem například hello následující:</span><span class="sxs-lookup"><span data-stu-id="2471b-145">In that sample, we send events tooIoT Hub with code such as hello following:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="2471b-146">Hello první tři řádky vytvořte uvítací zprávu, a odešle poslední řádek hello hello událostí.</span><span class="sxs-lookup"><span data-stu-id="2471b-146">hello first three lines create hello message, and hello last line sends hello event.</span></span> <span data-ttu-id="2471b-147">Ale jak je uvedeno nahoře, "odesílání" hello událost znamená, že hello data jednoduše umístí do vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="2471b-147">However, as mentioned previously, "sending" hello event means that hello data is simply placed in a buffer.</span></span> <span data-ttu-id="2471b-148">Nic se přenášejí v síti hello při říkáme **IoTHubClient\_UDOU\_SendEventAsync**.</span><span class="sxs-lookup"><span data-stu-id="2471b-148">Nothing is transmitted on hello network when we call **IoTHubClient\_LL\_SendEventAsync**.</span></span> <span data-ttu-id="2471b-149">V pořadí tooactually příjem příchozích dat hello data tooIoT rozbočovače, je třeba volat **IoTHubClient\_UDOU\_DoWork**, protože v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="2471b-149">In order tooactually ingress hello data tooIoT Hub, you must call **IoTHubClient\_LL\_DoWork**, as in this example:</span></span>

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="2471b-150">Tento kód (z hello **iothub\_klienta\_ukázka\_http** aplikace) opakovaného volá **IoTHubClient\_UDOU\_DoWork** .</span><span class="sxs-lookup"><span data-stu-id="2471b-150">This code (from hello **iothub\_client\_sample\_http** application) repeatedly calls **IoTHubClient\_LL\_DoWork**.</span></span> <span data-ttu-id="2471b-151">Pokaždé, když **IoTHubClient\_UDOU\_DoWork** je volána, odešle některé události z vyrovnávací paměti tooIoT hello rozbočovače a načte zprávu ve frontě odesílaných toohello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2471b-151">Each time **IoTHubClient\_LL\_DoWork** is called, it sends some events from hello buffer tooIoT Hub and it retrieves a queued message being sent toohello device.</span></span> <span data-ttu-id="2471b-152">Hello druhém případě znamená, že pokud jsme zaregistrován funkce zpětného volání pro zprávy, pak hello zpětné volání je volána (za předpokladu, že všechny zprávy jsou zařazeny do fronty).</span><span class="sxs-lookup"><span data-stu-id="2471b-152">hello latter case means that if we registered a callback function for messages, then hello callback is invoked (assuming any messages are queued up).</span></span> <span data-ttu-id="2471b-153">By zaregistrovali funkci zpětného volání s kódem například hello následující:</span><span class="sxs-lookup"><span data-stu-id="2471b-153">We would have registered such a callback function with code such as hello following:</span></span>

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

<span data-ttu-id="2471b-154">Hello důvodu, že **IoTHubClient\_UDOU\_DoWork** se často říká smyčku je, že pokaždé, když je volána, odešle *některé* uložená do vyrovnávací paměti události tooIoT rozbočovače a načte *hello vedle* zprávy do fronty pro hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="2471b-154">hello reason that **IoTHubClient\_LL\_DoWork** is often called in a loop is that each time it’s called, it sends *some* buffered events tooIoT Hub and retrieves *hello next* message queued up for hello device.</span></span> <span data-ttu-id="2471b-155">Každé volání není zaručena toosend uložená do vyrovnávací paměti události nebo zařazených do fronty tooretrieve všechny zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-155">Each call isn’t guaranteed toosend all buffered events or tooretrieve all queued messages.</span></span> <span data-ttu-id="2471b-156">Pokud chcete, aby toosend všechny události v hello vyrovnávací paměti a pokračujte na další zpracování můžete nahradit kódu třeba následující hello Tato smyčka:</span><span class="sxs-lookup"><span data-stu-id="2471b-156">If you want toosend all events in hello buffer and then continue on with other processing you can replace this loop with code such as hello following:</span></span>

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

<span data-ttu-id="2471b-157">Tento kód zavolá **IoTHubClient\_UDOU\_DoWork** dokud všechny události ve vyrovnávací paměti hello byly odeslány tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2471b-157">This code calls **IoTHubClient\_LL\_DoWork** until all events in hello buffer have been sent tooIoT Hub.</span></span> <span data-ttu-id="2471b-158">Všimněte si, že to také neznamená, že všechny zprávy ve frontě byly přijaty.</span><span class="sxs-lookup"><span data-stu-id="2471b-158">Note this does not also imply that all queued messages have been received.</span></span> <span data-ttu-id="2471b-159">Součástí hello důvodem je, že kontrola "vše" zpráv není deterministický jako akce.</span><span class="sxs-lookup"><span data-stu-id="2471b-159">Part of hello reason for this is that checking for "all" messages isn’t as deterministic an action.</span></span> <span data-ttu-id="2471b-160">Co se stane, když je načíst "všechny" hello zprávy, ale jinou se pak odešlou toohello zařízení hned po?</span><span class="sxs-lookup"><span data-stu-id="2471b-160">What happens if you retrieve "all" of hello messages, but then another one is sent toohello device immediately after?</span></span> <span data-ttu-id="2471b-161">Lepší způsob toodeal, pomocí které je s naprogramovaných vypršení časového limitu.</span><span class="sxs-lookup"><span data-stu-id="2471b-161">A better way toodeal with that is with a programmed timeout.</span></span> <span data-ttu-id="2471b-162">Funkce zpětného volání zprávy hello mohli například resetovat časovač pokaždé, když je volána.</span><span class="sxs-lookup"><span data-stu-id="2471b-162">For example, hello message callback function could reset a timer every time it’s invoked.</span></span> <span data-ttu-id="2471b-163">Poté si můžete napsat logiku toocontinue zpracování, pokud například přijaty žádné zprávy byla v hello poslední *X* sekund.</span><span class="sxs-lookup"><span data-stu-id="2471b-163">You can then write logic toocontinue processing if, for example, no messages have been received in hello last *X* seconds.</span></span>

<span data-ttu-id="2471b-164">Pokud jste dokončení ingressing události a přijímání zpráv, být jisti toocall hello odpovídající funkce tooclean systémové prostředky.</span><span class="sxs-lookup"><span data-stu-id="2471b-164">When you’re finished ingressing events and receiving messages, be sure toocall hello corresponding function tooclean up resources.</span></span>

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

<span data-ttu-id="2471b-165">V podstatě je jen jednu sadu rozhraní API toosend a přijímat data z vlákna na pozadí a další sadu rozhraní API, která hello samé bez hello pozadí přístup z více vláken.</span><span class="sxs-lookup"><span data-stu-id="2471b-165">Basically there’s only one set of APIs toosend and receive data with a background thread and another set of APIs that does hello same thing without hello background thread.</span></span> <span data-ttu-id="2471b-166">Mnoho vývojáři můžou upřednostňovat hello jiný - UDOU rozhraní API, ale hello nižší úrovně rozhraní API je užitečný v případě hello vývojáře chce explicitní kontrolu nad síťových přenosů.</span><span class="sxs-lookup"><span data-stu-id="2471b-166">A lot of developers may prefer hello non-LL APIs, but hello lower-level APIs are useful when hello developer wants explicit control over network transmissions.</span></span> <span data-ttu-id="2471b-167">Například některá zařízení shromažďování dat přes čas a události příchozího pouze v určitých intervalech (například jednou za hodinu nebo jednou za den).</span><span class="sxs-lookup"><span data-stu-id="2471b-167">For example, some devices collect data over time and only ingress events at specified intervals (for example, once an hour or once a day).</span></span> <span data-ttu-id="2471b-168">Hello nižší úrovně rozhraní API udělení hello řízení tooexplicitly možnost při odesílání a příjem dat ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-168">hello lower-level APIs give you hello ability tooexplicitly control when you send and receive data from IoT Hub.</span></span> <span data-ttu-id="2471b-169">Ostatní bude jednoduše přednost hello jednoduchost této hello nižší úrovně rozhraní API nabízejí.</span><span class="sxs-lookup"><span data-stu-id="2471b-169">Others will simply prefer hello simplicity that hello lower-level APIs provide.</span></span> <span data-ttu-id="2471b-170">Všechno, co se stane na hello hlavního vlákna než některé pracovní situaci hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="2471b-170">Everything happens on hello main thread rather than some work happening in hello background.</span></span>

<span data-ttu-id="2471b-171">Podle toho, která modelu si zvolíte, být konzistentní se toobe které rozhraní API používáte.</span><span class="sxs-lookup"><span data-stu-id="2471b-171">Whichever model you choose, be sure toobe consistent in which APIs you use.</span></span> <span data-ttu-id="2471b-172">Pokud spustíte voláním **IoTHubClient\_UDOU\_CreateFromConnectionString**, ujistěte se, budete používat jenom hello odpovídající nižší úrovně rozhraní API pro všechny následné pracovní:</span><span class="sxs-lookup"><span data-stu-id="2471b-172">If you start by calling **IoTHubClient\_LL\_CreateFromConnectionString**, be sure you only use hello corresponding lower-level APIs for any follow-up work:</span></span>

* <span data-ttu-id="2471b-173">IoTHubClient\_UDOU\_SendEventAsync</span><span class="sxs-lookup"><span data-stu-id="2471b-173">IoTHubClient\_LL\_SendEventAsync</span></span>
* <span data-ttu-id="2471b-174">IoTHubClient\_UDOU\_SetMessageCallback</span><span class="sxs-lookup"><span data-stu-id="2471b-174">IoTHubClient\_LL\_SetMessageCallback</span></span>
* <span data-ttu-id="2471b-175">IoTHubClient\_UDOU\_Destroy</span><span class="sxs-lookup"><span data-stu-id="2471b-175">IoTHubClient\_LL\_Destroy</span></span>
* <span data-ttu-id="2471b-176">IoTHubClient\_UDOU\_DoWork</span><span class="sxs-lookup"><span data-stu-id="2471b-176">IoTHubClient\_LL\_DoWork</span></span>

<span data-ttu-id="2471b-177">Hello opačné je také hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="2471b-177">hello opposite is true as well.</span></span> <span data-ttu-id="2471b-178">Pokud začnete se **IoTHubClient\_CreateFromConnectionString**, potom použijte hello jiný - UDOU rozhraní API pro žádné další zpracování.</span><span class="sxs-lookup"><span data-stu-id="2471b-178">If you start with **IoTHubClient\_CreateFromConnectionString**, then use hello non-LL APIs for any additional processing.</span></span>

<span data-ttu-id="2471b-179">V hello Azure IoT zařízení SDK pro jazyk C, naleznete v části hello **iothub\_klienta\_ukázka\_http** aplikace pro kompletní příklad, jak hello nižší úrovně rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-179">In hello Azure IoT device SDK for C, see hello **iothub\_client\_sample\_http** application for a complete example of hello lower-level APIs.</span></span> <span data-ttu-id="2471b-180">Hello **iothub\_klienta\_ukázka\_amqp** aplikace může být odkazováno kompletní příklad hello jiný - UDOU rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-180">hello **iothub\_client\_sample\_amqp** application can be referenced for a full example of hello non-LL APIs.</span></span>

## <a name="property-handling"></a><span data-ttu-id="2471b-181">Vlastnost zpracování</span><span class="sxs-lookup"><span data-stu-id="2471b-181">Property handling</span></span>
<span data-ttu-id="2471b-182">Pokud při Popsali jsme odesílání dat, jsme jste byla odkazující toohello těla zprávy hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-182">So far when we've described sending data, we've been referring toohello body of hello message.</span></span> <span data-ttu-id="2471b-183">Představte si třeba tento kód:</span><span class="sxs-lookup"><span data-stu-id="2471b-183">For example, consider this code:</span></span>

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

<span data-ttu-id="2471b-184">Tento příklad odešle zprávu tooIoT rozbočovače s hello text "Hello, World".</span><span class="sxs-lookup"><span data-stu-id="2471b-184">This example sends a message tooIoT Hub with hello text "Hello World."</span></span> <span data-ttu-id="2471b-185">IoT Hub však také umožňuje vlastnosti toobe připojené tooeach zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-185">However, IoT Hub also allows properties toobe attached tooeach message.</span></span> <span data-ttu-id="2471b-186">Vlastnosti jsou páry název/hodnota, které mohou být připojené toohello zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-186">Properties are name/value pairs that can be attached toohello message.</span></span> <span data-ttu-id="2471b-187">Například můžeme můžete upravit hello předchozí kód tooattach zprávu toohello vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2471b-187">For example, we can modify hello previous code tooattach a property toohello message:</span></span>

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

<span data-ttu-id="2471b-188">Začneme voláním **IoTHubMessage\_vlastnosti** a předání hello popisovač našem zpráv.</span><span class="sxs-lookup"><span data-stu-id="2471b-188">We start by calling **IoTHubMessage\_Properties** and passing it hello handle of our message.</span></span> <span data-ttu-id="2471b-189">Se nám získat zpět **MAPY\_zpracování** odkaz, který umožňuje nám toostart přidání vlastností.</span><span class="sxs-lookup"><span data-stu-id="2471b-189">What we get back is a **MAP\_HANDLE** reference that enables us toostart adding properties.</span></span> <span data-ttu-id="2471b-190">Hello pozdější provádí volání **mapy\_AddOrUpdate**, což trvá odkaz tooa MAPY\_POPISOVAČ, hello název vlastnosti a hodnotu vlastnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-190">hello latter is accomplished by calling **Map\_AddOrUpdate**, which takes a reference tooa MAP\_HANDLE, hello property name, and hello property value.</span></span> <span data-ttu-id="2471b-191">S tímto rozhraním API přidáme jako mnoho vlastností, jako jsme jako.</span><span class="sxs-lookup"><span data-stu-id="2471b-191">With this API we can add as many properties as we like.</span></span>

<span data-ttu-id="2471b-192">Když je hello událostí pro čtení z **Event Hubs**, hello příjemce můžete vytvořit výčet vlastností hello a načíst jejich příslušné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="2471b-192">When hello event is read from **Event Hubs**, hello receiver can enumerate hello properties and retrieve their corresponding values.</span></span> <span data-ttu-id="2471b-193">Například v rozhraní .NET to by se udělat přístup k hello [vlastnosti kolekce u objektu EventData hello](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span><span class="sxs-lookup"><span data-stu-id="2471b-193">For example, in .NET this would be accomplished by accessing hello [Properties collection on hello EventData object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).</span></span>

<span data-ttu-id="2471b-194">V předchozím příkladu hello jsme se připojuje vlastnosti tooan událostí, odešleme tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2471b-194">In hello previous example, we’re attaching properties tooan event that we send tooIoT Hub.</span></span> <span data-ttu-id="2471b-195">Vlastnosti může být také připojené toomessages přijatých ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-195">Properties can also be attached toomessages received from IoT Hub.</span></span> <span data-ttu-id="2471b-196">Pokud chceme tooretrieve vlastnosti zprávy, můžeme použít kód například následující hello v našem zpráv funkce zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="2471b-196">If we want tooretrieve properties from a message, we can use code such as hello following in our message callback function:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from hello message
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

<span data-ttu-id="2471b-197">Hello volání příliš**IoTHubMessage\_vlastnosti** vrátí hello **MAPY\_zpracování** odkaz.</span><span class="sxs-lookup"><span data-stu-id="2471b-197">hello call too**IoTHubMessage\_Properties** returns hello **MAP\_HANDLE** reference.</span></span> <span data-ttu-id="2471b-198">Jsme pak předejte tento odkaz příliš**mapy\_GetInternals** tooobtain odkaz na pole tooan hello název hodnota páry (a také počet hello vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="2471b-198">We then pass that reference too**Map\_GetInternals** tooobtain a reference tooan array of hello name/value pairs (as well as a count of hello properties).</span></span> <span data-ttu-id="2471b-199">V tomto okamžiku je jednoduché vytváření výčtu hello vlastnosti tooget toohello hodnoty, které má být.</span><span class="sxs-lookup"><span data-stu-id="2471b-199">At that point it's a simple matter of enumerating hello properties tooget toohello values we want.</span></span>

<span data-ttu-id="2471b-200">Nemáte toouse vlastnosti ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2471b-200">You don't have toouse properties in your application.</span></span> <span data-ttu-id="2471b-201">Ale pokud budete potřebovat tooset je na události nebo je načtou ze zprávy, hello **IoTHubClient** knihovny lze snadno.</span><span class="sxs-lookup"><span data-stu-id="2471b-201">However, if you need tooset them on events or retrieve them from messages, hello **IoTHubClient** library makes it easy.</span></span>

## <a name="message-handling"></a><span data-ttu-id="2471b-202">Zpracování zpráv</span><span class="sxs-lookup"><span data-stu-id="2471b-202">Message handling</span></span>
<span data-ttu-id="2471b-203">Jak jsme uvedli dříve, při doručování zpráv ze služby IoT Hub hello **IoTHubClient** knihovny reaguje vyvoláním registrovaná funkce zpětného volání.</span><span class="sxs-lookup"><span data-stu-id="2471b-203">As stated previously, when messages arrive from IoT Hub hello **IoTHubClient** library responds by invoking a registered callback function.</span></span> <span data-ttu-id="2471b-204">Návratový parametr této funkce, které si zaslouží další vysvětlení není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="2471b-204">There is a return parameter of this function that deserves some additional explanation.</span></span> <span data-ttu-id="2471b-205">Tady je výňatek hello funkce zpětného volání v hello **iothub\_klienta\_ukázka\_http** ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="2471b-205">Here’s an excerpt of hello callback function in hello **iothub\_client\_sample\_http** sample application:</span></span>

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

<span data-ttu-id="2471b-206">Všimněte si, že je návratový typ hello **IOTHUBMESSAGE\_DISPOZICE\_výsledek** a v tomto případě vrátíme **IOTHUBMESSAGE\_platných**.</span><span class="sxs-lookup"><span data-stu-id="2471b-206">Note that hello return type is **IOTHUBMESSAGE\_DISPOSITION\_RESULT** and in this particular case we return **IOTHUBMESSAGE\_ACCEPTED**.</span></span> <span data-ttu-id="2471b-207">Existují jiné hodnoty může vrátíme z této funkce jak hello změnu **IoTHubClient** knihovny reaguje toohello zpětné volání zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-207">There are other values we can return from this function that change how hello **IoTHubClient** library reacts toohello message callback.</span></span> <span data-ttu-id="2471b-208">Zde jsou možnosti hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-208">Here are hello options.</span></span>

* <span data-ttu-id="2471b-209">**IOTHUBMESSAGE\_platných** – hello zpráva byla úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="2471b-209">**IOTHUBMESSAGE\_ACCEPTED** – hello message has been processed successfully.</span></span> <span data-ttu-id="2471b-210">Hello **IoTHubClient** knihovny nebude vyvolání funkce zpětného volání hello znovu s hello stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2471b-210">hello **IoTHubClient** library will not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="2471b-211">**IOTHUBMESSAGE\_ZAMÍTNUTÝ** – uvítací zprávu nebyla zpracována a neexistuje žádný desire toodo hello takže v budoucí.</span><span class="sxs-lookup"><span data-stu-id="2471b-211">**IOTHUBMESSAGE\_REJECTED** – hello message was not processed and there is no desire toodo so in hello future.</span></span> <span data-ttu-id="2471b-212">Hello **IoTHubClient** knihovny nesmí vyvolání funkce zpětného volání hello znovu s hello stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2471b-212">hello **IoTHubClient** library should not invoke hello callback function again with hello same message.</span></span>
* <span data-ttu-id="2471b-213">**IOTHUBMESSAGE\_ABANDONED** – uvítací zprávu nebyla úspěšně zpracována, ale hello **IoTHubClient** knihovny by měla vyvolat funkce zpětného volání hello znovu s hello stejnou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2471b-213">**IOTHUBMESSAGE\_ABANDONED** – hello message was not processed successfully, but hello **IoTHubClient** library should invoke hello callback function again with hello same message.</span></span>

<span data-ttu-id="2471b-214">Pro hello první dva návratové kódy, hello **IoTHubClient** knihovny odešle zprávu tooIoT rozbočovače, která určuje, že uvítací zprávu by se odstraní z fronty hello zařízení a nejsou doručeny znovu.</span><span class="sxs-lookup"><span data-stu-id="2471b-214">For hello first two return codes, hello **IoTHubClient** library sends a message tooIoT Hub indicating that hello message should be deleted from hello device queue and not delivered again.</span></span> <span data-ttu-id="2471b-215">čistý efekt Hello je hello stejné (hello odstraní zprávu z fronty zařízení hello), ale stále zaznamenat zda byla zpráva hello přijetí nebo odmítnutí.</span><span class="sxs-lookup"><span data-stu-id="2471b-215">hello net effect is hello same (hello message is deleted from hello device queue), but whether hello message was accepted or rejected is still recorded.</span></span>  <span data-ttu-id="2471b-216">Záznam tento rozdíl je užitečné toosenders hello zprávy, který může sledovat zpětnou vazbu a zjistit, pokud má zařízení přijímat nebo konkrétní zprávu odmítl.</span><span class="sxs-lookup"><span data-stu-id="2471b-216">Recording this distinction is useful toosenders of hello message who can listen for feedback and find out if a device has accepted or rejected a particular message.</span></span>

<span data-ttu-id="2471b-217">V případě poslední hello je odeslána zpráva taky tooIoT rozbočovače, ale znamená, že by měl vysláním hello zprávy.</span><span class="sxs-lookup"><span data-stu-id="2471b-217">In hello last case a message is also sent tooIoT Hub, but it indicates that hello message should be redelivered.</span></span> <span data-ttu-id="2471b-218">Obvykle budete abandon zprávu, pokud dojde k nějaké chybě, ale chtít tootry tooprocess hello zprávu znovu.</span><span class="sxs-lookup"><span data-stu-id="2471b-218">Typically you’ll abandon a message if you encounter some error but want tootry tooprocess hello message again.</span></span> <span data-ttu-id="2471b-219">Spočívá v tom, odmítat zprávu odpovídající když dojde k neodstranitelné chybě (nebo jednoduše se rozhodnete, že nechcete, aby tooprocess uvítací zprávu).</span><span class="sxs-lookup"><span data-stu-id="2471b-219">In contrast, rejecting a message is appropriate when you encounter an unrecoverable error (or if you simply decide you don’t want tooprocess hello message).</span></span>

<span data-ttu-id="2471b-220">V každém případě mějte na paměti hello různých návratových kódů tak, aby dokáže vyvolat hello chování, které chcete z hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2471b-220">In any case, be aware of hello different return codes so that you can elicit hello behavior you want from hello **IoTHubClient** library.</span></span>

## <a name="alternate-device-credentials"></a><span data-ttu-id="2471b-221">Přihlašovací údaje jiného zařízení</span><span class="sxs-lookup"><span data-stu-id="2471b-221">Alternate device credentials</span></span>
<span data-ttu-id="2471b-222">Jak je popsáno dříve, hello nejprve thing toodo při práci s hello **IoTHubClient** knihovna je tooobtain **IOTHUB\_klienta\_zpracování** pomocí volání například hello následující:</span><span class="sxs-lookup"><span data-stu-id="2471b-222">As explained previously, hello first thing toodo when working with hello **IoTHubClient** library is tooobtain a **IOTHUB\_CLIENT\_HANDLE** with a call such as hello following:</span></span>

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

<span data-ttu-id="2471b-223">Hello argumenty příliš**IoTHubClient\_CreateFromConnectionString** jsou hello zařízení připojovací řetězec a parametr, který označuje hello protokol používáme toocommunicate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-223">hello arguments too**IoTHubClient\_CreateFromConnectionString** are hello device connection string and a parameter that indicates hello protocol we use toocommunicate with IoT Hub.</span></span> <span data-ttu-id="2471b-224">Hello zařízení připojovací řetězec má formát, který se zobrazí takto:</span><span class="sxs-lookup"><span data-stu-id="2471b-224">hello device connection string has a format that appears as follows:</span></span>

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

<span data-ttu-id="2471b-225">Existují čtyři údaje v tento řetězec: název služby IoT Hub, příponu IoT Hub, ID zařízení a sdílený přístupový klíč.</span><span class="sxs-lookup"><span data-stu-id="2471b-225">There are four pieces of information in this string: IoT Hub name, IoT Hub suffix, device ID, and shared access key.</span></span> <span data-ttu-id="2471b-226">Získat hello plně kvalifikovaný název domény (FQDN) služby IoT hub při vytváření IoT hub instance v hello portál Azure – to vám dává název centra IoT hello (hello první část hello plně kvalifikovaný název domény) a hello IoT hub přípony (hello zbytek hello plně kvalifikovaný název domény).</span><span class="sxs-lookup"><span data-stu-id="2471b-226">You obtain hello fully qualified domain name (FQDN) of an IoT hub when you create your IoT hub instance in hello Azure portal — this gives you hello IoT hub name (hello first part of hello FQDN) and hello IoT hub suffix (hello rest of hello FQDN).</span></span> <span data-ttu-id="2471b-227">Získat ID zařízení hello a hello sdílený přístupový klíč při registraci zařízení s centrem IoT (jak je popsáno v hello [předchozí článek](iot-hub-device-sdk-c-intro.md)).</span><span class="sxs-lookup"><span data-stu-id="2471b-227">You get hello device ID and hello shared access key when you register your device with IoT Hub (as described in hello [previous article](iot-hub-device-sdk-c-intro.md)).</span></span>

<span data-ttu-id="2471b-228">**IoTHubClient\_CreateFromConnectionString** vám dá tooinitialize hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="2471b-228">**IoTHubClient\_CreateFromConnectionString** gives you one way tooinitialize hello library.</span></span> <span data-ttu-id="2471b-229">Pokud dáváte přednost, můžete vytvořit nový **IOTHUB\_klienta\_zpracování** pomocí těchto jednotlivé parametry místo hello zařízení připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2471b-229">If you prefer, you can create a new **IOTHUB\_CLIENT\_HANDLE** by using these individual parameters rather than hello device connection string.</span></span> <span data-ttu-id="2471b-230">Můžete toho dosáhnout s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="2471b-230">This is achieved with hello following code:</span></span>

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

<span data-ttu-id="2471b-231">To provede hello samé jako **IoTHubClient\_CreateFromConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="2471b-231">This accomplishes hello same thing as **IoTHubClient\_CreateFromConnectionString**.</span></span>

<span data-ttu-id="2471b-232">To nemusí připadat zřejmé, že chcete by toouse **IoTHubClient\_CreateFromConnectionString** místo Tato podrobnější metoda inicializace.</span><span class="sxs-lookup"><span data-stu-id="2471b-232">It may seem obvious that you would want toouse **IoTHubClient\_CreateFromConnectionString** rather than this more verbose method of initialization.</span></span> <span data-ttu-id="2471b-233">Mějte na paměti, ale, že při registraci zařízení do služby IoT Hub můžete získat je ID zařízení a klíč zařízení (ne připojovací řetězec).</span><span class="sxs-lookup"><span data-stu-id="2471b-233">Keep in mind, however, that when you register a device in IoT Hub what you get is a device ID and device key (not a connection string).</span></span> <span data-ttu-id="2471b-234">Hello *explorer zařízení* nástroj sady SDK byla zavedená v hello [předchozí článek](iot-hub-device-sdk-c-intro.md) používá knihovny v hello **sady SDK služby Azure IoT** toocreate hello zařízení připojovacího řetězce z ID zařízení Hello, klíč zařízení a název hostitele služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2471b-234">hello *device explorer* SDK tool introduced in hello [previous article](iot-hub-device-sdk-c-intro.md) uses libraries in hello **Azure IoT service SDK** toocreate hello device connection string from hello device ID, device key, and IoT Hub host name.</span></span> <span data-ttu-id="2471b-235">Proto volání **IoTHubClient\_UDOU\_vytvořit** může být vhodnější, protože díky tomu můžete hello krok generování připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="2471b-235">So calling **IoTHubClient\_LL\_Create** may be preferable because it saves you hello step of generating a connection string.</span></span> <span data-ttu-id="2471b-236">Použijte kteroukoli metodou je vhodné.</span><span class="sxs-lookup"><span data-stu-id="2471b-236">Use whichever method is convenient.</span></span>

## <a name="configuration-options"></a><span data-ttu-id="2471b-237">Možnosti konfigurace</span><span class="sxs-lookup"><span data-stu-id="2471b-237">Configuration options</span></span>
<span data-ttu-id="2471b-238">Pokud vše popsané o hello způsob hello **IoTHubClient** knihovny funguje odráží jeho výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="2471b-238">So far everything described about hello way hello **IoTHubClient** library works reflects its default behavior.</span></span> <span data-ttu-id="2471b-239">Existují však několik možností, které se dají nastavit toochange fungování hello knihovně.</span><span class="sxs-lookup"><span data-stu-id="2471b-239">However, there are a few options that you can set toochange how hello library works.</span></span> <span data-ttu-id="2471b-240">Toho dosahuje využitím hello **IoTHubClient\_UDOU\_SetOption** rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2471b-240">This is accomplished by leveraging hello **IoTHubClient\_LL\_SetOption** API.</span></span> <span data-ttu-id="2471b-241">Vezměte v úvahu v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="2471b-241">Consider this example:</span></span>

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

<span data-ttu-id="2471b-242">Existuje několik možností, které se běžně používají:</span><span class="sxs-lookup"><span data-stu-id="2471b-242">There are a couple of options that are commonly used:</span></span>

* <span data-ttu-id="2471b-243">**SetBatching** (logická hodnota) – Pokud **true**, data odeslaná tooIoT centra je odeslaný v dávkách.</span><span class="sxs-lookup"><span data-stu-id="2471b-243">**SetBatching** (bool) – If **true**, then data sent tooIoT Hub is sent in batches.</span></span> <span data-ttu-id="2471b-244">Pokud **false**, pak jsou zprávy odesílány jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="2471b-244">If **false**, then messages are sent individually.</span></span> <span data-ttu-id="2471b-245">Výchozí hodnota Hello je **false**.</span><span class="sxs-lookup"><span data-stu-id="2471b-245">hello default is **false**.</span></span> <span data-ttu-id="2471b-246">Všimněte si, že hello **SetBatching** možnost se vztahuje pouze protokol toohello HTTP a není toohello MQTT nebo AMQP protokoly.</span><span class="sxs-lookup"><span data-stu-id="2471b-246">Note that hello **SetBatching** option only applies toohello HTTP protocol and not toohello MQTT or AMQP protocols.</span></span>
* <span data-ttu-id="2471b-247">**Časový limit** (bez znaménka int) – Tato hodnota je reprezentována v milisekundách.</span><span class="sxs-lookup"><span data-stu-id="2471b-247">**Timeout** (unsigned int) – This value is represented in milliseconds.</span></span> <span data-ttu-id="2471b-248">Pokud odesílání požadavku protokolu HTTP nebo přijímání odpověď trvá déle, než tuto chvíli a pak časový limit připojení hello.</span><span class="sxs-lookup"><span data-stu-id="2471b-248">If sending an HTTP request or receiving a response takes longer than this time, then hello connection times out.</span></span>

<span data-ttu-id="2471b-249">dávkování možnost Hello je důležité.</span><span class="sxs-lookup"><span data-stu-id="2471b-249">hello batching option is important.</span></span> <span data-ttu-id="2471b-250">Ve výchozím nastavení, hello knihovně ingresses události jednotlivě (je jedinou událost, ať předáte příliš**IoTHubClient\_UDOU\_SendEventAsync**).</span><span class="sxs-lookup"><span data-stu-id="2471b-250">By default, hello library ingresses events individually (a single event is whatever you pass too**IoTHubClient\_LL\_SendEventAsync**).</span></span> <span data-ttu-id="2471b-251">Pokud je hello dávkování možnost **true**, hello knihovně shromažďuje tolik události, jak je možné z vyrovnávací paměti hello (až toohello maximální velikost zprávy, bude přijímat IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="2471b-251">If hello batching option is **true**, hello library collects as many events as it can from hello buffer (up toohello maximum message size that IoT Hub will accept).</span></span>  <span data-ttu-id="2471b-252">Hello batch událost je odeslána tooIoT centra v jediném volání protokolu HTTP (hello jednotlivé události jsou seskupeny do pole JSON).</span><span class="sxs-lookup"><span data-stu-id="2471b-252">hello event batch is sent tooIoT Hub in a single HTTP call (hello individual events are bundled into a JSON array).</span></span> <span data-ttu-id="2471b-253">Povolení dávkování obvykle za následek zvýšení výkonu velkých vzhledem k tomu, že se snižuje odezvy sítě.</span><span class="sxs-lookup"><span data-stu-id="2471b-253">Enabling batching typically results in big performance gains since you’re reducing network round-trips.</span></span> <span data-ttu-id="2471b-254">Také významně snižuje šířky pásma, protože při odesílání jednu sadu hlaviček protokolu HTTP s batch událostí a nikoli sadu hlavičky pro každé jednotlivé události.</span><span class="sxs-lookup"><span data-stu-id="2471b-254">It also significantly reduces bandwidth since you are sending one set of HTTP headers with an event batch rather than a set of headers for each individual event.</span></span> <span data-ttu-id="2471b-255">Pokud nemáte konkrétní důvod, proč toodo jinak, obvykle budete muset tooenable dávkování.</span><span class="sxs-lookup"><span data-stu-id="2471b-255">Unless you have a specific reason toodo otherwise, typically you’ll want tooenable batching.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2471b-256">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2471b-256">Next steps</span></span>
<span data-ttu-id="2471b-257">Tento článek popisuje podrobnosti hello chování aplikace hello **IoTHubClient** knihovna nalezena v hello **zařízení Azure IoT SDK pro jazyk C**. Pomocí těchto informací byste měli mít dobrou znalost jazyka hello možnosti hello **IoTHubClient** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2471b-257">This article describes in detail hello behavior of hello **IoTHubClient** library found in hello **Azure IoT device SDK for C**. With this information, you should have a good understanding of hello capabilities of hello **IoTHubClient** library.</span></span> <span data-ttu-id="2471b-258">Hello [následující článek](iot-hub-device-sdk-c-serializer.md) poskytuje podobné podrobností na hello **serializátor** knihovny.</span><span class="sxs-lookup"><span data-stu-id="2471b-258">hello [next article](iot-hub-device-sdk-c-serializer.md) provides similar detail on hello **serializer** library.</span></span>

<span data-ttu-id="2471b-259">toolearn Další informace o vývoji pro IoT Hub, najdete v části hello [SDK služby Azure IoT][lnk-sdks].</span><span class="sxs-lookup"><span data-stu-id="2471b-259">toolearn more about developing for IoT Hub, see hello [Azure IoT SDKs][lnk-sdks].</span></span>

<span data-ttu-id="2471b-260">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="2471b-260">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2471b-261">[Simulaci zařízení s Azure IoT Edge][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="2471b-261">[Simulating a device with Azure IoT Edge][lnk-iotedge]</span></span>

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
