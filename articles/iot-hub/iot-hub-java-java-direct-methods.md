---
title: "Používat přímé metody Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak používat Azure IoT Hub přímé metody. Zařízení Azure IoT SDK pro jazyk Java, kterou použijete k implementaci aplikaci ze simulovaného zařízení, která zahrnuje přímá metoda a sady SDK pro jazyk Java k implementaci aplikační služby, která volá metodu přímé služby Azure IoT."
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
ms.openlocfilehash: 6243a1a8cc971c53c797182b2beb6f594d2ac5f7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-direct-methods-java"></a><span data-ttu-id="4f283-104">Používat přímé metody (Java)</span><span class="sxs-lookup"><span data-stu-id="4f283-104">Use direct methods (Java)</span></span>

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="4f283-105">V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="4f283-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="4f283-106">**vyvolání metody přímo**, back-end aplikaci Java, která volá metodu v aplikaci simulovaného zařízení a zobrazí odpověď.</span><span class="sxs-lookup"><span data-stu-id="4f283-106">**invoke-direct-method**, a Java back-end app that calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="4f283-107">**simulated-device**, aplikaci Java, která simuluje zařízení do služby IoT hub s identitou zařízení můžete vytvořit připojení.</span><span class="sxs-lookup"><span data-stu-id="4f283-107">**simulated-device**, a Java app that simulates a device connecting to your IoT hub with the device identity you create.</span></span> <span data-ttu-id="4f283-108">Tato aplikace odpoví na přímo volat z back-end.</span><span class="sxs-lookup"><span data-stu-id="4f283-108">This app responds to the direct invoked from the back end.</span></span>

