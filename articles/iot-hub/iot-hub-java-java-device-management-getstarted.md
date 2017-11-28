---
title: "Začínáme se správou zařízení Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak používat k zahájení restartu zařízení vzdálenou správou zařízení Azure IoT Hub. Zařízení Azure IoT SDK pro jazyk Java, kterou použijete k implementaci aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro jazyk Java k implementaci aplikační služby, která volá metodu přímé služby Azure IoT."
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
ms.openlocfilehash: c75635f366f5ced4bf91792d1a905dd6aab8ed79
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-device-management-java"></a><span data-ttu-id="8eb5e-104">Začínáme se správou zařízení (Java)</span><span class="sxs-lookup"><span data-stu-id="8eb5e-104">Get started with device management (Java)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="8eb5e-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="8eb5e-106">Použití portálu Azure k vytvoření služby IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-106">Use the Azure portal to create an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="8eb5e-107">Vytvořte aplikaci ze simulovaného zařízení, která používá přímý způsob restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-107">Create a simulated device app that implements a direct method to reboot the device.</span></span> <span data-ttu-id="8eb5e-108">Přímé metody jsou vyvolány z cloudu.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-108">Direct methods are invoked from the cloud.</span></span>
* <span data-ttu-id="8eb5e-109">Vytvořte aplikaci, která volá metodu přímé restartování v aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-109">Create an app that invokes the reboot direct method in the simulated device app through your IoT hub.</span></span> <span data-ttu-id="8eb5e-110">Tuto aplikaci poté sleduje hlášen vlastnostech ze zařízení, které najdete v části po dokončení operace restartování.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-110">This app then monitors the reported properties from the device to see when the reboot operation is complete.</span></span>

<span data-ttu-id="8eb5e-111">Na konci tohoto kurzu máte dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-111">At the end of this tutorial, you have two Java console apps:</span></span>

<span data-ttu-id="8eb5e-112">**simulated-device**.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-112">**simulated-device**.</span></span> <span data-ttu-id="8eb5e-113">Tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-113">This app:</span></span>

* <span data-ttu-id="8eb5e-114">Připojí se ke službě IoT hub s dříve vytvořenou identitou zařízení.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-114">Connects to your IoT hub with the device identity created earlier.</span></span>
* <span data-ttu-id="8eb5e-115">Přijme hovor přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-115">Receives a reboot direct method call.</span></span>
* <span data-ttu-id="8eb5e-116">Simuluje fyzické restartování.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-116">Simulates a physical reboot.</span></span>
* <span data-ttu-id="8eb5e-117">Čas poslední restartování prostřednictvím hlášené vlastnosti sestavy.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-117">Reports the time of the last reboot through a reported property.</span></span>

<span data-ttu-id="8eb5e-118">**aktivační událost restartování**.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-118">**trigger-reboot**.</span></span> <span data-ttu-id="8eb5e-119">Tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-119">This app:</span></span>

