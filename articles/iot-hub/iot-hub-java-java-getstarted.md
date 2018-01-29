---
title: "Začínáme se službou Azure IoT Hub (Java) | Dokumentace Microsoftu"
description: "Zjistěte, jak odesílat zprávy typu zařízení-cloud do služby Azure IoT Hub pomocí sad IoT SDK pro Javu. Vytvořte simulované zařízení a aplikace služeb pro registraci vašeho zařízení, odesílání zpráv a čtení zpráv ze služby IoT Hub."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f41dd62d3f074eebe0c996a9c14bc7811d5b365
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a>Připojení zařízení ke službě IoT Hub pomocí Javy
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci tohoto kurzu budete mít tři konzolové aplikace Java:

* **create-device-identity** vytváří identitu zařízení a přidružený klíč zabezpečení k připojení aplikace pro zařízení.
* **read-d2c-messages** zobrazuje telemetrická data odesílaná aplikací pro zařízení.
* **simulated-device** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.

> [!NOTE]
> Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].

Pro absolvování tohoto kurzu potřebujete:

* Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Na závěr si poznamenejte hodnotu **Primární klíč**. Potom klikněte na **Koncové body** a na integrovaný koncový bod **Události**. V okně **Vlastnosti** si poznamenejte **název kompatibilní s centrem událostí** a adresu **koncového bodu kompatibilního s centrem událostí**. Tyto tři hodnoty budete potřebovat k vytvoření aplikace **read-d2c-messages**.

![Okno Zasílání zpráv služby IoT Hub na webu Azure Portal][6]

Nyní jste vytvořili svůj IoT Hub. Máte název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub, primární klíč služby IoT Hub a název a koncový bod kompatibilní s centrem událostí, které potřebujete k dokončení tohoto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci Java, která v registru identit ve službě IoT Hub vytvoří identitu zařízení. Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit. Další informace najdete v části **Registr identit** v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity]. Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. 

1. Vytvořte prázdnou složku s názvem iot-java-get-started. Ve složce iot-java-get-started vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **create-device-identity**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do složky create-device-identity.

3. Pomocí textového editoru otevřete ve složce create-device-identity soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. Tato závislost vám umožní použít v aplikaci balíček iot-service-client:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].

4. Soubor pom.xml uložte a zavřete.

5. Pomocí textového editoru otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Do třídy **App** přidejte následující proměnné na úrovni třídy a nahraďte zástupné symboly **{yourhubconnectionstring}** hodnotou, kterou jste si předtím poznamenali:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Upravte podpis metody **Main** tak, aby zahrnoval následující výjimky:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Přidejte následující kód jako obsah metody **Main**. Tento kód vytvoří v registru identit služby IoT Hub zařízení s názvem *javadevice* (pokud zatím neexistuje). Potom zobrazí ID a klíč zařízení, které budete později potřebovat:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Soubor App.java uložte a zavřete.

11. Aplikaci **create-device-identity** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Aplikaci **create-device-identity** pomocí nástroje Maven spustíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Poznamenejte si **ID zařízení** a **Klíč zařízení**. Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.

> [!NOTE]
> V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub. Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením. Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub. Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hub][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud. Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení. Způsoby zpracování zpráv typu zařízení-cloud v různých škálách najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial]. Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **read-d2c-messages**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do složky read-d2c-messages.