> [!NOTE]
> <span data-ttu-id="4f283-109">Informace o sadách SDK, které můžete použít k vytváření aplikací pro spuštění na zařízení a back end vašeho řešení najdete v tématu [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="4f283-109">For information about the SDKs that you can use to build applications to run on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="4f283-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="4f283-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="4f283-111">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="4f283-111">Java SE 8.</span></span> <br/> <span data-ttu-id="4f283-112">Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace Javy pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4f283-112">[Prepare your development environment][lnk-dev-setup] describes how to install Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="4f283-113">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="4f283-113">Maven 3.</span></span>  <br/> <span data-ttu-id="4f283-114">Kapitola [Příprava vývojového prostředí][lnk-dev-setup] popisuje postup instalace nástroje [Maven][lnk-maven] pro tento návod v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="4f283-114">[Prepare your development environment][lnk-dev-setup] describes how to install [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="4f283-115">[Verze Node.js 0.10.0 nebo novější](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="4f283-115">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="4f283-116">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="4f283-116">Create a simulated device app</span></span>

<span data-ttu-id="4f283-117">V této části vytvoříte konzolovou aplikaci Java, která reaguje na metodu s názvem řešení zpět end.</span><span class="sxs-lookup"><span data-stu-id="4f283-117">In this section, you create a Java console app that responds to a method called by the solution back end.</span></span>

1. <span data-ttu-id="4f283-118">Vytvořte prázdnou složku s názvem iot-java-direct-method.</span><span class="sxs-lookup"><span data-stu-id="4f283-118">Create an empty folder called iot-java-direct-method.</span></span>

1. <span data-ttu-id="4f283-119">Ve složce iot-java-direct-method vytvořit projekt Maven s názvem **simulated-device** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="4f283-119">In the iot-java-direct-method folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="4f283-120">Příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f283-120">The following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="4f283-121">Na příkazovém řádku přejděte do složky simulated-devices.</span><span class="sxs-lookup"><span data-stu-id="4f283-121">At your command prompt, navigate to the simulated-device folder.</span></span>

1. <span data-ttu-id="4f283-122">Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti.</span><span class="sxs-lookup"><span data-stu-id="4f283-122">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="4f283-123">Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta zařízení iot ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4f283-123">This dependency enables you to use the iot-device-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="4f283-124">Můžete vyhledat nejnovější verzi **iot-device-client** pomocí [vyhledávání Maven][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="4f283-124">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="4f283-125">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f283-125">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="4f283-126">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="4f283-126">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="4f283-127">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="4f283-127">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="4f283-128">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="4f283-128">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="4f283-129">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="4f283-129">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="4f283-130">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="4f283-130">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="4f283-131">Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` klíčem zařízení hodnotou, kterou jste vygenerovaných *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="4f283-131">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="4f283-132">Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**.</span><span class="sxs-lookup"><span data-stu-id="4f283-132">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="4f283-133">V současné době používání přímé metod musí používat protokol MQTT.</span><span class="sxs-lookup"><span data-stu-id="4f283-133">Currently, to use direct methods you must use the MQTT protocol.</span></span>

1. <span data-ttu-id="4f283-134">Chcete-li vrátit stavový kód do služby IoT hub, přidejte následující vnořenou třídu k **aplikace** – třída:</span><span class="sxs-lookup"><span data-stu-id="4f283-134">To return a status code to your IoT hub, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="4f283-135">Pro zpracování metoda přímé volání z back-end řešení, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="4f283-135">To handle the direct method invocations from the solution back end, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="4f283-136">K vytvoření **DeviceClient** a naslouchat volání přímé metod, přidat **hlavní** metodu **aplikace** – třída:</span><span class="sxs-lookup"><span data-stu-id="4f283-136">To create a **DeviceClient** and listen for direct method invocations, add a **main** method to the **App** class:</span></span>

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
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="4f283-137">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java</span><span class="sxs-lookup"><span data-stu-id="4f283-137">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="4f283-138">Sestavení **simulated-device** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="4f283-138">Build the **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="4f283-139">Na příkazovém řádku přejděte do složky simulated-device a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f283-139">At your command prompt, navigate to the simulated-device folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="4f283-140">Volání metody přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="4f283-140">Call a direct method on a device</span></span>

<span data-ttu-id="4f283-141">V této části vytvoříte konzolovou aplikaci Java, která volá metodu přímé a potom zobrazí odpověď.</span><span class="sxs-lookup"><span data-stu-id="4f283-141">In this section, you create a Java console app that invokes a direct method and then displays the response.</span></span> <span data-ttu-id="4f283-142">Tato Konzolová aplikace připojí do služby IoT Hub k vyvolání metody direct.</span><span class="sxs-lookup"><span data-stu-id="4f283-142">This console app connects to your IoT Hub to invoke the direct method.</span></span>

1. <span data-ttu-id="4f283-143">Ve složce iot-java-direct-method vytvořit projekt Maven s názvem **vyvolání metody přímo** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="4f283-143">In the iot-java-direct-method folder, create a Maven project called **invoke-direct-method** using the following command at your command prompt.</span></span> <span data-ttu-id="4f283-144">Příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f283-144">The following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="4f283-145">Na příkazovém řádku přejděte do složky invoke-direct-method.</span><span class="sxs-lookup"><span data-stu-id="4f283-145">At your command prompt, navigate to the invoke-direct-method folder.</span></span>

1. <span data-ttu-id="4f283-146">Pomocí textového editoru, otevřete soubor pom.xml ve složce metodu invoke přímo a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f283-146">Using a text editor, open the pom.xml file in the invoke-direct-method folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="4f283-147">Tuto závislost umožňuje komunikovat se službou IoT hub pomocí balíčku klienta služby iot ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="4f283-147">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="4f283-148">Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="4f283-148">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="4f283-149">Přidejte následující **sestavení** uzlu po **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="4f283-149">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="4f283-150">Tato konfigurace se dá pokyn Maven k sestavení aplikace pomocí Java 1.8:</span><span class="sxs-lookup"><span data-stu-id="4f283-150">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="4f283-151">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="4f283-151">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="4f283-152">Pomocí textového editoru otevřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="4f283-152">Using a text editor, open the invoke-direct-method\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="4f283-153">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="4f283-153">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. <span data-ttu-id="4f283-154">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="4f283-154">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="4f283-155">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="4f283-155">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. <span data-ttu-id="4f283-156">K vyvolání metody na simulované zařízení, přidejte následující kód, který **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="4f283-156">To invoke the method on the simulated device, add the following code to the **main** method:</span></span>

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

1. <span data-ttu-id="4f283-157">Uložte a zavřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java</span><span class="sxs-lookup"><span data-stu-id="4f283-157">Save and close the invoke-direct-method\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="4f283-158">Sestavení **vyvolání metody přímo** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="4f283-158">Build the **invoke-direct-method** app and correct any errors.</span></span> <span data-ttu-id="4f283-159">Na příkazovém řádku přejděte do složky, metodu invoke přímo a spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4f283-159">At your command prompt, navigate to the invoke-direct-method folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="4f283-160">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="4f283-160">Run the apps</span></span>

<span data-ttu-id="4f283-161">Nyní jste připraveni ke spuštění aplikace konzoly.</span><span class="sxs-lookup"><span data-stu-id="4f283-161">You are now ready to run the console apps.</span></span>

1. <span data-ttu-id="4f283-162">V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby začal přijímat volání metody ze služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="4f283-162">At a command prompt in the simulated-device folder, run the following command to begin listening for method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikaci simulovaného zařízení Java IoT Hub naslouchat volání přímá metoda][8]

1. <span data-ttu-id="4f283-164">Na příkazovém řádku ve složce metodu invoke přímo spusťte následující příkaz pro volání metody na simulovaného zařízení ze služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="4f283-164">At a command prompt in the invoke-direct-method folder, run the following command to call a method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Aplikace služby Java IoT Hub volat přímá metoda][7]

1. <span data-ttu-id="4f283-166">Simulované zařízení reaguje na volání přímé metody:</span><span class="sxs-lookup"><span data-stu-id="4f283-166">The simulated device responds to the direct method call:</span></span>

    ![Aplikaci simulovaného zařízení Java IoT Hub odpoví na volání přímá metoda][9]

## <a name="next-steps"></a><span data-ttu-id="4f283-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f283-168">Next steps</span></span>

<span data-ttu-id="4f283-169">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="4f283-169">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="4f283-170">Povolit aplikaci simulovaného zařízení reagování na metody vyvolané cloudu použijete tuto identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="4f283-170">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="4f283-171">Můžete také vytvořit aplikaci, která volá metody na zařízení a zobrazí odpověď ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="4f283-171">You also created an app that invokes methods on the device and displays the response from the device.</span></span>

<span data-ttu-id="4f283-172">Chcete-li prozkoumat dalších scénářů platformy IoT, přečtěte si téma [plánování úloh na několika zařízeních][lnk-devguide-jobs].</span><span class="sxs-lookup"><span data-stu-id="4f283-172">To explore other IoT scenarios, see [Schedule jobs on multiple devices][lnk-devguide-jobs].</span></span>

<span data-ttu-id="4f283-173">Zjistěte, jak rozšířit vaše IoT řešení a plán metoda volá na několika zařízeních, najdete v článku [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="4f283-173">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
