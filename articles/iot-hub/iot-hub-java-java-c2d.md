---
title: "Zprávy typu cloud zařízení s Azure IoT Hub (Java) | Microsoft Docs"
description: "Postupy pro odesílání zpráv typu cloud zařízení pro zařízení ze služby Azure IoT hub pro jazyk Java pomocí sady SDK služby Azure IoT. Můžete upravit aplikaci simulovaného zařízení příjem zpráv typu cloud zařízení a úpravě back-end aplikace k odesílání zpráv typu cloud zařízení."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: f5e3ac46f4d144b12e2ab7fcfb456665ff6cc68f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="dfe5b-104">Odesílání zpráv typu cloud zařízení službou IoT Hub (Java)</span><span class="sxs-lookup"><span data-stu-id="dfe5b-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="dfe5b-105">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="dfe5b-106">[Začínáme se službou IoT Hub] kurz ukazuje, jak k vytvoření služby IoT hub, zřídit identitu zařízení v ní a kódu aplikaci ze simulovaného zařízení, která odesílá zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-106">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="dfe5b-107">V tomto kurzu vychází [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="dfe5b-108">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-108">It shows you how to:</span></span>

* <span data-ttu-id="dfe5b-109">Z back-end vašeho řešení odesílání zpráv typu cloud zařízení na jedno zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-109">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="dfe5b-110">Příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="dfe5b-111">Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané do zařízení ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="dfe5b-112">Můžete najít další informace o zprávy typu cloud zařízení v [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-112">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="dfe5b-113">Na konci tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-113">At the end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="dfe5b-114">**simulated-device**, upravenou verzi aplikace vytvořená v [Začínáme se službou IoT Hub], který se připojuje ke službě IoT hub a přijímá zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-114">**simulated-device**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="dfe5b-115">**Send-c2d-zprávy**, který odešle zprávu cloud zařízení na aplikaci simulovaného zařízení prostřednictvím služby IoT Hub a pak obdrží jeho potvrzení o doručení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-115">**send-c2d-messages**, which sends a cloud-to-device message to the simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="dfe5b-116">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="dfe5b-117">Podrobné pokyny o tom, jak připojit zařízení ke kódu v tomto kurzu a obecně do služby Azure IoT Hub, najdete v článku [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-117">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="dfe5b-118">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-118">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="dfe5b-119">Úplnou verzi práci [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) nebo [zprávy typu zařízení cloud proces IoT Hub](iot-hub-java-java-process-d2c.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-119">A complete working version of the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="dfe5b-120">Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="dfe5b-120">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="dfe5b-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="dfe5b-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="dfe5b-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-122">An active Azure account.</span></span> <span data-ttu-id="dfe5b-123">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="dfe5b-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-simulated-device-app"></a><span data-ttu-id="dfe5b-124">Příjem zpráv v aplikaci simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="dfe5b-124">Receive messages in the simulated device app</span></span>

<span data-ttu-id="dfe5b-125">V této části upravíte aplikaci simulovaného zařízení, kterou jste vytvořili v [Začínáme se službou IoT Hub] pro příjem zpráv typu cloud zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-125">In this section, you modify the simulated device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="dfe5b-126">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-126">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="dfe5b-127">Přidejte následující **MessageCallback** jako vnořené třída uvnitř **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-127">Add the following **MessageCallback** class as a nested class inside the **App** class.</span></span> <span data-ttu-id="dfe5b-128">**Provést** metoda je volána, když zařízení obdrží zprávu ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-128">The **execute** method is invoked when the device receives a message from IoT Hub.</span></span> <span data-ttu-id="dfe5b-129">V tomto příkladu zařízení vždy upozorní služby IoT hub splnění zpráva:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-129">In this example, the device always notifies the IoT hub that it has completed the message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="dfe5b-130">Změnit **hlavní** metodu pro vytvoření **AppMessageCallback** instance a volání **setMessageCallback** metoda před jeho otevřením klienta následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-130">Modify the **main** method to create an **AppMessageCallback** instance and call the **setMessageCallback** method before it opens the client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="dfe5b-131">Pokud použijete protokol HTTP místo MQTT nebo AMQP jako přenos, **DeviceClient** instance vyhledává zprávy ze služby IoT Hub zřídka (méně než každých 25 minut).</span><span class="sxs-lookup"><span data-stu-id="dfe5b-131">If you use HTTP instead of MQTT or AMQP as the transport, the **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="dfe5b-132">Další informace o rozdílech mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-132">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="dfe5b-133">Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-133">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="dfe5b-134">Odeslání zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="dfe5b-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="dfe5b-135">V této části vytvoříte konzolovou aplikaci Java, která posílání zpráv typu cloud zařízení do aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-135">In this section, you create a Java console app that sends cloud-to-device messages to the simulated device app.</span></span> <span data-ttu-id="dfe5b-136">Je třeba ID zařízení v zařízení, které jste přidali v [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-136">You need the device ID of the device you added in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="dfe5b-137">Připojovací řetězec služby IoT Hub pro vaše centrum, které můžete najít v musíte taky [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-137">You also need the IoT Hub connection string for your hub that you can find in the [Azure portal].</span></span>

1. <span data-ttu-id="dfe5b-138">Vytvořte projekt Maven s názvem **send-c2d-zprávy** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-138">Create a Maven project called **send-c2d-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="dfe5b-139">Poznámka: Tento příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="dfe5b-140">Na příkazovém řádku přejděte do nové složky send-c2d-zprávy.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-140">At your command prompt, navigate to the new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="dfe5b-141">Pomocí textového editoru otevřete soubor pom.xml ve složce send-c2d-zprávy a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-141">Using a text editor, open the pom.xml file in the send-c2d-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="dfe5b-142">Přidání závislost umožňuje používat **iothub-java-service-client** balíček ve vaší aplikaci ke komunikaci s vaší služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-142">Adding the dependency enables you to use the **iothub-java-service-client** package in your application to communicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="dfe5b-143">Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-143">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="dfe5b-144">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-144">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="dfe5b-145">Pomocí textového editoru otevřete soubor send-c2d-messages\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-145">Using a text editor, open the send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="dfe5b-146">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-146">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="dfe5b-147">Přidejte následující proměnné na úrovni třídy k **aplikace** třídy a nahraďte **{yourhubconnectionstring}** a **{yourdeviceid}** s hodnotami jste si dříve poznamenali:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-147">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with the values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="dfe5b-148">Nahraďte **hlavní** metoda následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-148">Replace the **main** method with the following code.</span></span> <span data-ttu-id="dfe5b-149">Tento kód se připojí ke službě IoT hub, odešle zprávu do zařízení a pak čeká na potvrzení, že zařízení přijme a zpracuje zprávy:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-149">This code connects to your IoT hub, sends a message to your device, and then waits for an acknowledgment that the device received and processed the message:</span></span>
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="dfe5b-150">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="dfe5b-151">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="dfe5b-152">Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-152">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a><span data-ttu-id="dfe5b-153">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="dfe5b-153">Run the applications</span></span>

<span data-ttu-id="dfe5b-154">Nyní můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-154">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="dfe5b-155">Na příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby se začala odesílat telemetrická data do služby IoT hub a přijímat zprávy typu cloud zařízení odeslané z rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-155">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry to your IoT hub and to listen for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Spusťte aplikaci simulovaného zařízení][img-simulated-device]

2. <span data-ttu-id="dfe5b-157">Na příkazovém řádku ve složce send-c2d-zprávy spusťte následující příkaz k odeslání zprávy typu cloud zařízení a počkat na potvrzení o názor:</span><span class="sxs-lookup"><span data-stu-id="dfe5b-157">At a command prompt in the send-c2d-messages folder, run the following command to send a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spusťte příkaz k odeslání zprávy typu cloud zařízení][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="dfe5b-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dfe5b-159">Next steps</span></span>

<span data-ttu-id="dfe5b-160">V tomto kurzu jste zjistili, jak odesílat a přijímat zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="dfe5b-160">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="dfe5b-161">Příklady dokončení začátku do konce řešení, které pomocí služby IoT Hub, najdete v sekci [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-161">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="dfe5b-162">Další informace o vývoji řešení službou IoT Hub, najdete v článku [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="dfe5b-162">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

<span data-ttu-id="dfe5b-163">[Začínáme se službou IoT Hub]: iot-hub-java-java-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="dfe5b-163">[Get started with IoT Hub]: iot-hub-java-java-getstarted.md</span></span>
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
<span data-ttu-id="dfe5b-164">[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="dfe5b-164">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="dfe5b-165">[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="dfe5b-165">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
<span data-ttu-id="dfe5b-166">[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="dfe5b-166">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="dfe5b-167">[portál Azure]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="dfe5b-167">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="dfe5b-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span><span class="sxs-lookup"><span data-stu-id="dfe5b-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span></span>
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22