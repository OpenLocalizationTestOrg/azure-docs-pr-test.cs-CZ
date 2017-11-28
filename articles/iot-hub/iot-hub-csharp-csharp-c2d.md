---
title: "zprávy aaaCloud zařízení s Azure IoT Hub (.NET) | Microsoft Docs"
description: "Jak toosend cloud zařízení zpráv tooa zařízení ze služby Azure IoT hub pomocí .NET SDK služby Azure IoT hello. Upravte zprávy typu cloud zařízení tooreceive aplikace zařízení a změny zpráv back-end aplikace hello toosend cloud zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f6a7618b164d95c8ddaf28943f244aeeb568217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a><span data-ttu-id="8f079-104">Odesílání zpráv z hello cloud tooyour zařízení s centra IoT (.NET)</span><span class="sxs-lookup"><span data-stu-id="8f079-104">Send messages from hello cloud tooyour device with IoT Hub (.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="8f079-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="8f079-105">Introduction</span></span>
<span data-ttu-id="8f079-106">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="8f079-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="8f079-107">Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze zařízení, která odesílá zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="8f079-107">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="8f079-108">V tomto kurzu vychází [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="8f079-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="8f079-109">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="8f079-109">It shows you how to:</span></span>

* <span data-ttu-id="8f079-110">Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f079-110">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="8f079-111">Příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="8f079-112">Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f079-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="8f079-113">Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="8f079-113">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="8f079-114">Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="8f079-114">At hello end of this tutorial, you run two .NET console apps:</span></span>

* <span data-ttu-id="8f079-115">**SimulatedDevice**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-115">**SimulatedDevice**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="8f079-116">**SendCloudToDevice**, který odesílá aplikaci pomocí zpráv typu cloud zařízení toohello zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.</span><span class="sxs-lookup"><span data-stu-id="8f079-116">**SendCloudToDevice**, which sends a cloud-to-device message toohello device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="8f079-117">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím [sady SDK pro zařízení Azure IoT].</span><span class="sxs-lookup"><span data-stu-id="8f079-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through [Azure IoT device SDKs].</span></span> <span data-ttu-id="8f079-118">Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="8f079-118">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [IoT Hub developer guide].</span></span>
> 
> 

<span data-ttu-id="8f079-119">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8f079-119">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="8f079-120">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8f079-120">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="8f079-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8f079-121">An active Azure account.</span></span> <span data-ttu-id="8f079-122">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="8f079-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-device-app"></a><span data-ttu-id="8f079-123">Příjem zpráv v aplikaci zařízení hello</span><span class="sxs-lookup"><span data-stu-id="8f079-123">Receive messages in hello device app</span></span>
<span data-ttu-id="8f079-124">V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="8f079-124">In this section, you'll modify hello device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="8f079-125">V sadě Visual Studio v hello **SimulatedDevice** projekt, přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="8f079-125">In Visual Studio, in hello **SimulatedDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud toodevice messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    <span data-ttu-id="8f079-126">Hello `ReceiveAsync` metoda hello přijaté zprávy asynchronně vrátí v čase hello, který je přijatých hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-126">hello `ReceiveAsync` method asynchronously returns hello received message at hello time that it is received by hello device.</span></span> <span data-ttu-id="8f079-127">Vrátí *null* po specifiable časový limit (v takovém případě se používá výchozí hello minutu).</span><span class="sxs-lookup"><span data-stu-id="8f079-127">It returns *null* after a specifiable timeout period (in this case, hello default of one minute is used).</span></span> <span data-ttu-id="8f079-128">Když aplikace hello obdrží *null*, i nadále toowait nových zpráv.</span><span class="sxs-lookup"><span data-stu-id="8f079-128">When hello app receives a *null*, it should continue toowait for new messages.</span></span> <span data-ttu-id="8f079-129">Tento požadavek je hello důvod hello `if (receivedMessage == null) continue` řádku.</span><span class="sxs-lookup"><span data-stu-id="8f079-129">This requirement is hello reason for hello `if (receivedMessage == null) continue` line.</span></span>
   
    <span data-ttu-id="8f079-130">Hello volání příliš`CompleteAsync()` IoT Hub upozorní, že hello zpráva byla úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="8f079-130">hello call too`CompleteAsync()` notifies IoT Hub that hello message has been successfully processed.</span></span> <span data-ttu-id="8f079-131">uvítací zprávu lze bezpečně odebrat z fronty hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-131">hello message can be safely removed from hello device queue.</span></span> <span data-ttu-id="8f079-132">Pokud tuto aplikaci zabránila hello zařízení z důvodu hello zpracování zprávy hello se něco stalo, IoT Hub zajišťuje ho znovu.</span><span class="sxs-lookup"><span data-stu-id="8f079-132">If something happened that prevented hello device app from completing hello processing of hello message, IoT Hub delivers it again.</span></span> <span data-ttu-id="8f079-133">Je důležité zpracování logiky v aplikaci zařízení hello této zprávy *idempotent*tak, aby přijímá hello vytváří vícekrát stejnou zprávu hello stejného výsledku.</span><span class="sxs-lookup"><span data-stu-id="8f079-133">It is then important that message processing logic in hello device app is *idempotent*, so that receiving hello same message multiple times produces hello same result.</span></span> <span data-ttu-id="8f079-134">Aplikace můžete také dočasně abandon zprávu, což vede k zachování uvítací zprávu ve frontě hello pro budoucí používání služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8f079-134">An application can also temporarily abandon a message, which results in IoT hub retaining hello message in hello queue for future consumption.</span></span> <span data-ttu-id="8f079-135">Nebo můžete aplikace hello odmítnout zprávu, která trvale odstraní uvítací zprávu z fronty hello.</span><span class="sxs-lookup"><span data-stu-id="8f079-135">Or, hello application can reject a message, which permanently removes hello message from hello queue.</span></span> <span data-ttu-id="8f079-136">Další informace o životní cyklus zpráv typu cloud zařízení hello najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="8f079-136">For more information about hello cloud-to-device message lifecycle, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8f079-137">Při použití protokolu HTTP místo MQTT nebo AMQP jako přenosového mechanismu, hello `ReceiveAsync` metoda vrátí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="8f079-137">When using HTTP instead of MQTT or AMQP as a transport, hello `ReceiveAsync` method returns immediately.</span></span> <span data-ttu-id="8f079-138">vzor Hello podporované pro zprávy typu cloud zařízení s protokolem HTTP je občas připojená zařízení, které zkontrolujte zprávy zřídka (méně než každých 25 minut).</span><span class="sxs-lookup"><span data-stu-id="8f079-138">hello supported pattern for cloud-to-device messages with HTTP is intermittently connected devices that check for messages infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="8f079-139">Vydání další HTTP přijme výsledky v omezení hello požadavky služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f079-139">Issuing more HTTP receives results in IoT Hub throttling hello requests.</span></span> <span data-ttu-id="8f079-140">Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="8f079-140">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 
