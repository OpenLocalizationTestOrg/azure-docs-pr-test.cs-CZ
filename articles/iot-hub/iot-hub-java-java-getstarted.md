---
title: "aaaGet začít s Azure IoT Hub (Java) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pro jazyk Java pomocí sady SDK služby IoT. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
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
ms.openlocfilehash: ac954f0522b46ed2a5b4a819bc611c13be0b9a9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a><span data-ttu-id="9573f-104">Připojení zařízení tooyour IoT hub pomocí jazyka Java</span><span class="sxs-lookup"><span data-stu-id="9573f-104">Connect your device tooyour IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="9573f-105">Na konci hello tohoto kurzu máte tři aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="9573f-105">At hello end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="9573f-106">**Create-device-identity**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="9573f-106">**create-device-identity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="9573f-107">**Read-d2c-messages**, který zobrazuje hello telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="9573f-107">**read-d2c-messages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="9573f-108">**simulated-device**, který připojí tooyour IoT hub s dříve vytvořenou identitou zařízení hello a odešle zprávu telemetrie každý druhý pomocí hello MQTT protokolu.</span><span class="sxs-lookup"><span data-stu-id="9573f-108">**simulated-device**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="9573f-109">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="9573f-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both apps toorun on devices and your solution back end.</span></span>

<span data-ttu-id="9573f-110">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="9573f-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="9573f-111">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="9573f-111">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="9573f-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="9573f-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="9573f-113">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="9573f-113">An active Azure account.</span></span> <span data-ttu-id="9573f-114">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="9573f-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="9573f-115">V posledním kroku, si poznamenejte hello **primární klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="9573f-115">As a final step, make a note of hello **Primary key** value.</span></span> <span data-ttu-id="9573f-116">Pak klikněte na tlačítko **koncové body** a hello **události** vestavěným koncovým bodem.</span><span class="sxs-lookup"><span data-stu-id="9573f-116">Then click **Endpoints** and hello **Events** built-in endpoint.</span></span> <span data-ttu-id="9573f-117">Na hello **vlastnosti** okno, poznamenejte hello **název kompatibilní s centrem událostí** a hello **koncový bod kompatibilní s centrem událostí** adresu.</span><span class="sxs-lookup"><span data-stu-id="9573f-117">On hello **Properties** blade, make a note of hello **Event Hub-compatible name** and hello **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="9573f-118">Tyto tři hodnoty budete potřebovat k vytvoření aplikace **read-d2c-messages**.</span><span class="sxs-lookup"><span data-stu-id="9573f-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Okno Zasílání zpráv služby IoT Hub na webu Azure Portal][6]

<span data-ttu-id="9573f-120">Nyní jste vytvořili svůj IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-120">You have now created your IoT hub.</span></span> <span data-ttu-id="9573f-121">Máte hello název hostitele služby IoT Hub, připojovací řetězec služby IoT Hub, IoT Hub primární klíč, název kompatibilní s centrem událostí a koncový bod kompatibilní s centrem událostí je třeba toocomplete v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9573f-121">You have hello IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need toocomplete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="9573f-122">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="9573f-122">Create a device identity</span></span>
<span data-ttu-id="9573f-123">V této části vytvoříte konzolovou aplikaci Java, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-123">In this section, you create a Java console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="9573f-124">Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9573f-124">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="9573f-125">Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="9573f-125">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="9573f-126">Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="9573f-126">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="9573f-127">Vytvořte prázdnou složku s názvem iot-java-get-started.</span><span class="sxs-lookup"><span data-stu-id="9573f-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="9573f-128">Ve složce iot-java-get-started hello vytvořte projekt Maven s názvem **create-device-identity** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9573f-128">In hello iot-java-get-started folder, create a Maven project called **create-device-identity** using hello following command at your command prompt.</span></span> <span data-ttu-id="9573f-129">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="9573f-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="9573f-130">Na příkazovém řádku přejděte toohello složky create-device-identity.</span><span class="sxs-lookup"><span data-stu-id="9573f-130">At your command prompt, navigate toohello create-device-identity folder.</span></span>

