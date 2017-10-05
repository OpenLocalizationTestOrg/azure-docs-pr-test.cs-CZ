---
title: "Zpracování zpráv typu zařízení cloud Azure IoT Hub pomocí tras (.Net) | Microsoft Docs"
description: "Postupy zpracování zpráv typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body k odeslání zprávy do dalších služeb back-end."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 1d2b52ea005ab520bf294efa603512c00a92ee63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="0e067-103">Zpracování zpráv typu zařízení cloud IoT Hub pomocí tras (.NET)</span><span class="sxs-lookup"><span data-stu-id="0e067-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="0e067-104">V tomto kurzu vychází [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="0e067-104">This tutorial builds on the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="0e067-105">Kurz:</span><span class="sxs-lookup"><span data-stu-id="0e067-105">The tutorial:</span></span>

* <span data-ttu-id="0e067-106">Ukazuje, jak používat směrování pravidla k odesílání zpráv typu zařízení cloud způsobem snadno, založené na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="0e067-106">Shows you how to use routing rules to dispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="0e067-107">Ukazuje, jak izolovat interaktivní zprávy, které vyžadují okamžitou akci z back-end řešení pro další zpracování.</span><span class="sxs-lookup"><span data-stu-id="0e067-107">Illustrates how to isolate interactive messages that require immediate action from the solution back end for further processing.</span></span> <span data-ttu-id="0e067-108">Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM.</span><span class="sxs-lookup"><span data-stu-id="0e067-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="0e067-109">Naproti tomu datového bodu zprávy, jako je například teplotní telemetrie informačního kanálu do modul analytics.</span><span class="sxs-lookup"><span data-stu-id="0e067-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="0e067-110">Na konci tohoto kurzu můžete spustit tři aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="0e067-110">At the end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="0e067-111">**SimulatedDevice**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub] kurzu se odešle zprávy typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="0e067-111">**SimulatedDevice**, a modified version of the app created in the [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="0e067-112">**ReadDeviceToCloudMessages** který zobrazí nekritické telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="0e067-112">**ReadDeviceToCloudMessages** that displays the non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="0e067-113">**ReadCriticalQueue** zrušte fronty kritické zprávy odeslané aplikace zařízení z fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0e067-113">**ReadCriticalQueue** de-queues the critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="0e067-114">Tato fronta je připojený ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-114">This queue is attached to the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="0e067-115">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="0e067-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="0e067-116">Zjistěte, jak nahradit simulované zařízení v tomto kurzu fyzické zařízení, najdete v článku [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="0e067-116">To learn how to replace the simulated device in this tutorial with a physical device, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="0e067-117">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="0e067-117">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="0e067-118">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0e067-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="0e067-119">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="0e067-119">An active Azure account.</span></span> <br/><span data-ttu-id="0e067-120">Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="0e067-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="0e067-121">Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="0e067-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="0e067-122">Odesílat interaktivní zprávy</span><span class="sxs-lookup"><span data-stu-id="0e067-122">Send interactive messages</span></span>

<span data-ttu-id="0e067-123">Upravit aplikaci zařízení, kterou jste vytvořili v [Začínáme se službou IoT Hub] kurzu příležitostně odesílat interaktivní zprávy.</span><span class="sxs-lookup"><span data-stu-id="0e067-123">Modify the device app you created in the [Get started with IoT Hub] tutorial to occasionally send interactive messages.</span></span>

<span data-ttu-id="0e067-124">V sadě Visual Studio v **SimulatedDevice** projektu, nahraďte `SendDeviceToCloudMessagesAsync` metoda následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="0e067-124">In Visual Studio, in the **SimulatedDevice** project, replace the `SendDeviceToCloudMessagesAsync` method with the following code:</span></span>

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

<span data-ttu-id="0e067-125">Tato metoda náhodně přidá vlastnost `"level": "critical"` na zprávy odeslané zařízením, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="0e067-125">This method randomly adds the property `"level": "critical"` to messages sent by the device, which simulates a message that requires immediate action by the solution back-end.</span></span> <span data-ttu-id="0e067-126">Aplikace zařízení předá tyto informace ve vlastnostech zpráv místo v textu zprávy, že IoT Hub může směrovat zprávy Cíl správné zprávy.</span><span class="sxs-lookup"><span data-stu-id="0e067-126">The device app passes this information in the message properties, instead of in the message body, so that IoT Hub can route the message to the proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="0e067-127">Vlastnosti zprávy pro směrování zpráv můžete použít pro různé scénáře, včetně studený cesty při zpracování, kromě zde ukazuje příklad hot-path.</span><span class="sxs-lookup"><span data-stu-id="0e067-127">You can use message properties to route messages for various scenarios including cold-path processing, in addition to the hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="0e067-128">Z důvodu zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="0e067-128">For the sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="0e067-129">V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="0e067-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in the MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-to-a-queue-in-your-iot-hub"></a><span data-ttu-id="0e067-130">Směrování zpráv do fronty ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="0e067-130">Route messages to a queue in your IoT hub</span></span>

<span data-ttu-id="0e067-131">V této části:</span><span class="sxs-lookup"><span data-stu-id="0e067-131">In this section, you:</span></span>

* <span data-ttu-id="0e067-132">Vytvoření fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0e067-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="0e067-133">Připojí ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-133">Connect it to your IoT hub.</span></span>
* <span data-ttu-id="0e067-134">Konfigurace služby IoT hub odesílat zprávy do fronty na základě přítomnosti vlastnosti na zprávu.</span><span class="sxs-lookup"><span data-stu-id="0e067-134">Configure your IoT hub to send messages to the queue based on the presence of a property on the message.</span></span>

<span data-ttu-id="0e067-135">Další informace o tom, jak zpracování zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="0e067-135">For more information about how to process messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="0e067-136">Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="0e067-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="0e067-137">Fronty musí být ve stejném předplatném, oblasti jako službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-137">The queue must be in the same subscription and region as your IoT hub.</span></span> <span data-ttu-id="0e067-138">Poznamenejte si název oboru názvů a fronty.</span><span class="sxs-lookup"><span data-stu-id="0e067-138">Make a note of the namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0e067-139">Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena.</span><span class="sxs-lookup"><span data-stu-id="0e067-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="0e067-140">Pokud některá z těchto možností jsou povolené, koncový bod se zobrazí jako **Unreachable** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="0e067-140">If either of those options are enabled, the endpoint appears as **Unreachable** in the Azure portal.</span></span>

2. <span data-ttu-id="0e067-141">Na portálu Azure otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="0e067-141">In the Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Koncové body v centru IoT][30]