2. <span data-ttu-id="8f079-141">Přidejte následující metodu v hello hello **hlavní** metoda bezprostředně před hello `Console.ReadLine()` řádku:</span><span class="sxs-lookup"><span data-stu-id="8f079-141">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>
   
        ReceiveC2dAsync();

> [!NOTE]
> <span data-ttu-id="8f079-142">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="8f079-142">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="8f079-143">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="8f079-143">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="8f079-144">Odeslání zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="8f079-144">Send a cloud-to-device message</span></span>
<span data-ttu-id="8f079-145">V této části napíšete konzolovou aplikaci .NET, která odešle aplikace zařízení toohello zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-145">In this section, you write a .NET console app that sends cloud-to-device messages toohello device app.</span></span>

1. <span data-ttu-id="8f079-146">V aktuálním řešení sady Visual Studio hello, vytvoření projektu Visual C# plochy aplikace pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="8f079-146">In hello current Visual Studio solution, create a Visual C# Desktop App project by using hello **Console Application** project template.</span></span> <span data-ttu-id="8f079-147">Název projektu hello **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="8f079-147">Name hello project **SendCloudToDevice**.</span></span>
   
    ![Nový projekt v sadě Visual Studio][20]
2. <span data-ttu-id="8f079-149">V Průzkumníku řešení klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet pro řešení...** .</span><span class="sxs-lookup"><span data-stu-id="8f079-149">In Solution Explorer, right-click hello solution, and then click **Manage NuGet Packages for Solution...**.</span></span> 
   
    <span data-ttu-id="8f079-150">Tato akce otevře hello **spravovat balíčky NuGet** okno.</span><span class="sxs-lookup"><span data-stu-id="8f079-150">This action opens hello **Manage NuGet Packages** window.</span></span>
3. <span data-ttu-id="8f079-151">Vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="8f079-151">Search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span> 
   
    <span data-ttu-id="8f079-152">To stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK služby Azure IoT].</span><span class="sxs-lookup"><span data-stu-id="8f079-152">This downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package].</span></span>

