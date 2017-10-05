---
title: "Plánování úloh službou Azure IoT Hub (Java) | Microsoft Docs"
description: "Popisuje, jak naplánovat úlohu služby Azure IoT Hub vyvolat přímé metodu a nastavit požadovanou vlastnost na několika zařízeních. Použití zařízení Azure IoT SDK pro jazyk Java k implementaci aplikace simulovaného zařízení a sady SDK pro jazyk Java k implementaci aplikační služby, který chcete spustit úlohu služby Azure IoT."
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
ms.openlocfilehash: 003a548ef2da2921a699df1aa9f7aee366d341ab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a><span data-ttu-id="d3583-104">Úlohy plán a všesměrového vysílání (Java)</span><span class="sxs-lookup"><span data-stu-id="d3583-104">Schedule and broadcast jobs (Java)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="d3583-105">K plánování a sledování úloh, které aktualizují miliony zařízení pomocí služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d3583-105">Use Azure IoT Hub to schedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="d3583-106">Pomocí úloh:</span><span class="sxs-lookup"><span data-stu-id="d3583-106">Use jobs to:</span></span>

* <span data-ttu-id="d3583-107">Aktualizace požadovaných vlastností</span><span class="sxs-lookup"><span data-stu-id="d3583-107">Update desired properties</span></span>
* <span data-ttu-id="d3583-108">Aktualizace značky</span><span class="sxs-lookup"><span data-stu-id="d3583-108">Update tags</span></span>
* <span data-ttu-id="d3583-109">Vyvolání metody přímé</span><span class="sxs-lookup"><span data-stu-id="d3583-109">Invoke direct methods</span></span>

<span data-ttu-id="d3583-110">Úloha zabalí jednu z těchto akcí a sleduje provádění na sadu zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3583-110">A job wraps one of these actions and tracks the execution against a set of devices.</span></span> <span data-ttu-id="d3583-111">Dotaz zařízení twin definuje sadu zařízení, která provede úlohu proti.</span><span class="sxs-lookup"><span data-stu-id="d3583-111">A device twin query defines the set of devices the job executes against.</span></span> <span data-ttu-id="d3583-112">Například můžete použít back-end aplikačním úlohu k vyvolání přímé metodu na 10 000 zařízení, která restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3583-112">For example, a back-end app can use a job to invoke a direct method on 10,000 devices that reboots the devices.</span></span> <span data-ttu-id="d3583-113">Zadejte sadu zařízení s dotazem twin zařízení a úlohu naplánovat na spuštění v budoucnosti.</span><span class="sxs-lookup"><span data-stu-id="d3583-113">You specify the set of devices with a device twin query and schedule the job to run at a future time.</span></span> <span data-ttu-id="d3583-114">Průběh úlohy sleduje jako každé zařízení obdrží a provést přímý metodu restartování.</span><span class="sxs-lookup"><span data-stu-id="d3583-114">The job tracks progress as each of the devices receive and execute the reboot direct method.</span></span>

<span data-ttu-id="d3583-115">Další informace o každém z těchto funkcí najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d3583-115">To learn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="d3583-116">Dvojče zařízení a vlastností: [začít pracovat s dvojčata zařízení](iot-hub-java-java-twin-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="d3583-116">Device twin and properties: [Get started with device twins](iot-hub-java-java-twin-getstarted.md)</span></span>
* <span data-ttu-id="d3583-117">Přímé metody: [Příručka vývojáře pro službu IoT Hub - přímé metody](iot-hub-devguide-direct-methods.md) a [kurz: použijte přímý metody](iot-hub-java-java-direct-methods.md)</span><span class="sxs-lookup"><span data-stu-id="d3583-117">Direct methods: [IoT Hub developer guide - direct methods](iot-hub-devguide-direct-methods.md) and [Tutorial: Use direct methods](iot-hub-java-java-direct-methods.md)</span></span>

<span data-ttu-id="d3583-118">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="d3583-118">This tutorial shows you how to:</span></span>

* <span data-ttu-id="d3583-119">Vytvoření aplikace zařízení, která implementuje přímé metodu s názvem **lockDoor**.</span><span class="sxs-lookup"><span data-stu-id="d3583-119">Create a device app that implements a direct method called **lockDoor**.</span></span> <span data-ttu-id="d3583-120">Aplikace zařízení také přijme požadovanou vlastnost změny z back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="d3583-120">The device app also receives desired property changes from the back-end app.</span></span>
* <span data-ttu-id="d3583-121">Vytvořit aplikaci back-end, která vytvoří úlohu k volání **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3583-121">Create a back-end app that creates a job to call the **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="d3583-122">Jiná úloha odešle aktualizace požadované vlastností do více zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3583-122">Another job sends desired property updates to multiple devices.</span></span>

