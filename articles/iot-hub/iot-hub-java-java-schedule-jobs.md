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
# <a name="schedule-and-broadcast-jobs-java"></a><span data-ttu-id="6960b-104">Úlohy plán a všesměrového vysílání (Java)</span><span class="sxs-lookup"><span data-stu-id="6960b-104">Schedule and broadcast jobs (Java)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="6960b-105">Pomocí Azure IoT Hub tooschedule a sledování úloh, které aktualizují miliony zařízení.</span><span class="sxs-lookup"><span data-stu-id="6960b-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="6960b-106">Pomocí úloh:</span><span class="sxs-lookup"><span data-stu-id="6960b-106">Use jobs to:</span></span>

* <span data-ttu-id="6960b-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="6960b-107">Update desired properties</span></span>
* <span data-ttu-id="6960b-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="6960b-108">Update tags</span></span>
* <span data-ttu-id="6960b-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="6960b-109">Invoke direct methods</span></span>

<span data-ttu-id="6960b-110">Úlohu zabalí jednu z těchto akcí a sleduje hello vůči sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="6960b-110">A job wraps one of these actions and tracks hello execution against a set of devices.</span></span> <span data-ttu-id="6960b-111">Dotaz zařízení twin definuje hello sadu zařízení, která provede úlohu hello proti.</span><span class="sxs-lookup"><span data-stu-id="6960b-111">A device twin query defines hello set of devices hello job executes against.</span></span> <span data-ttu-id="6960b-112">Back-end aplikačním můžete například použít úlohy tooinvoke přímá metoda na 10 000 zařízení, která restartuje hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="6960b-112">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="6960b-113">Zadejte hello sadu zařízení s dotazem twin zařízení a naplánovat úlohu toorun hello na datum v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="6960b-113">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="6960b-114">sleduje průběh úlohy Hello jako každý hello zařízení obdrží a provést přímý metodu hello restartování.</span><span class="sxs-lookup"><span data-stu-id="6960b-114">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="6960b-115">v tématu toolearn Další informace o každé z těchto funkcí:</span><span class="sxs-lookup"><span data-stu-id="6960b-115">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="6960b-116">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení](iot-hub-java-java-twin-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="6960b-116">Device twin and properties: [Get started with device twins](iot-hub-java-java-twin-getstarted.md)</span></span>
* <span data-ttu-id="6960b-117">Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody](iot-hub-devguide-direct-methods.md) a [kurz: použijte přímý metody](iot-hub-java-java-direct-methods.md)</span><span class="sxs-lookup"><span data-stu-id="6960b-117">Direct methods: [IoT Hub developer guide - direct methods](iot-hub-devguide-direct-methods.md) and [Tutorial: Use direct methods](iot-hub-java-java-direct-methods.md)</span></span>

<span data-ttu-id="6960b-118">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="6960b-118">This tutorial shows you how to:</span></span>

* <span data-ttu-id="6960b-119">Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor**.</span><span class="sxs-lookup"><span data-stu-id="6960b-119">Create a device app that implements a direct method called **lockDoor**.</span></span> <span data-ttu-id="6960b-120">aplikace Hello zařízení také obdrží požadovanou vlastnost změny z back-end aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="6960b-120">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="6960b-121">Vytvořit aplikaci back-end, která vytvoří úlohy toocall hello **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="6960b-121">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="6960b-122">Jiná úloha odešle požadovanou vlastnost aktualizací toomultiple zařízení.</span><span class="sxs-lookup"><span data-stu-id="6960b-122">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="6960b-123">Na konci hello tohoto kurzu máte zařízení konzolovou aplikaci java a back-end konzolovou aplikaci java:</span><span class="sxs-lookup"><span data-stu-id="6960b-123">At hello end of this tutorial, you have a java console device app and a java console back-end app:</span></span>

<span data-ttu-id="6960b-124">**simulated-device** , připojí tooyour IoT hub, implementuje hello **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.</span><span class="sxs-lookup"><span data-stu-id="6960b-124">**simulated-device** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="6960b-125">**plán úlohy** , které využívají úloh toocall hello **lockDoor** přímá metoda a aktualizace hello dvojče zařízení požadovaných vlastností na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="6960b-125">**schedule-jobs** that use jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="6960b-126">článek Hello [SDK služby Azure IoT](iot-hub-devguide-sdks.md) poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="6960b-126">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6960b-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="6960b-127">Prerequisites</span></span>

