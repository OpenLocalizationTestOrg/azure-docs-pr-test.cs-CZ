---
title: "aaaGet spuštění se správou zařízení Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak tooinitiate správy zařízení Azure IoT Hub toouse vzdáleném zařízení restartovat. Použití zařízení Azure IoT hello SDK pro Javu tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro Java tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: .java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7aaeda9d4ff7002e5c66adfd61e2dfd5bcea964f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-java"></a>Začínáme se správou zařízení (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

V tomto kurzu získáte informace o následujících postupech:

* Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.
* Vytvoření aplikace simulovaného zařízení, která implementuje přímá metoda tooreboot hello zařízení. Přímé metody jsou vyvolány z cloudu hello.
* Vytvořte aplikaci, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub. Tato aplikace pak monitorování hello hlášené vlastnosti z toosee hello zařízení po dokončení operace restartování hello.

Na konci hello tohoto kurzu máte dvě aplikace konzoly v jazyce Java:

**simulated-device**. Tuto aplikaci:

* Připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello.
* Přijme hovor přímá metoda restartování.
* Simuluje fyzické restartování.
* Sestavy hello čas poslední restartování hello prostřednictvím hlášené vlastnosti.

**aktivační událost restartování**. Tuto aplikaci:

* Přímá metoda volá v aplikaci simulovaného zařízení hello.
* Zobrazí hello odpovědi toohello přímé volání metody, které poslal hello simulovaného zařízení
* Zobrazí hello aktualizovat hlášené vlastnosti.

> [!NOTE]
> Informace o hello sady SDK, které můžete toorun toobuild aplikace na zařízení a back end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].

toocomplete tohoto kurzu potřebujete:

* Java SE 8. <br/> [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Java v tomto kurzu v systému Windows nebo Linux.
* Maven 3.  <br/> [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall [Maven] [ lnk-maven] v tomto kurzu v systému Windows nebo Linux.
* [Verze Node.js 0.10.0 nebo novější](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda

V této části vytvoříte konzolovou aplikaci Java:

1. Vyvolá metodu přímé hello restartování v aplikaci simulovaného zařízení hello.
1. Zobrazí hello odpovědi.
1. Hlasování hello hlášené vlastnosti odeslaný toodetermine hello zařízení po dokončení restartování hello.

Tato Konzolová aplikace připojí tooyour IoT Hub tooinvoke hello přímá metoda a čtení hello hlášené vlastnosti.

1. Vytvořte prázdnou složku s názvem dm-get-started.

1. Ve složce hello dm-get-started vytvořte projekt Maven s názvem **restartování aktivační událost** pomocí hello následující příkaz na příkazovém řádku. Následující Hello zobrazuje jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte složky toohello restartování aktivační události.

1. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce hello restartování aktivační události a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].

1. Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu. Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Uložte a zavřete soubor pom.xml hello.

1. Pomocí textového editoru otevřete hello trigger-reboot\src\main\java\com\mycompany\app\App.java zdrojový soubor.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. tooimplement podproces, který čte hello hlášené vlastnosti z dvojče zařízení hello každých 10 sekund, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Přímá metoda restartování tooinvoke hello na hello simulované zařízení, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. toostart hello vlákno toopoll hello hlášené vlastnosti z hello simulovaného zařízení, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. tooenable toostop hello aplikace, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. Uložte a zavřete soubor trigger-reboot\src\main\java\com\mycompany\app\App.java hello.

1. Sestavení hello **restartování aktivační událost** back-end aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello restartování aktivační událost složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení. Hello aplikace čeká na restartování hello přímá metoda. volání ze služby IoT hub a okamžitě odpoví toothat volání. Hello aplikace pak chvíli v režimu spánku toosimulate hello restartování procesu předtím, než použije hlášené vlastnost toonotify hello **restartování aktivační událost** back-end aplikace, která hello restartování je dokončena.

1. Ve složce hello dm-get-started vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku. Hello následuje jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello složky simulated Devices.

1. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání][lnk-maven-device-search].

1. Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu. Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Uložte a zavřete soubor pom.xml hello.

1. Pomocí textového editoru otevřete hello simulated-device\src\main\java\com\mycompany\app\App.java zdrojový soubor.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahraďte `{yourdeviceconnectionstring}` s řetězcem připojení zařízení hello jste si poznamenali v hello *vytvoření identity zařízení* části:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. tooimplement obslužná rutina zpětného volání pro přímé metoda události stavu, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. tooimplement obslužná rutina zpětného volání pro události twin stavu zařízení, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. tooimplement obslužná rutina zpětného volání pro vlastnosti události, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. tooimplement restartu vlákno toosimulate hello zařízení přidat hello následující vnořené třídy toohello **aplikace** třídy. vlákno Hello pět sekund v režimu spánku a poté nastaví hello **lastReboot** hlášené vlastnost:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. Přímá metoda hello tooimplement na hello zařízení, přidejte následující hello vnořené třídy toohello **aplikace** třídy. Když simulované aplikace hello obdrží volání toohello **restartovat** přímá metoda vrátí volající toohello potvrzení a pak spustí vlákno tooprocess hello restartovat:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Upravte podpis hello hello **hlavní** hello toothrow metoda následující výjimky:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. tooinstantiate **DeviceClient**, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. toostart přijímá volání přímá metoda, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed toodirect methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. tooshut dolů hello simulátor zařízení, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.

1. Sestavení hello **simulated-device** back-end aplikace a opravte všechny chyby. Na příkazovém řádku přejděte složky simulated Devices toohello a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello aplikace.

1. Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin přijímá restartování metoda volání ze služby IoT hub hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub Java simulované zařízení aplikaci toolisten pro volání metod přímé restartování][1]

1. Na příkazovém řádku ve složce hello restartování aktivační událost spusťte následující příkaz toocall hello restartování metodu na simulovaného zařízení ze služby IoT hub hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hello toocall aplikace služby Java IoT Hub restartovat přímá metoda][2]

1. Simulované zařízení Hello odpoví toohello restartování přímá metoda volání:

    ![Volání metody přímé toohello odpoví aplikaci simulovaného zařízení Java IoT Hub][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22