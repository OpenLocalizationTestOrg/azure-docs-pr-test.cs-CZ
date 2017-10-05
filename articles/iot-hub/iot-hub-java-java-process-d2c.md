---
title: "Zpracování zpráv typu zařízení cloud Azure IoT Hub (Java) | Microsoft Docs"
description: "Postupy zpracování zpráv typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body k odeslání zprávy do dalších služeb back-end."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: d1aca8f39e305105d4ec9f63fbe7bee95487e294
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a><span data-ttu-id="e3836-103">Zpracování zpráv typu zařízení cloud IoT Hub (Java)</span><span class="sxs-lookup"><span data-stu-id="e3836-103">Process IoT Hub device-to-cloud messages (Java)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="e3836-104">Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečené obousměrnou komunikaci mezi miliony zařízení a řešením back-end.</span><span class="sxs-lookup"><span data-stu-id="e3836-104">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="e3836-105">Ostatní kurzy ([Začínáme se službou IoT Hub] a [odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d]) ukazují, jak využívat základní funkce zařízení cloud a z cloudu do zařízení zasílání zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-105">Other tutorials ([Get started with IoT Hub] and [Send cloud-to-device messages with IoT Hub][lnk-c2d]) show you how to use the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span>

<span data-ttu-id="e3836-106">V tomto kurzu vychází uvedeném v kódu [Začínáme se službou IoT Hub] kurzu a ukazuje, jak pomocí směrování zpráv ke zpracování zpráv typu zařízení cloud škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="e3836-106">This tutorial builds on the code shown in the [Get started with IoT Hub] tutorial, and shows you how to use message routing to process device-to-cloud messages in a scalable way.</span></span> <span data-ttu-id="e3836-107">Tento kurz ukazuje, jak ke zpracování zpráv, které vyžadují okamžitou akci z back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="e3836-107">The tutorial illustrates how to process messages that require immediate action from the solution back end.</span></span> <span data-ttu-id="e3836-108">Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM.</span><span class="sxs-lookup"><span data-stu-id="e3836-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="e3836-109">Naopak datového bodu zprávy jednoduše informačního kanálu do modul analytics.</span><span class="sxs-lookup"><span data-stu-id="e3836-109">By contrast, data-point messages simply feed into an analytics engine.</span></span> <span data-ttu-id="e3836-110">Například teplotní telemetrie ze zařízení, které je nutné uložit pro pozdější analýzu je zpráva datového bodu.</span><span class="sxs-lookup"><span data-stu-id="e3836-110">For example, temperature telemetry from a device that is to be stored for later analysis is a data-point message.</span></span>

<span data-ttu-id="e3836-111">Na konci tohoto kurzu můžete spustit tři aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="e3836-111">At the end of this tutorial, you run three Java console apps:</span></span>