<span data-ttu-id="d3583-123">Na konci tohoto kurzu máte zařízení konzolovou aplikaci java a back-end konzolovou aplikaci java:</span><span class="sxs-lookup"><span data-stu-id="d3583-123">At the end of this tutorial, you have a java console device app and a java console back-end app:</span></span>

<span data-ttu-id="d3583-124">**simulated-device** která se připojuje ke službě IoT hub, implementuje **lockDoor** přímá metoda a zpracovává potřeby změny vlastností.</span><span class="sxs-lookup"><span data-stu-id="d3583-124">**simulated-device** that connects to your IoT hub, implements the **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="d3583-125">**plán úlohy** , úlohy využít k volání **lockDoor** přímá metoda a aktualizovat vlastnosti twin požadovaného zařízení na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3583-125">**schedule-jobs** that use jobs to call the **lockDoor** direct method and update the device twin desired properties on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="d3583-126">Článek [SDK služby Azure IoT](iot-hub-devguide-sdks.md) poskytuje informace o SDK služby Azure IoT, můžete použít k tvorbě aplikací, zařízení a back-end.</span><span class="sxs-lookup"><span data-stu-id="d3583-126">The article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about the Azure IoT SDKs that you can use to build both device and back-end apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3583-127">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d3583-127">Prerequisites</span></span>

<span data-ttu-id="d3583-128">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="d3583-128">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="d3583-129">Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="d3583-129">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="d3583-130">Maven 3</span><span class="sxs-lookup"><span data-stu-id="d3583-130">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="d3583-131">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d3583-131">An active Azure account.</span></span> <span data-ttu-id="d3583-132">(Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="d3583-132">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="d3583-133">Pokud dáváte přednost pro vytvoření identity zařízení prostřednictvím kódu programu, najdete v příslušné části [připojení zařízení do služby IoT hub pomocí jazyka Java](iot-hub-java-java-getstarted.md#create-a-device-identity) článku.</span><span class="sxs-lookup"><span data-stu-id="d3583-133">If you prefer to create the device identity programmatically, read the corresponding section in the [Connect your device to your IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span> <span data-ttu-id="d3583-134">Můžete také [iothub-explorer](https://github.com/Azure/iothub-explorer) nástroje pro přidání zařízení do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d3583-134">You can also use the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to add a device to your IoT hub.</span></span>

## <a name="create-the-service-app"></a><span data-ttu-id="d3583-135">Vytvořit aplikaci aplikační služby</span><span class="sxs-lookup"><span data-stu-id="d3583-135">Create the service app</span></span>

<span data-ttu-id="d3583-136">V této části vytvoříte konzolovou aplikaci Java, která používá úloh:</span><span class="sxs-lookup"><span data-stu-id="d3583-136">In this section, you create a Java console app that uses jobs to:</span></span>

* <span data-ttu-id="d3583-137">Volání **lockDoor** přímá metoda na několika zařízeních.</span><span class="sxs-lookup"><span data-stu-id="d3583-137">Call the **lockDoor** direct method on multiple devices.</span></span>
* <span data-ttu-id="d3583-138">Požadované vlastnosti poslat více zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3583-138">Send desired properties to multiple devices.</span></span>

<span data-ttu-id="d3583-139">Vytvoření aplikace:</span><span class="sxs-lookup"><span data-stu-id="d3583-139">To create the app:</span></span>

1. <span data-ttu-id="d3583-140">Na vývojovém počítači, vytvořte prázdnou složku s názvem `iot-java-schedule-jobs`.</span><span class="sxs-lookup"><span data-stu-id="d3583-140">On your development machine, create an empty folder called `iot-java-schedule-jobs`.</span></span>

