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
# <a name="get-started-with-device-management-java"></a><span data-ttu-id="f248f-104">Začínáme se správou zařízení (Java)</span><span class="sxs-lookup"><span data-stu-id="f248f-104">Get started with device management (Java)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="f248f-105">V tomto kurzu získáte informace o následujících postupech:</span><span class="sxs-lookup"><span data-stu-id="f248f-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="f248f-106">Použijte hello portálu toocreate Azure IoT Hub a vytvoření identity zařízení ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f248f-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="f248f-107">Vytvoření aplikace simulovaného zařízení, která implementuje přímá metoda tooreboot hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="f248f-107">Create a simulated device app that implements a direct method tooreboot hello device.</span></span> <span data-ttu-id="f248f-108">Přímé metody jsou vyvolány z cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="f248f-109">Vytvořte aplikaci, která volá metodu přímé hello restartování v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="f248f-109">Create an app that invokes hello reboot direct method in hello simulated device app through your IoT hub.</span></span> <span data-ttu-id="f248f-110">Tato aplikace pak monitorování hello hlášené vlastnosti z toosee hello zařízení po dokončení operace restartování hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-110">This app then monitors hello reported properties from hello device toosee when hello reboot operation is complete.</span></span>

<span data-ttu-id="f248f-111">Na konci hello tohoto kurzu máte dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="f248f-111">At hello end of this tutorial, you have two Java console apps:</span></span>

<span data-ttu-id="f248f-112">**simulated-device**.</span><span class="sxs-lookup"><span data-stu-id="f248f-112">**simulated-device**.</span></span> <span data-ttu-id="f248f-113">Tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f248f-113">This app:</span></span>

* <span data-ttu-id="f248f-114">Připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-114">Connects tooyour IoT hub with hello device identity created earlier.</span></span>
* <span data-ttu-id="f248f-115">Přijme hovor přímá metoda restartování.</span><span class="sxs-lookup"><span data-stu-id="f248f-115">Receives a reboot direct method call.</span></span>
* <span data-ttu-id="f248f-116">Simuluje fyzické restartování.</span><span class="sxs-lookup"><span data-stu-id="f248f-116">Simulates a physical reboot.</span></span>
* <span data-ttu-id="f248f-117">Sestavy hello čas poslední restartování hello prostřednictvím hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f248f-117">Reports hello time of hello last reboot through a reported property.</span></span>

<span data-ttu-id="f248f-118">**aktivační událost restartování**.</span><span class="sxs-lookup"><span data-stu-id="f248f-118">**trigger-reboot**.</span></span> <span data-ttu-id="f248f-119">Tuto aplikaci:</span><span class="sxs-lookup"><span data-stu-id="f248f-119">This app:</span></span>

* <span data-ttu-id="f248f-120">Přímá metoda volá v aplikaci simulovaného zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-120">Calls a direct method in hello simulated device app.</span></span>
* <span data-ttu-id="f248f-121">Zobrazí hello odpovědi toohello přímé volání metody, které poslal hello simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="f248f-121">Displays hello response toohello direct method call sent by hello simulated device</span></span>
* <span data-ttu-id="f248f-122">Zobrazí hello aktualizovat hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f248f-122">Displays hello updated reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="f248f-123">Informace o hello sady SDK, které můžete toorun toobuild aplikace na zařízení a back end vašeho řešení, najdete v části [SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="f248f-123">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="f248f-124">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="f248f-124">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="f248f-125">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="f248f-125">Java SE 8.</span></span> <br/> <span data-ttu-id="f248f-126">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall Java v tomto kurzu v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="f248f-126">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="f248f-127">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="f248f-127">Maven 3.</span></span>  <br/> <span data-ttu-id="f248f-128">[Příprava vývojového prostředí] [ lnk-dev-setup] popisuje, jak tooinstall [Maven] [ lnk-maven] v tomto kurzu v systému Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="f248f-128">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="f248f-129">[Verze Node.js 0.10.0 nebo novější](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="f248f-129">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="f248f-130">Aktivační události restartu vzdálené na zařízení hello pomocí přímá metoda</span><span class="sxs-lookup"><span data-stu-id="f248f-130">Trigger a remote reboot on hello device using a direct method</span></span>

<span data-ttu-id="f248f-131">V této části vytvoříte konzolovou aplikaci Java:</span><span class="sxs-lookup"><span data-stu-id="f248f-131">In this section, you create a Java console app that:</span></span>

