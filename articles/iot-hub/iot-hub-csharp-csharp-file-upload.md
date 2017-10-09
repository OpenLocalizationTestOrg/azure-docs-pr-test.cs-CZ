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
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub-using-net"></a><span data-ttu-id="8f516-104">Ukládání souborů z vašeho zařízení toohello cloud službou IoT Hub pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="8f516-104">Upload files from your device toohello cloud with IoT Hub using .NET</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="8f516-105">V tomto kurzu vychází hello kód v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurz tooshow můžete jak toouse hello možnosti nahrávání souboru IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial tooshow you how toouse hello file upload capabilities of IoT Hub.</span></span> <span data-ttu-id="8f516-106">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="8f516-106">It shows you how to:</span></span>

- <span data-ttu-id="8f516-107">Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.</span><span class="sxs-lookup"><span data-stu-id="8f516-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="8f516-108">Použijte hello IoT Hub souboru odesílání oznámení tootrigger zpracování souboru hello ve vaší aplikaci back-end.</span><span class="sxs-lookup"><span data-stu-id="8f516-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="8f516-109">Hello [Začínáme se službou IoT Hub](iot-hub-csharp-csharp-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzy zobrazit hello základních zařízení cloud a z cloudu do zařízení zasílání zpráv funkcí služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-109">hello [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="8f516-110">Hello [zprávy procesu zařízení-Cloud](iot-hub-csharp-csharp-process-d2c.md) kurz popisuje zprávy typu zařízení cloud způsob tooreliably úložiště v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="8f516-110">hello [Process Device-to-Cloud messages](iot-hub-csharp-csharp-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="8f516-111">Ale v některých scénářích nelze mapovat snadno hello data, která vaše zařízení odesílají do hello poměrně malý zařízení cloud zprávy, které přijímá IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="8f516-112">Například:</span><span class="sxs-lookup"><span data-stu-id="8f516-112">For example:</span></span>

* <span data-ttu-id="8f516-113">Velkých souborů, které obsahují Image</span><span class="sxs-lookup"><span data-stu-id="8f516-113">Large files that contain images</span></span>
* <span data-ttu-id="8f516-114">Videa</span><span class="sxs-lookup"><span data-stu-id="8f516-114">Videos</span></span>
* <span data-ttu-id="8f516-115">Data vibrace odebírána data v vysoká frekvence</span><span class="sxs-lookup"><span data-stu-id="8f516-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="8f516-116">Určitou formu předběžně zpracované dat</span><span class="sxs-lookup"><span data-stu-id="8f516-116">Some form of preprocessed data</span></span>

<span data-ttu-id="8f516-117">Tyto soubory jsou obvykle dávkové zpracování v hello cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo hello [Hadoop](../hdinsight/index.md) zásobníku.</span><span class="sxs-lookup"><span data-stu-id="8f516-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="8f516-118">Pokud budete potřebovat tooupload soubory ze zařízení, můžete nadále používat hello zabezpečení a spolehlivost služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-118">When you need tooupload files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="8f516-119">Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="8f516-119">At hello end of this tutorial you run two .NET console apps:</span></span>

* <span data-ttu-id="8f516-120">**SimulatedDevice**, upravenou verzi hello aplikace vytvořená v hello [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="8f516-120">**SimulatedDevice**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial.</span></span> <span data-ttu-id="8f516-121">Tato aplikace odešle soubor toostorage, pomocí SAS URI poskytované služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="8f516-122">**ReadFileUploadNotification**, který obdrží oznámení o odeslání souboru ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-122">**ReadFileUploadNotification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="8f516-123">IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, Javy a JavaScriptu) prostřednictvím sady SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="8f516-123">IoT Hub supports many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="8f516-124">Odkazovat toohello [Azure střediska pro vývojáře IoT] pro podrobné pokyny, jak tooconnect tooAzure vaše zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="8f516-125">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="8f516-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="8f516-126">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8f516-126">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="8f516-127">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8f516-127">An active Azure account.</span></span> <span data-ttu-id="8f516-128">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="8f516-128">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="8f516-129">Nahrát soubor z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="8f516-129">Upload a file from a device app</span></span>

<span data-ttu-id="8f516-130">V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="8f516-130">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="8f516-131">V sadě Visual Studio, klikněte pravým tlačítkem na hello **SimulatedDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="8f516-131">In Visual Studio, right-click hello **SimulatedDevice** project, click **Add**, and then click **Existing Item**.</span></span> <span data-ttu-id="8f516-132">Přejděte tooan soubor bitové kopie a její zahrnutí do projektu.</span><span class="sxs-lookup"><span data-stu-id="8f516-132">Navigate tooan image file and include it in your project.</span></span> <span data-ttu-id="8f516-133">Tento kurz předpokládá hello image jmenuje `image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="8f516-133">This tutorial assumes hello image is named `image.jpg`.</span></span>

1. <span data-ttu-id="8f516-134">Klikněte pravým tlačítkem na hello image a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="8f516-134">Right-click on hello image, and then click **Properties**.</span></span> <span data-ttu-id="8f516-135">Ujistěte se, že **zkopírujte tooOutput Directory** je nastaven příliš**vždy Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="8f516-135">Make sure that **Copy tooOutput Directory** is set too**Copy always**.</span></span>

    ![][1]

1. <span data-ttu-id="8f516-136">V hello **Program.cs** soubor, přidejte následující příkazy hello horní části souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="8f516-136">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using System.IO;
    ```

1. <span data-ttu-id="8f516-137">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="8f516-137">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="8f516-138">Hello `UploadToBlobAsync` metoda přebírá v názvu souboru hello a zdroj datového proudu souboru toobe hello odeslán a zpracovává toostorage nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="8f516-138">hello `UploadToBlobAsync` method takes in hello file name and stream source of hello file toobe uploaded and handles hello upload toostorage.</span></span> <span data-ttu-id="8f516-139">konzolové aplikace Hello zobrazí hello doba trvání tooupload hello souboru.</span><span class="sxs-lookup"><span data-stu-id="8f516-139">hello console app displays hello time it takes tooupload hello file.</span></span>

1. <span data-ttu-id="8f516-140">Přidejte následující metodu v hello hello **hlavní** metoda bezprostředně před hello `Console.ReadLine()` řádku:</span><span class="sxs-lookup"><span data-stu-id="8f516-140">Add hello following method in hello **Main** method, right before hello `Console.ReadLine()` line:</span></span>

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> <span data-ttu-id="8f516-141">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="8f516-141">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="8f516-142">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="8f516-142">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="8f516-143">Nechte si zaslat oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="8f516-143">Receive a file upload notification</span></span>

<span data-ttu-id="8f516-144">V této části napíšete konzolovou aplikaci .NET, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-144">In this section, you write a .NET console app that receives file upload notification messages from IoT Hub.</span></span>

1. <span data-ttu-id="8f516-145">V hello aktuální řešení nástroje Visual Studio vytvořte projekt Visual C# Windows pomocí hello **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="8f516-145">In hello current Visual Studio solution, create a Visual C# Windows project by using hello **Console Application** project template.</span></span> <span data-ttu-id="8f516-146">Název projektu hello **ReadFileUploadNotification**.</span><span class="sxs-lookup"><span data-stu-id="8f516-146">Name hello project **ReadFileUploadNotification**.</span></span>

    ![Nový projekt v sadě Visual Studio][2]

1. <span data-ttu-id="8f516-148">V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadFileUploadNotification** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="8f516-148">In Solution Explorer, right-click hello **ReadFileUploadNotification** project, and then click **Manage NuGet Packages...**.</span></span>

1. <span data-ttu-id="8f516-149">V hello **Správce balíčků NuGet** vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="8f516-149">In hello **NuGet Package Manager** window, search for **Microsoft.Azure.Devices**, click **Install**, and accept hello terms of use.</span></span>

    <span data-ttu-id="8f516-150">Tato akce stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK služby Azure IoT] v hello **ReadFileUploadNotification** projektu.</span><span class="sxs-lookup"><span data-stu-id="8f516-150">This action downloads, installs, and adds a reference toohello [Azure IoT service SDK NuGet package] in hello **ReadFileUploadNotification** project.</span></span>

1. <span data-ttu-id="8f516-151">V hello **Program.cs** soubor, přidejte následující příkazy hello horní části souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="8f516-151">In hello **Program.cs** file, add hello following statements at hello top of hello file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. <span data-ttu-id="8f516-152">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="8f516-152">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="8f516-153">Nahraďte hodnotu zástupného symbolu hello s hello IoT hub připojovacího řetězce z [Začínáme s centrem IoT]:</span><span class="sxs-lookup"><span data-stu-id="8f516-153">Substitute hello placeholder value with hello IoT hub connection string from [Get started with IoT Hub]:</span></span>

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. <span data-ttu-id="8f516-154">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="8f516-154">Add hello following method toohello **Program** class:</span></span>

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

    <span data-ttu-id="8f516-155">Poznámka: Tento vzor receive je hello stejné jeden použité tooreceive zprávy typu cloud zařízení z aplikace hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="8f516-155">Note this receive pattern is hello same one used tooreceive cloud-to-device messages from hello device app.</span></span>

1. <span data-ttu-id="8f516-156">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="8f516-156">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter tooexit\n");
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="8f516-157">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="8f516-157">Run hello applications</span></span>

<span data-ttu-id="8f516-158">Teď je připraven toorun hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8f516-158">Now you are ready toorun hello applications.</span></span>

1. <span data-ttu-id="8f516-159">V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="8f516-159">In Visual Studio, right-click your solution, and select **Set StartUp projects**.</span></span> <span data-ttu-id="8f516-160">Vyberte **více projektů po spuštění**, pak vyberte hello **spustit** akce pro **ReadFileUploadNotification** a **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="8f516-160">Select **Multiple startup projects**, then select hello **Start** action for **ReadFileUploadNotification** and **SimulatedDevice**.</span></span>

1. <span data-ttu-id="8f516-161">Stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="8f516-161">Press **F5**.</span></span> <span data-ttu-id="8f516-162">Obě aplikace by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="8f516-162">Both applications should start.</span></span> <span data-ttu-id="8f516-163">Měli byste vidět hello nahrávání skončil za jeden konzolovou aplikaci a hello odesílání oznámení přijatých hello jiných konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8f516-163">You should see hello upload completed in one console app and hello upload notification message received by hello other console app.</span></span> <span data-ttu-id="8f516-164">Můžete použít hello [portál Azure] nebo Průzkumníka serveru Visual Studia toocheck přítomnost hello hello souboru v účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="8f516-164">You can use hello [Azure portal] or Visual Studio Server Explorer toocheck for hello presence of hello uploaded file in your Azure Storage account.</span></span>

    ![][50]

## <a name="next-steps"></a><span data-ttu-id="8f516-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8f516-165">Next steps</span></span>

<span data-ttu-id="8f516-166">V tomto kurzu jste se dozvěděli, jak nahrát soubor hello toouse možnosti nahrávání souborů toosimplify ze zařízení IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="8f516-166">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="8f516-167">Scénáře a tooexplore funkcí služby IoT hub můžete pokračovat v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="8f516-167">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="8f516-168">[Vytvoření služby IoT hub prostřednictvím kódu programu][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="8f516-168">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="8f516-169">[Úvod tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="8f516-169">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="8f516-170">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="8f516-170">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="8f516-171">toofurther prozkoumat hello služby IoT Hub, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="8f516-171">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="8f516-172">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="8f516-172">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

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
