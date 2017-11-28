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
# <a name="get-started-with-device-twins-java"></a><span data-ttu-id="5b0bf-104">Začínáme s dvojčata zařízení (Java)</span><span class="sxs-lookup"><span data-stu-id="5b0bf-104">Get started with device twins (Java)</span></span>

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="5b0bf-105">V tomto kurzu vytvoříte dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="5b0bf-106">**Přidání dotazu značky**, back-end aplikaci Java, která přidá značky a dotazuje dvojčata zařízení.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-106">**add-tags-query**, a Java back-end app that adds tags and queries device twins.</span></span>
* <span data-ttu-id="5b0bf-107">**simulated-device**, aplikace v jazyce Java zařízení, která se připojuje tooyour IoT hub a sestav stavu připojení při používání hlášené vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-107">**simulated-device**, a Java device app that that connects tooyour IoT hub and reports its connectivity condition using a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="5b0bf-108">článek Hello [SDK služby Azure IoT](iot-hub-devguide-sdks.md) poskytuje informace o hello SDK služby Azure IoT, které můžete použít toobuild zařízení i back-end aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-108">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

<span data-ttu-id="5b0bf-109">toocomplete tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="5b0bf-110">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="5b0bf-110">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="5b0bf-111">Maven 3</span><span class="sxs-lookup"><span data-stu-id="5b0bf-111">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="5b0bf-112">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-112">An active Azure account.</span></span> <span data-ttu-id="5b0bf-113">(Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="5b0bf-113">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="5b0bf-114">Pokud dáváte přednost identitu zařízení hello toocreate prostřednictvím kódu programu, najdete v hello hello odpovídající části [připojit zařízení tooyour IoT hub pomocí jazyka Java](iot-hub-java-java-getstarted.md#create-a-device-identity) článku.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-114">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="5b0bf-115">Vytvoření aplikace hello service</span><span class="sxs-lookup"><span data-stu-id="5b0bf-115">Create hello service app</span></span>

<span data-ttu-id="5b0bf-116">V této části vytvoříte aplikaci Java, která přidá umístění metadat jako přidružený dvojče značky toohello zařízení IoT hub **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-116">In this section, you create a Java app that adds location metadata as a tag toohello device twin in IoT Hub associated with **myDeviceId**.</span></span> <span data-ttu-id="5b0bf-117">aplikace Hello nejprve dotazuje služby IoT hub pro zařízení, které jsou umístěné v hello USA a potom pro zařízení, která sestavy připojení k mobilní síti.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-117">hello app first queries IoT hub for devices located in hello US, and then for devices that report a cellular network connection.</span></span>

1. <span data-ttu-id="5b0bf-118">Na vývojovém počítači, vytvořte prázdnou složku s názvem `iot-java-twin-getstarted`.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-118">On your development machine, create an empty folder called `iot-java-twin-getstarted`.</span></span>

1. <span data-ttu-id="5b0bf-119">V hello `iot-java-twin-getstarted` složku vytvořit projekt Maven s názvem **přidat dotazu značky** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-119">In hello `iot-java-twin-getstarted` folder, create a Maven project called **add-tags-query** using hello following command at your command prompt.</span></span> <span data-ttu-id="5b0bf-120">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-120">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="5b0bf-121">Na příkazovém řádku přejděte toohello `add-tags-query` složky.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-121">At your command prompt, navigate toohello `add-tags-query` folder.</span></span>

