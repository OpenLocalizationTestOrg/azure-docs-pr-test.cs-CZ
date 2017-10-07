---
title: "aaaUse Azure IoT Hub přímé metody (Java) | Microsoft Docs"
description: "Jak toouse Azure IoT Hub přímé metody. Použití zařízení Azure IoT hello SDK pro Javu tooimplement aplikaci simulovaného zařízení, která zahrnuje přímá metoda a hello sady SDK služby Azure IoT pro Java tooimplement aplikační služby, která volá metodu přímé hello."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: b6f2f4a64535ab649a3965cd9c5a19bebaf88eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-java"></a>Používat přímé metody (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:

* **vyvolání metody přímo**, back-end aplikaci Java, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.
* **simulated-device**, která simuluje zařízení připojení tooyour IoT hub s identitou zařízení hello vytvoříte aplikaci v jazyce Java. Tato aplikace odpovídá toohello přímo volat z back-end hello.

> [!NOTE]
> Informace o hello sady SDK, které můžete toorun toobuild aplikace na zařízení a back end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].

toocomplete tohoto kurzu potřebujete:

* Java SE 8. <br/> [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Java v tomto kurzu v systému Windows nebo Linux.
* Maven 3.  <br/> [Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall [Maven] [ lnk-maven] v tomto kurzu v systému Windows nebo Linux.
* [Verze Node.js 0.10.0 nebo novější](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení

V této části vytvoříte konzolovou aplikaci Java, která odpovídá tooa metodu s názvem řešení hello zpět koncové.

1. Vytvořte prázdnou složku s názvem iot-java-direct-method.

1. Ve složce iot-java-direct-method hello vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku. Následující příkaz Hello je jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello složky simulated Devices.

1. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislosti toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello klienta zařízení iot balíček ve vaší aplikaci toocommunicate službou IoT hub:

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

1. Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu. V současné době toouse přímé metody, je nutné použít protokol MQTT hello.

1. tooreturn stav kódu tooyour IoT hub, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. toohandle hello metoda přímé volání z hello back-end řešení, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
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

1. toocreate **DeviceClient** a naslouchat volání přímé metod, přidat **hlavní** metoda toohello **aplikace** třídy:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed toodirect methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key tooexit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello

1. Sestavení hello **simulated-device** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte složky simulated Devices toohello a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Volání metody přímé na zařízení

V této části vytvoříte konzolovou aplikaci Java, která volá metodu přímé a potom zobrazí hello odpovědi. Tato Konzolová aplikace připojí tooyour IoT Hub tooinvoke hello přímá metoda.

1. Ve složce iot-java-direct-method hello vytvořte projekt Maven s názvem **vyvolání metody přímo** pomocí hello následující příkaz na příkazovém řádku. Následující příkaz Hello je jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello vyvolání metody přímo složku.

1. Pomocí textového editoru, otevřete soubor pom.xml hello ve složce hello vyvolání metody přímo a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:

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

1. Pomocí textového editoru otevřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java hello.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. Metoda hello tooinvoke na hello simulované zařízení, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Uložte a zavřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java hello

1. Sestavení hello **vyvolání metody přímo** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello vyvolání metody přímo složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello konzole aplikace.

1. Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin přijímá metoda volání ze služby IoT hub hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub Java simulované zařízení aplikaci toolisten pro volání přímá metoda][8]

1. Na příkazovém řádku ve složce hello vyvolání metody přímo spusťte následující příkaz toocall hello metodu na simulovaného zařízení ze služby IoT hub:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Toocall aplikace služby Java IoT Hub přímá metoda][7]

1. Simulované zařízení Hello odpoví toohello přímá metoda volání:

    ![Volání metody přímé toohello odpoví aplikaci simulovaného zařízení Java IoT Hub][9]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu. Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení.

tooexplore najdete v části dalších scénářů platformy IoT [plánování úloh na několika zařízeních][lnk-devguide-jobs].

toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