<span data-ttu-id="6960b-128">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="6960b-128">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="6960b-129">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="6960b-129">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="6960b-130">Maven 3</span><span class="sxs-lookup"><span data-stu-id="6960b-130">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="6960b-131">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="6960b-131">An active Azure account.</span></span> <span data-ttu-id="6960b-132">(Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="6960b-132">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="6960b-133">Pokud dáváte přednost identitu zařízení hello toocreate prostřednictvím kódu programu, najdete v hello hello odpovídající části [připojit zařízení tooyour IoT hub pomocí jazyka Java](iot-hub-java-java-getstarted.md#create-a-device-identity) článku.</span><span class="sxs-lookup"><span data-stu-id="6960b-133">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span> <span data-ttu-id="6960b-134">Můžete taky hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tooadd nástroj zařízení tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="6960b-134">You can also use hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool tooadd a device tooyour IoT hub.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="6960b-135">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="6960b-135">Create hello service app</span></span>

<span data-ttu-id="6960b-136">V této části vytvoříte konzolovou aplikaci Java, která používá úloh:</span><span class="sxs-lookup"><span data-stu-id="6960b-136">In this section, you create a Java console app that uses jobs to:</span></span>

* <span data-ttu-id="6960b-137">Volání hello **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="6960b-137">Call hello **lockDoor** direct method on multiple devices.</span></span>
* <span data-ttu-id="6960b-138">Odešlete zařízení toomultiple požadované vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6960b-138">Send desired properties toomultiple devices.</span></span>

<span data-ttu-id="6960b-139">aplikace hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="6960b-139">toocreate hello app:</span></span>

1. <span data-ttu-id="6960b-140">Na vývojovém počítači, vytvořte prázdnou složku s názvem `iot-java-schedule-jobs`.</span><span class="sxs-lookup"><span data-stu-id="6960b-140">On your development machine, create an empty folder called `iot-java-schedule-jobs`.</span></span>

1. <span data-ttu-id="6960b-141">V hello `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **plán úloh** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="6960b-141">In hello `iot-java-schedule-jobs` folder, create a Maven project called **schedule-jobs** using hello following command at your command prompt.</span></span> <span data-ttu-id="6960b-142">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="6960b-142">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="6960b-143">Na příkazovém řádku přejděte toohello `schedule-jobs` složky.</span><span class="sxs-lookup"><span data-stu-id="6960b-143">At your command prompt, navigate toohello `schedule-jobs` folder.</span></span>

1. <span data-ttu-id="6960b-144">Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `schedule-jobs` složky a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6960b-144">Using a text editor, open hello `pom.xml` file in hello `schedule-jobs` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="6960b-145">Tuto závislost vám umožní toouse hello **klienta služby iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="6960b-145">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="6960b-146">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="6960b-146">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="6960b-147">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6960b-147">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="6960b-148">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="6960b-148">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="6960b-149">Uložte a zavřete hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="6960b-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="6960b-150">Pomocí textového editoru otevřete hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="6960b-150">Using a text editor, open hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="6960b-151">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="6960b-151">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="6960b-152">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="6960b-152">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="6960b-153">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="6960b-153">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. <span data-ttu-id="6960b-154">Přidejte následující metodu toohello hello **aplikace** tooschedule třídy úlohy tooupdate hello **vytváření** a **podlaží** požadovaných vlastností v hello dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="6960b-154">Add hello following method toohello **App** class tooschedule a job tooupdate hello **Building** and **Floor** desired properties in hello device twin:</span></span>

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

1. <span data-ttu-id="6960b-155">tooschedule úlohy toocall hello **lockDoor** metoda, přidejte následující metodu toohello hello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="6960b-155">tooschedule a job toocall hello **lockDoor** method, add hello following method toohello **App** class:</span></span>

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

1. <span data-ttu-id="6960b-156">toomonitor úlohu, přidejte následující metodu toohello hello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="6960b-156">toomonitor a job, add hello following method toohello **App** class:</span></span>

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

1. <span data-ttu-id="6960b-157">tooquery podrobnosti hello hello úloh, které jste spustili, přidejte následující metodu hello:</span><span class="sxs-lookup"><span data-stu-id="6960b-157">tooquery for hello details of hello jobs you ran, add hello following method:</span></span>

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

1. <span data-ttu-id="6960b-158">Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:</span><span class="sxs-lookup"><span data-stu-id="6960b-158">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. <span data-ttu-id="6960b-159">sledování a toorun dvě úlohy postupně, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="6960b-159">toorun and monitor two jobs sequentially, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="6960b-160">Uložte a zavřete hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru</span><span class="sxs-lookup"><span data-stu-id="6960b-160">Save and close hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="6960b-161">Sestavení hello **plán úloh** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="6960b-161">Build hello **schedule-jobs** app and correct any errors.</span></span> <span data-ttu-id="6960b-162">Na příkazovém řádku přejděte toohello `schedule-jobs` složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6960b-162">At your command prompt, navigate toohello `schedule-jobs` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="6960b-163">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="6960b-163">Create a device app</span></span>

<span data-ttu-id="6960b-164">V této části vytvoříte konzolovou aplikaci Java, která zpracovává hello požadované vlastnosti odeslaný IoT Hub a implementuje volání přímá metoda hello.</span><span class="sxs-lookup"><span data-stu-id="6960b-164">In this section, you create a Java console app that handles hello desired properties sent from IoT Hub and implements hello direct method call.</span></span>

1. <span data-ttu-id="6960b-165">V hello `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="6960b-165">In hello `iot-java-schedule-jobs` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="6960b-166">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="6960b-166">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="6960b-167">Na příkazovém řádku přejděte toohello `simulated-device` složky.</span><span class="sxs-lookup"><span data-stu-id="6960b-167">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="6960b-168">Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `simulated-device` složky a přidejte následující závislosti toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6960b-168">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="6960b-169">Tuto závislost vám umožní toouse hello **klienta zařízení iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="6960b-169">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="6960b-170">Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="6960b-170">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="6960b-171">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="6960b-171">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="6960b-172">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="6960b-172">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="6960b-173">Uložte a zavřete hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="6960b-173">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="6960b-174">Pomocí textového editoru otevřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="6960b-174">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="6960b-175">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="6960b-175">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="6960b-176">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="6960b-176">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="6960b-177">Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="6960b-177">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="6960b-178">Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="6960b-178">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="6960b-179">V současné době toouse twin funkce zařízení je nutné použít protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="6960b-179">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="6960b-180">tooprint zařízení twin oznámení toohello konzole, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="6960b-180">tooprint device twin notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="6960b-181">tooprint přímá metoda oznámení toohello konzole, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="6960b-181">tooprint direct method notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="6960b-182">toohandle přímá metoda volání ze služby IoT Hub, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="6960b-182">toohandle direct method calls from IoT Hub, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="6960b-183">Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:</span><span class="sxs-lookup"><span data-stu-id="6960b-183">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="6960b-184">Přidejte následující kód toohello hello **hlavní** metody:</span><span class="sxs-lookup"><span data-stu-id="6960b-184">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="6960b-185">Vytvořte zařízení klienta toocommunicate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="6960b-185">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="6960b-186">Vytvoření **zařízení** toostore hello zařízení twin vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="6960b-186">Create a **Device** object toostore hello device twin properties.</span></span>

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

1. <span data-ttu-id="6960b-187">toostart hello zařízení klienta služby, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="6960b-187">toostart hello device client services, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="6960b-188">toowait pro hello uživatele toopress hello **Enter** klíče před ukončením, přidejte následující kód toohello konec hello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="6960b-188">toowait for hello user toopress hello **Enter** key before shutting down, add hello following code toohello end of hello **main** method:</span></span>

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. <span data-ttu-id="6960b-189">Uložte a zavřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="6960b-189">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="6960b-190">Sestavení hello **simulated-device** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="6960b-190">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="6960b-191">Na příkazovém řádku přejděte toohello `simulated-device` složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6960b-191">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="6960b-192">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="6960b-192">Run hello apps</span></span>

<span data-ttu-id="6960b-193">Nyní je připraven toorun hello konzole aplikace.</span><span class="sxs-lookup"><span data-stu-id="6960b-193">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="6960b-194">Na příkazovém řádku v hello `simulated-device` složky, spusťte následující příkaz toostart hello zařízení aplikace naslouchá pro požadovanou vlastnost změny a volání metod přímé hello:</span><span class="sxs-lookup"><span data-stu-id="6960b-194">At a command prompt in hello `simulated-device` folder, run hello following command toostart hello device app listening for desired property changes and direct method calls:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![spuštění klienta zařízení Hello](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. <span data-ttu-id="6960b-196">Na příkazovém řádku v hello `schedule-jobs` složky, spusťte následující příkaz toorun hello hello **plán úloh** služby aplikace toorun dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="6960b-196">At a command prompt in hello `schedule-jobs` folder, run hello following command toorun hello **schedule-jobs** service app toorun two jobs.</span></span> <span data-ttu-id="6960b-197">Hello nejprve nastaví hodnoty vlastností hello potřeby, druhé volání hello hello přímá metoda:</span><span class="sxs-lookup"><span data-stu-id="6960b-197">hello first sets hello desired property values, hello second calls hello direct method:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub vytvoří dvě úlohy](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. <span data-ttu-id="6960b-199">aplikace zařízení Hello zpracovává změnu vlastnosti hello potřeby a volání metody přímé hello:</span><span class="sxs-lookup"><span data-stu-id="6960b-199">hello device app handles hello desired property change and hello direct method call:</span></span>

    ![Hello zařízení klient odpoví toohello změny](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a><span data-ttu-id="6960b-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6960b-201">Next steps</span></span>

<span data-ttu-id="6960b-202">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="6960b-202">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="6960b-203">Vytvoříte back-end aplikačním toorun dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="6960b-203">You created a back-end app toorun two jobs.</span></span> <span data-ttu-id="6960b-204">první úlohy Hello nastavit hodnoty požadované vlastnosti a druhý úlohy hello přímá metoda volána.</span><span class="sxs-lookup"><span data-stu-id="6960b-204">hello first job set desired property values, and hello second job called a direct method.</span></span>

<span data-ttu-id="6960b-205">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="6960b-205">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="6960b-206">Odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="6960b-206">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="6960b-207">Kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody](iot-hub-java-java-direct-methods.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="6960b-207">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>
