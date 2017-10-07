---
title: "aaaGet začít s Azure IoT Hub (Java) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pro jazyk Java pomocí sady SDK služby IoT. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
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
ms.openlocfilehash: ac954f0522b46ed2a5b4a819bc611c13be0b9a9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a>Připojení zařízení tooyour IoT hub pomocí jazyka Java
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci hello tohoto kurzu máte tři aplikace konzoly v jazyce Java:

* **Create-device-identity**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace zařízení.
* **Read-d2c-messages**, který zobrazuje hello telemetrické zprávy odesílané aplikace zařízení.
* **simulated-device**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každý druhý pomocí hello MQTT protokolu.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

V posledním kroku, si poznamenejte hello **primární klíč** hodnotu. Pak klikněte na tlačítko **koncové body** a hello **události** vestavěným koncovým bodem. Na hello **vlastnosti** okno, poznamenejte hello **název kompatibilní s centrem událostí** a hello **koncový bod kompatibilní s centrem událostí** adresu. Tyto tři hodnoty budete potřebovat k vytvoření aplikace **read-d2c-messages**.

![Okno Zasílání zpráv služby IoT Hub na webu Azure Portal][6]

Nyní jste vytvořili svůj IoT Hub. Máte hello název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub, IoT Hub primární klíč, název kompatibilní s centrem událostí a koncový bod kompatibilní s centrem událostí je třeba toocomplete v tomto kurzu.

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci Java, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub. Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače. Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.

1. Vytvořte prázdnou složku s názvem iot-java-get-started. Ve složce iot-java-get-started hello vytvořte projekt Maven s názvem **create-device-identity** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte toohello složky create-device-identity.

3. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce create-device-identity hello a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci:

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

5. Pomocí textového editoru otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java hello.

6. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy a nahraďte **{yourhubconnectionstring}** s hello hodnotu vaší uvedené výše:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Upravte podpis hello hello **hlavní** metoda tooinclude hello výjimky následujícím způsobem:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Přidejte následující kód jako hello textu hello hello **hlavní** metoda. Tento kód vytvoří v registru identit služby IoT Hub zařízení s názvem *javadevice* (pokud zatím neexistuje). Potom zobrazí hello zařízení ID a klíč, který budete potřebovat později:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
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
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
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

10. Uložte a zavřete soubor App.java hello.

11. toobuild hello **create-device-identity** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce create-device-identity hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. toorun hello **create-device-identity** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce create-device-identity hello hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Poznamenejte si hello **ID zařízení** a **klíč zařízení**. Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.

> [!NOTE]
> Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub. Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením. Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště specifické pro aplikace. Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Příjem zpráv typu zařízení-cloud

