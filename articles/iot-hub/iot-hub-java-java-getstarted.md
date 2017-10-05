---
title: "Začínáme se službou Azure IoT Hub (Java) | Dokumentace Microsoftu"
description: "Zjistěte, jak odesílat zprávy typu zařízení-cloud do služby Azure IoT Hub pomocí sad IoT SDK pro Javu. Vytvořte simulované zařízení a aplikace služeb pro registraci vašeho zařízení, odesílání zpráv a čtení zpráv ze služby IoT Hub."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 707356a49970bcd76a55ee1b8a6fbddf6a6ba390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a><span data-ttu-id="97fd3-104">Připojení zařízení ke službě IoT Hub pomocí Javy</span><span class="sxs-lookup"><span data-stu-id="97fd3-104">Connect your device to your IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="97fd3-105">Na konci tohoto kurzu budete mít tři konzolové aplikace Java:</span><span class="sxs-lookup"><span data-stu-id="97fd3-105">At the end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="97fd3-106">**create-device-identity** vytváří identitu zařízení a přidružený klíč zabezpečení k připojení aplikace pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="97fd3-106">**create-device-identity**, which creates a device identity and associated security key to connect your device app.</span></span>
* <span data-ttu-id="97fd3-107">**read-d2c-messages** zobrazuje telemetrická data odesílaná aplikací pro zařízení.</span><span class="sxs-lookup"><span data-stu-id="97fd3-107">**read-d2c-messages**, which displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="97fd3-108">**simulated-device** propojuje službu IoT Hub s dříve vytvořenou identitou zařízení a každou druhou sekundu zasílá telemetrickou zprávu pomocí protokolu MQTT.</span><span class="sxs-lookup"><span data-stu-id="97fd3-108">**simulated-device**, which connects to your IoT hub with the device identity created earlier, and sends a telemetry message every second using the MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="97fd3-109">Informace o sadách Azure IoT SDK, s jejichž pomocí můžete vytvářet aplikace pro zařízení i back-end vašeho řešení, najdete v tématu [Sady SDK služby Azure IoT][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="97fd3-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both apps to run on devices and your solution back end.</span></span>

<span data-ttu-id="97fd3-110">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="97fd3-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="97fd3-111">Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="97fd3-111">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="97fd3-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="97fd3-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="97fd3-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="97fd3-113">An active Azure account.</span></span> <span data-ttu-id="97fd3-114">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="97fd3-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="97fd3-115">Na závěr si poznamenejte hodnotu **Primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-115">As a final step, make a note of the **Primary key** value.</span></span> <span data-ttu-id="97fd3-116">Potom klikněte na **Koncové body** a na integrovaný koncový bod **Události**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-116">Then click **Endpoints** and the **Events** built-in endpoint.</span></span> <span data-ttu-id="97fd3-117">V okně **Vlastnosti** si poznamenejte **název kompatibilní s centrem událostí** a adresu **koncového bodu kompatibilního s centrem událostí**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-117">On the **Properties** blade, make a note of the **Event Hub-compatible name** and the **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="97fd3-118">Tyto tři hodnoty budete potřebovat k vytvoření aplikace **read-d2c-messages**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Okno Zasílání zpráv služby IoT Hub na webu Azure Portal][6]

<span data-ttu-id="97fd3-120">Nyní jste vytvořili svůj IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-120">You have now created your IoT hub.</span></span> <span data-ttu-id="97fd3-121">Máte název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub, primární klíč služby IoT Hub a název a koncový bod kompatibilní s centrem událostí, které potřebujete k dokončení tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="97fd3-121">You have the IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need to complete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="97fd3-122">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="97fd3-122">Create a device identity</span></span>
<span data-ttu-id="97fd3-123">V této části vytvoříte konzolovou aplikaci Java, která v registru identit ve službě IoT Hub vytvoří identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="97fd3-123">In this section, you create a Java console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="97fd3-124">Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit.</span><span class="sxs-lookup"><span data-stu-id="97fd3-124">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="97fd3-125">Další informace najdete v části **Registr identit** v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="97fd3-125">For more information, see the **Identity Registry** section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="97fd3-126">Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. </span><span class="sxs-lookup"><span data-stu-id="97fd3-126">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span>

1. <span data-ttu-id="97fd3-127">Vytvořte prázdnou složku s názvem iot-java-get-started.</span><span class="sxs-lookup"><span data-stu-id="97fd3-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="97fd3-128">Ve složce iot-java-get-started vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **create-device-identity**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-128">In the iot-java-get-started folder, create a Maven project called **create-device-identity** using the following command at your command prompt.</span></span> <span data-ttu-id="97fd3-129">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="97fd3-130">Na příkazovém řádku přejděte do složky create-device-identity.</span><span class="sxs-lookup"><span data-stu-id="97fd3-130">At your command prompt, navigate to the create-device-identity folder.</span></span>

