---
title: "Odeslání souborů ze zařízení do Azure IoT Hub pro Javu | Microsoft Docs"
description: "Postup nahrání souborů ze zařízení do cloudu pomocí zařízení Azure IoT SDK pro jazyk Java. Odeslané soubory jsou uloženy v kontejneru objektů blob úložiště Azure."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: c917a3b3e16f1e84f202d6c87a04faf642266701
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a><span data-ttu-id="8d95e-104">Odeslání souborů ze zařízení do cloudu s centrem IoT</span><span class="sxs-lookup"><span data-stu-id="8d95e-104">Upload files from your device to the cloud with IoT Hub</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="8d95e-105">V tomto kurzu vychází kód [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurzu ukazují, jak používat [souboru nahrávání služby IoT Hub](iot-hub-devguide-file-upload.md) nahrát soubor do [objektů blob v Azure úložiště](../storage/index.md).</span><span class="sxs-lookup"><span data-stu-id="8d95e-105">This tutorial builds on the code in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorial to show you how to use the [file upload capabilities of IoT Hub](iot-hub-devguide-file-upload.md) to upload a file to [Azure blob storage](../storage/index.md).</span></span> <span data-ttu-id="8d95e-106">Tento kurz ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="8d95e-106">The tutorial shows you how to:</span></span>

- <span data-ttu-id="8d95e-107">Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.</span><span class="sxs-lookup"><span data-stu-id="8d95e-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="8d95e-108">Oznámení o odeslání souboru IoT Hub použijte k aktivaci zpracování souboru ve vaší aplikaci back-end.</span><span class="sxs-lookup"><span data-stu-id="8d95e-108">Use the IoT Hub file upload notifications to trigger processing the file in your app back end.</span></span>

<span data-ttu-id="8d95e-109">[Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurzy zobrazit základních funkcí zařízení cloud a z cloudu do zařízení zasílání zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-109">The [Get started with IoT Hub](iot-hub-java-java-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorials show the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="8d95e-110">[Zprávy procesu zařízení-Cloud](iot-hub-java-java-process-d2c.md) kurz popisuje způsob, jak spolehlivě ukládat zprávy typu zařízení cloud do úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="8d95e-110">The [Process Device-to-Cloud messages](iot-hub-java-java-process-d2c.md) tutorial describes a way to reliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="8d95e-111">Ale v některých scénářích nelze mapovat snadno data, která vaše zařízení odesílají do poměrně malý zprávy typu zařízení cloud, které IoT Hub přijímá.</span><span class="sxs-lookup"><span data-stu-id="8d95e-111">However, in some scenarios you cannot easily map the data your devices send into the relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="8d95e-112">Například:</span><span class="sxs-lookup"><span data-stu-id="8d95e-112">For example:</span></span>

* <span data-ttu-id="8d95e-113">Velkých souborů, které obsahují Image</span><span class="sxs-lookup"><span data-stu-id="8d95e-113">Large files that contain images</span></span>
* <span data-ttu-id="8d95e-114">Videa</span><span class="sxs-lookup"><span data-stu-id="8d95e-114">Videos</span></span>
* <span data-ttu-id="8d95e-115">Data vibrace odebírána data v vysoká frekvence</span><span class="sxs-lookup"><span data-stu-id="8d95e-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="8d95e-116">Určitou formu předběžně zpracované data.</span><span class="sxs-lookup"><span data-stu-id="8d95e-116">Some form of preprocessed data.</span></span>

<span data-ttu-id="8d95e-117">Tyto soubory jsou obvykle dávkové zpracování v cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo [Hadoop](../hdinsight/index.md) zásobníku.</span><span class="sxs-lookup"><span data-stu-id="8d95e-117">These files are typically batch processed in the cloud using tools such as [Azure Data Factory](../data-factory/index.md) or the [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="8d95e-118">Když potřebujete hornatých soubory ze zařízení, když můžete nadále používat zabezpečení a spolehlivost služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-118">When you need to upland files from a device, you can still use the security and reliability of IoT Hub.</span></span>

<span data-ttu-id="8d95e-119">Na konci tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="8d95e-119">At the end of this tutorial you run two Java console apps:</span></span>

* <span data-ttu-id="8d95e-120">**simulated-device**, upravenou verzi aplikace vytvořená v kurzu [odesílání Cloud-zařízení zprávy službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="8d95e-120">**simulated-device**, a modified version of the app created in the [Send Cloud-to-Device messages with IoT Hub] tutorial.</span></span> <span data-ttu-id="8d95e-121">Tato aplikace se uloží soubor do úložiště pomocí identifikátoru URI SAS poskytované služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-121">This app uploads a file to storage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="8d95e-122">**čtení souboru odesílání oznámení**, který obdrží oznámení o odeslání souboru ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-122">**read-file-upload-notification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="8d95e-123">IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, rozhraní .NET a Javascript) prostřednictvím sady SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="8d95e-123">IoT Hub supports many device platforms and languages (including C, .NET, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="8d95e-124">Odkazovat [Azure střediska pro vývojáře IoT] podrobné pokyny o tom, jak připojit zařízení ke službě Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-124">Refer to the [Azure IoT Developer Center] for step-by-step instructions on how to connect your device to Azure IoT Hub.</span></span>

<span data-ttu-id="8d95e-125">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="8d95e-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="8d95e-126">Nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="8d95e-126">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="8d95e-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="8d95e-127">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="8d95e-128">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8d95e-128">An active Azure account.</span></span> <span data-ttu-id="8d95e-129">(Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="8d95e-129">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="8d95e-130">Nahrát soubor z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="8d95e-130">Upload a file from a device app</span></span>

<span data-ttu-id="8d95e-131">V této části upravíte zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) pro nahrání souboru do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-131">In this section, you modify the device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) to upload a file to IoT hub.</span></span>

1. <span data-ttu-id="8d95e-132">Zkopírujte soubor obrázku na `simulated-device` složku a přejmenujte ji `myimage.png`.</span><span class="sxs-lookup"><span data-stu-id="8d95e-132">Copy an image file to the `simulated-device` folder and rename it `myimage.png`.</span></span>

1. <span data-ttu-id="8d95e-133">Pomocí textového editoru, otevřete `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="8d95e-133">Using a text editor, open the `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="8d95e-134">Přidání deklarace proměnné na **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8d95e-134">Add the variable declaration to the **App** class:</span></span>

    ```java
    private static String fileName = "myimage.png";
    ```

1. <span data-ttu-id="8d95e-135">Při zpracování zprávy zpětného volání stav nahrávání souborů, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8d95e-135">To process file upload status callback messages, add the following nested class to the **App** class:</span></span>

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="8d95e-136">Odesílání obrázků na IoT Hub, přidejte následující metodu do **aplikace** třída odesílání obrázků na IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="8d95e-136">To upload images to IoT Hub, add the following method to the **App** class to upload images to IoT Hub:</span></span>

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. <span data-ttu-id="8d95e-137">Změnit **hlavní** metoda k volání **uploadFile** metoda, jak je znázorněno v následujícím fragmentu kódu:</span><span class="sxs-lookup"><span data-stu-id="8d95e-137">Modify the **main** method to call the **uploadFile** method as shown in the following snippet:</span></span>

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. <span data-ttu-id="8d95e-138">Použijte následující příkaz k vytvoření **simulated-device** aplikaci a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="8d95e-138">Use the following command to build the **simulated-device** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="8d95e-139">Nechte si zaslat oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="8d95e-139">Receive a file upload notification</span></span>

<span data-ttu-id="8d95e-140">V této části vytvoříte konzolovou aplikaci Java, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8d95e-140">In this section, you create a Java console app that receives file upload notification messages from IoT Hub.</span></span>

<span data-ttu-id="8d95e-141">Je nutné **iothubowner** připojovací řetězec služby IoT hub pro dokončení této části.</span><span class="sxs-lookup"><span data-stu-id="8d95e-141">You need the **iothubowner** connection string for your IoT Hub to complete this section.</span></span> <span data-ttu-id="8d95e-142">Můžete najít připojovací řetězec [portál Azure](https://portal.azure.com/) na **zásady sdíleného přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="8d95e-142">You can find the connection string in the [Azure portal](https://portal.azure.com/) on the **Shared access policy** blade.</span></span>

1. <span data-ttu-id="8d95e-143">Vytvořte projekt Maven s názvem **čtení souboru odesílání oznámení** pomocí následujícího příkazu na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="8d95e-143">Create a Maven project called **read-file-upload-notification** using the following command at your command prompt.</span></span> <span data-ttu-id="8d95e-144">Poznámka: Tento příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="8d95e-144">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. <span data-ttu-id="8d95e-145">Na příkazovém řádku přejděte do nové `read-file-upload-notification` složky.</span><span class="sxs-lookup"><span data-stu-id="8d95e-145">At your command prompt, navigate to the new `read-file-upload-notification` folder.</span></span>

1. <span data-ttu-id="8d95e-146">Pomocí textového editoru, otevřete `pom.xml` v soubor `read-file-upload-notification` složky a přidejte následující závislost na **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d95e-146">Using a text editor, open the `pom.xml` file in the `read-file-upload-notification` folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="8d95e-147">Přidání závislost umožňuje používat **iothub-java-service-client** balíček ve vaší aplikaci ke komunikaci s vaší služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="8d95e-147">Adding the dependency enables you to use the **iothub-java-service-client** package in your application to communicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="8d95e-148">Můžete zkontrolovat pro nejnovější verzi **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="8d95e-148">You can check for the latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="8d95e-149">Uložte a zavřete `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="8d95e-149">Save and close the `pom.xml` file.</span></span>

1. <span data-ttu-id="8d95e-150">Pomocí textového editoru, otevřete `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="8d95e-150">Using a text editor, open the `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="8d95e-151">Do souboru přidejte následující příkazy pro **import**:</span><span class="sxs-lookup"><span data-stu-id="8d95e-151">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. <span data-ttu-id="8d95e-152">Přidejte následující proměnné na úrovni třídy k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8d95e-152">Add the following class-level variables to the **App** class:</span></span>

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. <span data-ttu-id="8d95e-153">Tisknout informace o nahrávání souborů do konzoly, přidejte následující vnořenou třídu k **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="8d95e-153">To print information about the file upload to the console, add the following nested class to the **App** class:</span></span>

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. <span data-ttu-id="8d95e-154">Chcete-li spustit vlákno, která čeká na oznámení o odeslání souboru, přidejte následující kód do **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8d95e-154">To start the thread that listens for file upload notifications, add the following code to the **main** method:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. <span data-ttu-id="8d95e-155">Uložte a zavřete `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="8d95e-155">Save and close the `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="8d95e-156">Použijte následující příkaz k vytvoření **čtení souboru odesílání oznámení** aplikaci a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="8d95e-156">Use the following command to build the **read-file-upload-notification** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a><span data-ttu-id="8d95e-157">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="8d95e-157">Run the applications</span></span>

<span data-ttu-id="8d95e-158">Nyní můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d95e-158">Now you are ready to run the applications.</span></span>

<span data-ttu-id="8d95e-159">Na příkazovém řádku v `read-file-upload-notification` složky, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8d95e-159">At a command prompt in the `read-file-upload-notification` folder, run the following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="8d95e-160">Na příkazovém řádku v `simulated-device` složky, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8d95e-160">At a command prompt in the `simulated-device` folder, run the following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="8d95e-161">Následující snímek obrazovky ukazuje výstup **simulated-device** aplikace:</span><span class="sxs-lookup"><span data-stu-id="8d95e-161">The following screenshot shows the output from the **simulated-device** app:</span></span>

![Výstup z aplikace simulated-device](media/iot-hub-java-java-upload/simulated-device.png)

<span data-ttu-id="8d95e-163">Následující snímek obrazovky ukazuje výstup **čtení souboru odesílání oznámení** aplikace:</span><span class="sxs-lookup"><span data-stu-id="8d95e-163">The following screenshot shows the output from the **read-file-upload-notification** app:</span></span>

![Výstup z aplikace pro čtení souboru odesílání oznámení](media/iot-hub-java-java-upload/read-file-upload-notification.png)

<span data-ttu-id="8d95e-165">Na portálu můžete použít k zobrazení nahrávaný soubor v kontejneru úložiště, které jste nakonfigurovali:</span><span class="sxs-lookup"><span data-stu-id="8d95e-165">You can use the portal to view the uploaded file in the storage container you configured:</span></span>

![Nahrávaný soubor](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a><span data-ttu-id="8d95e-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d95e-167">Next steps</span></span>

<span data-ttu-id="8d95e-168">V tomto kurzu jste zjistili, jak používat funkce nahrávání souboru služby IoT Hub pro zjednodušení nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="8d95e-168">In this tutorial, you learned how to use the file upload capabilities of IoT Hub to simplify file uploads from devices.</span></span> <span data-ttu-id="8d95e-169">Můžete dál prozkoumat funkcí služby IoT hub a scénáře v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="8d95e-169">You can continue to explore IoT hub features and scenarios with the following articles:</span></span>

* <span data-ttu-id="8d95e-170">[Vytvoření služby IoT hub prostřednictvím kódu programu][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="8d95e-170">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="8d95e-171">[Úvod do jazyka C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d95e-171">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="8d95e-172">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="8d95e-172">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="8d95e-173">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="8d95e-173">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8d95e-174">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8d95e-174">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md


