---
title: "Zprávy typu cloud zařízení s Azure IoT Hub (.NET) | Microsoft Docs"
description: "Postupy pro odesílání zpráv typu cloud zařízení pro zařízení ze služby Azure IoT hub pomocí sady Azure IoT SDK pro .NET. Můžete upravit aplikaci zařízení příjem zpráv typu cloud zařízení a úpravě back-end aplikace k odesílání zpráv typu cloud zařízení."
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
ms.openlocfilehash: 3f5f83671054c30afde3d7f18ff0edcdb8f78a01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a><span data-ttu-id="1a490-104">Odesílání zpráv z cloudu do zařízení s centra IoT (.NET)</span><span class="sxs-lookup"><span data-stu-id="1a490-104">Send messages from the cloud to your device with IoT Hub (.NET)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a><span data-ttu-id="1a490-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="1a490-105">Introduction</span></span>
<span data-ttu-id="1a490-106">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="1a490-106">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="1a490-107">[Začínáme se službou IoT Hub] kurz ukazuje, jak k vytvoření služby IoT hub, zřídit identitu zařízení v ní a kódu aplikaci ze zařízení, která odesílá zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="1a490-107">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="1a490-108">V tomto kurzu vychází [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="1a490-108">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="1a490-109">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="1a490-109">It shows you how to:</span></span>

* <span data-ttu-id="1a490-110">Z back-end vašeho řešení odesílání zpráv typu cloud zařízení na jedno zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1a490-110">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="1a490-111">Příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-111">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="1a490-112">Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané do zařízení ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1a490-112">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="1a490-113">Můžete najít další informace o zprávy typu cloud zařízení v [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="1a490-113">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="1a490-114">Na konci tohoto kurzu můžete spustit dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="1a490-114">At the end of this tutorial, you run two .NET console apps:</span></span>

* <span data-ttu-id="1a490-115">**SimulatedDevice**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub], který se připojuje ke službě IoT hub a přijímá zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-115">**SimulatedDevice**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="1a490-116">**SendCloudToDevice**, který odešle zprávu cloud zařízení do aplikace zařízení prostřednictvím služby IoT Hub a pak obdrží jeho potvrzení o doručení.</span><span class="sxs-lookup"><span data-stu-id="1a490-116">**SendCloudToDevice**, which sends a cloud-to-device message to the device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="1a490-117">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím [sady SDK pro zařízení Azure IoT].</span><span class="sxs-lookup"><span data-stu-id="1a490-117">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through [Azure IoT device SDKs].</span></span> <span data-ttu-id="1a490-118">Podrobné pokyny o tom, jak připojit zařízení ke kódu v tomto kurzu a obecně do služby Azure IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="1a490-118">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [IoT Hub developer guide].</span></span>
> 
> 