1. <span data-ttu-id="f248f-132">Vyvolá metodu přímé hello restartování v aplikaci simulovaného zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-132">Invokes hello reboot direct method in hello simulated device app.</span></span>
1. <span data-ttu-id="f248f-133">Zobrazí hello odpovědi.</span><span class="sxs-lookup"><span data-stu-id="f248f-133">Displays hello response.</span></span>
1. <span data-ttu-id="f248f-134">Hlasování hello hlášené vlastnosti odeslaný toodetermine hello zařízení po dokončení restartování hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-134">Polls hello reported properties sent from hello device toodetermine when hello reboot is complete.</span></span>

<span data-ttu-id="f248f-135">Tato Konzolová aplikace připojí tooyour IoT Hub tooinvoke hello přímá metoda a čtení hello hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f248f-135">This console app connects tooyour IoT Hub tooinvoke hello direct method and read hello reported properties.</span></span>

1. <span data-ttu-id="f248f-136">Vytvořte prázdnou složku s názvem dm-get-started.</span><span class="sxs-lookup"><span data-stu-id="f248f-136">Create an empty folder called dm-get-started.</span></span>

1. <span data-ttu-id="f248f-137">Ve složce hello dm-get-started vytvořte projekt Maven s názvem **restartování aktivační událost** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="f248f-137">In hello dm-get-started folder, create a Maven project called **trigger-reboot** using hello following command at your command prompt.</span></span> <span data-ttu-id="f248f-138">Následující Hello zobrazuje jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="f248f-138">hello following shows a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f248f-139">Na příkazovém řádku přejděte složky toohello restartování aktivační události.</span><span class="sxs-lookup"><span data-stu-id="f248f-139">At your command prompt, navigate toohello trigger-reboot folder.</span></span>

1. <span data-ttu-id="f248f-140">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce hello restartování aktivační události a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f248f-140">Using a text editor, open hello pom.xml file in hello trigger-reboot folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="f248f-141">Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="f248f-141">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f248f-142">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="f248f-142">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="f248f-143">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f248f-143">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="f248f-144">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="f248f-144">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="f248f-145">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-145">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="f248f-146">Pomocí textového editoru otevřete hello trigger-reboot\src\main\java\com\mycompany\app\App.java zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="f248f-146">Using a text editor, open hello trigger-reboot\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="f248f-147">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="f248f-147">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="f248f-148">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="f248f-148">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="f248f-149">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="f248f-149">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. <span data-ttu-id="f248f-150">tooimplement podproces, který čte hello hlášené vlastnosti z dvojče zařízení hello každých 10 sekund, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="f248f-150">tooimplement a thread that reads hello reported properties from hello device twin every 10 seconds, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="f248f-151">Přímá metoda restartování tooinvoke hello na hello simulované zařízení, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-151">tooinvoke hello reboot direct method on hello simulated device, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="f248f-152">toostart hello vlákno toopoll hello hlášené vlastnosti z hello simulovaného zařízení, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-152">toostart hello thread toopoll hello reported properties from hello simulated device, add hello following code toohello **main** method:</span></span>

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. <span data-ttu-id="f248f-153">tooenable toostop hello aplikace, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-153">tooenable you toostop hello app, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. <span data-ttu-id="f248f-154">Uložte a zavřete soubor trigger-reboot\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-154">Save and close hello trigger-reboot\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="f248f-155">Sestavení hello **restartování aktivační událost** back-end aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="f248f-155">Build hello **trigger-reboot** back-end app and correct any errors.</span></span> <span data-ttu-id="f248f-156">Na příkazovém řádku přejděte toohello restartování aktivační událost složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f248f-156">At your command prompt, navigate toohello trigger-reboot folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="f248f-157">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="f248f-157">Create a simulated device app</span></span>

<span data-ttu-id="f248f-158">V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení.</span><span class="sxs-lookup"><span data-stu-id="f248f-158">In this section, you create a Java console app that simulates a device.</span></span> <span data-ttu-id="f248f-159">Hello aplikace čeká na restartování hello přímá metoda. volání ze služby IoT hub a okamžitě odpoví toothat volání.</span><span class="sxs-lookup"><span data-stu-id="f248f-159">hello app listens for hello reboot direct method call from your IoT hub and immediately responds toothat call.</span></span> <span data-ttu-id="f248f-160">Hello aplikace pak chvíli v režimu spánku toosimulate hello restartování procesu předtím, než použije hlášené vlastnost toonotify hello **restartování aktivační událost** back-end aplikace, která hello restartování je dokončena.</span><span class="sxs-lookup"><span data-stu-id="f248f-160">hello app then sleeps for a while toosimulate hello reboot process before it uses a reported property toonotify hello **trigger-reboot** back-end app that hello reboot is complete.</span></span>

