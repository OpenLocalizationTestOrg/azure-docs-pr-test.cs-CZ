---
title: "použití tras (.Net) zpráv typu zařízení cloud aaaProcess Azure IoT Hub | Microsoft Docs"
description: "Jak tooprocess zprávy typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body toodispatch zpráv tooother back endové služby."
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
ms.openlocfilehash: c1dd5be04ca30c65af2be466ba6c8c1858333154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a><span data-ttu-id="36c64-103">Zpracování zpráv typu zařízení cloud IoT Hub pomocí tras (.NET)</span><span class="sxs-lookup"><span data-stu-id="36c64-103">Process IoT Hub device-to-cloud messages using routes (.NET)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="36c64-104">V tomto kurzu vychází hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="36c64-104">This tutorial builds on hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="36c64-105">kurz Hello:</span><span class="sxs-lookup"><span data-stu-id="36c64-105">hello tutorial:</span></span>

* <span data-ttu-id="36c64-106">Ukazuje, jak směrování toouse pravidla zpráv typu zařízení cloud toodispatch způsobem snadno, založené na konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="36c64-106">Shows you how toouse routing rules toodispatch device-to-cloud messages in an easy, configuration-based way.</span></span>
* <span data-ttu-id="36c64-107">Ukazuje, jak tooisolate interaktivní zprávy, které vyžadují okamžitou akci z řešení hello back-end pro další zpracování.</span><span class="sxs-lookup"><span data-stu-id="36c64-107">Illustrates how tooisolate interactive messages that require immediate action from hello solution back end for further processing.</span></span> <span data-ttu-id="36c64-108">Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM.</span><span class="sxs-lookup"><span data-stu-id="36c64-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="36c64-109">Naproti tomu datového bodu zprávy, jako je například teplotní telemetrie informačního kanálu do modul analytics.</span><span class="sxs-lookup"><span data-stu-id="36c64-109">In contrast, data-point messages, such as temperature telemetry, feed into an analytics engine.</span></span>

<span data-ttu-id="36c64-110">Na konci hello tohoto kurzu můžete spustit tři aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="36c64-110">At hello end of this tutorial, you run three .NET console apps:</span></span>

* <span data-ttu-id="36c64-111">**SimulatedDevice**, upravenou verzi hello aplikace vytvořená v hello [Začínáme se službou IoT Hub] kurzu se odešle zprávy typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="36c64-111">**SimulatedDevice**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span>
* <span data-ttu-id="36c64-112">**ReadDeviceToCloudMessages** , zobrazí hello nekritické telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="36c64-112">**ReadDeviceToCloudMessages** that displays hello non-critical telemetry sent by your device app.</span></span>
* <span data-ttu-id="36c64-113">**ReadCriticalQueue** zrušte fronty hello kritické zprávy odeslané aplikace zařízení z fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="36c64-113">**ReadCriticalQueue** de-queues hello critical messages sent by your device app from a Service Bus queue.</span></span> <span data-ttu-id="36c64-114">Tato fronta je připojený toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-114">This queue is attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="36c64-115">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="36c64-115">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="36c64-116">toolearn způsobu tooreplace hello simulované zařízení v tomto kurzu se fyzické zařízení, najdete v hello [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="36c64-116">toolearn how tooreplace hello simulated device in this tutorial with a physical device, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="36c64-117">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="36c64-117">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="36c64-118">Visual Studio 2015 nebo Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="36c64-118">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="36c64-119">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="36c64-119">An active Azure account.</span></span> <br/><span data-ttu-id="36c64-120">Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.</span><span class="sxs-lookup"><span data-stu-id="36c64-120">If you don't have an account, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

<span data-ttu-id="36c64-121">Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="36c64-121">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages"></a><span data-ttu-id="36c64-122">Odesílat interaktivní zprávy</span><span class="sxs-lookup"><span data-stu-id="36c64-122">Send interactive messages</span></span>

<span data-ttu-id="36c64-123">Upravit hello zařízení aplikaci, kterou jste vytvořili v hello [Začínáme se službou IoT Hub] kurz toooccasionally odesílat interaktivní zprávy.</span><span class="sxs-lookup"><span data-stu-id="36c64-123">Modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send interactive messages.</span></span>

<span data-ttu-id="36c64-124">V sadě Visual Studio v hello **SimulatedDevice** projektu, nahraďte hello `SendDeviceToCloudMessagesAsync` metoda s hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="36c64-124">In Visual Studio, in hello **SimulatedDevice** project, replace hello `SendDeviceToCloudMessagesAsync` method with hello following code:</span></span>

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