1. <span data-ttu-id="d3583-141">V `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **plán úloh** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="d3583-141">In the `iot-java-schedule-jobs` folder, create a Maven project called **schedule-jobs** using the following command at your command prompt.</span></span> <span data-ttu-id="d3583-142">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3583-142">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="d3583-143">Na příkazovém řádku přejděte do `schedule-jobs` složky.</span><span class="sxs-lookup"><span data-stu-id="d3583-143">At your command prompt, navigate to the `schedule-jobs` folder.</span></span>

1. <span data-ttu-id="d3583-144">Pomocí textového editoru, otevřete `pom.xml` v soubor `schedule-jobs` složky a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d3583-144">Using a text editor, open the `pom.xml` file in the `schedule-jobs` folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="d3583-145">Tuto závislost umožňuje používat **klienta služby iot** balíček ve vaší aplikaci pro komunikaci se službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="d3583-145">This dependency enables you to use the **iot-service-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d3583-146">Můžete zkontrolovat pro nejnovější verzi **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="d3583-146">You can check for the latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="d3583-147">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d3583-147">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="d3583-148">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="d3583-148">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="d3583-149">Uložte a zavřete `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3583-149">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="d3583-150">Pomocí textového editoru, otevřete `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3583-150">Using a text editor, open the `schedule-jobs\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d3583-151">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="d3583-151">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="d3583-152">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="d3583-152">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="d3583-153">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="d3583-153">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long the job is permitted to run without
    // completing its work on the set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. <span data-ttu-id="d3583-154">Přidejte následující metodu do **aplikace** třída naplánovat úlohu, která aktualizuje **vytváření** a **podlaží** požadovaných vlastností v dvojče zařízení:</span><span class="sxs-lookup"><span data-stu-id="d3583-154">Add the following method to the **App** class to schedule a job to update the **Building** and **Floor** desired properties in the device twin:</span></span>

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule the update twin job to run now
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

1. <span data-ttu-id="d3583-155">Naplánování úlohy k volání **lockDoor** metoda, přidejte následující metodu do **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d3583-155">To schedule a job to call the **lockDoor** method, add the following method to the **App** class:</span></span>

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now to call the lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set to 5 seconds.
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

1. <span data-ttu-id="d3583-156">Monitorování úlohy, přidejte následující metodu do **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d3583-156">To monitor a job, add the following method to the **App** class:</span></span>

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check the job result until it's completed
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

1. <span data-ttu-id="d3583-157">Se dotázat na podrobnosti o úlohách, které jste spustili, přidejte následující metodu:</span><span class="sxs-lookup"><span data-stu-id="d3583-157">To query for the details of the jobs you ran, add the following method:</span></span>

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using the time the jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over the list of jobs and print the details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. <span data-ttu-id="d3583-158">Aktualizace **hlavní** podpis metody k patří `throws` klauzule:</span><span class="sxs-lookup"><span data-stu-id="d3583-158">Update the **main** method signature to include the following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. <span data-ttu-id="d3583-159">Ke spuštění a monitorování dvě úlohy postupně, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="d3583-159">To run and monitor two jobs sequentially, add the following code to the **main** method:</span></span>

    ```java
    // Record the start time
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

    // Run a query to show the job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. <span data-ttu-id="d3583-160">Uložte a zavřete `schedule-jobs\src\main\java\com\mycompany\app\App.java` souboru</span><span class="sxs-lookup"><span data-stu-id="d3583-160">Save and close the `schedule-jobs\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="d3583-161">Sestavení **plán úloh** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="d3583-161">Build the **schedule-jobs** app and correct any errors.</span></span> <span data-ttu-id="d3583-162">Na příkazovém řádku přejděte do `schedule-jobs` složky a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3583-162">At your command prompt, navigate to the `schedule-jobs` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="d3583-163">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="d3583-163">Create a device app</span></span>

<span data-ttu-id="d3583-164">V této části vytvoříte konzolovou aplikaci Java, která zpracovává požadované vlastnosti odeslané ze služby IoT Hub a implementuje volání přímé metody.</span><span class="sxs-lookup"><span data-stu-id="d3583-164">In this section, you create a Java console app that handles the desired properties sent from IoT Hub and implements the direct method call.</span></span>

1. <span data-ttu-id="d3583-165">V `iot-java-schedule-jobs` složku vytvořit projekt Maven s názvem **simulated-device** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="d3583-165">In the `iot-java-schedule-jobs` folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="d3583-166">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3583-166">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="d3583-167">Na příkazovém řádku přejděte do `simulated-device` složky.</span><span class="sxs-lookup"><span data-stu-id="d3583-167">At your command prompt, navigate to the `simulated-device` folder.</span></span>

1. <span data-ttu-id="d3583-168">Pomocí textového editoru, otevřete `pom.xml` v soubor `simulated-device` složky a přidejte následující závislosti na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d3583-168">Using a text editor, open the `pom.xml` file in the `simulated-device` folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="d3583-169">Tuto závislost umožňuje používat **klienta zařízení iot** balíček ve vaší aplikaci pro komunikaci se službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="d3583-169">This dependency enables you to use the **iot-device-client** package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d3583-170">Můžete zkontrolovat pro nejnovější verzi **klienta zařízení iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="d3583-170">You can check for the latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="d3583-171">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d3583-171">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="d3583-172">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="d3583-172">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="d3583-173">Uložte a zavřete `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3583-173">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="d3583-174">Pomocí textového editoru, otevřete `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3583-174">Using a text editor, open the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d3583-175">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="d3583-175">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="d3583-176">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="d3583-176">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="d3583-177">Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` klíčem zařízení hodnotou, kterou jste vygenerovaných *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="d3583-177">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="d3583-178">Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**.</span><span class="sxs-lookup"><span data-stu-id="d3583-178">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="d3583-179">V současné době používat funkce twin zařízení musí používat protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="d3583-179">Currently, to use device twin features you must use the MQTT protocol.</span></span>