3. <span data-ttu-id="0e067-143">V **koncové body** okně klikněte na tlačítko **přidat** v horní části přidat fronty do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-143">In the **Endpoints** blade, click **Add** at the top to add your queue to your IoT hub.</span></span> <span data-ttu-id="0e067-144">Název koncového bodu **CriticalQueue** a použijte rozevírací seznamy a vyberte **fronty Service Bus**, oboru názvů Service Bus, ve které je umístěn fronty a názvu fronty.</span><span class="sxs-lookup"><span data-stu-id="0e067-144">Name the endpoint **CriticalQueue** and use the drop-downs to select **Service Bus queue**, the Service Bus namespace in which your queue resides, and the name of your queue.</span></span> <span data-ttu-id="0e067-145">Až budete hotovi, klikněte na tlačítko **Uložit** dole.</span><span class="sxs-lookup"><span data-stu-id="0e067-145">When you are done, click **Save** at the bottom.</span></span>
    
    ![Přidání koncového bodu][31]
    
4. <span data-ttu-id="0e067-147">Klikněte na tlačítko **trasy** ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="0e067-148">Klikněte na tlačítko **přidat** v horní části okna Vytvořit pravidlo směrování pro směrování zpráv do fronty jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="0e067-148">Click **Add** at the top of the blade to create a routing rule that routes messages to the queue you just added.</span></span> <span data-ttu-id="0e067-149">Vyberte **DeviceTelemetry** jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="0e067-149">Select **DeviceTelemetry** as the source of data.</span></span> <span data-ttu-id="0e067-150">Zadejte `level="critical"` jako podmínka a vyberte fronty, který jste právě přidali jako vlastní koncový bod jako koncový bod směrování pravidlo.</span><span class="sxs-lookup"><span data-stu-id="0e067-150">Enter `level="critical"` as the condition, and choose the queue you just added as a custom endpoint as the routing rule endpoint.</span></span> <span data-ttu-id="0e067-151">Až budete hotovi, klikněte na tlačítko **Uložit** dole.</span><span class="sxs-lookup"><span data-stu-id="0e067-151">When you are done, click **Save** at the bottom.</span></span>
    
    ![Přidání trasu][32]
    
    <span data-ttu-id="0e067-153">Ujistěte se, že záložní cesta je nastavena na **ON**.</span><span class="sxs-lookup"><span data-stu-id="0e067-153">Make sure the fallback route is set to **ON**.</span></span> <span data-ttu-id="0e067-154">Tato hodnota je výchozí konfiguraci pro Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="0e067-154">This value is the default configuration for an IoT hub.</span></span>
    
    ![Záložní trasy][33]