<span data-ttu-id="36c64-125">Tato metoda náhodně přidá vlastnost hello `"level": "critical"` toomessages poslal hello zařízení, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end řešení hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-125">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello solution back-end.</span></span> <span data-ttu-id="36c64-126">aplikace Hello zařízení předá tyto informace ve vlastnostech zprávy hello místo v textu zprávy hello tak této služby IoT Hub může směrovat hello zpráva toohello správné zpráva cílový.</span><span class="sxs-lookup"><span data-stu-id="36c64-126">hello device app passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>

> [!NOTE]
> <span data-ttu-id="36c64-127">Zpráv vlastnosti tooroute zprávy můžete použít pro různé scénáře, včetně studený path zpracování, kromě příklad horkou cesty toohello zobrazeny zde.</span><span class="sxs-lookup"><span data-stu-id="36c64-127">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot-path example shown here.</span></span>

> [!NOTE]
> <span data-ttu-id="36c64-128">Pro hello zájmu jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="36c64-128">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="36c64-129">V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="36c64-129">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a><span data-ttu-id="36c64-130">Směrování zprávy tooa fronty ve službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="36c64-130">Route messages tooa queue in your IoT hub</span></span>

<span data-ttu-id="36c64-131">V této části:</span><span class="sxs-lookup"><span data-stu-id="36c64-131">In this section, you:</span></span>

* <span data-ttu-id="36c64-132">Vytvoření fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="36c64-132">Create a Service Bus queue.</span></span>
* <span data-ttu-id="36c64-133">Připojí tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-133">Connect it tooyour IoT hub.</span></span>
* <span data-ttu-id="36c64-134">Konfigurace vaší IoT hub toosend zprávy toohello frontu na základě přítomnosti hello vlastnosti na uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="36c64-134">Configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span>

<span data-ttu-id="36c64-135">Další informace o tom, jak tooprocess zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="36c64-135">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][Service Bus queue].</span></span>

1. <span data-ttu-id="36c64-136">Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][Service Bus queue].</span><span class="sxs-lookup"><span data-stu-id="36c64-136">Create a Service Bus queue as described in [Get started with queues][Service Bus queue].</span></span> <span data-ttu-id="36c64-137">Hello fronty musí být v hello stejném předplatném, oblasti jako službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-137">hello queue must be in hello same subscription and region as your IoT hub.</span></span> <span data-ttu-id="36c64-138">Poznamenejte si název oboru názvů a fronty hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-138">Make a note of hello namespace and queue name.</span></span>

    > [!NOTE]
    > <span data-ttu-id="36c64-139">Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena.</span><span class="sxs-lookup"><span data-stu-id="36c64-139">Service Bus queues and topics used as IoT Hub endpoints must not have **Sessions** or **Duplicate Detection** enabled.</span></span> <span data-ttu-id="36c64-140">Pokud některá z těchto možností jsou povolené, hello koncový bod se zobrazí jako **Unreachable** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="36c64-140">If either of those options are enabled, hello endpoint appears as **Unreachable** in hello Azure portal.</span></span>

2. <span data-ttu-id="36c64-141">V hello portálu Azure, otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="36c64-141">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>
    
    ![Koncové body v centru IoT][30]

3. <span data-ttu-id="36c64-143">V hello **koncové body** okně klikněte na tlačítko **přidat** na hello nejvyšší tooadd fronty tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-143">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="36c64-144">Koncový bod hello název **CriticalQueue** a použijte rozevírací seznamy tooselect hello **fronty Service Bus**hello oboru názvů Service Bus, ve které je umístěn fronty a hello název fronty.</span><span class="sxs-lookup"><span data-stu-id="36c64-144">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="36c64-145">Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-145">When you are done, click **Save** at hello bottom.</span></span>
    
    ![Přidání koncového bodu][31]
    
4. <span data-ttu-id="36c64-147">Klikněte na tlačítko **trasy** ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-147">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="36c64-148">Klikněte na tlačítko **přidat** hello horní části toocreate hello okno Přidat pravidlo směrování, který směruje zprávy toohello fronty je právě.</span><span class="sxs-lookup"><span data-stu-id="36c64-148">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="36c64-149">Vyberte **DeviceTelemetry** jako zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-149">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="36c64-150">Zadejte `level="critical"` jako hello podmínka a zvolte hello fronty, které jste právě přidali jako vlastní koncový bod jako hello směrování pravidlo koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="36c64-150">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="36c64-151">Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-151">When you are done, click **Save** at hello bottom.</span></span>
    
    ![Přidání trasu][32]
    
    <span data-ttu-id="36c64-153">Zajistěte, aby záložní trasy hello je nastaven příliš**ON**.</span><span class="sxs-lookup"><span data-stu-id="36c64-153">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="36c64-154">Tato hodnota je hello výchozí konfiguraci pro Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="36c64-154">This value is hello default configuration for an IoT hub.</span></span>
    
    ![Záložní trasy][33]