1. <span data-ttu-id="5b0bf-122">Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `add-tags-query` složky a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-122">Using a text editor, open hello `pom.xml` file in hello `add-tags-query` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="5b0bf-123">Tuto závislost vám umožní toouse hello **klienta služby iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-123">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="5b0bf-124">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="5b0bf-124">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="5b0bf-125">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="5b0bf-126">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="5b0bf-127">Uložte a zavřete hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-127">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="5b0bf-128">Pomocí textového editoru otevřete hello `add-tags-query\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-128">Using a text editor, open hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="5b0bf-129">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. <span data-ttu-id="5b0bf-130">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="5b0bf-131">Nahraďte `{youriothubconnectionstring}` IoT hub připojovacím řetězcem jste si poznamenali v hello *vytvoření služby IoT Hub* části:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-131">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. <span data-ttu-id="5b0bf-132">Aktualizace hello **hlavní** metoda podpis tooinclude hello následující `throws` klauzule:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-132">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. <span data-ttu-id="5b0bf-133">Přidejte následující kód toohello hello **hlavní** metoda toocreate hello **DeviceTwin** a **DeviceTwinDevice** objekty.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-133">Add hello following code toohello **main** method toocreate hello **DeviceTwin** and **DeviceTwinDevice** objects.</span></span> <span data-ttu-id="5b0bf-134">Hello **DeviceTwin** objekt zpracovává hello komunikace se službou IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-134">hello **DeviceTwin** object handles hello communication with your IoT hub.</span></span> <span data-ttu-id="5b0bf-135">Hello **DeviceTwinDevice** objekt představuje hello dvojče zařízení s jeho vlastnosti a značky:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-135">hello **DeviceTwinDevice** object represents hello device twin with its properties and tags:</span></span>

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. <span data-ttu-id="5b0bf-136">Přidejte následující hello `try/catch` blokovat toohello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-136">Add hello following `try/catch` block toohello **main** method:</span></span>

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. <span data-ttu-id="5b0bf-137">tooupdate hello **oblast** a **závodu** značky twin zařízení ve vaší dvojče zařízení, přidejte následující kód v hello hello `try` bloku:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-137">tooupdate hello **region** and **plant** device twin tags in your device twin, add hello following code in hello `try` block:</span></span>

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

1. <span data-ttu-id="5b0bf-138">tooquery hello dvojčata zařízení IoT hub, přidejte následující kód toohello hello `try` blok za kódem hello jste přidali v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-138">tooquery hello device twins in IoT hub, add hello following code toohello `try` block after hello code you added in hello previous step.</span></span> <span data-ttu-id="5b0bf-139">Spustí kód Hello dva dotazy.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-139">hello code runs two queries.</span></span> <span data-ttu-id="5b0bf-140">Každý dotaz vrací maximálně 100 zařízení:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-140">Each query returns a maximum of 100 devices:</span></span>

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

1. <span data-ttu-id="5b0bf-141">Uložte a zavřete hello `add-tags-query\src\main\java\com\mycompany\app\App.java` souboru</span><span class="sxs-lookup"><span data-stu-id="5b0bf-141">Save and close hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="5b0bf-142">Sestavení hello **přidat dotazu značky** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-142">Build hello **add-tags-query** app and correct any errors.</span></span> <span data-ttu-id="5b0bf-143">Na příkazovém řádku přejděte toohello `add-tags-query` složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-143">At your command prompt, navigate toohello `add-tags-query` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="5b0bf-144">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="5b0bf-144">Create a device app</span></span>

<span data-ttu-id="5b0bf-145">V této části vytvoříte konzolovou aplikaci Java, která nastaví hlášené vlastnost hodnotu, která je odeslána tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-145">In this section, you create a Java console app that sets a reported property value that is sent tooIoT Hub.</span></span>

1. <span data-ttu-id="5b0bf-146">V hello `iot-java-twin-getstarted` složku vytvořit projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-146">In hello `iot-java-twin-getstarted` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="5b0bf-147">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-147">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="5b0bf-148">Na příkazovém řádku přejděte toohello `simulated-device` složky.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-148">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="5b0bf-149">Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `simulated-device` složky a přidejte následující závislosti toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-149">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="5b0bf-150">Tuto závislost vám umožní toouse hello **klienta zařízení iot** balíček ve vaší aplikaci toocommunicate službou IoT hub:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-150">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="5b0bf-151">Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="5b0bf-151">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="5b0bf-152">Přidejte následující hello **sestavení** uzlu po hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-152">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="5b0bf-153">Tato konfigurace se dá pokyn hello aplikace v jazyce Java 1,8 toobuild Maven toouse:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-153">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="5b0bf-154">Uložte a zavřete hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-154">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="5b0bf-155">Pomocí textového editoru otevřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-155">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="5b0bf-156">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-156">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="5b0bf-157">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-157">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="5b0bf-158">Nahrazení `{youriothubname}` názvem služby IoT hub, a `{yourdevicekey}` s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-158">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    <span data-ttu-id="5b0bf-159">Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-159">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="5b0bf-160">V současné době toouse twin funkce zařízení je nutné použít protokol MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-160">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="5b0bf-161">Přidejte následující kód toohello hello **hlavní** metody:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-161">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="5b0bf-162">Vytvořte zařízení klienta toocommunicate službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-162">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="5b0bf-163">Vytvoření **zařízení** toostore hello zařízení twin vlastnosti objektu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-163">Create a **Device** object toostore hello device twin properties.</span></span>

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