* <span data-ttu-id="e3836-112">**simulated-device**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub] kurzu posílání zpráv typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="e3836-112">**simulated-device**, a modified version of the app created in the [Get started with IoT Hub] tutorial, sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span> <span data-ttu-id="e3836-113">Tato aplikace používá protokol AMQP ke komunikaci se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-113">This app uses the AMQP protocol to communicate with IoT Hub.</span></span>
* <span data-ttu-id="e3836-114">**Read-d2c-messages** zobrazuje telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="e3836-114">**read-d2c-messages** displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="e3836-115">**čtení kritické fronty** zrušte fronty kritické zprávy z fronty Service Bus připojit ke službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-115">**read-critical-queue** de-queues the critical messages from the Service Bus queue attached to the IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="e3836-116">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e3836-116">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="e3836-117">Pokyny, jak nahradit zařízení v tomto kurzu fyzické zařízení a jak pro připojení zařízení do služby IoT Hub, najdete v článku [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="e3836-117">For instructions on how to replace the device in this tutorial with a physical device, and how to connect devices to an IoT Hub, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="e3836-118">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="e3836-118">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="e3836-119">Úplnou verzi práci [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3836-119">A complete working version of the [Get started with IoT Hub] tutorial.</span></span>
* <span data-ttu-id="e3836-120">Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="e3836-120">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="e3836-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="e3836-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="e3836-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="e3836-122">An active Azure account.</span></span> <span data-ttu-id="e3836-123">(Pokud nemáte účet, můžete vytvořit [bezplatný účet] [lnk-free-trial] si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="e3836-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="e3836-124">Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="e3836-124">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages-from-a-device-app"></a><span data-ttu-id="e3836-125">Odesílat interaktivní zprávy z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="e3836-125">Send interactive messages from a device app</span></span>
<span data-ttu-id="e3836-126">V této části upravíte zařízení aplikaci, kterou jste vytvořili v [Začínáme se službou IoT Hub] kurzu příležitostně odesílat zprávy, které vyžadují okamžitou zpracování.</span><span class="sxs-lookup"><span data-stu-id="e3836-126">In this section, you modify the device app you created in the [Get started with IoT Hub] tutorial to occasionally send messages that require immediate processing.</span></span>

1. <span data-ttu-id="e3836-127">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="e3836-127">Use a text editor to open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span> <span data-ttu-id="e3836-128">Tento soubor obsahuje kód pro **simulated-device** aplikace, které jste vytvořili v [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="e3836-128">This file contains the code for the **simulated-device** app you created in the [Get started with IoT Hub] tutorial.</span></span>

2. <span data-ttu-id="e3836-129">Nahraďte **MessageSender** třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="e3836-129">Replace the **MessageSender** class with the following code:</span></span>

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    <span data-ttu-id="e3836-130">Tato metoda náhodně přidá vlastnost `"level": "critical"` na zprávy odeslané zařízením, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="e3836-130">This method randomly adds the property `"level": "critical"` to messages sent by the device, which simulates a message that requires immediate action by the application back-end.</span></span> <span data-ttu-id="e3836-131">Aplikace předá tyto informace ve vlastnostech zpráv místo v textu zprávy, že IoT Hub může směrovat zprávy Cíl správné zprávy.</span><span class="sxs-lookup"><span data-stu-id="e3836-131">The application passes this information in the message properties, instead of in the message body, so that IoT Hub can route the message to the proper message destination.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e3836-132">Vlastnosti zprávy pro směrování zpráv můžete použít pro různé scénáře, včetně studený cesty při zpracování, kromě zde ukazuje příklad aktivní trase.</span><span class="sxs-lookup"><span data-stu-id="e3836-132">You can use message properties to route messages for various scenarios including cold-path processing, in addition to the hot path example shown here.</span></span>

2. <span data-ttu-id="e3836-133">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="e3836-133">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e3836-134">Z důvodu zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="e3836-134">For the sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="e3836-135">V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="e3836-135">In production code, you should implement a retry policy such as exponential backoff, as suggested in the MSDN article [Transient Fault Handling].</span></span>

3. <span data-ttu-id="e3836-136">Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3836-136">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a><span data-ttu-id="e3836-137">Přidat do fronty pro IoT hub a směrování zpráv do ní</span><span class="sxs-lookup"><span data-stu-id="e3836-137">Add a queue to your IoT hub and route messages to it</span></span>

<span data-ttu-id="e3836-138">V této části Vytvoření fronty Service Bus, připojte ho do služby IoT hub a konfigurace služby IoT hub odesílat zprávy do fronty na základě přítomnosti vlastnosti na zprávu.</span><span class="sxs-lookup"><span data-stu-id="e3836-138">In this section, you create a Service Bus queue, connect it to your IoT hub, and configure your IoT hub to send messages to the queue based on the presence of a property on the message.</span></span> <span data-ttu-id="e3836-139">Další informace o tom, jak zpracování zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="e3836-139">For more information about how to process messages from Service Bus queues, see [Get started with queues][lnk-sb-queues-java].</span></span>

1. <span data-ttu-id="e3836-140">Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="e3836-140">Create a Service Bus queue as described in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="e3836-141">Poznamenejte si název oboru názvů a fronty.</span><span class="sxs-lookup"><span data-stu-id="e3836-141">Make a note of the namespace and queue name.</span></span>

2. <span data-ttu-id="e3836-142">Na portálu Azure otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="e3836-142">In the Azure portal, open your IoT hub and click **Endpoints**.</span></span>

    ![Koncové body v centru IoT][30]

3. <span data-ttu-id="e3836-144">V **koncové body** okně klikněte na tlačítko **přidat** v horní části přidat fronty do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-144">In the **Endpoints** blade, click **Add** at the top to add your queue to your IoT hub.</span></span> <span data-ttu-id="e3836-145">Název koncového bodu **CriticalQueue** a použijte rozevírací seznamy a vyberte **fronty Service Bus**, oboru názvů Service Bus, ve které je umístěn fronty a názvu fronty.</span><span class="sxs-lookup"><span data-stu-id="e3836-145">Name the endpoint **CriticalQueue** and use the drop-downs to select **Service Bus queue**, the Service Bus namespace in which your queue resides, and the name of your queue.</span></span> <span data-ttu-id="e3836-146">Až budete hotovi, klikněte na tlačítko **Uložit** dole.</span><span class="sxs-lookup"><span data-stu-id="e3836-146">When you are done, click **Save** at the bottom.</span></span>

    ![Přidání koncového bodu][31]

4. <span data-ttu-id="e3836-148">Klikněte na tlačítko **trasy** ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-148">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="e3836-149">Klikněte na tlačítko **přidat** v horní části okna Vytvořit pravidlo směrování pro směrování zpráv do fronty jste právě přidali.</span><span class="sxs-lookup"><span data-stu-id="e3836-149">Click **Add** at the top of the blade to create a routing rule that routes messages to the queue you just added.</span></span> <span data-ttu-id="e3836-150">Vyberte **DeviceTelemetry** jako zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="e3836-150">Select **DeviceTelemetry** as the source of data.</span></span> <span data-ttu-id="e3836-151">Zadejte `level="critical"` jako podmínka a vyberte fronty, který jste právě přidali jako vlastní koncový bod jako koncový bod směrování pravidlo.</span><span class="sxs-lookup"><span data-stu-id="e3836-151">Enter `level="critical"` as the condition, and choose the queue you just added as a custom endpoint as the routing rule endpoint.</span></span> <span data-ttu-id="e3836-152">Až budete hotovi, klikněte na tlačítko **Uložit** dole.</span><span class="sxs-lookup"><span data-stu-id="e3836-152">When you are done, click **Save** at the bottom.</span></span>

    ![Přidání trasu][32]

    <span data-ttu-id="e3836-154">Ujistěte se, že záložní cesta je nastavena na **ON**.</span><span class="sxs-lookup"><span data-stu-id="e3836-154">Make sure the fallback route is set to **ON**.</span></span> <span data-ttu-id="e3836-155">Toto nastavení je výchozí konfigurace služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-155">This setting is the default configuration of an IoT hub.</span></span>

    ![Záložní trasy][33]

## <a name="optional-read-from-the-queue-endpoint"></a><span data-ttu-id="e3836-157">(Volitelné) Čtení z fronty koncového bodu</span><span class="sxs-lookup"><span data-stu-id="e3836-157">(Optional) Read from the queue endpoint</span></span>

<span data-ttu-id="e3836-158">Volitelně můžete čtení zpráv z fronty koncového bodu podle pokynů v [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="e3836-158">You can optionally read the messages from the queue endpoint by following the instructions in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="e3836-159">Název aplikace **čtení kritické fronty**.</span><span class="sxs-lookup"><span data-stu-id="e3836-159">Name the app **read-critical-queue**.</span></span>

## <a name="run-the-applications"></a><span data-ttu-id="e3836-160">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="e3836-160">Run the applications</span></span>

<span data-ttu-id="e3836-161">Nyní jste připraveni ke spuštění tři aplikací.</span><span class="sxs-lookup"><span data-stu-id="e3836-161">Now you are ready to run the three applications.</span></span>

1. <span data-ttu-id="e3836-162">Ke spuštění **read-d2c-messages** aplikace, v příkazovém řádku nebo prostředí přejděte do složky read-d2c a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3836-162">To run the **read-d2c-messages** application, in a command prompt or shell navigate to the read-d2c folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Spustit read-d2c-messages][readd2c]

2. <span data-ttu-id="e3836-164">Ke spuštění **čtení kritické fronty** aplikace, v příkazovém řádku nebo prostředí přejděte do složky pro čtení kritické fronty a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3836-164">To run the **read-critical-queue** application, in a command prompt or shell navigate to the read-critical-queue folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit pro čtení kritické zprávy][readqueue]

3. <span data-ttu-id="e3836-166">Ke spuštění **simulated-device** aplikaci, v příkazovém řádku nebo prostředí přejděte do složky simulated-device a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e3836-166">To run the **simulated-device** app, in a command prompt or shell navigate to the simulated-device folder and execute the following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit simulated-device][simulateddevice]

## <a name="next-steps"></a><span data-ttu-id="e3836-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e3836-168">Next steps</span></span>

<span data-ttu-id="e3836-169">V tomto kurzu jste zjistili, jak spolehlivě odesláním zprávy typu zařízení cloud pomocí funkce směrování zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="e3836-169">In this tutorial, you learned how to reliably dispatch device-to-cloud messages by using the message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="e3836-170">[Odesílání zpráv typu cloud zařízení s centrem IoT] [ lnk-c2d] ukazuje, jak k odesílání zpráv do vašeho zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="e3836-170">The [How to send cloud-to-device messages with IoT Hub][lnk-c2d] shows you how to send messages to your devices from your solution back end.</span></span>

<span data-ttu-id="e3836-171">Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="e3836-171">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="e3836-172">Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="e3836-172">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<span data-ttu-id="e3836-173">Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="e3836-173">To learn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

<span data-ttu-id="e3836-174">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span><span class="sxs-lookup"><span data-stu-id="e3836-174">[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/</span></span>
<span data-ttu-id="e3836-175">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span><span class="sxs-lookup"><span data-stu-id="e3836-175">[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/</span></span>

<span data-ttu-id="e3836-176">[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="e3836-176">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
<span data-ttu-id="e3836-177">[Začínáme se službou IoT Hub]: iot-hub-java-java-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="e3836-177">[Get started with IoT Hub]: iot-hub-java-java-getstarted.md</span></span>
<span data-ttu-id="e3836-178">[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="e3836-178">[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot</span></span>
<span data-ttu-id="e3836-179">[přechodných chyb]: https://msdn.microsoft.com/library/hh675232.aspx</span><span class="sxs-lookup"><span data-stu-id="e3836-179">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh675232.aspx</span></span>

<!-- Links -->
<span data-ttu-id="e3836-180">[Přechodná chyba zpracování]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="e3836-180">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