1. <span data-ttu-id="f248f-161">Ve složce hello dm-get-started vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="f248f-161">In hello dm-get-started folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="f248f-162">Hello následuje jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="f248f-162">hello following is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f248f-163">Na příkazovém řádku přejděte toohello složky simulated Devices.</span><span class="sxs-lookup"><span data-stu-id="f248f-163">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="f248f-164">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f248f-164">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="f248f-165">Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="f248f-165">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f248f-166">Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="f248f-166">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="f248f-167">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f248f-167">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="f248f-168">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="f248f-168">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="f248f-169">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-169">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="f248f-170">Pomocí textového editoru otevřete hello simulated-device\src\main\java\com\mycompany\app\App.java zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="f248f-170">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="f248f-171">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="f248f-171">Add hello following **import** statements toohello file:</span></span>

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

1. <span data-ttu-id="f248f-172">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="f248f-172">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="f248f-173">Nahraďte `{yourdeviceconnectionstring}` s řetězcem připojení zařízení hello jste si poznamenali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="f248f-173">Replace `{yourdeviceconnectionstring}` with hello device connection string you noted in hello *Create a device identity* section:</span></span>

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. <span data-ttu-id="f248f-174">tooimplement obslužná rutina zpětného volání pro přímé metoda události stavu, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="f248f-174">tooimplement a callback handler for direct method status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="f248f-175">tooimplement obslužná rutina zpětného volání pro události twin stavu zařízení, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="f248f-175">tooimplement a callback handler for device twin status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. <span data-ttu-id="f248f-176">tooimplement obslužná rutina zpětného volání pro vlastnosti události, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="f248f-176">tooimplement a callback handler for property events, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="f248f-177">tooimplement restartu vlákno toosimulate hello zařízení přidat hello následující vnořené třídy toohello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="f248f-177">tooimplement a thread toosimulate hello device reboot, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="f248f-178">vlákno Hello pět sekund v režimu spánku a poté nastaví hello **lastReboot** hlášené vlastnost:</span><span class="sxs-lookup"><span data-stu-id="f248f-178">hello thread sleeps for five seconds and then sets hello **lastReboot** reported property:</span></span>

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

1. <span data-ttu-id="f248f-179">Přímá metoda hello tooimplement na hello zařízení, přidejte následující hello vnořené třídy toohello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="f248f-179">tooimplement hello direct method on hello device, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="f248f-180">Když simulované aplikace hello obdrží volání toohello **restartovat** přímá metoda vrátí volající toohello potvrzení a pak spustí vlákno tooprocess hello restartovat:</span><span class="sxs-lookup"><span data-stu-id="f248f-180">When hello simulated app receives a call toohello **reboot** direct method, it returns an acknowledgement toohello caller and then starts a thread tooprocess hello reboot:</span></span>

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

1. <span data-ttu-id="f248f-181">Upravte podpis hello hello **hlavní** hello toothrow metoda následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="f248f-181">Modify hello signature of hello **main** method toothrow hello following exceptions:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="f248f-182">tooinstantiate **DeviceClient**, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-182">tooinstantiate a **DeviceClient**, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. <span data-ttu-id="f248f-183">toostart přijímá volání přímá metoda, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-183">toostart listening for direct method calls, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="f248f-184">tooshut dolů hello simulátor zařízení, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="f248f-184">tooshut down hello device simulator, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. <span data-ttu-id="f248f-185">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="f248f-185">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="f248f-186">Sestavení hello **simulated-device** back-end aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="f248f-186">Build hello **simulated-device** back-end app and correct any errors.</span></span> <span data-ttu-id="f248f-187">Na příkazovém řádku přejděte složky simulated Devices toohello a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f248f-187">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="f248f-188">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="f248f-188">Run hello apps</span></span>

<span data-ttu-id="f248f-189">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="f248f-189">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="f248f-190">Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin přijímá restartování metoda volání ze služby IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="f248f-190">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for reboot method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![IoT Hub Java simulované zařízení aplikaci toolisten pro volání metod přímé restartování][1]

1. <span data-ttu-id="f248f-192">Na příkazovém řádku ve složce hello restartování aktivační událost spusťte následující příkaz toocall hello restartování metodu na simulovaného zařízení ze služby IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="f248f-192">At a command prompt in hello trigger-reboot folder, run hello following command toocall hello reboot method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hello toocall aplikace služby Java IoT Hub restartovat přímá metoda][2]

1. <span data-ttu-id="f248f-194">Simulované zařízení Hello odpoví toohello restartování přímá metoda volání:</span><span class="sxs-lookup"><span data-stu-id="f248f-194">hello simulated device responds toohello reboot direct method call:</span></span>

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