## <a name="read-from-hello-queue-endpoint"></a><span data-ttu-id="36c64-156">Čtení z fronty hello koncového bodu</span><span class="sxs-lookup"><span data-stu-id="36c64-156">Read from hello queue endpoint</span></span>

<span data-ttu-id="36c64-157">V této části číst hello zprávy z fronty hello koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="36c64-157">In this section, you read hello messages from hello queue endpoint.</span></span>

1. <span data-ttu-id="36c64-158">V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="36c64-158">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution, by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="36c64-159">Název projektu hello **ReadCriticalQueue**.</span><span class="sxs-lookup"><span data-stu-id="36c64-159">Name hello project **ReadCriticalQueue**.</span></span>

2. <span data-ttu-id="36c64-160">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadCriticalQueue** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="36c64-160">In Solution Explorer, right-click hello **ReadCriticalQueue** project, and then click **Manage NuGet Packages**.</span></span> <span data-ttu-id="36c64-161">Tato operace zobrazí hello **Správce balíčků NuGet** okno.</span><span class="sxs-lookup"><span data-stu-id="36c64-161">This operation displays hello **NuGet Package Manager** window.</span></span>

3. <span data-ttu-id="36c64-162">Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="36c64-162">Search for **WindowsAzure.ServiceBus**, click **Install**, and accept hello terms of use.</span></span> <span data-ttu-id="36c64-163">Tato operace stáhne, nainstaluje a přidá odkaz toohello Azure Service Bus se všemi jeho závislostmi.</span><span class="sxs-lookup"><span data-stu-id="36c64-163">This operation downloads, installs, and adds a reference toohello Azure Service Bus, with all its dependencies.</span></span>

4. <span data-ttu-id="36c64-164">Přidejte následující hello **pomocí** příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="36c64-164">Add hello following **using** statements at hello top of hello **Program.cs** file:</span></span>
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. <span data-ttu-id="36c64-165">Nakonec přidejte následující řádky toohello hello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="36c64-165">Finally, add hello following lines toohello **Main** method.</span></span> <span data-ttu-id="36c64-166">Nahraďte hello připojovací řetězec s **naslouchání** oprávnění pro frontu hello:</span><span class="sxs-lookup"><span data-stu-id="36c64-166">Substitute hello connection string with **Listen** permissions for hello queue:</span></span>
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C tooexit.\n");
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

## <a name="run-hello-applications"></a><span data-ttu-id="36c64-167">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="36c64-167">Run hello applications</span></span>
<span data-ttu-id="36c64-168">Teď je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="36c64-168">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="36c64-169">V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="36c64-169">In Visual Studio, in Solution Explorer, right-click your solution and select **Set StartUp Projects**.</span></span> <span data-ttu-id="36c64-170">Vyberte **více projektů po spuštění**, pak vyberte **spustit** jako hello akce pro hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **ReadCriticalQueue** projekty.</span><span class="sxs-lookup"><span data-stu-id="36c64-170">Select **Multiple startup projects**, then select **Start** as hello action for hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, and **ReadCriticalQueue** projects.</span></span>
2. <span data-ttu-id="36c64-171">Stiskněte klávesu **F5** toostart hello tři aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="36c64-171">Press **F5** toostart hello three console apps.</span></span> <span data-ttu-id="36c64-172">Hello **ReadDeviceToCloudMessages** aplikace má pouze nekritické zpráv odeslaných z hello **SimulatedDevice** aplikace a hello **ReadCriticalQueue** aplikace má pouze kritické zprávy.</span><span class="sxs-lookup"><span data-stu-id="36c64-172">hello **ReadDeviceToCloudMessages** app has only non-critical messages sent from hello **SimulatedDevice** application, and hello **ReadCriticalQueue** app has only critical messages.</span></span>
   
   ![Tři aplikace konzoly][50]

## <a name="next-steps"></a><span data-ttu-id="36c64-174">Další kroky</span><span class="sxs-lookup"><span data-stu-id="36c64-174">Next steps</span></span>
<span data-ttu-id="36c64-175">V tomto kurzu jste se dozvěděli, jak tooreliably odesílání zpráv typu zařízení cloud pomocí hello funkci směrování zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="36c64-175">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="36c64-176">Hello [jak toosend cloud zařízení zpráv službou IoT Hub] [ lnk-c2d] ukazuje, jak toosend zprávy tooyour zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="36c64-176">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="36c64-177">Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="36c64-177">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="36c64-178">toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="36c64-178">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="36c64-179">toolearn Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="36c64-179">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