3. Pomocí textového editoru otevřete ve složce read-d2c-messages soubor pom.xml a k uzlu **závislosti** přidejte následující závislost. Tato závislost umožňuje čtení z koncového bodu kompatibilního s centrem událostí pomocí balíčku eventhubs-client ve vaší aplikaci:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.15.0</version> 
    </dependency>
    ```

4. Soubor pom.xml uložte a zavřete.

5. Pomocí textového editoru otevřete soubor  read-d2c-messages\src\main\java\com\mycompany\app\App.java.

6. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Do třídy **App** přidejte následující proměnnou na úrovni třídy. Zástupné symboly **{youriothubkey}**, **{youreventhubcompatibleendpoint}** a **{youreventhubcompatiblename}** nahraďte hodnotami, které jste si předtím poznamenali:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Do třídy **App** přidejte následující metodu **receiveMessages**. Tato metoda vytvoří instanci **EventHubClient** k připojení ke koncovému bodu kompatibilnímu s centrem událostí a poté asynchronně vytvoří instanci **PartitionReceiver** ke čtení z oddílu centra událostí. Až do ukončení aplikace se bude nepřetržitě provádět v cyklu a vypisovat podrobnosti o zprávách.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some events");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Tato metoda při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění. Tato metoda je užitečná v testovacím prostředí, protože uvidíte aktuální sadu zpráv. V produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].

9. Upravte podpis metody **Main** tak, aby zahrnoval následující výjimku:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Ve třídě **App** přidejte do metody **Main** následující kód. Tento kód vytvoří dvě instance **EventHubClient** a **PartitionReceiver** a umožní vám po dokončení zpracování zpráv zavřít aplikaci:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (Exception e) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Tento kód předpokládá, že jste vytvořili službu IoT Hub na úrovni F1 (Free). Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.

11. Soubor App.java uložte a zavřete.

12. Aplikaci **read-d2c-messages** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce read-d2c-messages spustíte následující příkaz:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Vytvoření aplikace pro zařízení
V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.

1. Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **simulated-device**. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte do složky simulated-devices.

3. Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti. Tato závislost vám umožní komunikovat se službou IoT Hub a serializovat objekty Java do formátu JSON pomocí balíčku iothub-java-client ve vaší aplikaci:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete vyhledat nejnovější verzi **iot-device-client** pomocí [vyhledávání Maven][lnk-maven-device-search].

4. Soubor pom.xml uložte a zavřete.

5. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.

6. Do souboru přidejte následující příkazy pro **import**:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Do třídy **App** přidejte následující proměnné na úrovni třídy. Nahraďte hodnotu **{youriothubname}** názvem vaší služby IoT Hub a hodnotu **{yourdevicekey}** klíčem zařízení, který jste vygenerovali v části *Vytvoření identity zařízení*:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**. Ke komunikaci se službou IoT Hub můžete použít protokol MQTT, AMQP nebo HTTPS.

8. Telemetrická data, která vaše zařízení odesílá do služby IoT Hub, určete přidáním následující vnořené třídy **TelemetryDataPoint** do třídy **App**.

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. Za účelem zobrazení stavu potvrzení, které služba IoT Hub vrací po zpracování zprávy z aplikace pro zařízení, přidejte do třídy **App** následující vnořenou třídu **EventCallback**. Tato metoda také po zpracování zprávy upozorní hlavní vlákno v aplikaci:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Do třídy **App** přidejte následující vnořenou třídu **MessageSender**. Metoda **run** v této třídě vygeneruje ukázková telemetrická data, která se odešlou do služby IoT Hub, a před odesláním další zprávy počká na potvrzení:

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
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

    Tato metoda odešle novou zprávu typu zařízení-cloud sekundu poté, co služba IoT Hub potvrdí předchozí zprávu. Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovanými čísly, který simuluje snímač teploty a snímač vlhkosti.

11. Metodu **Main** nahraďte následujícím kódem, který vytvoří vlákno k odesílání zpráv typu zařízení-cloud do služby IoT Hub:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Soubor App.java uložte a zavřete.

13. Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování. V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.

## <a name="run-the-apps"></a>Spouštění aplikací

Nyní jste připraveni aplikaci spustit.

1. V příkazovém řádku ve složce read-d2c spusťte následující příkaz, aby se začal monitorovat první oddíl služby IoT Hub:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aplikace služby Java IoT Hub pro monitorování zpráv typu zařízení-cloud][7]

2. V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Aplikace zařízení služby Java IoT Hub pro odesílání zpráv typu zařízení-cloud][8]

3. Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:

    ![Dlaždice Použití webu Azure Portal se zobrazením počtu zpráv odeslaných do služby IoT Hub][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub. Pomocí identity zařízení jste aplikaci pro zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub. Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub.

Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou Azure IoT Edge][lnk-iot-edge]

Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
