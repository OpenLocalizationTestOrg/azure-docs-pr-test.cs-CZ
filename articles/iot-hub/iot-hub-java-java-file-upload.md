---
title: "soubory aaaUpload z tooAzure zařízení IoT Hub pro Javu | Microsoft Docs"
description: "Jak tooupload souborů z cloudu toohello zařízení pomocí zařízení Azure IoT SDK pro jazyk Java. Odeslané soubory jsou uloženy v kontejneru objektů blob úložiště Azure."
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
ms.openlocfilehash: e305fe61bf7ca0aeb2c092bc2c7efebdc78d4f68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a><span data-ttu-id="d77a2-104">Ukládání souborů z vašeho zařízení toohello cloud s centrem IoT</span><span class="sxs-lookup"><span data-stu-id="d77a2-104">Upload files from your device toohello cloud with IoT Hub</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="d77a2-105">V tomto kurzu vychází hello kód v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurz tooshow můžete jak toouse hello [souboru nahrávání služby IoT Hub](iot-hub-devguide-file-upload.md) tooupload soubor příliš[ Úložiště objektů blob Azure](../storage/index.md).</span><span class="sxs-lookup"><span data-stu-id="d77a2-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorial tooshow you how toouse hello [file upload capabilities of IoT Hub](iot-hub-devguide-file-upload.md) tooupload a file too[Azure blob storage](../storage/index.md).</span></span> <span data-ttu-id="d77a2-106">Hello kurz ukazuje, jak na:</span><span class="sxs-lookup"><span data-stu-id="d77a2-106">hello tutorial shows you how to:</span></span>