1. <span data-ttu-id="5b0bf-164">Přidejte následující kód toohello hello **hlavní** metoda toocreate **connectivityType** hlášené vlastnost a jeho odeslání tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-164">Add hello following code toohello **main** method toocreate a **connectivityType** reported property and send it tooIoT Hub:</span></span>

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

1. <span data-ttu-id="5b0bf-165">Přidejte následující kód toohello konec hello hello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-165">Add hello following code toohello end of hello **main** method.</span></span> <span data-ttu-id="5b0bf-166">Čeká se dobrý **Enter** klíč umožňuje dobu IoT Hub tooreport hello stav hello zařízení twin operací:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-166">Waiting for hello **Enter** key allows time for IoT Hub tooreport hello status of hello device twin operations:</span></span>

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. <span data-ttu-id="5b0bf-167">Uložte a zavřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-167">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="5b0bf-168">Sestavení hello **simulated-device** aplikace a opravte všechny chyby.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-168">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="5b0bf-169">Na příkazovém řádku přejděte toohello `simulated-device` složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-169">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="5b0bf-170">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5b0bf-170">Run hello apps</span></span>

<span data-ttu-id="5b0bf-171">Nyní je připraven toorun hello konzole aplikace.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-171">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="5b0bf-172">Na příkazovém řádku v hello `add-tags-query` složky, spusťte následující příkaz toorun hello hello **přidat dotazu značky** služby App Service:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-172">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Tooupdate aplikace služby Java IoT Hub značky hodnoty a spouštět dotazy zařízení](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    <span data-ttu-id="5b0bf-174">Můžete zobrazit hello **závodu** a **oblast** dvojče zařízení toohello přidat značky.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-174">You can see hello **plant** and **region** tags added toohello device twin.</span></span> <span data-ttu-id="5b0bf-175">Hello první dotaz vrátí zařízení, ale hello druhý není.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-175">hello first query returns your device, but hello second does not.</span></span>

1. <span data-ttu-id="5b0bf-176">Na příkazovém řádku v hello `simulated-device` složky, spusťte následující příkaz tooadd hello hello **connectivityType** hlášené dvojče zařízení toohello vlastnost:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-176">At a command prompt in hello `simulated-device` folder, run hello following command tooadd hello **connectivityType** reported property toohello device twin:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Přidá klienta zařízení Hello hello ** connectivityType ** hlášené vlastnost](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. <span data-ttu-id="5b0bf-178">Na příkazovém řádku v hello `add-tags-query` složky, spusťte následující příkaz toorun hello hello **přidat dotazu značky** aplikační služby ještě jednou:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-178">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app a second time:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Tooupdate aplikace služby Java IoT Hub značky hodnoty a spouštět dotazy zařízení](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    <span data-ttu-id="5b0bf-180">Nyní zařízení odeslal hello **connectivityType** vlastnost tooIoT centru, druhý hello dotaz vrátí zařízení.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-180">Now your device has sent hello **connectivityType** property tooIoT Hub, hello second query returns your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b0bf-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b0bf-181">Next steps</span></span>

<span data-ttu-id="5b0bf-182">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="5b0bf-183">Přidat zařízení metadat jako značky z back-end aplikace a napsali informací o zařízení aplikaci tooreport zařízení připojení v dvojče zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-183">You added device metadata as tags from a back-end app, and wrote a device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="5b0bf-184">Také jste zjistili, jak tooquery hello informace o dvojici zařízení pomocí jazyka dotazů SQL jako IoT Hub hello.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-184">You also learned how tooquery hello device twin information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="5b0bf-185">Použití hello následující toolearn prostředky jak pro:</span><span class="sxs-lookup"><span data-stu-id="5b0bf-185">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="5b0bf-186">Odesílat telemetrická data ze zařízení s hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-186">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="5b0bf-187">Kontroly nad zařízeními interaktivně (například zapnutí ventilátor z aplikace řízené uživatele) s hello [použít přímé metody](iot-hub-java-java-direct-methods.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="5b0bf-187">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