3. <span data-ttu-id="9573f-131">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce create-device-identity hello a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="9573f-131">Using a text editor, open hello pom.xml file in hello create-device-identity folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="9573f-132">Tuto závislost umožňuje, aby vám toouse hello klienta služby iot balíček ve vaší aplikaci:</span><span class="sxs-lookup"><span data-stu-id="9573f-132">This dependency enables you toouse hello iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="9573f-133">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="9573f-133">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="9573f-134">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-134">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="9573f-135">Pomocí textového editoru otevřete soubor create-device-identity\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-135">Using a text editor, open hello create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="9573f-136">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="9573f-136">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="9573f-137">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy a nahraďte **{yourhubconnectionstring}** s hello hodnotu vaší uvedené výše:</span><span class="sxs-lookup"><span data-stu-id="9573f-137">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** with hello value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="9573f-138">Upravte podpis hello hello **hlavní** metoda tooinclude hello výjimky následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9573f-138">Modify hello signature of hello **main** method tooinclude hello exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="9573f-139">Přidejte následující kód jako hello textu hello hello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="9573f-139">Add hello following code as hello body of hello **main** method.</span></span> <span data-ttu-id="9573f-140">Tento kód vytvoří v registru identit služby IoT Hub zařízení s názvem *javadevice* (pokud zatím neexistuje).</span><span class="sxs-lookup"><span data-stu-id="9573f-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="9573f-141">Potom zobrazí hello zařízení ID a klíč, který budete potřebovat později:</span><span class="sxs-lookup"><span data-stu-id="9573f-141">It then displays hello device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
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
      // If hello device already exists.
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

10. <span data-ttu-id="9573f-142">Uložte a zavřete soubor App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-142">Save and close hello App.java file.</span></span>

11. <span data-ttu-id="9573f-143">toobuild hello **create-device-identity** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce create-device-identity hello hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-143">toobuild hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="9573f-144">toorun hello **create-device-identity** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce create-device-identity hello hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-144">toorun hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="9573f-145">Poznamenejte si hello **ID zařízení** a **klíč zařízení**.</span><span class="sxs-lookup"><span data-stu-id="9573f-145">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="9573f-146">Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.</span><span class="sxs-lookup"><span data-stu-id="9573f-146">You need these values later when you create an app that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="9573f-147">Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-147">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="9573f-148">Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="9573f-148">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="9573f-149">Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště specifické pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="9573f-149">If your app needs toostore other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="9573f-150">Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="9573f-150">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="9573f-151">Příjem zpráv typu zařízení-cloud</span><span class="sxs-lookup"><span data-stu-id="9573f-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="9573f-152">V této části vytvoříte konzolovou aplikaci Java, která čte zprávy typu zařízení-cloud ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="9573f-153">IoT hub zpřístupní [centra událostí][lnk-event-hubs-overview]-koncový bod kompatibilní tooenable jste tooread zpráv typu zařízení cloud.</span><span class="sxs-lookup"><span data-stu-id="9573f-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="9573f-154">věcí tookeep jednoduchý, v tomto kurzu vytvoří základní čtečka, která není vhodná pro vysoce výkonná nasazení.</span><span class="sxs-lookup"><span data-stu-id="9573f-154">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="9573f-155">Hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurz ukazuje, jak tooprocess zařízení cloud zpráv ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="9573f-155">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="9573f-156">Hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz obsahuje další informace o tom, jak tooprocess zpráv ze služby Event Hubs a je použít toohello koncové body kompatibilní s centrem událostí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-156">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="9573f-157">Hello koncový bod kompatibilní s centrem událostí pro čtení zpráv typu zařízení cloud vždy používá protokol AMQP hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-157">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="9573f-158">Ve složce iot-java-get-started hello jste vytvořili v hello *vytvoření identity zařízení* , vytvořte projekt Maven s názvem **read-d2c-messages** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9573f-158">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **read-d2c-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="9573f-159">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="9573f-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="9573f-160">Na příkazovém řádku přejděte toohello read-d2c-messages složky.</span><span class="sxs-lookup"><span data-stu-id="9573f-160">At your command prompt, navigate toohello read-d2c-messages folder.</span></span>

