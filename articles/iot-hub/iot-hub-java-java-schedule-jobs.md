---
title: "úlohy aaaSchedule službou Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak tooschedule Azure IoT Hub úlohy tooinvoke přímé metodu a nastavit požadovanou vlastnost na několika zařízeních. Použití zařízení Azure IoT hello SDK pro aplikace v jazyce Java tooimplement hello simulované zařízení a hello sady SDK služby Azure IoT pro Java tooimplement úlohu služby toorun aplikace hello."
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
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: b1b05fa56c3ce96af0b33d4cca0dd54da0f4e927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a>Úlohy plán a všesměrového vysílání (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Pomocí Azure IoT Hub tooschedule a sledování úloh, které aktualizují miliony zařízení. Pomocí úloh:

* Aktualizace požadovaných vlastností
* Aktualizace značky
* Vyvolání metody přímé

Úlohu zabalí jednu z těchto akcí a sleduje hello vůči sadu zařízení. Dotaz zařízení twin definuje hello sadu zařízení, která provede úlohu hello proti. Back-end aplikačním můžete například použít úlohy tooinvoke přímá metoda na 10 000 zařízení, která restartuje hello zařízení. Zadejte hello sadu zařízení s dotazem twin zařízení a naplánovat úlohu toorun hello na datum v budoucnosti. sleduje průběh úlohy Hello jako každý hello zařízení obdrží a provést přímý metodu hello restartování.

v tématu toolearn Další informace o každé z těchto funkcí:

* Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení](iot-hub-java-java-twin-getstarted.md)
* Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody](iot-hub-devguide-direct-methods.md) a [kurz: použijte přímý metody](iot-hub-java-java-direct-methods.md)

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor**. aplikace Hello zařízení také obdrží požadovanou vlastnost změny z back-end aplikace hello.
* Vytvořit aplikaci back-end, která vytvoří úlohy toocall hello **lockDoor** přímá metoda na několika zařízeních. Jiná úloha odešle požadovanou vlastnost aktualizací toomultiple zařízení.

Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci java a back-end konzolovou aplikaci java:

**simulated-device** , připojí tooyour IoT hub, implementuje hello **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.

**plán úlohy** , které využívají úloh toocall hello **lockDoor** přímá metoda a aktualizace hello dvojče zařízení požadovaných vlastností na několika zařízeních.

> [!NOTE]
> článek Hello [SDK služby Azure IoT](iot-hub-devguide-sdks.md) poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.

## <a name="prerequisites"></a>Požadavky

toocomplete tohoto kurzu potřebujete:

* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Pokud dáváte přednost identitu zařízení hello toocreate prostřednictvím kódu programu, najdete v hello hello odpovídající části [připojit zařízení tooyour IoT hub pomocí jazyka Java](iot-hub-java-java-getstarted.md#create-a-device-identity) článku. Můžete taky hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tooadd nástroj zařízení tooyour IoT hub.

## <a name="create-hello-service-app"></a>Vytvoření aplikace hello service

V této části vytvoříte konzolovou aplikaci Java, která používá úloh:

* Volání hello **lockDoor** přímá metoda na několika zařízeních.
* Odešlete zařízení toomultiple požadované vlastnosti.

aplikace hello toocreate:

1. Na vývojovém počítači, vytvořte prázdnou složku s názvem `iot-java-schedule-jobs`.

1. V hello `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **plán úloh** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Na příkazovém řádku přejděte toohello `schedule-jobs` složky.

1. Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `schedule-jobs` složky a přidejte následující závislost toohello hello **závislosti** uzlu. Tuto závislost vám umožní toouse hello **klienta služby iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:

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

1. Pomocí textového editoru otevřete hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy. Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Přidejte následující metodu toohello hello **aplikace** tooschedule třídy úlohy tooupdate hello **vytváření** a **podlaží** požadovaných vlastností v hello dvojče zařízení:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule hello update twin job toorun now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. tooschedule úlohy toocall hello **lockDoor** metoda, přidejte následující metodu toohello hello **aplikace** třídy:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now toocall hello lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set too5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. toomonitor úlohu, přidejte následující metodu toohello hello **aplikace** třídy:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check hello job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. tooquery podrobnosti hello hello úloh, které jste spustili, přidejte následující metodu hello:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using hello time hello jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over hello list of jobs and print hello details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. sledování a toorun dvě úlohy postupně, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    // Record hello start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query tooshow hello job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Uložte a zavřete hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru

1. Sestavení hello **plán úloh** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello `schedule-jobs` složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Vytvoření aplikace pro zařízení

V této části vytvoříte konzolovou aplikaci Java, která zpracovává hello požadované vlastnosti odeslaný IoT Hub a implementuje volání přímá metoda hello.

1. V hello `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku. Všimněte si, že se jedná o jeden dlouhý příkaz:

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu. V současné době toouse twin funkce zařízení je nutné použít protokol MQTT hello.

1. tooprint zařízení twin oznámení toohello konzole, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. tooprint přímá metoda oznámení toohello konzole, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. toohandle přímá metoda volání ze služby IoT Hub, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Přidejte následující kód toohello hello **hlavní** metody:
    * Vytvořte zařízení klienta toocommunicate službou IoT Hub.
    * Vytvoření **zařízení** toostore hello zařízení twin vlastnosti objektu.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object toomanage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. toostart hello zařízení klienta služby, přidejte následující kód toohello hello **hlavní** metoda:

    ```java
    try {
      // Open hello DeviceClient
      // Start hello device twin services
      // Subscribe toodirect method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. toowait pro hello uživatele toopress hello **Enter** klíče před ukončením, přidejte následující kód toohello konec hello hello **hlavní** metoda:

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Uložte a zavřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.

1. Sestavení hello **simulated-device** aplikace a opravte všechny chyby. Na příkazovém řádku přejděte toohello `simulated-device` složky a spuštění hello následující příkaz:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Spuštění aplikace hello

Nyní je připraven toorun hello konzole aplikace.

1. Na příkazovém řádku v hello `simulated-device` složky, spusťte následující příkaz toostart hello zařízení aplikace naslouchá pro požadovanou vlastnost změny a volání metod přímé hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![spuštění klienta zařízení Hello](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Na příkazovém řádku v hello `schedule-jobs` složky, spusťte následující příkaz toorun hello hello **plán úloh** služby aplikace toorun dvě úlohy. Hello nejprve nastaví hodnoty vlastností hello potřeby, druhé volání hello hello přímá metoda:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub vytvoří dvě úlohy](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. aplikace zařízení Hello zpracovává změnu vlastnosti hello potřeby a volání metody přímé hello:

    ![Hello zařízení klient odpoví toohello změny](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Vytvoříte back-end aplikačním toorun dvě úlohy. první úlohy Hello nastavit hodnoty požadované vlastnosti a druhý úlohy hello přímá metoda volána.

Použití hello následující toolearn prostředky jak pro:

* Odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) kurzu.
* Kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody](iot-hub-java-java-direct-methods.md) kurzu.
