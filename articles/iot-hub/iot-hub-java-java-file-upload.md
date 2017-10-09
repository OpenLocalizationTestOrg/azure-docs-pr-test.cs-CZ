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
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a>Ukládání souborů z vašeho zařízení toohello cloud s centrem IoT

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

V tomto kurzu vychází hello kód v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurz tooshow můžete jak toouse hello [souboru nahrávání služby IoT Hub](iot-hub-devguide-file-upload.md) tooupload soubor příliš[ Úložiště objektů blob Azure](../storage/index.md). Hello kurz ukazuje, jak na:

- Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.
- Použijte hello IoT Hub souboru odesílání oznámení tootrigger zpracování souboru hello ve vaší aplikaci back-end.

Hello [Začínáme se službou IoT Hub](iot-hub-java-java-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) kurzy zobrazit hello základních zařízení cloud a z cloudu do zařízení zasílání zpráv funkcí služby IoT Hub. Hello [zprávy procesu zařízení-Cloud](iot-hub-java-java-process-d2c.md) kurz popisuje zprávy typu zařízení cloud způsob tooreliably úložiště v Azure blob storage. Ale v některých scénářích nelze mapovat snadno hello data, která vaše zařízení odesílají do hello poměrně malý zařízení cloud zprávy, které přijímá IoT Hub. Například:

* Velkých souborů, které obsahují Image
* Videa
* Data vibrace odebírána data v vysoká frekvence
* Určitou formu předběžně zpracované data.

Tyto soubory jsou obvykle dávkové zpracování v hello cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo hello [Hadoop](../hdinsight/index.md) zásobníku. Pokud budete potřebovat tooupland soubory ze zařízení, můžete nadále používat hello zabezpečení a spolehlivost služby IoT Hub.

Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly v jazyce Java:

* **simulated-device**, upravenou verzi hello aplikace vytvořená v kurzu hello [odesílání Cloud-zařízení zprávy službou IoT Hub]. Tato aplikace odešle soubor toostorage, pomocí SAS URI poskytované služby IoT hub.
* **čtení souboru odesílání oznámení**, který obdrží oznámení o odeslání souboru ze služby IoT hub.

> [!NOTE]
> IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, rozhraní .NET a Javascript) prostřednictvím sady SDK pro zařízení Azure IoT. Odkazovat toohello [Azure střediska pro vývojáře IoT] pro podrobné pokyny, jak tooconnect tooAzure vaše zařízení IoT Hub.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud nemáte účet, můžete vytvořit [bezplatný účet](http://azure.microsoft.com/pricing/free-trial/) si během několika minut.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Nahrát soubor z aplikace na zařízení

V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-java-java-c2d.md) tooupload rozbočovač tooIoT souboru.

1. Zkopírujte bitovou kopii souboru toohello `simulated-device` složku a přejmenujte ji `myimage.png`.

1. Pomocí textového editoru otevřete hello `simulated-device\src\main\java\com\mycompany\app\App.java` souboru.

1. Přidat hello deklarace proměnné toohello **aplikace** třídy:

    ```java
    private static String fileName = "myimage.png";
    ```

1. tooprocess souboru nahrávání stavové zpětné volání zprávy, přidejte následující hello vnořené třídy toohello **aplikace** třídy:

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. tooupload bitové kopie tooIoT rozbočovače, přidejte následující metoda toohello hello **aplikace** třída tooupload Image tooIoT rozbočovače:

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

1. Upravit hello **hlavní** metoda toocall hello **uploadFile** metoda, jak je znázorněno v následujícím fragmentu kódu hello:

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

1. Použití hello následující příkaz toobuild hello **simulated-device** aplikaci a zkontrolujte chyby:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Nechte si zaslat oznámení nahrávání souborů

V této části vytvoříte konzolovou aplikaci Java, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.

Je třeba hello **iothubowner** připojovací řetězec pro vaše Centrum IoT toocomplete v této části. Připojovací řetězec hello můžete najít v hello [portál Azure](https://portal.azure.com/) na hello **zásady sdíleného přístupu** okno.

1. Vytvořte projekt Maven s názvem **čtení souboru odesílání oznámení** pomocí hello následující příkaz na příkazovém řádku. Poznámka: Tento příkaz je jeden dlouhý příkaz:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Na příkazovém řádku přejděte toohello nové `read-file-upload-notification` složky.

1. Pomocí textového editoru otevřete hello `pom.xml` souboru v hello `read-file-upload-notification` složky a přidejte následující závislost toohello hello **závislosti** uzlu. Přidává se závislá hello vám umožní toouse hello **iothub-java-service-client** balíček v toocommunicate vaší aplikace pomocí služby IoT hub:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Můžete zkontrolovat nejnovější verze hello **klienta služby iot** pomocí [Maven vyhledávání](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Uložte a zavřete hello `pom.xml` souboru.

1. Pomocí textového editoru otevřete hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.

1. Přidejte následující hello **importovat** souboru toohello příkazy:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Přidejte následující proměnné na úrovni toohello hello **aplikace** třídy:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. tooprint informace o konzole toohello nahrávání souboru hello přidat hello následující vnořené třídy toohello **aplikace** třídy:

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

1. toostart hello vlákno, které čeká na oznámení o odeslání souboru, přidejte následující kód toohello hello **hlavní** metoda:

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

1. Uložte a zavřete hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` souboru.

1. Použití hello následující příkaz toobuild hello **čtení souboru odesílání oznámení** aplikaci a zkontrolujte chyby:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Spouštění aplikací hello

Teď je připraven toorun hello aplikace.

Na příkazovém řádku v hello `read-file-upload-notification` spusťte hello následující příkaz:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Na příkazovém řádku v hello `simulated-device` spusťte hello následující příkaz:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Hello následující snímek obrazovky ukazuje výstup hello hello **simulated-device** aplikace:

![Výstup z aplikace simulated-device](media/iot-hub-java-java-upload/simulated-device.png)

Hello následující snímek obrazovky ukazuje výstup hello hello **čtení souboru odesílání oznámení** aplikace:

![Výstup z aplikace pro čtení souboru odesílání oznámení](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Hello portálu tooview hello nahrát soubor v kontejneru hello úložiště, které jste nakonfigurovali, můžete použít:

![Nahrávaný soubor](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, jak nahrát soubor hello toouse možnosti nahrávání souborů toosimplify ze zařízení IoT Hub. Scénáře a tooexplore funkcí služby IoT hub můžete pokračovat v hello následující články:

* [Vytvoření služby IoT hub prostřednictvím kódu programu][lnk-create-hub]
* [Úvod tooC SDK][lnk-c-sdk]
* [Sady SDK služby Azure IoT][lnk-sdks]

toofurther prozkoumat hello služby IoT Hub, najdete v tématu:

* [Simulaci zařízení s hranou IoT][lnk-iotedge]

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