1. <span data-ttu-id="d3583-180">Tisknout oznámení twin zařízení ke konzole, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d3583-180">To print device twin notifications to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to device twin operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="d3583-181">Tisknout přímá metoda oznámení ke konzole, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d3583-181">To print direct method notifications to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to direct method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="d3583-182">Pro zpracování přímá metoda volání ze služby IoT Hub, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d3583-182">To handle direct method calls from IoT Hub, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="d3583-183">Aktualizace **hlavní** podpis metody k patří `throws` klauzule:</span><span class="sxs-lookup"><span data-stu-id="d3583-183">Update the **main** method signature to include the following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="d3583-184">Přidejte následující kód, který **hlavní** metody:</span><span class="sxs-lookup"><span data-stu-id="d3583-184">Add the following code to the **main** method to:</span></span>
    * <span data-ttu-id="d3583-185">Vytvoření klienta zařízení pro komunikaci se službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d3583-185">Create a device client to communicate with IoT Hub.</span></span>
    * <span data-ttu-id="d3583-186">Vytvoření **zařízení** objekt pro uložení twin vlastnosti zařízení.</span><span class="sxs-lookup"><span data-stu-id="d3583-186">Create a **Device** object to store the device twin properties.</span></span>

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object to manage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. <span data-ttu-id="d3583-187">Spuštění služby klienta zařízení, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="d3583-187">To start the device client services, add the following code to the **main** method:</span></span>

    ```java
    try {
      // Open the DeviceClient
      // Start the device twin services
      // Subscribe to direct method calls
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

1. <span data-ttu-id="d3583-188">Čekání na uživatele ke stisknutí **Enter** klíče před ukončením, přidejte následující kód do konce **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="d3583-188">To wait for the user to press the **Enter** key before shutting down, add the following code to the end of the **main** method:</span></span>

    ```java
    // Close the app
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. <span data-ttu-id="d3583-189">Uložte a zavřete `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d3583-189">Save and close the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d3583-190">Sestavení **simulated-device** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="d3583-190">Build the **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="d3583-191">Na příkazovém řádku přejděte do `simulated-device` složky a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d3583-191">At your command prompt, navigate to the `simulated-device` folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="d3583-192">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="d3583-192">Run the apps</span></span>

<span data-ttu-id="d3583-193">Nyní jste připraveni ke spuštění aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="d3583-193">You are now ready to run the console apps.</span></span>

1. <span data-ttu-id="d3583-194">Na příkazovém řádku v `simulated-device` složky, spusťte následující příkaz a spusťte aplikaci zařízení naslouchání pro požadovanou vlastnost změny a volání přímé metod:</span><span class="sxs-lookup"><span data-stu-id="d3583-194">At a command prompt in the `simulated-device` folder, run the following command to start the device app listening for desired property changes and direct method calls:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Spuštění klienta zařízení](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. <span data-ttu-id="d3583-196">Na příkazovém řádku v `schedule-jobs` složky, spusťte následující příkaz ke spuštění **plán úloh** aplikační služby ke spuštění dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="d3583-196">At a command prompt in the `schedule-jobs` folder, run the following command to run the **schedule-jobs** service app to run two jobs.</span></span> <span data-ttu-id="d3583-197">První nastaví hodnoty požadovanou vlastnost, druhý volá metodu přímé:</span><span class="sxs-lookup"><span data-stu-id="d3583-197">The first sets the desired property values, the second calls the direct method:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub vytvoří dvě úlohy](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. <span data-ttu-id="d3583-199">Aplikace zařízení zpracovává změnu požadované vlastnosti a volání přímé metody:</span><span class="sxs-lookup"><span data-stu-id="d3583-199">The device app handles the desired property change and the direct method call:</span></span>

    ![Klient v zařízení reaguje na změny](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a><span data-ttu-id="d3583-201">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d3583-201">Next steps</span></span>

<span data-ttu-id="d3583-202">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d3583-202">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="d3583-203">Vytvořili jste back-end aplikačním spustit dvě úlohy.</span><span class="sxs-lookup"><span data-stu-id="d3583-203">You created a back-end app to run two jobs.</span></span> <span data-ttu-id="d3583-204">První úlohy nastavte hodnoty požadované vlastnosti a druhý úloha s názvem přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="d3583-204">The first job set desired property values, and the second job called a direct method.</span></span>

<span data-ttu-id="d3583-205">Použijte v následujících zdrojích informací další postup:</span><span class="sxs-lookup"><span data-stu-id="d3583-205">Use the following resources to learn how to:</span></span>

* <span data-ttu-id="d3583-206">Odesílat telemetrická data ze zařízení pomocí [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="d3583-206">Send telemetry from devices with the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="d3583-207">S kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) [použít přímé metody](iot-hub-java-java-direct-methods.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="d3583-207">Control devices interactively (such as turning on a fan from a user-controlled app) with the [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>