## <a name="read-from-the-queue-endpoint"></a><span data-ttu-id="0e067-156">Čtení z fronty koncového bodu</span><span class="sxs-lookup"><span data-stu-id="0e067-156">Read from the queue endpoint</span></span>

<span data-ttu-id="0e067-157">V této části čtení zpráv z fronty koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="0e067-157">In this section, you read the messages from the queue endpoint.</span></span>

1. <span data-ttu-id="0e067-158">V sadě Visual Studio přidejte ke stávajícímu řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="0e067-158">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution, by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="0e067-159">Název projektu **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="0e067-159">Name the project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="0e067-160">V Průzkumníku řešení klikněte pravým tlačítkem myši **ReadCriticalQueue** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="0e067-160">In Solution Explorer, right-click the **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="0e067-161">Zobrazí tuto operaci **Správce balíčků NuGet** okno.</span><span class="sxs-lookup"><span data-stu-id="0e067-161">This operation displays the **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="0e067-162">Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="0e067-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept the terms of use.</span></span> <span data-ttu-id="0e067-163">Tato operace stáhne, nainstaluje a přidá odkaz na Azure Service Bus, se všemi jeho závislostmi.</span><span class="sxs-lookup"><span data-stu-id="0e067-163">This operation downloads, installs, and adds a reference to the Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="0e067-164">Přidejte následující **pomocí** příkazy v horní části **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="0e067-164">Add the following **using** statements at the top of the **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="0e067-165">Nakonec přidejte následující řádky, které se **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="0e067-165">Finally, add the following lines to the **Main** method.</span></span> <span data-ttu-id="0e067-166">Nahraďte připojovacího řetězce s **naslouchání** oprávnění pro fronty:</span><span class="sxs-lookup"><span data-stu-id="0e067-166">Substitute the connection string with **Listen** permissions for the queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a><span data-ttu-id="0e067-167">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="0e067-167">Run the applications</span></span>
<span data-ttu-id="0e067-168">Nyní můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e067-168">Now you are ready to run the applications.</span></span>

1. <span data-ttu-id="0e067-169">V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="0e067-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="0e067-170">Vyberte **více projektů po spuštění**, pak vyberte **spustit** jako akce pro **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **ReadCriticalQueue** projekty.</span><span class="sxs-lookup"><span data-stu-id="0e067-170">Select **Multiple startup projects**, then select **Start** as the action for the **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="0e067-171">Stiskněte klávesu **F5** zahájíte jsou tři konzole aplikace.</span><span class="sxs-lookup"><span data-stu-id="0e067-171">Press **F5** to start the three console apps.</span></span> <span data-ttu-id="0e067-172">**ReadDeviceToCloudMessages** aplikace má pouze nekritické zpráv odeslaných z **SimulatedDevice** aplikace a **ReadCriticalQueue** aplikace má pouze kritických zprávy.</span><span class="sxs-lookup"><span data-stu-id="0e067-172">The **ReadDeviceToCloudMessages** app has only non-critical messages sent from the **SimulatedDevice** application, and the **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Tři aplikace konzoly][50]

## <a name="next-steps"></a><span data-ttu-id="0e067-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0e067-174">Next steps</span></span>
<span data-ttu-id="0e067-175">V tomto kurzu jste zjistili, jak spolehlivě odesláním zprávy typu zařízení cloud pomocí funkce směrování zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="0e067-175">In this tutorial, you learned how to reliably dispatch device-to-cloud messages by using the message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="0e067-176">[Odesílání zpráv typu cloud zařízení s centrem IoT] [ lnk-c2d] ukazuje, jak k odesílání zpráv do vašeho zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="0e067-176">The [How to send cloud-to-device messages with IoT Hub][lnk-c2d] shows you how to send messages to your devices from your solution back end.</span></span>

<span data-ttu-id="0e067-177">Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="0e067-177">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="0e067-178">Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="0e067-178">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<span data-ttu-id="0e067-179">Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="0e067-179">To learn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
<span data-ttu-id="0e067-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span><span class="sxs-lookup"><span data-stu-id="0e067-180">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span></span>
<span data-ttu-id="0e067-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span><span class="sxs-lookup"><span data-stu-id="0e067-181">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span></span>
<span data-ttu-id="0e067-182">[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="0e067-182">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="0e067-183">[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="0e067-183">[Get started with IoT Hub]: iot-hub-csharp-csharp-getstarted.md</span></span>
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
<span data-ttu-id="0e067-184">[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="0e067-184">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span></span>
<span data-ttu-id="0e067-185">[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="0e067-185">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
