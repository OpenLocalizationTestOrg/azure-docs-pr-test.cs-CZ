---
title: "soubory aaaUpload z tooAzure zařízení IoT Hub s .NET | Microsoft Docs"
description: "Jak tooupload souborů z cloudu toohello zařízení pomocí zařízení Azure IoT SDK pro .NET. Odeslané soubory jsou uloženy v kontejneru objektů blob úložiště Azure."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: elioda
ms.openlocfilehash: 07d555f6ba8b067bbd3233bc8eebaa220ad2388b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a>Ukládání souborů z vašeho zařízení toohello cloud službou IoT Hub pomocí rozhraní .NET

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

V tomto kurzu vychází hello kód v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurz tooshow můžete jak toouse hello možnosti nahrávání souboru IoT Hub. Jak ukazuje na:

- Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.
- Použijte hello IoT Hub souboru odesílání oznámení tootrigger zpracování souboru hello ve vaší aplikaci back-end.

Hello [Začínáme se službou IoT Hub](iot-hub-csharp-csharp-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzy zobrazit hello základních zařízení cloud a z cloudu do zařízení zasílání zpráv funkcí služby IoT Hub. Hello [zprávy procesu zařízení-Cloud](iot-hub-csharp-csharp-process-d2c.md) kurz popisuje zprávy typu zařízení cloud způsob tooreliably úložiště v Azure blob storage. Ale v některých scénářích nelze mapovat snadno hello data, která vaše zařízení odesílají do hello poměrně malý zařízení cloud zprávy, které přijímá IoT Hub. Například:

* Velkých souborů, které obsahují Image
* Videa
* Data vibrace odebírána data v vysoká frekvence
* Určitou formu předběžně zpracované dat

Tyto soubory jsou obvykle dávkové zpracování v hello cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo hello [Hadoop](../hdinsight/index.md) zásobníku. Pokud budete potřebovat tooupload soubory ze zařízení, můžete nadále používat hello zabezpečení a spolehlivost služby IoT Hub.

Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly .NET:

* **SimulatedDevice**, upravenou verzi hello aplikace vytvořená v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzu. Tato aplikace odešle soubor toostorage, pomocí SAS URI poskytované služby IoT hub.
* **ReadFileUploadNotification**, který obdrží oznámení o odeslání souboru ze služby IoT hub.

> [!NOTE]
> IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, Javy a JavaScriptu) prostřednictvím sady SDK pro zařízení Azure IoT. Odkazovat toohello [Azure střediska pro vývojáře IoT] pro podrobné pokyny, jak tooconnect tooAzure vaše zařízení IoT Hub.

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Nahrát soubor z aplikace na zařízení

V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.

1. V sadě Visual Studio, klikněte pravým tlačítkem na hello **SimulatedDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**. Přejděte tooan soubor bitové kopie a její zahrnutí do projektu. Tento kurz předpokládá hello image jmenuje `image.jpg`.

1. Klikněte pravým tlačítkem na hello image a pak klikněte na **vlastnosti**. Ujistěte se, že **zkopírujte tooOutput Directory** je nastaven příliš**vždy Kopírovat**.

    ![][1]

1. V hello **Program.cs** soubor, přidejte následující příkazy hello horní části souboru hello hello:

    ```csharp
    using System.IO;
    ```

1. Přidejte následující metodu toohello hello **Program** třídy:

    ```csharp
    private static async void SendToBlobAsync()
    {
        string fileName = "image.jpg";
        Console.WriteLine("Uploading file: {0}", fileName);
        var watch = System.Diagnostics.Stopwatch.StartNew();

        using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
        {
            await deviceClient.UploadToBlobAsync(fileName, sourceData);
        }

        watch.Stop();
        Console.WriteLine("Time tooupload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    Hello `UploadToBlobAsync` metoda přebírá v názvu souboru hello a zdroj datového proudu souboru toobe hello odeslán a zpracovává toostorage nahrávání hello. konzolové aplikace Hello zobrazí hello doba trvání tooupload hello souboru.

1. Přidejte následující metodu v hello hello **hlavní** metoda bezprostředně před hello `Console.ReadLine()` řádku:

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].

## <a name="receive-a-file-upload-notification"></a>Nechte si zaslat oznámení nahrávání souborů

V této části napíšete konzolovou aplikaci .NET, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.

1. V hello aktuální řešení nástroje Visual Studio vytvořte projekt Visual C# Windows pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **ReadFileUploadNotification**.

    ![Nový projekt v sadě Visual Studio][2]

1. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadFileUploadNotification** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .

1. V hello **Správce balíčků NuGet** vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.

    Tato akce stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK služby Azure IoT] v hello **ReadFileUploadNotification** projektu.

1. V hello **Program.cs** soubor, přidejte následující příkazy hello horní části souboru hello hello:

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello s hello IoT hub připojovacího řetězce z [Začínáme s centrem IoT]:

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. Přidejte následující metodu toohello hello **Program** třídy:

    ```csharp
    private async static void ReceiveFileUploadNotificationAsync()
    {
        var notificationReceiver = serviceClient.GetFileNotificationReceiver();

        Console.WriteLine("\nReceiving file upload notification from service");
        while (true)
        {
            var fileUploadNotification = await notificationReceiver.ReceiveAsync();
            if (fileUploadNotification == null) continue;

            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
            Console.ResetColor();

            await notificationReceiver.CompleteAsync(fileUploadNotification);
        }
    }
    ```

    Poznámka: Tento vzor receive je hello stejné jeden použité tooreceive zprávy typu cloud zařízení z aplikace hello zařízení.

1. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a>Spouštění aplikací hello

Teď je připraven toorun hello aplikace.

1. V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**. Vyberte **více projektů po spuštění**, pak vyberte hello **spustit** akce pro **ReadFileUploadNotification** a **SimulatedDevice**.

1. Stiskněte klávesu **F5**. Obě aplikace by se měl spustit. Měli byste vidět hello nahrávání skončil za jeden konzolovou aplikaci a hello odesílání oznámení přijatých hello jiných konzolovou aplikaci. Můžete použít hello [portál Azure] nebo Průzkumníka serveru Visual Studia toocheck přítomnost hello hello souboru v účtu úložiště Azure.

    ![][50]

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

<!-- Links -->

[portál Azure]: https://portal.azure.com/

[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot

[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[balíček NuGet sady SDK služby Azure IoT]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md