<span data-ttu-id="1a490-119">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="1a490-119">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="1a490-120">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1a490-120">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="1a490-121">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1a490-121">An active Azure account.</span></span> <span data-ttu-id="1a490-122">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="1a490-122">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-device-app"></a><span data-ttu-id="1a490-123">Příjem zpráv v aplikaci zařízení</span><span class="sxs-lookup"><span data-stu-id="1a490-123">Receive messages in the device app</span></span>
<span data-ttu-id="1a490-124">V této části upravíte zařízení aplikaci, kterou jste vytvořili v [Začínáme se službou IoT Hub] pro příjem zpráv typu cloud zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1a490-124">In this section, you'll modify the device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="1a490-125">V sadě Visual Studio v **SimulatedDevice** projekt, přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="1a490-125">In Visual Studio, in the **SimulatedDevice** project, add the following method to the **Program** class.</span></span>
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
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
   
    <span data-ttu-id="1a490-126">`ReceiveAsync` Metoda přijaté zprávy asynchronně vrátí v čase, který je přijatých zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-126">The `ReceiveAsync` method asynchronously returns the received message at the time that it is received by the device.</span></span> <span data-ttu-id="1a490-127">Vrátí *null* po specifiable časový limit (v takovém případě se používá výchozí minutu).</span><span class="sxs-lookup"><span data-stu-id="1a490-127">It returns *null* after a specifiable timeout period (in this case, the default of one minute is used).</span></span> <span data-ttu-id="1a490-128">Pokud aplikace obdrží *null*, by měly být nadále čekat na nové zprávy.</span><span class="sxs-lookup"><span data-stu-id="1a490-128">When the app receives a *null*, it should continue to wait for new messages.</span></span> <span data-ttu-id="1a490-129">Tento požadavek je důvody, proč `if (receivedMessage == null) continue` řádku.</span><span class="sxs-lookup"><span data-stu-id="1a490-129">This requirement is the reason for the `if (receivedMessage == null) continue` line.</span></span>
   
    <span data-ttu-id="1a490-130">Volání `CompleteAsync()` IoT Hub upozorní, že zpráva byla úspěšně zpracována.</span><span class="sxs-lookup"><span data-stu-id="1a490-130">The call to `CompleteAsync()` notifies IoT Hub that the message has been successfully processed.</span></span> <span data-ttu-id="1a490-131">Zprávu lze bezpečně odebrat z fronty zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-131">The message can be safely removed from the device queue.</span></span> <span data-ttu-id="1a490-132">Pokud se něco stalo, která zabránila aplikace zařízení dokončení zpracování zprávy, IoT Hub zajišťuje ho znovu.</span><span class="sxs-lookup"><span data-stu-id="1a490-132">If something happened that prevented the device app from completing the processing of the message, IoT Hub delivers it again.</span></span> <span data-ttu-id="1a490-133">Je důležité, že zpráva zpracování logiky v aplikaci zařízení *idempotent*tak, aby přijímá stejnou zprávu vícekrát vytváří stejný výsledek.</span><span class="sxs-lookup"><span data-stu-id="1a490-133">It is then important that message processing logic in the device app is *idempotent*, so that receiving the same message multiple times produces the same result.</span></span> <span data-ttu-id="1a490-134">Aplikace můžete také dočasně abandon zprávu, což vede k zachování zprávu ve frontě pro budoucí používání služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1a490-134">An application can also temporarily abandon a message, which results in IoT hub retaining the message in the queue for future consumption.</span></span> <span data-ttu-id="1a490-135">Nebo můžete aplikaci odmítnout zprávu, která trvale odstraní zprávu z fronty.</span><span class="sxs-lookup"><span data-stu-id="1a490-135">Or, the application can reject a message, which permanently removes the message from the queue.</span></span> <span data-ttu-id="1a490-136">Další informace o životním cyklu zprávy typu cloud zařízení najdete v tématu [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="1a490-136">For more information about the cloud-to-device message lifecycle, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1a490-137">Při použití protokolu HTTP místo MQTT nebo AMQP přenos, `ReceiveAsync` metoda vrátí okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1a490-137">When using HTTP instead of MQTT or AMQP as a transport, the `ReceiveAsync` method returns immediately.</span></span> <span data-ttu-id="1a490-138">Podporované vzor pro zprávy typu cloud zařízení s protokolem HTTP je občas připojená zařízení, které zkontrolujte zprávy zřídka (méně než každých 25 minut).</span><span class="sxs-lookup"><span data-stu-id="1a490-138">The supported pattern for cloud-to-device messages with HTTP is intermittently connected devices that check for messages infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="1a490-139">Vydání další HTTP přijme výsledky v IoT Hub, omezení požadavků.</span><span class="sxs-lookup"><span data-stu-id="1a490-139">Issuing more HTTP receives results in IoT Hub throttling the requests.</span></span> <span data-ttu-id="1a490-140">Další informace o rozdílech mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="1a490-140">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>
   > 
   > 
2. <span data-ttu-id="1a490-141">Přidejte následující metodu v **hlavní** metoda, těsně před `Console.ReadLine()` řádku:</span><span class="sxs-lookup"><span data-stu-id="1a490-141">Add the following method in the **Main** method, right before the `Console.ReadLine()` line:</span></span>
   
        ReceiveC2dAsync();

> [!NOTE]
> <span data-ttu-id="1a490-142">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="1a490-142">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1a490-143">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="1a490-143">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="1a490-144">Odeslání zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="1a490-144">Send a cloud-to-device message</span></span>
<span data-ttu-id="1a490-145">V této části napíšete konzolovou aplikaci .NET, která odesílá zprávy typu cloud zařízení na zařízení aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1a490-145">In this section, you write a .NET console app that sends cloud-to-device messages to the device app.</span></span>

1. <span data-ttu-id="1a490-146">V aktuálním řešení sady Visual Studio, vytvoření projektu Visual C# plochy aplikace pomocí **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="1a490-146">In the current Visual Studio solution, create a Visual C# Desktop App project by using the **Console Application** project template.</span></span> <span data-ttu-id="1a490-147">Název projektu **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="1a490-147">Name the project **SendCloudToDevice**.</span></span>
   
    ![Nový projekt v sadě Visual Studio][20]
2. <span data-ttu-id="1a490-149">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a pak klikněte na tlačítko **spravovat balíčky NuGet pro řešení...** .</span><span class="sxs-lookup"><span data-stu-id="1a490-149">In Solution Explorer, right-click the solution, and then click **Manage NuGet Packages for Solution...**.</span></span> 
   
    <span data-ttu-id="1a490-150">Tato akce otevře **spravovat balíčky NuGet** okno.</span><span class="sxs-lookup"><span data-stu-id="1a490-150">This action opens the **Manage NuGet Packages** window.</span></span>
3. <span data-ttu-id="1a490-151">Vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="1a490-151">Search for **Microsoft.Azure.Devices**, click **Install**, and accept the terms of use.</span></span> 
   
    <span data-ttu-id="1a490-152">To stáhne, nainstaluje a přidá odkaz na [balíček NuGet sady SDK služby Azure IoT].</span><span class="sxs-lookup"><span data-stu-id="1a490-152">This downloads, installs, and adds a reference to the [Azure IoT service SDK NuGet package].</span></span>

4. <span data-ttu-id="1a490-153">Do horní části souboru **Program.cs** přidejte následující příkaz `using`:</span><span class="sxs-lookup"><span data-stu-id="1a490-153">Add the following `using` statement at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
5. <span data-ttu-id="1a490-154">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="1a490-154">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="1a490-155">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem IoT hub z [Začínáme se službou IoT Hub]:</span><span class="sxs-lookup"><span data-stu-id="1a490-155">Substitute the placeholder value with the IoT hub connection string from [Get started with IoT Hub]:</span></span>
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="1a490-156">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="1a490-156">Add the following method to the **Program** class:</span></span>
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    <span data-ttu-id="1a490-157">Tato metoda odesílá novou zprávu typu cloud zařízení na zařízení s ID, `myFirstDevice`.</span><span class="sxs-lookup"><span data-stu-id="1a490-157">This method sends a new cloud-to-device message to the device with the ID, `myFirstDevice`.</span></span> <span data-ttu-id="1a490-158">Tento parametr změnit, pouze v případě, že se změnil z používaný v [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="1a490-158">Change this parameter only if you modified it from the one used in [Get started with IoT Hub].</span></span>
7. <span data-ttu-id="1a490-159">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="1a490-159">Finally, add the following lines to the **Main** method:</span></span>
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="1a490-160">V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . Vyberte **více projektů po spuštění**, vyberte **spustit** akce pro **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **SendCloudToDevice**.</span><span class="sxs-lookup"><span data-stu-id="1a490-160">From within Visual Studio, right-click your solution, and select **Set StartUp projects...**. Select **Multiple startup projects**, then select the **Start** action for **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **SendCloudToDevice**.</span></span>
9. <span data-ttu-id="1a490-161">Stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="1a490-161">Press **F5**.</span></span> <span data-ttu-id="1a490-162">Všechny tři aplikace by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="1a490-162">All three applications should start.</span></span> <span data-ttu-id="1a490-163">Vyberte **SendCloudToDevice** windows a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1a490-163">Select the **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="1a490-164">Měli byste vidět zprávu přijímá aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-164">You should see the message being received by the device app.</span></span>
   
   ![Zprávy přijímající aplikace][21]

## <a name="receive-delivery-feedback"></a><span data-ttu-id="1a490-166">Přijímat zpětnou vazbu o doručení</span><span class="sxs-lookup"><span data-stu-id="1a490-166">Receive delivery feedback</span></span>
<span data-ttu-id="1a490-167">Je možné k potvrzení žádosti o doručení (nebo vypršení platnosti) ze služby IoT Hub pro každou zprávu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-167">It is possible to request delivery (or expiration) acknowledgements from IoT Hub for each cloud-to-device message.</span></span> <span data-ttu-id="1a490-168">Tato možnost umožňuje back-end řešení snadno informovat opakovat, nebo kompenzace logiku.</span><span class="sxs-lookup"><span data-stu-id="1a490-168">This option enables the solution back end to easily inform retry or compensation logic.</span></span> <span data-ttu-id="1a490-169">Další informace o zpětné vazbě z cloudu do zařízení, najdete v článku [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="1a490-169">For more information about cloud-to-device feedback, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="1a490-170">V této části upravíte **SendCloudToDevice** aplikaci žádosti o zpětnou vazbu a přijímat ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1a490-170">In this section, you modify the **SendCloudToDevice** app to request feedback, and receive it from IoT Hub.</span></span>

1. <span data-ttu-id="1a490-171">V sadě Visual Studio v **SendCloudToDevice** projekt, přidejte následující metodu do **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="1a490-171">In Visual Studio, in the **SendCloudToDevice** project, add the following method to the **Program** class.</span></span>
   
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
   
    <span data-ttu-id="1a490-172">Všimněte si, že tento vzor receive se shoduje s klíčem pro příjem zpráv typu cloud zařízení z aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-172">Note this receive pattern is the same one used to receive cloud-to-device messages from the device app.</span></span>
2. <span data-ttu-id="1a490-173">Přidejte následující metodu v **hlavní** metoda hned po `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` řádku:</span><span class="sxs-lookup"><span data-stu-id="1a490-173">Add the following method in the **Main** method, right after the `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` line:</span></span>
   
        ReceiveFeedbackAsync();
3. <span data-ttu-id="1a490-174">K žádosti o zpětnou vazbu pro doručení zprávy typu cloud zařízení, budete muset určit na vlastnost ve **SendCloudToDeviceMessageAsync** metoda.</span><span class="sxs-lookup"><span data-stu-id="1a490-174">To request feedback for the delivery of your cloud-to-device message, you have to specify a property in the **SendCloudToDeviceMessageAsync** method.</span></span> <span data-ttu-id="1a490-175">Přidejte následující řádek, hned po `var commandMessage = new Message(...);` řádku:</span><span class="sxs-lookup"><span data-stu-id="1a490-175">Add the following line, right after the `var commandMessage = new Message(...);` line:</span></span>
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. <span data-ttu-id="1a490-176">Spuštění aplikace stisknutím **F5**.</span><span class="sxs-lookup"><span data-stu-id="1a490-176">Run the apps by pressing **F5**.</span></span> <span data-ttu-id="1a490-177">Měli byste vidět všechny tři aplikace spustit.</span><span class="sxs-lookup"><span data-stu-id="1a490-177">You should see all three applications start.</span></span> <span data-ttu-id="1a490-178">Vyberte **SendCloudToDevice** windows a stiskněte klávesu **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1a490-178">Select the **SendCloudToDevice** windows, and press **Enter**.</span></span> <span data-ttu-id="1a490-179">Měli byste vidět zprávu přijímání aplikace zařízení a za několik sekund, přijímá zprávy zpětnou vazbu vaše **SendCloudToDevice** aplikace.</span><span class="sxs-lookup"><span data-stu-id="1a490-179">You should see the message being received by the device app, and after a few seconds, the feedback message being received by your **SendCloudToDevice** application.</span></span>
   
   ![Zprávy přijímající aplikace][22]

> [!NOTE]
> <span data-ttu-id="1a490-181">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="1a490-181">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1a490-182">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="1a490-182">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="1a490-183">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1a490-183">Next steps</span></span>
<span data-ttu-id="1a490-184">V tomto kurzu jste zjistili, jak odesílat a přijímat zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="1a490-184">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="1a490-185">Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="1a490-185">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="1a490-186">Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="1a490-186">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

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