* <span data-ttu-id="8eb5e-120">Přímá metoda volá v aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-120">Calls a direct method in the simulated device app.</span></span>
* <span data-ttu-id="8eb5e-121">Zobrazí odpověď na volání metody přímé poslal simulované zařízení</span><span class="sxs-lookup"><span data-stu-id="8eb5e-121">Displays the response to the direct method call sent by the simulated device</span></span>
* <span data-ttu-id="8eb5e-122">Zobrazí aktualizovaná hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-122">Displays the updated reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="8eb5e-123">Informace o sadách SDK, které můžete použít k vytváření aplikací pro spuštění na zařízení a back end vašeho řešení najdete v tématu [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="8eb5e-123">For information about the SDKs that you can use to build applications to run on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="8eb5e-124">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-124">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="8eb5e-125">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-125">Java SE 8.</span></span> <br/> <span data-ttu-id="8eb5e-126">Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace Javy pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-126">[Prepare your development environment][lnk-dev-setup] describes how to install Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="8eb5e-127">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-127">Maven 3.</span></span>  <br/> <span data-ttu-id="8eb5e-128">Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace nástroje [Maven][lnk-maven] pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-128">[Prepare your development environment][lnk-dev-setup] describes how to install [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="8eb5e-129">[Verze Node.js 0.10.0 nebo novější](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="8eb5e-129">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a><span data-ttu-id="8eb5e-130">Aktivační události restartu vzdálené v zařízení s přímá metoda</span><span class="sxs-lookup"><span data-stu-id="8eb5e-130">Trigger a remote reboot on the device using a direct method</span></span>

<span data-ttu-id="8eb5e-131">V této části vytvoříte konzolovou aplikaci Java:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-131">In this section, you create a Java console app that:</span></span>

1. <span data-ttu-id="8eb5e-132">Vyvolá metodu přímé restartování v aplikaci simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-132">Invokes the reboot direct method in the simulated device app.</span></span>
1. <span data-ttu-id="8eb5e-133">Zobrazí odpověď.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-133">Displays the response.</span></span>
1. <span data-ttu-id="8eb5e-134">Hlasování hlášen vlastnostech odesílaných ze zařízení k určení po dokončení restartování.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-134">Polls the reported properties sent from the device to determine when the reboot is complete.</span></span>

<span data-ttu-id="8eb5e-135">Tato Konzolová aplikace připojí do služby IoT Hub pro vyvolání metody přímé a čtení hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-135">This console app connects to your IoT Hub to invoke the direct method and read the reported properties.</span></span>

1. <span data-ttu-id="8eb5e-136">Vytvořte prázdnou složku s názvem dm-get-started.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-136">Create an empty folder called dm-get-started.</span></span>

1. <span data-ttu-id="8eb5e-137">Ve složce dm-get-started vytvořte projekt Maven s názvem **restartování aktivační událost** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-137">In the dm-get-started folder, create a Maven project called **trigger-reboot** using the following command at your command prompt.</span></span> <span data-ttu-id="8eb5e-138">Následující obrázek znázorňuje jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-138">The following shows a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="8eb5e-139">Na příkazovém řádku přejděte do složky restartování aktivační události.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-139">At your command prompt, navigate to the trigger-reboot folder.</span></span>

1. <span data-ttu-id="8eb5e-140">Pomocí textového editoru, otevřete soubor pom.xml ve složce restartování aktivační události a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-140">Using a text editor, open the pom.xml file in the trigger-reboot folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="8eb5e-141">Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-141">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="8eb5e-142">Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="8eb5e-142">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="8eb5e-143">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-143">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="8eb5e-144">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-144">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="8eb5e-145">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-145">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="8eb5e-146">Pomocí textového editoru otevřete trigger-reboot\src\main\java\com\mycompany\app\App.java zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-146">Using a text editor, open the trigger-reboot\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="8eb5e-147">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-147">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="8eb5e-148">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-148">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="8eb5e-149">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-149">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. <span data-ttu-id="8eb5e-150">Chcete-li implementovat podproces, který čte hlášené vlastnosti z dvojče zařízení každých 10 sekund, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-150">To implement a thread that reads the reported properties from the device twin every 10 seconds, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="8eb5e-151">K vyvolání metody přímé restartování na simulované zařízení, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-151">To invoke the reboot direct method on the simulated device, add the following code to the **main** method:</span></span>

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

1. <span data-ttu-id="8eb5e-152">Chcete-li spustit vlákno pro cyklické dotazování hlášen vlastnostech ze simulovaného zařízení, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-152">To start the thread to poll the reported properties from the simulated device, add the following code to the **main** method:</span></span>

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. <span data-ttu-id="8eb5e-153">Chcete-li umožňují zastavte aplikaci, přidejte následující kód do **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-153">To enable you to stop the app, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. <span data-ttu-id="8eb5e-154">Uložte a zavřete soubor trigger-reboot\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-154">Save and close the trigger-reboot\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="8eb5e-155">Sestavení **restartování aktivační událost** back-end aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-155">Build the **trigger-reboot** back-end app and correct any errors.</span></span> <span data-ttu-id="8eb5e-156">Na příkazovém řádku přejděte do složky, restartování aktivační události a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-156">At your command prompt, navigate to the trigger-reboot folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="8eb5e-157">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="8eb5e-157">Create a simulated device app</span></span>

<span data-ttu-id="8eb5e-158">V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-158">In this section, you create a Java console app that simulates a device.</span></span> <span data-ttu-id="8eb5e-159">Aplikace naslouchá restartování přímé volání metody ze služby IoT hub a okamžitě odpoví na toto volání.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-159">The app listens for the reboot direct method call from your IoT hub and immediately responds to that call.</span></span> <span data-ttu-id="8eb5e-160">Aplikace pak chvíli k simulaci procesu restartování předtím, než použije hlášené vlastnost oznámení v režimu spánku **restartování aktivační událost** back-end aplikačním dokončení restartování.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-160">The app then sleeps for a while to simulate the reboot process before it uses a reported property to notify the **trigger-reboot** back-end app that the reboot is complete.</span></span>

1. <span data-ttu-id="8eb5e-161">Ve složce dm-get-started vytvořte projekt Maven s názvem **simulated-device** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-161">In the dm-get-started folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="8eb5e-162">Toto je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-162">The following is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="8eb5e-163">Na příkazovém řádku přejděte do složky simulated-devices.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-163">At your command prompt, navigate to the simulated-device folder.</span></span>

1. <span data-ttu-id="8eb5e-164">Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-164">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="8eb5e-165">Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-165">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="8eb5e-166">Můžete vyhledat nejnovější verzi **iot-device-client** pomocí [vyhledávání Maven][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="8eb5e-166">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="8eb5e-167">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-167">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="8eb5e-168">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-168">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="8eb5e-169">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-169">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="8eb5e-170">Pomocí textového editoru, otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java zdroje.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-170">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="8eb5e-171">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-171">Add the following **import** statements to the file:</span></span>

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

1. <span data-ttu-id="8eb5e-172">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-172">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="8eb5e-173">Nahraďte `{yourdeviceconnectionstring}` jste si poznamenali v připojovacím řetězcem zařízení *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-173">Replace `{yourdeviceconnectionstring}` with the device connection string you noted in the *Create a device identity* section:</span></span>

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. <span data-ttu-id="8eb5e-174">K implementaci obslužná rutina zpětného volání pro přímé metoda události stavu, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-174">To implement a callback handler for direct method status events, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="8eb5e-175">K implementaci obslužná rutina zpětného volání pro zařízení twin stav události, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-175">To implement a callback handler for device twin status events, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded to device twin operation with status " + status.name());
        }
    }
    ```

1. <span data-ttu-id="8eb5e-176">Chcete-li implementovat obslužná rutina zpětného volání pro vlastnosti události, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-176">To implement a callback handler for property events, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="8eb5e-177">Chcete-li implementovat vlákno k simulaci restartování zařízení, přidejte následující vnořenou třídu k **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-177">To implement a thread to simulate the device reboot, add the following nested class to the **App** class.</span></span> <span data-ttu-id="8eb5e-178">Vlákno pět sekund v režimu spánku a poté nastaví **lastReboot** hlášené vlastnost:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-178">The thread sleeps for five seconds and then sets the **lastReboot** reported property:</span></span>

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

1. <span data-ttu-id="8eb5e-179">Chcete-li implementovat metodu přímé na zařízení, přidejte následující vnořenou třídu k **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-179">To implement the direct method on the device, add the following nested class to the **App** class.</span></span> <span data-ttu-id="8eb5e-180">Když simulované aplikace obdrží volání **restartovat** přímá metoda ji vrátí potvrzení volající a pak spustí vlákna ke zpracování restartování:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-180">When the simulated app receives a call to the **reboot** direct method, it returns an acknowledgement to the caller and then starts a thread to process the reboot:</span></span>

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

1. <span data-ttu-id="8eb5e-181">Upravte podpis metody **hlavní** metoda k vyvolání následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-181">Modify the signature of the **main** method to throw the following exceptions:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="8eb5e-182">K vytváření instancí **DeviceClient**, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-182">To instantiate a **DeviceClient**, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. <span data-ttu-id="8eb5e-183">Pokud chcete spustit přijímá volání přímá metoda, přidejte následující kód do **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-183">To start listening for direct method calls, add the following code to the **main** method:</span></span>

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed to direct methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="8eb5e-184">Chcete-li vypnout v simulátoru zařízení, přidejte následující kód do **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-184">To shut down the device simulator, add the following code to the **main** method:</span></span>

    ```java
    System.out.println("Press any key to exit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. <span data-ttu-id="8eb5e-185">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-185">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="8eb5e-186">Sestavení **simulated-device** back-end aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-186">Build the **simulated-device** back-end app and correct any errors.</span></span> <span data-ttu-id="8eb5e-187">Na příkazovém řádku přejděte do složky simulated-device a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-187">At your command prompt, navigate to the simulated-device folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="8eb5e-188">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="8eb5e-188">Run the apps</span></span>

<span data-ttu-id="8eb5e-189">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="8eb5e-189">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="8eb5e-190">V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby začal přijímat volání metody restartování ze služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-190">At a command prompt in the simulated-device folder, run the following command to begin listening for reboot method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Přímá metoda volání restartovat aplikaci simulovaného zařízení Java IoT Hub pro naslouchání][1]

1. <span data-ttu-id="8eb5e-192">Na příkazovém řádku ve složce Aktivační událost restartování spusťte následující příkaz k volání metody restartování na simulovaného zařízení ze služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-192">At a command prompt in the trigger-reboot folder, run the following command to call the reboot method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub k volání metody přímé restartování][2]

1. <span data-ttu-id="8eb5e-194">Simulované zařízení reaguje na volání metody přímé restartování:</span><span class="sxs-lookup"><span data-stu-id="8eb5e-194">The simulated device responds to the reboot direct method call:</span></span>

    ![Aplikaci simulovaného zařízení Java IoT Hub odpoví na volání přímá metoda][3]

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