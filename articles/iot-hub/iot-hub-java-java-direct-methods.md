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
# <a name="use-direct-methods-java"></a><span data-ttu-id="5476f-104">Používat přímé metody (Java)</span><span class="sxs-lookup"><span data-stu-id="5476f-104">Use direct methods (Java)</span></span>

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="5476f-105">V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="5476f-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="5476f-106">**vyvolání metody přímo**, back-end aplikaci Java, která volá metodu v aplikaci simulovaného zařízení hello a zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5476f-106">**invoke-direct-method**, a Java back-end app that calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="5476f-107">**simulated-device**, která simuluje zařízení připojení tooyour IoT hub s identitou zařízení hello vytvoříte aplikaci v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="5476f-107">**simulated-device**, a Java app that simulates a device connecting tooyour IoT hub with hello device identity you create.</span></span> <span data-ttu-id="5476f-108">Tato aplikace odpovídá toohello přímo volat z back-end hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-108">This app responds toohello direct invoked from hello back end.</span></span>

> [!NOTE]
> <span data-ttu-id="5476f-109">Informace o hello sady SDK, které můžete toorun toobuild aplikace na zařízení a back end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="5476f-109">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="5476f-110">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5476f-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="5476f-111">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="5476f-111">Java SE 8.</span></span> <br/> <span data-ttu-id="5476f-112">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Java v tomto kurzu v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="5476f-112">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="5476f-113">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="5476f-113">Maven 3.</span></span>  <br/> <span data-ttu-id="5476f-114">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall [Maven] [ lnk-maven] v tomto kurzu v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="5476f-114">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="5476f-115">[Verze Node.js 0.10.0 nebo novější](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="5476f-115">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="5476f-116">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="5476f-116">Create a simulated device app</span></span>

<span data-ttu-id="5476f-117">V této části vytvoříte konzolovou aplikaci Java, která odpovídá tooa metodu s názvem řešení hello zpět koncové.</span><span class="sxs-lookup"><span data-stu-id="5476f-117">In this section, you create a Java console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="5476f-118">Vytvořte prázdnou složku s názvem iot-java-direct-method.</span><span class="sxs-lookup"><span data-stu-id="5476f-118">Create an empty folder called iot-java-direct-method.</span></span>

1. <span data-ttu-id="5476f-119">Ve složce iot-java-direct-method hello vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5476f-119">In hello iot-java-direct-method folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="5476f-120">Následující příkaz Hello je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="5476f-120">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="5476f-121">Na příkazovém řádku přejděte toohello složky simulated Devices.</span><span class="sxs-lookup"><span data-stu-id="5476f-121">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="5476f-122">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislosti toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5476f-122">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="5476f-123">Tuto závislost umožňuje, aby vám toouse hello klienta zařízení iot balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5476f-123">This dependency enables you toouse hello iot-device-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="5476f-124">Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="5476f-124">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="5476f-125">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5476f-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="5476f-126">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="5476f-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="5476f-127">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-127">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="5476f-128">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-128">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="5476f-129">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="5476f-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="5476f-130">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="5476f-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="5476f-131">Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="5476f-131">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="5476f-132">Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="5476f-132">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="5476f-133">V současné době toouse přímé metody, je nutné použít protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-133">Currently, toouse direct methods you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="5476f-134">tooreturn stav kódu tooyour IoT hub, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="5476f-134">tooreturn a status code tooyour IoT hub, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="5476f-135">toohandle hello metoda přímé volání z hello back-end řešení, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="5476f-135">toohandle hello direct method invocations from hello solution back end, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="5476f-136">toocreate **DeviceClient** a naslouchat volání přímé metod, přidat **hlavní** metoda toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="5476f-136">toocreate a **DeviceClient** and listen for direct method invocations, add a **main** method toohello **App** class:</span></span>

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

1. <span data-ttu-id="5476f-137">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello</span><span class="sxs-lookup"><span data-stu-id="5476f-137">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="5476f-138">Sestavení hello **simulated-device** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="5476f-138">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="5476f-139">Na příkazovém řádku přejděte složky simulated Devices toohello a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5476f-139">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="5476f-140">Volání metody přímé na zařízení</span><span class="sxs-lookup"><span data-stu-id="5476f-140">Call a direct method on a device</span></span>

<span data-ttu-id="5476f-141">V této části vytvoříte konzolovou aplikaci Java, která volá metodu přímé a potom zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="5476f-141">In this section, you create a Java console app that invokes a direct method and then displays hello response.</span></span> <span data-ttu-id="5476f-142">Tato Konzolová aplikace připojí tooyour IoT Hub tooinvoke hello přímá metoda.</span><span class="sxs-lookup"><span data-stu-id="5476f-142">This console app connects tooyour IoT Hub tooinvoke hello direct method.</span></span>

1. <span data-ttu-id="5476f-143">Ve složce iot-java-direct-method hello vytvořte projekt Maven s názvem **vyvolání metody přímo** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5476f-143">In hello iot-java-direct-method folder, create a Maven project called **invoke-direct-method** using hello following command at your command prompt.</span></span> <span data-ttu-id="5476f-144">Následující příkaz Hello je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="5476f-144">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="5476f-145">Na příkazovém řádku přejděte toohello vyvolání metody přímo složku.</span><span class="sxs-lookup"><span data-stu-id="5476f-145">At your command prompt, navigate toohello invoke-direct-method folder.</span></span>

1. <span data-ttu-id="5476f-146">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce hello vyvolání metody přímo a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5476f-146">Using a text editor, open hello pom.xml file in hello invoke-direct-method folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="5476f-147">Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5476f-147">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="5476f-148">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="5476f-148">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="5476f-149">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5476f-149">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="5476f-150">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="5476f-150">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="5476f-151">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-151">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="5476f-152">Pomocí textového editoru otevřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-152">Using a text editor, open hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="5476f-153">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="5476f-153">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. <span data-ttu-id="5476f-154">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="5476f-154">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="5476f-155">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="5476f-155">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. <span data-ttu-id="5476f-156">Metoda hello tooinvoke na hello simulované zařízení, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="5476f-156">tooinvoke hello method on hello simulated device, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="5476f-157">Uložte a zavřete soubor invoke-direct-method\src\main\java\com\mycompany\app\App.java hello</span><span class="sxs-lookup"><span data-stu-id="5476f-157">Save and close hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="5476f-158">Sestavení hello **vyvolání metody přímo** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="5476f-158">Build hello **invoke-direct-method** app and correct any errors.</span></span> <span data-ttu-id="5476f-159">Na příkazovém řádku přejděte toohello vyvolání metody přímo složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5476f-159">At your command prompt, navigate toohello invoke-direct-method folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="5476f-160">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5476f-160">Run hello apps</span></span>

<span data-ttu-id="5476f-161">Nyní je připraven toorun hello konzole aplikace.</span><span class="sxs-lookup"><span data-stu-id="5476f-161">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="5476f-162">Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin přijímá metoda volání ze služby IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="5476f-162">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub Java simulované zařízení aplikaci toolisten pro volání přímá metoda][8]