- <span data-ttu-id="d77a2-107">Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="d77a2-108">Použijte hello IoT Hub souboru odesílání oznámení tootrigger zpracování souboru hello ve vaší aplikaci back-end.</span><span class="sxs-lookup"><span data-stu-id="d77a2-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="d77a2-109">Hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurzy zobrazit hello základních zařízení cloud a z cloudu do zařízení zasílání zpráv funkcí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-109">hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="d77a2-110">Hello [zprávy procesu zařízení-Cloud](iot-hub-java-java-process-d2c.md) kurz popisuje zprávy typu zařízení cloud způsob tooreliably úložiště v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="d77a2-110">hello [Process Device-to-Cloud messages](iot-hub-java-java-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="d77a2-111">Ale v některých scénářích nelze mapovat snadno hello data, která vaše zařízení odesílají do hello poměrně malý zařízení cloud zprávy, které přijímá IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="d77a2-112">Například:</span><span class="sxs-lookup"><span data-stu-id="d77a2-112">For example:</span></span>

* <span data-ttu-id="d77a2-113">Velkých souborů, které obsahují Image</span><span class="sxs-lookup"><span data-stu-id="d77a2-113">Large files that contain images</span></span>
* <span data-ttu-id="d77a2-114">Videa</span><span class="sxs-lookup"><span data-stu-id="d77a2-114">Videos</span></span>
* <span data-ttu-id="d77a2-115">Data vibrace odebírána data v vysoká frekvence</span><span class="sxs-lookup"><span data-stu-id="d77a2-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="d77a2-116">Určitou formu předběžně zpracované data.</span><span class="sxs-lookup"><span data-stu-id="d77a2-116">Some form of preprocessed data.</span></span>

<span data-ttu-id="d77a2-117">Tyto soubory jsou obvykle dávkové zpracování v hello cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo hello [Hadoop](../hdinsight/index.md) zásobníku.</span><span class="sxs-lookup"><span data-stu-id="d77a2-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="d77a2-118">Pokud budete potřebovat tooupland soubory ze zařízení, můžete nadále používat hello zabezpečení a spolehlivost služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-118">When you need tooupland files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="d77a2-119">Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="d77a2-119">At hello end of this tutorial you run two Java console apps:</span></span>

* <span data-ttu-id="d77a2-120">**simulated-device**, upravenou verzi hello aplikace vytvořená v kurzu hello [odesílání Cloud-zařízení zprávy službou IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="d77a2-120">**simulated-device**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub] tutorial.</span></span> <span data-ttu-id="d77a2-121">Tato aplikace odešle soubor toostorage, pomocí SAS URI poskytované služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="d77a2-122">**čtení souboru odesílání oznámení**, který obdrží oznámení o odeslání souboru ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-122">**read-file-upload-notification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="d77a2-123">IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, rozhraní .NET a Javascript) prostřednictvím sady SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="d77a2-123">IoT Hub supports many device platforms and languages (including C, .NET, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="d77a2-124">Odkazovat toohello [Azure střediska pro vývojáře IoT] pro podrobné pokyny, jak tooconnect tooAzure vaše zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="d77a2-125">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="d77a2-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="d77a2-126">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="d77a2-126">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="d77a2-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="d77a2-127">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="d77a2-128">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d77a2-128">An active Azure account.</span></span> <span data-ttu-id="d77a2-129">(Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="d77a2-129">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="d77a2-130">Nahrát soubor z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="d77a2-130">Upload a file from a device app</span></span>

<span data-ttu-id="d77a2-131">V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) tooupload rozbočovač tooIoT souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-131">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tooupload a file tooIoT hub.</span></span>

1. <span data-ttu-id="d77a2-132">Zkopírujte bitovou kopii souboru toohello `simulated-device` složku a přejmenujte ji `myimage.png`.</span><span class="sxs-lookup"><span data-stu-id="d77a2-132">Copy an image file toohello `simulated-device` folder and rename it `myimage.png`.</span></span>

1. <span data-ttu-id="d77a2-133">Pomocí textového editoru otevřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-133">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d77a2-134">Přidat hello deklarace proměnné toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d77a2-134">Add hello variable declaration toohello **App** class:</span></span>

    ```java
    private static String fileName = "myimage.png";
    ```

1. <span data-ttu-id="d77a2-135">tooprocess souboru nahrávání stavové zpětné volání zprávy, přidejte následující hello vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d77a2-135">tooprocess file upload status callback messages, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="d77a2-136">tooupload bitové kopie tooIoT rozbočovače, přidejte následující metoda toohello hello **aplikace** třída tooupload Image tooIoT rozbočovače:</span><span class="sxs-lookup"><span data-stu-id="d77a2-136">tooupload images tooIoT Hub, add hello following method toohello **App** class tooupload images tooIoT Hub:</span></span>

    ```java
    // Use IoT Hub tooupload a file asynchronously tooAzure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. <span data-ttu-id="d77a2-137">Upravit hello **hlavní** metoda toocall hello **uploadFile** metoda, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="d77a2-137">Modify hello **main** method toocall hello **uploadFile** method as shown in hello following snippet:</span></span>

    ```java
    client.open();

    try
    {
      // Get hello filename and start hello upload.
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

1. <span data-ttu-id="d77a2-138">Použití hello následující příkaz toobuild hello **simulated-device** aplikaci a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="d77a2-138">Use hello following command toobuild hello **simulated-device** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="d77a2-139">Nechte si zaslat oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="d77a2-139">Receive a file upload notification</span></span>

<span data-ttu-id="d77a2-140">V této části vytvoříte konzolovou aplikaci Java, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-140">In this section, you create a Java console app that receives file upload notification messages from IoT Hub.</span></span>

<span data-ttu-id="d77a2-141">Je třeba hello **iothubowner** připojovací řetězec pro vaše Centrum IoT toocomplete v této části.</span><span class="sxs-lookup"><span data-stu-id="d77a2-141">You need hello **iothubowner** connection string for your IoT Hub toocomplete this section.</span></span> <span data-ttu-id="d77a2-142">Připojovací řetězec hello můžete najít v hello [portál Azure](https://portal.azure.com/) na hello **zásady sdíleného přístupu** okno.</span><span class="sxs-lookup"><span data-stu-id="d77a2-142">You can find hello connection string in hello [Azure portal](https://portal.azure.com/) on hello **Shared access policy** blade.</span></span>

1. <span data-ttu-id="d77a2-143">Vytvořte projekt Maven s názvem **čtení souboru odesílání oznámení** pomocí hello následující příkaz na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="d77a2-143">Create a Maven project called **read-file-upload-notification** using hello following command at your command prompt.</span></span> <span data-ttu-id="d77a2-144">Poznámka: Tento příkaz je jeden dlouhý příkaz:</span><span class="sxs-lookup"><span data-stu-id="d77a2-144">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. <span data-ttu-id="d77a2-145">Na příkazovém řádku přejděte toohello nové `read-file-upload-notification` složky.</span><span class="sxs-lookup"><span data-stu-id="d77a2-145">At your command prompt, navigate toohello new `read-file-upload-notification` folder.</span></span>

1. <span data-ttu-id="d77a2-146">Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `read-file-upload-notification` složky a přidejte následující závislost toohello hello **závislosti** uzlu.</span><span class="sxs-lookup"><span data-stu-id="d77a2-146">Using a text editor, open hello `pom.xml` file in hello `read-file-upload-notification` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="d77a2-147">Přidává se závislá hello vám umožní toouse hello **iothub-java-service-client** balíček v toocommunicate vaší aplikace pomocí služby IoT hub:</span><span class="sxs-lookup"><span data-stu-id="d77a2-147">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d77a2-148">Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="d77a2-148">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="d77a2-149">Uložte a zavřete hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="d77a2-150">Pomocí textového editoru otevřete hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-150">Using a text editor, open hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d77a2-151">Přidejte následující hello **importovat** souboru toohello příkazy:</span><span class="sxs-lookup"><span data-stu-id="d77a2-151">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. <span data-ttu-id="d77a2-152">Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d77a2-152">Add hello following class-level variables toohello **App** class:</span></span>

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. <span data-ttu-id="d77a2-153">tooprint informace o konzole toohello nahrávání souboru hello přidat hello následující vnořené třídy toohello **aplikace** třídy:</span><span class="sxs-lookup"><span data-stu-id="d77a2-153">tooprint information about hello file upload toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Create a thread tooreceive file upload notifications.
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

1. <span data-ttu-id="d77a2-154">toostart hello vlákno, které čeká na oznámení o odeslání souboru, přidejte následující kód toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="d77a2-154">toostart hello thread that listens for file upload notifications, add hello following code toohello **main** method:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from hello ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start hello thread tooreceive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER tooexit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. <span data-ttu-id="d77a2-155">Uložte a zavřete hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="d77a2-155">Save and close hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="d77a2-156">Použití hello následující příkaz toobuild hello **čtení souboru odesílání oznámení** aplikaci a zkontrolujte chyby:</span><span class="sxs-lookup"><span data-stu-id="d77a2-156">Use hello following command toobuild hello **read-file-upload-notification** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="d77a2-157">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="d77a2-157">Run hello applications</span></span>

<span data-ttu-id="d77a2-158">Teď je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="d77a2-158">Now you are ready toorun hello applications.</span></span>

<span data-ttu-id="d77a2-159">Na příkazovém řádku v hello `read-file-upload-notification` spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d77a2-159">At a command prompt in hello `read-file-upload-notification` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="d77a2-160">Na příkazovém řádku v hello `simulated-device` spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d77a2-160">At a command prompt in hello `simulated-device` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="d77a2-161">Hello následující snímek obrazovky ukazuje výstup hello hello **simulated-device** aplikace:</span><span class="sxs-lookup"><span data-stu-id="d77a2-161">hello following screenshot shows hello output from hello **simulated-device** app:</span></span>

![Výstup z aplikace simulated-device](media/iot-hub-java-java-upload/simulated-device.png)

<span data-ttu-id="d77a2-163">Hello následující snímek obrazovky ukazuje výstup hello hello **čtení souboru odesílání oznámení** aplikace:</span><span class="sxs-lookup"><span data-stu-id="d77a2-163">hello following screenshot shows hello output from hello **read-file-upload-notification** app:</span></span>

![Výstup z aplikace pro čtení souboru odesílání oznámení](media/iot-hub-java-java-upload/read-file-upload-notification.png)

<span data-ttu-id="d77a2-165">Hello portálu tooview hello nahrát soubor v kontejneru hello úložiště, které jste nakonfigurovali, můžete použít:</span><span class="sxs-lookup"><span data-stu-id="d77a2-165">You can use hello portal tooview hello uploaded file in hello storage container you configured:</span></span>

![Nahrávaný soubor](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a><span data-ttu-id="d77a2-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d77a2-167">Next steps</span></span>

<span data-ttu-id="d77a2-168">V tomto kurzu jste se dozvěděli, jak nahrát soubor hello toouse možnosti nahrávání souborů toosimplify ze zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d77a2-168">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="d77a2-169">Scénáře a tooexplore funkcí služby IoT hub můžete pokračovat v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="d77a2-169">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="d77a2-170">[Vytvoření služby IoT hub prostřednictvím kódu programu][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="d77a2-170">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="d77a2-171">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="d77a2-171">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="d77a2-172">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="d77a2-172">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="d77a2-173">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="d77a2-173">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d77a2-174">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="d77a2-174">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

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