V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub. IoT hub zpřístupní [centra událostí][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud. věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení. Hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurz ukazuje, jak tooprocess zařízení cloud zpráv ve velkém měřítku. Hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz obsahuje další informace o tom, jak tooprocess zpráv ze služby Event Hubs a je použít toohello koncové body kompatibilní s centrem událostí služby IoT Hub.

> [!NOTE]
> Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.

1. Ve složce iot-java-get-started hello jste vytvořili v hello *vytvoření identity zařízení* , vytvořte projekt Maven s názvem **read-d2c-messages** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte toohello read-d2c-messages složky.

3. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce read-d2c-messages hello a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost umožňuje toouse hello balíčku eventhubs-client ve vaší aplikaci tooread z koncového bodu kompatibilního s centrem událostí hello:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. Uložte a zavřete soubor pom.xml hello.

5. Pomocí textového editoru otevřete soubor read-d2c-messages\src\main\java\com\mycompany\app\App.java hello.

6. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Přidejte následující proměnné toohello úrovni třídy hello **aplikace** třídy. Nahraďte **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, a **{youreventhubcompatiblename}** hello hodnotami, které jste si poznamenali dříve:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Přidejte následující hello **receiveMessages** metoda toohello **aplikace** třídy. Tato metoda vytvoří **EventHubClient** instance tooconnect toohello kompatibilní s centrem událostí koncový bod a poté asynchronně vytvoří **PartitionReceiver** instance tooread z centra událostí oddíl. To nepřetržitě provádět v cyklu a vytiskne hello podrobnosti zprávy, dokud neskončí aplikace hello.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed toocreate client: " + e.getMessage());
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
                    System.out.println("Got some evenst");
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
                System.out.println("Failed tooreceive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed toocreate receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Tato metoda používá filtr, při vytváření příjemce hello tak, aby hello přijímač četl pouze zprávy odeslané tooIoT Hub až po spuštění hello příjemce. Tato metoda je užitečná v testovacím prostředí, abyste viděli hello aktuální sadu zpráv. V produkčním prostředí by měl váš kód Ujistěte se, že zpracovává všechny zprávy hello – Další informace naleznete v tématu hello [jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-process-d2c-tutorial] kurzu.

9. Upravte podpis hello hello **hlavní** metoda tooinclude hello výjimka následujícím způsobem:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Přidejte následující kód toohello hello **hlavní** metoda v hello **aplikace** třídy. Tento kód vytvoří dvě hello **EventHubClient** a **PartitionReceiver** instance a umožní vám aplikace hello tooclose po dokončení zpracování zpráv:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Tento kód předpokládá, že jste vytvořili službu IoT hub v úroveň hello F1 (free). Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.

11. Uložte a zavřete soubor App.java hello.

12. toobuild hello **read-d2c-messages** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce read-d2c-messages hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Vytvoření aplikace pro zařízení
V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.

1. Ve složce iot-java-get-started hello jste vytvořili v hello *vytvoření identity zařízení* , vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Na příkazovém řádku přejděte toohello složky simulated Devices.

3. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislosti toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello iothub-java-client balíček ve vaší aplikaci toocommunicate s IoT hub a tooserialize tooJSON objekty Java:

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
    > Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání][lnk-maven-device-search].

4. Uložte a zavřete soubor pom.xml hello.

5. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.

6. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahrazení **{youriothubname}** názvem služby IoT hub, a **{yourdevicekey}** s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu. Můžete buď toocommunicate hello MQTT, AMQP nebo HTTP protocol službou IoT Hub.

8. Přidejte následující hello vnořenou **TelemetryDataPoint** třída uvnitř hello **aplikace** třídy toospecify hello telemetrická data vaše zařízení odesílá tooyour IoT hub:

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
9. Přidejte následující hello vnořenou **EventCallback** třída uvnitř hello **aplikace** třída toodisplay hello potvrzení stav, který hello služby IoT hub vrací po zpracování zprávy ze zařízení aplikaci hello. Tato metoda také upozorňován hello hlavní vlákno v aplikaci hello po zpracování zprávy hello:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toomessage with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Přidejte následující hello vnořenou **MessageSender** třída uvnitř hello **aplikace** třídy. Hello **spustit** metoda v této třídě vygeneruje ukázková telemetrická data toosend tooyour IoT hub a počká na potvrzení před odesláním další zprávy hello:

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

    Tato metoda odesílá novou zprávu typu zařízení cloud sekundu poté, co hello IoT hub potvrdí předchozí zprávu hello. Hello zpráva obsahuje objekt serializací JSON s ID hello zařízení a náhodně vygenerované čísla toosimulate senzor teploty a vlhkosti senzoru.

11. Nahraďte hello **hlavní** metoda s hello následující kód, který vytvoří vlákno toosend zpráv typu zařízení cloud tooyour IoT rozbočovači:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER tooexit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Uložte a zavřete soubor App.java hello.

13. toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku ve složce read-d2c hello spusťte následující příkaz toobegin monitorovat první oddíl služby IoT hub hello hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Zprávy typu zařízení cloud Java IoT Hub služby aplikace toomonitor][7]

2. Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin odesílání telemetrických dat tooyour IoT hub hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Zprávy typu zařízení cloud Java IoT Hub zařízení aplikace toosend][8]

3. Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:

    ![Azure portálu využití dlaždice zobrazuje počet zpráv odeslaných tooIoT rozbočovače][43]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT. Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub.

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou Azure IoT Edge][lnk-iot-edge]

toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.
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
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
