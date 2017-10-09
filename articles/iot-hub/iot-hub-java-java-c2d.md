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
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>Odesílání zpráv typu cloud zařízení službou IoT Hub (Java)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení. Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze simulovaného zařízení, která odesílá zprávy typu zařízení cloud.

V tomto kurzu vychází [Začínáme se službou IoT Hub]. Jak ukazuje na:

* Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.
* Příjem zpráv typu cloud zařízení na zařízení.
* Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.

Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].

Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:

* **simulated-device**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.
* **Send-c2d-zprávy**, který odesílá aplikace simulovaného zařízení toohello zpráv typu cloud zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím SDK pro zařízení Azure IoT. Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Azure střediska pro vývojáře IoT].

toocomplete tohoto kurzu budete potřebovat hello následující:

* Dokončení pracovní verzi hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) nebo [zprávy typu zařízení cloud proces IoT Hub](iot-hub-java-java-process-d2c.md) kurzu.
* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

## <a name="receive-messages-in-hello-simulated-device-app"></a>Příjem zpráv v aplikaci simulovaného zařízení hello

V této části upravíte jste vytvořili v aplikaci simulovaného zařízení hello [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.

1. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.

2. Přidejte následující hello **MessageCallback** jako vnořené třída uvnitř hello **aplikace** třídy. Hello **provést** metoda je volána, když zařízení hello přijme zprávu ze služby IoT Hub. V tomto příkladu hello zařízení vždy upozorní hello IoT hub splnění uvítací zprávu:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Upravit hello **hlavní** metoda toocreate **AppMessageCallback** instance a volání hello **setMessageCallback** metoda před jeho otevřením hello klientské následovně:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > Pokud používáte protokol HTTP místo MQTT nebo AMQP jako hello přenosu, hello **DeviceClient** instance vyhledává zprávy ze služby IoT Hub zřídka (méně než každých 25 minut). Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].

4. toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy typu cloud zařízení

V této části vytvoříte konzolovou aplikaci Java, která odešle aplikace simulovaného zařízení toohello zprávy typu cloud zařízení. ID zařízení hello zařízení, které jste přidali v hello potřebovat hello [Začínáme se službou IoT Hub] kurzu. Můžete také potřebovat hello připojovací řetězec služby IoT Hub pro vaše centrum, které můžete najít v hello [portál Azure].

1. Vytvořte projekt Maven s názvem **send-c2d-zprávy** pomocí hello následující příkaz na příkazovém řádku. Poznámka: Tento příkaz je jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte toohello novou složku send-c2d-zprávy.

3. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce send-c2d-zprávy hello a přidejte následující závislost toohello hello **závislosti** uzlu. Přidává se závislá hello vám umožní toouse hello **iothub-java-service-client** balíček v toocommunicate vaší aplikace pomocí služby IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].

4. Uložte a zavřete soubor pom.xml hello.

5. Pomocí textového editoru otevřete soubor send-c2d-messages\src\main\java\com\mycompany\app\App.java hello.

6. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy a nahraďte **{yourhubconnectionstring}** a **{yourdeviceid}** hello hodnotami vaší uvedené výše:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Nahraďte hello **hlavní** metoda s hello následující kód. Tento kód připojí tooyour IoT hub, odešle zprávu tooyour zařízení a pak čeká na potvrzení této zprávy přijaté a zpracovány hello hello zařízení:
   
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
    > Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].


9. toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Spouštění aplikací hello

Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin odesílání telemetrie tooyour IoT hub a toolisten pro cloud zařízení zpráv odeslaných z vašeho centra hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Spusťte aplikaci simulovaného zařízení hello][img-simulated-device]

2. Na příkazovém řádku ve složce hello send-c2d-zprávy spusťte následující příkaz toosend hello zpráv typu cloud zařízení a počkejte na potvrzení zpětné vazby:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Spusťte zprávu o hello příkaz toosend hello cloudu na zařízení][img-send-command]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení. 

Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].

toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].

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