1. <span data-ttu-id="5476f-164">Na příkazovém řádku ve složce hello vyvolání metody přímo spusťte následující příkaz toocall hello metodu na simulovaného zařízení ze služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5476f-164">At a command prompt in hello invoke-direct-method folder, run hello following command toocall a method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Toocall aplikace služby Java IoT Hub přímá metoda][7]

1. <span data-ttu-id="5476f-166">Simulované zařízení Hello odpoví toohello přímá metoda volání:</span><span class="sxs-lookup"><span data-stu-id="5476f-166">hello simulated device responds toohello direct method call:</span></span>

    ![Volání metody přímé toohello odpoví aplikaci simulovaného zařízení Java IoT Hub][9]

## <a name="next-steps"></a><span data-ttu-id="5476f-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5476f-168">Next steps</span></span>

<span data-ttu-id="5476f-169">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="5476f-169">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="5476f-170">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci tooreact toomethods vyvolané hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="5476f-170">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="5476f-171">Můžete také vytvořit aplikaci, která volá metody na hello zařízení a zobrazí hello odezvu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="5476f-171">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span>

<span data-ttu-id="5476f-172">tooexplore najdete v části dalších scénářů platformy IoT [plánování úloh na několika zařízeních][lnk-devguide-jobs].</span><span class="sxs-lookup"><span data-stu-id="5476f-172">tooexplore other IoT scenarios, see [Schedule jobs on multiple devices][lnk-devguide-jobs].</span></span>

<span data-ttu-id="5476f-173">toolearn o tooextend IoT řešení a plán metodu volá na několika zařízeních a v tématu hello [plán a všesměrového vysílání úlohy] [ lnk-tutorial-jobs] kurzu.</span><span class="sxs-lookup"><span data-stu-id="5476f-173">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
