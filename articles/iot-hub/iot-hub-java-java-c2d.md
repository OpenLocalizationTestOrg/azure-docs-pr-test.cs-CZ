---
title: "zprávy aaaCloud zařízení s Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak toosend cloud zařízení zpráv tooa zařízení ze služby Azure IoT hub pro jazyk Java pomocí sady SDK služby Azure IoT hello. Upravte zprávy typu cloud zařízení tooreceive aplikace simulovaného zařízení a změny zpráv back-end aplikace hello toosend cloud zařízení."
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
ms.openlocfilehash: 8721f18428c849ed9a04aa2e45c65605c3e38101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="28024-104">Odesílání zpráv typu cloud zařízení službou IoT Hub (Java)</span><span class="sxs-lookup"><span data-stu-id="28024-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="28024-105">Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení.</span><span class="sxs-lookup"><span data-stu-id="28024-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="28024-106">Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze simulovaného zařízení, která odesílá zprávy typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="28024-106">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="28024-107">V tomto kurzu vychází [Začínáme se službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="28024-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="28024-108">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="28024-108">It shows you how to:</span></span>

* <span data-ttu-id="28024-109">Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="28024-109">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="28024-110">Příjem zpráv typu cloud zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="28024-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="28024-111">Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="28024-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="28024-112">Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="28024-112">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="28024-113">Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="28024-113">At hello end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="28024-114">**simulated-device**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="28024-114">**simulated-device**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="28024-115">**Send-c2d-zprávy**, který odesílá aplikace simulovaného zařízení toohello zpráv typu cloud zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.</span><span class="sxs-lookup"><span data-stu-id="28024-115">**send-c2d-messages**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="28024-116">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="28024-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="28024-117">Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="28024-117">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="28024-118">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="28024-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="28024-119">Dokončení pracovní verzi hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) nebo [zprávy typu zařízení cloud proces IoT Hub](iot-hub-java-java-process-d2c.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="28024-119">A complete working version of hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="28024-120">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="28024-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="28024-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="28024-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="28024-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="28024-122">An active Azure account.</span></span> <span data-ttu-id="28024-123">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="28024-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="28024-124">Příjem zpráv v aplikaci simulovaného zařízení hello</span><span class="sxs-lookup"><span data-stu-id="28024-124">Receive messages in hello simulated device app</span></span>

<span data-ttu-id="28024-125">V této části upravíte jste vytvořili v aplikaci simulovaného zařízení hello [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="28024-125">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="28024-126">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="28024-126">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="28024-127">Přidejte následující hello **MessageCallback** jako vnořené třída uvnitř hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="28024-127">Add hello following **MessageCallback** class as a nested class inside hello **App** class.</span></span> <span data-ttu-id="28024-128">Hello **provést** metoda je volána, když zařízení hello přijme zprávu ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="28024-128">hello **execute** method is invoked when hello device receives a message from IoT Hub.</span></span> <span data-ttu-id="28024-129">V tomto příkladu hello zařízení vždy upozorní hello IoT hub splnění uvítací zprávu:</span><span class="sxs-lookup"><span data-stu-id="28024-129">In this example, hello device always notifies hello IoT hub that it has completed hello message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="28024-130">Upravit hello **hlavní** metoda toocreate **AppMessageCallback** instance a volání hello **setMessageCallback** metoda před jeho otevřením hello klientské následovně:</span><span class="sxs-lookup"><span data-stu-id="28024-130">Modify hello **main** method toocreate an **AppMessageCallback** instance and call hello **setMessageCallback** method before it opens hello client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="28024-131">Pokud používáte protokol HTTP místo MQTT nebo AMQP jako hello přenosu, hello **DeviceClient** instance vyhledává zprávy ze služby IoT Hub zřídka (méně než každých 25 minut).</span><span class="sxs-lookup"><span data-stu-id="28024-131">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="28024-132">Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="28024-132">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="28024-133">toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:</span><span class="sxs-lookup"><span data-stu-id="28024-133">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="28024-134">Odeslání zprávy typu cloud zařízení</span><span class="sxs-lookup"><span data-stu-id="28024-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="28024-135">V této části vytvoříte konzolovou aplikaci Java, která odešle aplikace simulovaného zařízení toohello zprávy typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="28024-135">In this section, you create a Java console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="28024-136">ID zařízení hello zařízení, které jste přidali v hello potřebovat hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="28024-136">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="28024-137">Můžete také potřebovat hello připojovací řetězec služby IoT Hub pro vaše centrum, které můžete najít v hello [portál Azure].</span><span class="sxs-lookup"><span data-stu-id="28024-137">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="28024-138">Vytvořte projekt Maven s názvem **send-c2d-zprávy** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="28024-138">Create a Maven project called **send-c2d-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="28024-139">Poznámka: Tento příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="28024-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="28024-140">Na příkazovém řádku přejděte toohello novou složku send-c2d-zprávy.</span><span class="sxs-lookup"><span data-stu-id="28024-140">At your command prompt, navigate toohello new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="28024-141">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce send-c2d-zprávy hello a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="28024-141">Using a text editor, open hello pom.xml file in hello send-c2d-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="28024-142">Přidává se závislá hello vám umožní toouse hello **iothub-java-service-client** balíček v toocommunicate vaší aplikace pomocí služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="28024-142">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="28024-143">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="28024-143">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="28024-144">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="28024-144">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="28024-145">Pomocí textového editoru otevřete soubor send-c2d-messages\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="28024-145">Using a text editor, open hello send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="28024-146">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="28024-146">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="28024-147">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy a nahraďte **{yourhubconnectionstring}** a **{yourdeviceid}** hello hodnotami vaší uvedené výše:</span><span class="sxs-lookup"><span data-stu-id="28024-147">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with hello values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="28024-148">Nahraďte hello **hlavní** metoda s hello následující kód.</span><span class="sxs-lookup"><span data-stu-id="28024-148">Replace hello **main** method with hello following code.</span></span> <span data-ttu-id="28024-149">Tento kód připojí tooyour IoT hub, odešle zprávu tooyour zařízení a pak čeká na potvrzení této zprávy přijaté a zpracovány hello hello zařízení:</span><span class="sxs-lookup"><span data-stu-id="28024-149">This code connects tooyour IoT hub, sends a message tooyour device, and then waits for an acknowledgment that hello device received and processed hello message:</span></span>
   
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
   
        Message messageToSend = new Message("Cloud toodevice message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent toodevice");
   
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
    > <span data-ttu-id="28024-150">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="28024-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="28024-151">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="28024-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="28024-152">toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:</span><span class="sxs-lookup"><span data-stu-id="28024-152">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="28024-153">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="28024-153">Run hello applications</span></span>

<span data-ttu-id="28024-154">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="28024-154">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="28024-155">Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin odesílání telemetrie tooyour IoT hub a toolisten pro cloud zařízení zpráv odeslaných z vašeho centra hello:</span><span class="sxs-lookup"><span data-stu-id="28024-155">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry tooyour IoT hub and toolisten for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Spusťte aplikaci simulovaného zařízení hello][img-simulated-device]

2. <span data-ttu-id="28024-157">Na příkazovém řádku ve složce hello send-c2d-zprávy spusťte následující příkaz toosend hello zpráv typu cloud zařízení a počkejte na potvrzení zpětné vazby:</span><span class="sxs-lookup"><span data-stu-id="28024-157">At a command prompt in hello send-c2d-messages folder, run hello following command toosend a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spusťte zprávu o hello příkaz toosend hello cloudu na zařízení][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="28024-159">Další kroky</span><span class="sxs-lookup"><span data-stu-id="28024-159">Next steps</span></span>

<span data-ttu-id="28024-160">V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení.</span><span class="sxs-lookup"><span data-stu-id="28024-160">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="28024-161">Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="28024-161">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="28024-162">toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="28024-162">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Začínáme se službou IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[portál Azure]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22