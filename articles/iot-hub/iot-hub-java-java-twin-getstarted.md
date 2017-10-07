---
title: "aaaGet začít s dvojčata zařízení Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak tooadd dvojčata zařízení Azure IoT Hub toouse značky a pak použít dotaz služby IoT Hub. Použití zařízení Azure IoT hello SDK pro aplikace v jazyce Java tooimplement hello zařízení a hello sady SDK služby Azure IoT pro Java tooimplement aplikační služby, které přidá značky hello a spustí hello dotazu IoT Hub."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: 25f6fc81471d59c62bcdc3766bb5c33f5733c930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-java"></a>Začínáme s dvojčata zařízení (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:

* **Přidání dotazu značky**, back-end aplikaci Java, která přidá značky a dotazuje dvojčata zařízení.
* **simulated-device**, aplikace v jazyce Java zařízení, která se připojuje tooyour IoT hub a sestav stavu připojení při používání hlášené vlastnosti.

> [!NOTE]
> článek Hello [SDK služby Azure IoT](iot-hub-devguide-sdks.md) poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.

toocomplete tohoto kurzu potřebujete:

* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Pokud dáváte přednost identitu zařízení hello toocreate prostřednictvím kódu programu, najdete v hello hello odpovídající části [připojit zařízení tooyour IoT hub pomocí jazyka Java](iot-hub-java-java-getstarted.md#create-a-device-identity) článku.

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service

V této části vytvoříte aplikaci Java, která přidá umístění metadat jako přidružený dvojče značky toohello zařízení IoT hub **myDeviceId**. aplikace Hello nejprve dotazuje služby IoT hub pro zařízení, které jsou umístěné v hello USA a potom pro zařízení, která sestavy připojení k mobilní síti.

1. Na vývojovém počítači, vytvořte prázdnou složku s názvem `iot-java-twin-getstarted`.

1. V hello `iot-java-twin-getstarted` složku vytvořit projekt Maven s názvem **přidat dotazu značky** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello `add-tags-query` složky.

1. Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `add-tags-query` složky a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost vám umožní toouse hello **klienta služby iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Uložte a zavřete hello `pom.xml` souboru.

1. Pomocí textového editoru otevřete hello `add-tags-query\src\main\java\com\mycompany\app\App.java` souboru.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Přidejte následující kód toohello hello **hlavní** metoda toocreate hello **DeviceTwin** a **DeviceTwinDevice** objekty. Hello **DeviceTwin** objekt zpracovává hello komunikace se službou IoT hub. Hello **DeviceTwinDevice** objekt představuje hello dvojče zařízení s jeho vlastnosti a značky:

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Přidejte následující hello `try/catch` blokovat toohello **hlavní** metoda:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. tooupdate hello **oblast** a **závodu** značky twin zařízení ve vaší dvojče zařízení, přidejte následující kód v hello hello `try` bloku:

    ```java
    // Get hello device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from hello existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create hello tags and attach them toohello DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update hello device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve hello device twin with hello tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. tooquery hello dvojčata zařízení IoT hub, přidejte následující kód toohello hello `try` blok za kódem hello jste přidali v předchozím kroku hello. Spustí kód Hello dva dotazy. Každý dotaz vrací maximálně 100 zařízení:

    ```java
    // Query hello device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct hello query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run hello query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct hello query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run hello query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Uložte a zavřete hello `add-tags-query\src\main\java\com\mycompany\app\App.java` souboru

1. Sestavení hello **přidat dotazu značky** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello `add-tags-query` složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Vytvoření aplikace pro zařízení

V této části vytvoříte konzolovou aplikaci Java, která nastaví hlášené vlastnost hodnotu, která je odeslána tooIoT rozbočovače.

1. V hello `iot-java-twin-getstarted` složku vytvořit projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello `simulated-device` složky.

1. Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `simulated-device` složky a přidejte následující závislosti toohello hello **závislosti** uzlu. Tuto závislost vám umožní toouse hello **klienta zařízení iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Uložte a zavřete hello `pom.xml` souboru.

1. Pomocí textového editoru otevřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.

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
    ```

    Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu. V současné době toouse twin funkce zařízení je nutné použít protokol MQTT hello.

1. Přidejte následující kód toohello hello **hlavní** metody:
    * Vytvořte zařízení klienta toocommunicate službou IoT Hub.
    * Vytvoření **zařízení** toostore hello zařízení twin vlastnosti objektu.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object toostore hello device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed too" + propertyValue);
      }
    };
    ```

1. Přidejte následující kód toohello hello **hlavní** metoda toocreate **connectivityType** hlášené vlastnost a jeho odeslání tooIoT rozbočovače:

    ```java
    try {
      // Open hello DeviceClient and start hello device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it tooyour IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Přidejte následující kód toohello konec hello hello **hlavní** metoda. Čeká se dobrý **Enter** klíč umožňuje dobu IoT Hub tooreport hello stav hello zařízení twin operací:

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Uložte a zavřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.

1. Sestavení hello **simulated-device** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello `simulated-device` složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello konzole aplikace.

1. Na příkazovém řádku v hello `add-tags-query` složky, spusťte následující příkaz toorun hello hello **přidat dotazu značky** služby App Service:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Tooupdate aplikace služby Java IoT Hub značky hodnoty a spouštět dotazy zařízení](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Můžete zobrazit hello **závodu** a **oblast** dvojče zařízení toohello přidat značky. Hello první dotaz vrátí zařízení, ale hello druhý není.

1. Na příkazovém řádku v hello `simulated-device` složky, spusťte následující příkaz tooadd hello hello **connectivityType** hlášené dvojče zařízení toohello vlastnost:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Přidá klienta zařízení Hello hello ** connectivityType ** hlášené vlastnost](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. Na příkazovém řádku v hello `add-tags-query` složky, spusťte následující příkaz toorun hello hello **přidat dotazu značky** aplikační služby ještě jednou:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Tooupdate aplikace služby Java IoT Hub značky hodnoty a spouštět dotazy zařízení](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Nyní zařízení odeslal hello **connectivityType** vlastnost tooIoT centru, druhý hello dotaz vrátí zařízení.

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Přidat zařízení metadat jako značky z back-end aplikace a napsali informací o zařízení aplikaci tooreport zařízení připojení v dvojče zařízení hello. Také jste zjistili, jak tooquery hello informace o dvojici zařízení pomocí jazyka dotazů SQL jako IoT Hub hello.

Použití hello následující toolearn prostředky jak pro:

* Odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) kurzu.
* Kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody](iot-hub-java-java-direct-methods.md) kurzu.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