3. <span data-ttu-id="9573f-161">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce read-d2c-messages hello a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="9573f-161">Using a text editor, open hello pom.xml file in hello read-d2c-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="9573f-162">Tuto závislost umožňuje toouse hello balíčku eventhubs-client ve vaší aplikaci tooread z koncového bodu kompatibilního s centrem událostí hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-162">This dependency enables you toouse hello eventhubs-client package in your app tooread from hello Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="9573f-163">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-163">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="9573f-164">Pomocí textového editoru otevřete soubor read-d2c-messages\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-164">Using a text editor, open hello read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="9573f-165">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="9573f-165">Add hello following **import** statements toohello file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="9573f-166">Přidejte následující proměnné toohello úrovni třídy hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="9573f-166">Add hello following class-level variable toohello **App** class.</span></span> <span data-ttu-id="9573f-167">Nahraďte **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, a **{youreventhubcompatiblename}** hello hodnotami, které jste si poznamenali dříve:</span><span class="sxs-lookup"><span data-stu-id="9573f-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with hello values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="9573f-168">Přidejte následující hello **receiveMessages** metoda toohello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="9573f-168">Add hello following **receiveMessages** method toohello **App** class.</span></span> <span data-ttu-id="9573f-169">Tato metoda vytvoří **EventHubClient** instance tooconnect toohello kompatibilní s centrem událostí koncový bod a poté asynchronně vytvoří **PartitionReceiver** instance tooread z centra událostí oddíl.</span><span class="sxs-lookup"><span data-stu-id="9573f-169">This method creates an **EventHubClient** instance tooconnect toohello Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance tooread from an Event Hub partition.</span></span> <span data-ttu-id="9573f-170">To nepřetržitě provádět v cyklu a vytiskne hello podrobnosti zprávy, dokud neskončí aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-170">It loops continuously and prints hello message details until hello app terminates.</span></span>

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed toocreate client: " + e.getMessage());
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
                System.out.println("Failed tooreceive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed toocreate receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="9573f-171">Tato metoda používá filtr, při vytváření příjemce hello tak, aby hello přijímač četl pouze zprávy odeslané tooIoT Hub až po spuštění hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="9573f-171">This method uses a filter when it creates hello receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="9573f-172">Tato metoda je užitečná v testovacím prostředí, abyste viděli hello aktuální sadu zpráv.</span><span class="sxs-lookup"><span data-stu-id="9573f-172">This technique is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="9573f-173">V produkčním prostředí by měl váš kód Ujistěte se, že zpracovává všechny zprávy hello – Další informace naleznete v tématu hello [jak tooprocess zpráv typu zařízení cloud IoT Hub] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="9573f-173">In a production environment, your code should make sure that it processes all hello messages - for more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="9573f-174">Upravte podpis hello hello **hlavní** metoda tooinclude hello výjimka následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="9573f-174">Modify hello signature of hello **main** method tooinclude hello exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="9573f-175">Přidejte následující kód toohello hello **hlavní** metoda v hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="9573f-175">Add hello following code toohello **main** method in hello **App** class.</span></span> <span data-ttu-id="9573f-176">Tento kód vytvoří dvě hello **EventHubClient** a **PartitionReceiver** instance a umožní vám aplikace hello tooclose po dokončení zpracování zpráv:</span><span class="sxs-lookup"><span data-stu-id="9573f-176">This code creates hello two **EventHubClient** and **PartitionReceiver** instances and enables you tooclose hello app when you have finished processing messages:</span></span>

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER tooexit.");
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
    > <span data-ttu-id="9573f-177">Tento kód předpokládá, že jste vytvořili službu IoT hub v úroveň hello F1 (free).</span><span class="sxs-lookup"><span data-stu-id="9573f-177">This code assumes you created your IoT hub in hello F1 (free) tier.</span></span> <span data-ttu-id="9573f-178">Služba IoT Hub na úrovni Free má dva oddíly s názvem „0“ a „1“.</span><span class="sxs-lookup"><span data-stu-id="9573f-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="9573f-179">Uložte a zavřete soubor App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-179">Save and close hello App.java file.</span></span>

12. <span data-ttu-id="9573f-180">toobuild hello **read-d2c-messages** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce read-d2c-messages hello hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-180">toobuild hello **read-d2c-messages** app using Maven, execute hello following command at hello command prompt in hello read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="9573f-181">Vytvoření aplikace pro zařízení</span><span class="sxs-lookup"><span data-stu-id="9573f-181">Create a device app</span></span>
<span data-ttu-id="9573f-182">V této části vytvoříte konzolovou aplikaci Java, která simuluje zařízení odesílající zprávy typu zařízení cloud tooan IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="9573f-183">Ve složce iot-java-get-started hello jste vytvořili v hello *vytvoření identity zařízení* , vytvořte projekt Maven s názvem **simulated-device** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="9573f-183">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="9573f-184">Všimněte si, že se jedná o jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="9573f-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="9573f-185">Na příkazovém řádku přejděte toohello složky simulated Devices.</span><span class="sxs-lookup"><span data-stu-id="9573f-185">At your command prompt, navigate toohello simulated-device folder.</span></span>

3. <span data-ttu-id="9573f-186">Pomocí textového editoru, otevřete soubor pom.xml hello ve složce simulated-device hello a přidejte následující závislosti toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="9573f-186">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="9573f-187">Tuto závislost umožňuje, aby vám toouse hello iothub-java-client balíček ve vaší aplikaci toocommunicate s IoT hub a tooserialize tooJSON objekty Java:</span><span class="sxs-lookup"><span data-stu-id="9573f-187">This dependency enables you toouse hello iothub-java-client package in your app toocommunicate with your IoT hub and tooserialize Java objects tooJSON:</span></span>

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
    > <span data-ttu-id="9573f-188">Můžete zkontrolovat nejnovější verze hello **klienta zařízení iot** pomocí [Maven vyhledávání][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="9573f-188">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="9573f-189">Uložte a zavřete soubor pom.xml hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-189">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="9573f-190">Pomocí textového editoru otevřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-190">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="9573f-191">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="9573f-191">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="9573f-192">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="9573f-192">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="9573f-193">Nahrazení **{youriothubname}** názvem služby IoT hub, a **{yourdevicekey}** s hodnotou klíče hello zařízení jste vygenerovali v hello *vytvoření identity zařízení* části:</span><span class="sxs-lookup"><span data-stu-id="9573f-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="9573f-194">Tato ukázková aplikace používá hello **protokol** proměnná při vytvoření instance **DeviceClient** objektu.</span><span class="sxs-lookup"><span data-stu-id="9573f-194">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="9573f-195">Můžete buď toocommunicate hello MQTT, AMQP nebo HTTP protocol službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-195">You can use either hello MQTT, AMQP, or HTTP protocol toocommunicate with IoT Hub.</span></span>

8. <span data-ttu-id="9573f-196">Přidejte následující hello vnořenou **TelemetryDataPoint** třída uvnitř hello **aplikace** třídy toospecify hello telemetrická data vaše zařízení odesílá tooyour IoT hub:</span><span class="sxs-lookup"><span data-stu-id="9573f-196">Add hello following nested **TelemetryDataPoint** class inside hello **App** class toospecify hello telemetry data your device sends tooyour IoT hub:</span></span>

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
9. <span data-ttu-id="9573f-197">Přidejte následující hello vnořenou **EventCallback** třída uvnitř hello **aplikace** třída toodisplay hello potvrzení stav, který hello služby IoT hub vrací po zpracování zprávy ze zařízení aplikaci hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-197">Add hello following nested **EventCallback** class inside hello **App** class toodisplay hello acknowledgement status that hello IoT hub returns when it processes a message from hello device app.</span></span> <span data-ttu-id="9573f-198">Tato metoda také upozorňován hello hlavní vlákno v aplikaci hello po zpracování zprávy hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-198">This method also notifies hello main thread in hello app when hello message has been processed:</span></span>
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toomessage with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. <span data-ttu-id="9573f-199">Přidejte následující hello vnořenou **MessageSender** třída uvnitř hello **aplikace** třídy.</span><span class="sxs-lookup"><span data-stu-id="9573f-199">Add hello following nested **MessageSender** class inside hello **App** class.</span></span> <span data-ttu-id="9573f-200">Hello **spustit** metoda v této třídě vygeneruje ukázková telemetrická data toosend tooyour IoT hub a počká na potvrzení před odesláním další zprávy hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-200">hello **run** method in this class generates sample telemetry data toosend tooyour IoT hub and waits for an acknowledgement before sending hello next message:</span></span>

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

    <span data-ttu-id="9573f-201">Tato metoda odesílá novou zprávu typu zařízení cloud sekundu poté, co hello IoT hub potvrdí předchozí zprávu hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-201">This method sends a new device-to-cloud message one second after hello IoT hub acknowledges hello previous message.</span></span> <span data-ttu-id="9573f-202">Hello zpráva obsahuje objekt serializací JSON s ID hello zařízení a náhodně vygenerované čísla toosimulate senzor teploty a vlhkosti senzoru.</span><span class="sxs-lookup"><span data-stu-id="9573f-202">hello message contains a JSON-serialized object with hello deviceId and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="9573f-203">Nahraďte hello **hlavní** metoda s hello následující kód, který vytvoří vlákno toosend zpráv typu zařízení cloud tooyour IoT rozbočovači:</span><span class="sxs-lookup"><span data-stu-id="9573f-203">Replace hello **main** method with hello following code that creates a thread toosend device-to-cloud messages tooyour IoT hub:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER tooexit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. <span data-ttu-id="9573f-204">Uložte a zavřete soubor App.java hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-204">Save and close hello App.java file.</span></span>

13. <span data-ttu-id="9573f-205">toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-205">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="9573f-206">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="9573f-206">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="9573f-207">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="9573f-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="9573f-208">Spuštění aplikace hello</span><span class="sxs-lookup"><span data-stu-id="9573f-208">Run hello apps</span></span>

<span data-ttu-id="9573f-209">Nyní je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="9573f-209">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="9573f-210">Na příkazovém řádku ve složce read-d2c hello spusťte následující příkaz toobegin monitorovat první oddíl služby IoT hub hello hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-210">At a command prompt in hello read-d2c folder, run hello following command toobegin monitoring hello first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Zprávy typu zařízení cloud Java IoT Hub služby aplikace toomonitor][7]

2. <span data-ttu-id="9573f-212">Na příkazovém řádku ve složce simulated-device hello spusťte následující příkaz toobegin odesílání telemetrických dat tooyour IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="9573f-212">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Zprávy typu zařízení cloud Java IoT Hub zařízení aplikace toosend][8]

3. <span data-ttu-id="9573f-214">Hello **využití** dlaždici v hello [portál Azure] [ lnk-portal] ukazuje hello počet zpráv odeslaných toohello služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="9573f-214">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Azure portálu využití dlaždice zobrazuje počet zpráv odeslaných tooIoT rozbočovače][43]

## <a name="next-steps"></a><span data-ttu-id="9573f-216">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9573f-216">Next steps</span></span>
<span data-ttu-id="9573f-217">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="9573f-217">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="9573f-218">Použili jste toto zařízení identity tooenable hello zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="9573f-218">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="9573f-219">Můžete také vytvořit aplikaci, která zobrazuje hello zprávy přijaté službou hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9573f-219">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="9573f-220">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="9573f-220">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="9573f-221">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="9573f-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="9573f-222">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="9573f-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="9573f-223">[Začínáme se službou Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="9573f-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="9573f-224">toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="9573f-224">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