3. <span data-ttu-id="97fd3-131">Pomocí textového editoru otevřete ve složce create-device-identity soubor pom.xml a k uzlu **závislosti** přidejte následující závislost.</span><span class="sxs-lookup"><span data-stu-id="97fd3-131">Using a text editor, open the pom.xml file in the create-device-identity folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="97fd3-132">Tato závislost vám umožní použít v aplikaci balíček iot-service-client:</span><span class="sxs-lookup"><span data-stu-id="97fd3-132">This dependency enables you to use the iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="97fd3-133">Můžete vyhledat nejnovější verzi **iot-service-client** pomocí [vyhledávání Maven][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="97fd3-133">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="97fd3-134">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-134">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="97fd3-135">Pomocí textového editoru otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="97fd3-135">Using a text editor, open the create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="97fd3-136">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="97fd3-136">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="97fd3-137">Do třídy **App** přidejte následující proměnné na úrovni třídy a nahraďte zástupné symboly **{yourhubconnectionstring}** hodnotou, kterou jste si předtím poznamenali:</span><span class="sxs-lookup"><span data-stu-id="97fd3-137">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** with the value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="97fd3-138">Upravte podpis metody **Main** tak, aby zahrnoval následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="97fd3-138">Modify the signature of the **main** method to include the exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="97fd3-139">Přidejte následující kód jako obsah metody **Main**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-139">Add the following code as the body of the **main** method.</span></span> <span data-ttu-id="97fd3-140">Tento kód vytvoří v registru identit služby IoT Hub zařízení s názvem *javadevice* (pokud zatím neexistuje).</span><span class="sxs-lookup"><span data-stu-id="97fd3-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="97fd3-141">Potom zobrazí ID a klíč zařízení, které budete později potřebovat:</span><span class="sxs-lookup"><span data-stu-id="97fd3-141">It then displays the device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. <span data-ttu-id="97fd3-142">Soubor App.java uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-142">Save and close the App.java file.</span></span>

11. <span data-ttu-id="97fd3-143">Aplikaci **create-device-identity** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-143">To build the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="97fd3-144">Aplikaci **create-device-identity** pomocí nástroje Maven spustíte tak, že v příkazovém řádku ve složce create-device-identity spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-144">To run the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="97fd3-145">Poznamenejte si **ID zařízení** a **Klíč zařízení**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-145">Make a note of the **Device ID** and **Device key**.</span></span> <span data-ttu-id="97fd3-146">Tyto hodnoty budete potřebovat později při vytváření aplikace, která se ke službě IoT Hub připojí jako zařízení.</span><span class="sxs-lookup"><span data-stu-id="97fd3-146">You need these values later when you create an app that connects to IoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="97fd3-147">V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-147">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="97fd3-148">Ukládají se tady ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="97fd3-148">It stores device IDs and keys to use as security credentials and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="97fd3-149">Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="97fd3-149">If your app needs to store other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="97fd3-150">Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="97fd3-150">For more information, see the [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="97fd3-151">Příjem zpráv typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="97fd3-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="97fd3-152">V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="97fd3-153">Služba IoT Hub zpřístupní koncový bod kompatibilní se službou [Event Hub][lnk-event-hubs-overview], který vám umožní číst zprávy typu zařízení-cloud.</span><span class="sxs-lookup"><span data-stu-id="97fd3-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint to enable you to read device-to-cloud messages.</span></span> <span data-ttu-id="97fd3-154">Z důvodu zjednodušení vytvoří tento kurz jednoduchou čtečku, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="97fd3-154">To keep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="97fd3-155">Způsoby zpracování zpráv typu zařízení-cloud v různých škálách najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="97fd3-155">The [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how to process device-to-cloud messages at scale.</span></span> <span data-ttu-id="97fd3-156">Kurz [Začínáme se službou Event Hubs][lnk-eventhubs-tutorial] vám poskytne další informace o zpracování zpráv ze služby Event Hubs a vztahuje se na koncové body kompatibilní s centrem událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-156">The [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how to process messages from Event Hubs and is applicable to the IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="97fd3-157">Koncový bod kompatibilní s centrem událostí pro čtení zpráv mezi zařízením a cloudem vždy používá protokol AMQP.</span><span class="sxs-lookup"><span data-stu-id="97fd3-157">The Event Hub-compatible endpoint for reading device-to-cloud messages always uses the AMQP protocol.</span></span>

1. <span data-ttu-id="97fd3-158">Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **read-d2c-messages**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-158">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **read-d2c-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="97fd3-159">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="97fd3-160">Na příkazovém řádku přejděte do složky read-d2c-messages.</span><span class="sxs-lookup"><span data-stu-id="97fd3-160">At your command prompt, navigate to the read-d2c-messages folder.</span></span>

3. <span data-ttu-id="97fd3-161">Pomocí textového editoru otevřete ve složce read-d2c-messages soubor pom.xml a k uzlu **závislosti** přidejte následující závislost.</span><span class="sxs-lookup"><span data-stu-id="97fd3-161">Using a text editor, open the pom.xml file in the read-d2c-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="97fd3-162">Tato závislost umožňuje čtení z koncového bodu kompatibilního s centrem událostí pomocí balíčku eventhubs-client ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="97fd3-162">This dependency enables you to use the eventhubs-client package in your app to read from the Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="97fd3-163">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-163">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="97fd3-164">Pomocí textového editoru otevřete soubor  read-d2c-messages\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="97fd3-164">Using a text editor, open the read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="97fd3-165">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="97fd3-165">Add the following **import** statements to the file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="97fd3-166">Do třídy **App** přidejte následující proměnnou na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="97fd3-166">Add the following class-level variable to the **App** class.</span></span> <span data-ttu-id="97fd3-167">Zástupné symboly **{youriothubkey}**, **{youreventhubcompatibleendpoint}** a **{youreventhubcompatiblename}** nahraďte hodnotami, které jste si předtím poznamenali:</span><span class="sxs-lookup"><span data-stu-id="97fd3-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with the values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="97fd3-168">Do třídy **App** přidejte následující metodu **receiveMessages**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-168">Add the following **receiveMessages** method to the **App** class.</span></span> <span data-ttu-id="97fd3-169">Tato metoda vytvoří instanci **EventHubClient** k připojení ke koncovému bodu kompatibilnímu s centrem událostí a poté asynchronně vytvoří instanci **PartitionReceiver** ke čtení z oddílu centra událostí.</span><span class="sxs-lookup"><span data-stu-id="97fd3-169">This method creates an **EventHubClient** instance to connect to the Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance to read from an Event Hub partition.</span></span> <span data-ttu-id="97fd3-170">Až do ukončení aplikace se bude nepřetržitě provádět v cyklu a vypisovat podrobnosti o zprávách.</span><span class="sxs-lookup"><span data-stu-id="97fd3-170">It loops continuously and prints the message details until the app terminates.</span></span>

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some evenst");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="97fd3-171">Tato metoda při vytváření přijímače používá filtr, aby přijímač četl pouze zprávy odeslané do služby IoT Hub až po jeho spuštění.</span><span class="sxs-lookup"><span data-stu-id="97fd3-171">This method uses a filter when it creates the receiver so that the receiver only reads messages sent to IoT Hub after the receiver starts running.</span></span> <span data-ttu-id="97fd3-172">Tato metoda je užitečná v testovacím prostředí, protože uvidíte aktuální sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="97fd3-172">This technique is useful in a test environment so you can see the current set of messages.</span></span> <span data-ttu-id="97fd3-173">V produkčním prostředí by měl kód zpracovávat všechny zprávy – další informace najdete v kurzu [Postupy zpracování zpráv typu zařízení-cloud ve službě IoT Hub][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="97fd3-173">In a production environment, your code should make sure that it processes all the messages - for more information, see the [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="97fd3-174">Upravte podpis metody **Main** tak, aby zahrnoval následující výjimku:</span><span class="sxs-lookup"><span data-stu-id="97fd3-174">Modify the signature of the **main** method to include the exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="97fd3-175">Ve třídě **App** přidejte do metody **Main** následující kód.</span><span class="sxs-lookup"><span data-stu-id="97fd3-175">Add the following code to the **main** method in the **App** class.</span></span> <span data-ttu-id="97fd3-176">Tento kód vytvoří dvě instance **EventHubClient** a **PartitionReceiver** a umožní vám po dokončení zpracování zpráv zavřít aplikaci:</span><span class="sxs-lookup"><span data-stu-id="97fd3-176">This code creates the two **EventHubClient** and **PartitionReceiver** instances and enables you to close the app when you have finished processing messages:</span></span>

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="97fd3-177">Tento kód předpokládá, že jste vytvořili službu IoT Hub na úrovni F1 (Free).</span><span class="sxs-lookup"><span data-stu-id="97fd3-177">This code assumes you created your IoT hub in the F1 (free) tier.</span></span> <span data-ttu-id="97fd3-178">Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.</span><span class="sxs-lookup"><span data-stu-id="97fd3-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="97fd3-179">Soubor App.java uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-179">Save and close the App.java file.</span></span>

12. <span data-ttu-id="97fd3-180">Aplikaci **read-d2c-messages** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce read-d2c-messages spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-180">To build the **read-d2c-messages** app using Maven, execute the following command at the command prompt in the read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="97fd3-181">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="97fd3-181">Create a device app</span></span>
<span data-ttu-id="97fd3-182">V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení-cloud do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages to an IoT hub.</span></span>

1. <span data-ttu-id="97fd3-183">Ve složce iot-java-get-started, kterou jste vytvořili v části *Vytvoření identity zařízení*, vytvořte pomocí následujícího příkazu v příkazovém řádku projekt Maven s názvem **simulated-device**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-183">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="97fd3-184">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="97fd3-185">Na příkazovém řádku přejděte do složky simulated-devices.</span><span class="sxs-lookup"><span data-stu-id="97fd3-185">At your command prompt, navigate to the simulated-device folder.</span></span>

3. <span data-ttu-id="97fd3-186">Pomocí textového editoru otevřete ve složce simulated-device soubor pom.xml a k uzlu **závislosti** přidejte následující závislosti.</span><span class="sxs-lookup"><span data-stu-id="97fd3-186">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="97fd3-187">Tato závislost vám umožní komunikovat se službou IoT Hub a serializovat objekty Java do formátu JSON pomocí balíčku iothub-java-client ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="97fd3-187">This dependency enables you to use the iothub-java-client package in your app to communicate with your IoT hub and to serialize Java objects to JSON:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="97fd3-188">Můžete vyhledat nejnovější verzi **iot-device-client** pomocí [vyhledávání Maven][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="97fd3-188">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="97fd3-189">Soubor pom.xml uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-189">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="97fd3-190">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="97fd3-190">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="97fd3-191">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="97fd3-191">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="97fd3-192">Do třídy **App** přidejte následující proměnné na úrovni třídy.</span><span class="sxs-lookup"><span data-stu-id="97fd3-192">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="97fd3-193">Nahraďte hodnotu **{youriothubname}** názvem vaší služby IoT Hub a hodnotu **{yourdevicekey}** klíčem zařízení, který jste vygenerovali v části *Vytvoření identity zařízení*:</span><span class="sxs-lookup"><span data-stu-id="97fd3-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="97fd3-194">Tato ukázková aplikace používá při vytváření instance objektu **DeviceClient** proměnnou **protocol**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-194">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="97fd3-195">Ke komunikaci se službou IoT Hub můžete použít protokol MQTT, AMQP nebo HTTP.</span><span class="sxs-lookup"><span data-stu-id="97fd3-195">You can use either the MQTT, AMQP, or HTTP protocol to communicate with IoT Hub.</span></span>

8. <span data-ttu-id="97fd3-196">Telemetrická data, která vaše zařízení odesílá do služby IoT Hub, určete přidáním následující vnořené třídy **TelemetryDataPoint** do třídy **App**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-196">Add the following nested **TelemetryDataPoint** class inside the **App** class to specify the telemetry data your device sends to your IoT hub:</span></span>

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. <span data-ttu-id="97fd3-197">Za účelem zobrazení stavu potvrzení, které služba IoT Hub vrací po zpracování zprávy z aplikace pro zařízení, přidejte do třídy **App** následující vnořenou třídu **EventCallback**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-197">Add the following nested **EventCallback** class inside the **App** class to display the acknowledgement status that the IoT hub returns when it processes a message from the device app.</span></span> <span data-ttu-id="97fd3-198">Tato metoda také po zpracování zprávy upozorní hlavní vlákno v aplikaci:</span><span class="sxs-lookup"><span data-stu-id="97fd3-198">This method also notifies the main thread in the app when the message has been processed:</span></span>
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. <span data-ttu-id="97fd3-199">Do třídy **App** přidejte následující vnořenou třídu **MessageSender**.</span><span class="sxs-lookup"><span data-stu-id="97fd3-199">Add the following nested **MessageSender** class inside the **App** class.</span></span> <span data-ttu-id="97fd3-200">Metoda **run** v této třídě vygeneruje ukázková telemetrická data, která se odešlou do služby IoT Hub, a před odesláním další zprávy počká na potvrzení:</span><span class="sxs-lookup"><span data-stu-id="97fd3-200">The **run** method in this class generates sample telemetry data to send to your IoT hub and waits for an acknowledgement before sending the next message:</span></span>

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
            System.out.println("Sending: " + msgStr);
    
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    <span data-ttu-id="97fd3-201">Tato metoda odešle novou zprávu typu zařízení-cloud sekundu poté, co služba IoT Hub potvrdí předchozí zprávu.</span><span class="sxs-lookup"><span data-stu-id="97fd3-201">This method sends a new device-to-cloud message one second after the IoT hub acknowledges the previous message.</span></span> <span data-ttu-id="97fd3-202">Zpráva obsahuje objekt serializovaný do formátu JSON, s ID zařízení a náhodně generovanými čísly, který simuluje snímač teploty a snímač vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="97fd3-202">The message contains a JSON-serialized object with the deviceId and randomly generated numbers to simulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="97fd3-203">Metodu **Main** nahraďte následujícím kódem, který vytvoří vlákno k odesílání zpráv typu zařízení-cloud do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="97fd3-203">Replace the **main** method with the following code that creates a thread to send device-to-cloud messages to your IoT hub:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. <span data-ttu-id="97fd3-204">Soubor App.java uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="97fd3-204">Save and close the App.java file.</span></span>

13. <span data-ttu-id="97fd3-205">Aplikaci **simulated-device** pomocí nástroje Maven sestavíte tak, že v příkazovém řádku ve složce simulated-device spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="97fd3-205">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="97fd3-206">Za účelem zjednodušení tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="97fd3-206">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="97fd3-207">V produkčním kódu byte měli implementovat zásady opakování (například exponenciální opakování), jak je navrženo v článku [Řešení přechodných chyb][lnk-transient-faults] na webu MSDN.</span><span class="sxs-lookup"><span data-stu-id="97fd3-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="97fd3-208">Spouštění aplikací</span><span class="sxs-lookup"><span data-stu-id="97fd3-208">Run the apps</span></span>

<span data-ttu-id="97fd3-209">Nyní jste připraveni aplikaci spustit.</span><span class="sxs-lookup"><span data-stu-id="97fd3-209">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="97fd3-210">V příkazovém řádku ve složce read-d2c spusťte následující příkaz, aby se začal monitorovat první oddíl služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="97fd3-210">At a command prompt in the read-d2c folder, run the following command to begin monitoring the first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Aplikace služby Java IoT Hub pro monitorování zpráv typu zařízení-cloud][7]

2. <span data-ttu-id="97fd3-212">V příkazovém řádku ve složce simulated-device spusťte následující příkaz, aby se do služby IoT Hub začala odesílat telemetrická data:</span><span class="sxs-lookup"><span data-stu-id="97fd3-212">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry data to your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Aplikace zařízení služby Java IoT Hub pro odesílání zpráv typu zařízení-cloud][8]

3. <span data-ttu-id="97fd3-214">Na dlaždici **Využití** na webu [Azure Portal][lnk-portal] se zobrazuje počet zpráv odeslaných do služby IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="97fd3-214">The **Usage** tile in the [Azure portal][lnk-portal] shows the number of messages sent to the IoT hub:</span></span>

    ![Dlaždice Použití webu Azure Portal se zobrazením počtu zpráv odeslaných do služby IoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="97fd3-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="97fd3-216">Next steps</span></span>
<span data-ttu-id="97fd3-217">V tomto kurzu jste nakonfigurovali novou službu IoT Hub na webu Azure Portal a potom jste vytvořili identitu zařízení v registru identit ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-217">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="97fd3-218">Pomocí identity zařízení jste aplikaci pro zařízení povolili odesílání zpráv typu zařízení-cloud do služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-218">You used this device identity to enable the device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="97fd3-219">Také jste vytvořili aplikaci, která zobrazuje zprávy přijaté službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="97fd3-219">You also created an app that displays the messages received by the IoT hub.</span></span>

<span data-ttu-id="97fd3-220">Chcete-li pokračovat v seznamování se službou IoT Hub a prozkoumat další scénáře IoT, podívejte se na tato témata:</span><span class="sxs-lookup"><span data-stu-id="97fd3-220">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="97fd3-221">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="97fd3-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="97fd3-222">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="97fd3-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="97fd3-223">[Začínáme se službou Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="97fd3-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="97fd3-224">Další informace o tom, jak rozšířit vaše řešení internetu věcí a zpracovávat škálované zprávy typu zařízení-cloud, najdete v kurzu [Zpracování zpráv typu zařízení-cloud][lnk-process-d2c-tutorial].</span><span class="sxs-lookup"><span data-stu-id="97fd3-224">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