4. <span data-ttu-id="8f079-153">Přidejte následující hello `using` příkaz hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="8f079-153">Add hello following `using` statement at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="8f079-154">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="8f079-154">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="8f079-155">Nahraďte hodnotu zástupného symbolu hello s hello IoT hub připojovacího řetězce z [Začínáme se službou IoT Hub]:</span><span class="sxs-lookup"><span data-stu-id="8f079-155">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="8f079-156">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="8f079-156">Add hello following method toohello **Program** class:</span></span>
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    <span data-ttu-id="8f079-157">Tato metoda odesílá nové zařízení toohello zpráv typu cloud zařízení s hello ID `myFirstDevice`.</span><span class="sxs-lookup"><span data-stu-id="8f079-157">This method sends a new cloud-to-device message toohello device with hello ID, `myFirstDevice`.</span></span> <span data-ttu-id="8f079-158">Tento parametr změnit, pouze v případě, že se změnil z hello používá v [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="8f079-158">Change this parameter only if you modified it from hello one used in [Get started with IoT Hub].</span></span>
7. <span data-ttu-id="8f079-159">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8f079-159">Finally, add hello following lines toohello **Main** method:</span></span>
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="8f079-160">V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . Vyberte **více projektů po spuštění**, pak vyberte hello **spustit** akce pro **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="8f079-160">From within Visual Studio, right-click your solution, and select **Set StartUp projects...**. Select **Multiple startup projects**, then select hello **Start** action for **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **SendCloudToDevice**.</span></span>
9. <span data-ttu-id="8f079-161">Stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="8f079-161">Press **F5**.</span></span> <span data-ttu-id="8f079-162">Všechny tři aplikace by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="8f079-162">All three applications should start.</span></span> <span data-ttu-id="8f079-163">Vyberte hello **SendCloudToDevice** windows a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8f079-163">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="8f079-164">Měli byste vidět uvítací zprávu přijímání hello zařízení aplikací.</span><span class="sxs-lookup"><span data-stu-id="8f079-164">You should see hello message being received by hello device app.</span></span>
   
   ![Zprávy přijímající aplikace][21]

## <a name="receive-delivery-feedback"></a><span data-ttu-id="8f079-166">Přijímat zpětnou vazbu o doručení</span><span class="sxs-lookup"><span data-stu-id="8f079-166">Receive delivery feedback</span></span>
<span data-ttu-id="8f079-167">Je možné toorequest doručení (nebo vypršení platnosti) potvrzení ze služby IoT Hub pro každou zprávu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-167">It is possible toorequest delivery (or expiration) acknowledgements from IoT Hub for each cloud-to-device message.</span></span> <span data-ttu-id="8f079-168">Tato možnost umožňuje hello řešení back-end tooeasily informujte opakovat, nebo kompenzace logiku.</span><span class="sxs-lookup"><span data-stu-id="8f079-168">This option enables hello solution back end tooeasily inform retry or compensation logic.</span></span> <span data-ttu-id="8f079-169">Další informace o zpětné vazbě z cloudu do zařízení, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="8f079-169">For more information about cloud-to-device feedback, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="8f079-170">V této části upravíte hello **SendCloudToDevice** toorequest zpětné vazby aplikace a přijímat ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f079-170">In this section, you modify hello **SendCloudToDevice** app toorequest feedback, and receive it from IoT Hub.</span></span>

1. <span data-ttu-id="8f079-171">V sadě Visual Studio v hello **SendCloudToDevice** projekt, přidejte následující metodu toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="8f079-171">In Visual Studio, in hello **SendCloudToDevice** project, add hello following method toohello **Program** class.</span></span>
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    <span data-ttu-id="8f079-172">Poznámka: Tento vzor receive je hello stejné jeden použité tooreceive zprávy typu cloud zařízení z aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-172">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>
2. <span data-ttu-id="8f079-173">Přidejte následující metodu v hello hello **hlavní** metoda hned po hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` řádku:</span><span class="sxs-lookup"><span data-stu-id="8f079-173">Add hello following method in hello **Main** method, right after hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` line:</span></span>
   
        ReceiveFeedbackAsync();
3. <span data-ttu-id="8f079-174">zpětná vazba toorequest hello doručení zprávy typu cloud zařízení, máte toospecify vlastností v hello **SendCloudToDeviceMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="8f079-174">toorequest feedback for hello delivery of your cloud-to-device message, you have toospecify a property in hello **SendCloudToDeviceMessageAsync** method.</span></span> <span data-ttu-id="8f079-175">Přidejte následující řádek, přímo po hello hello `var commandMessage = new Message(...);` řádku:</span><span class="sxs-lookup"><span data-stu-id="8f079-175">Add hello following line, right after hello `var commandMessage = new Message(...);` line:</span></span>
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. <span data-ttu-id="8f079-176">Spuštění aplikace hello stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="8f079-176">Run hello apps by pressing **F5**.</span></span> <span data-ttu-id="8f079-177">Měli byste vidět všechny tři aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="8f079-177">You should see all three applications start.</span></span> <span data-ttu-id="8f079-178">Vyberte hello **SendCloudToDevice** windows a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8f079-178">Select hello **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="8f079-179">Měli byste vidět hello zprávy přijímání aplikace hello zařízení a za několik sekund, hello přijímá zprávy zpětnou vazbu vaše **SendCloudToDevice** aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f079-179">You should see hello message being received by hello device app, and after a few seconds, hello feedback message being received by your **SendCloudToDevice** application.</span></span>
   
   ![Zprávy přijímající aplikace][22]

> [!NOTE]
> <span data-ttu-id="8f079-181">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="8f079-181">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="8f079-182">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="8f079-182">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="8f079-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f079-183">Next steps</span></span>
<span data-ttu-id="8f079-184">V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f079-184">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="8f079-185">Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="8f079-185">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="8f079-186">toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="8f079-186">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[balíček NuGet sady SDK služby Azure IoT]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/en-us/azure/iot-suite/
[sady SDK pro zařízení Azure IoT]: iot-hub-devguide-sdks.md