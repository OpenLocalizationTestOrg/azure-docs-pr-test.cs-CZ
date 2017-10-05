---
title: "Odeslání souborů ze zařízení do služby Azure IoT Hub s .NET | Microsoft Docs"
description: "Postup nahrání souborů ze zařízení do cloudu pomocí zařízení Azure IoT SDK pro .NET. Odeslané soubory jsou uloženy v kontejneru objektů blob úložiště Azure."
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
ms.openlocfilehash: b45d85d0d77cf47f36cb793bc8c0dbe2d5c12634
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub-using-net"></a><span data-ttu-id="ed405-104">Odeslání souborů ze zařízení do cloudu službou IoT Hub pomocí rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="ed405-104">Upload files from your device to the cloud with IoT Hub using .NET</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="ed405-105">V tomto kurzu vychází kód [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzu ukazují, jak využít možnosti nahrávání souboru služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-105">This tutorial builds on the code in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial to show you how to use the file upload capabilities of IoT Hub.</span></span> <span data-ttu-id="ed405-106">Jak ukazuje na:</span><span class="sxs-lookup"><span data-stu-id="ed405-106">It shows you how to:</span></span>

- <span data-ttu-id="ed405-107">Bezpečně zadejte zařízení s Azure blob identifikátor URI pro nahrání souboru.</span><span class="sxs-lookup"><span data-stu-id="ed405-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="ed405-108">Oznámení o odeslání souboru IoT Hub použijte k aktivaci zpracování souboru ve vaší aplikaci back-end.</span><span class="sxs-lookup"><span data-stu-id="ed405-108">Use the IoT Hub file upload notifications to trigger processing the file in your app back end.</span></span>

<span data-ttu-id="ed405-109">[Začínáme se službou IoT Hub](iot-hub-csharp-csharp-getstarted.md) a [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzy zobrazit základních funkcí zařízení cloud a z cloudu do zařízení zasílání zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-109">The [Get started with IoT Hub](iot-hub-csharp-csharp-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorials show the basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="ed405-110">[Zprávy procesu zařízení-Cloud](iot-hub-csharp-csharp-process-d2c.md) kurz popisuje způsob, jak spolehlivě ukládat zprávy typu zařízení cloud do úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ed405-110">The [Process Device-to-Cloud messages](iot-hub-csharp-csharp-process-d2c.md) tutorial describes a way to reliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="ed405-111">Ale v některých scénářích nelze mapovat snadno data, která vaše zařízení odesílají do poměrně malý zprávy typu zařízení cloud, které IoT Hub přijímá.</span><span class="sxs-lookup"><span data-stu-id="ed405-111">However, in some scenarios you cannot easily map the data your devices send into the relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="ed405-112">Například:</span><span class="sxs-lookup"><span data-stu-id="ed405-112">For example:</span></span>

* <span data-ttu-id="ed405-113">Velkých souborů, které obsahují Image</span><span class="sxs-lookup"><span data-stu-id="ed405-113">Large files that contain images</span></span>
* <span data-ttu-id="ed405-114">Videa</span><span class="sxs-lookup"><span data-stu-id="ed405-114">Videos</span></span>
* <span data-ttu-id="ed405-115">Data vibrace odebírána data v vysoká frekvence</span><span class="sxs-lookup"><span data-stu-id="ed405-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="ed405-116">Určitou formu předběžně zpracované dat</span><span class="sxs-lookup"><span data-stu-id="ed405-116">Some form of preprocessed data</span></span>

<span data-ttu-id="ed405-117">Tyto soubory jsou obvykle dávkové zpracování v cloudu pomocí nástrojů, jako [Azure Data Factory](../data-factory/index.md) nebo [Hadoop](../hdinsight/index.md) zásobníku.</span><span class="sxs-lookup"><span data-stu-id="ed405-117">These files are typically batch processed in the cloud using tools such as [Azure Data Factory](../data-factory/index.md) or the [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="ed405-118">Pokud budete potřebovat k nahrání souborů ze zařízení, když můžete nadále používat zabezpečení a spolehlivost služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-118">When you need to upload files from a device, you can still use the security and reliability of IoT Hub.</span></span>

<span data-ttu-id="ed405-119">Na konci tohoto kurzu můžete spustit dvě aplikace konzoly .NET:</span><span class="sxs-lookup"><span data-stu-id="ed405-119">At the end of this tutorial you run two .NET console apps:</span></span>

* <span data-ttu-id="ed405-120">**SimulatedDevice**, upravenou verzi aplikace vytvořená v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ed405-120">**SimulatedDevice**, a modified version of the app created in the [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) tutorial.</span></span> <span data-ttu-id="ed405-121">Tato aplikace se uloží soubor do úložiště pomocí identifikátoru URI SAS poskytované služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-121">This app uploads a file to storage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="ed405-122">**ReadFileUploadNotification**, který obdrží oznámení o odeslání souboru ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-122">**ReadFileUploadNotification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="ed405-123">IoT Hub podporuje mnoho zařízení platformy a jazyky (včetně C, Javy a JavaScriptu) prostřednictvím sady SDK pro zařízení Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="ed405-123">IoT Hub supports many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="ed405-124">Odkazovat [Azure střediska pro vývojáře IoT] podrobné pokyny o tom, jak připojit zařízení ke službě Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-124">Refer to the [Azure IoT Developer Center] for step-by-step instructions on how to connect your device to Azure IoT Hub.</span></span>

<span data-ttu-id="ed405-125">Pro absolvování tohoto kurzu potřebujete:</span><span class="sxs-lookup"><span data-stu-id="ed405-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="ed405-126">Visual Studio 2015 nebo Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ed405-126">Visual Studio 2015 or Visual Studio 2017</span></span>
* <span data-ttu-id="ed405-127">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ed405-127">An active Azure account.</span></span> <span data-ttu-id="ed405-128">(Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)</span><span class="sxs-lookup"><span data-stu-id="ed405-128">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="ed405-129">Nahrát soubor z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="ed405-129">Upload a file from a device app</span></span>

<span data-ttu-id="ed405-130">V této části upravíte zařízení aplikaci, kterou jste vytvořili v [odesílání zpráv typu Cloud-zařízení s centrem IoT](iot-hub-csharp-csharp-c2d.md) pro příjem zpráv typu cloud zařízení ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-130">In this section, you modify the device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-csharp-csharp-c2d.md) to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="ed405-131">V sadě Visual Studio, klikněte pravým tlačítkem myši **SimulatedDevice** projektu, klikněte na tlačítko **přidat**a potom klikněte na **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="ed405-131">In Visual Studio, right-click the **SimulatedDevice** project, click **Add**, and then click **Existing Item**.</span></span> <span data-ttu-id="ed405-132">Přejděte na soubor obrázku a její zahrnutí do projektu.</span><span class="sxs-lookup"><span data-stu-id="ed405-132">Navigate to an image file and include it in your project.</span></span> <span data-ttu-id="ed405-133">Tento kurz předpokládá bitovou kopii jmenuje `image.jpg`.</span><span class="sxs-lookup"><span data-stu-id="ed405-133">This tutorial assumes the image is named `image.jpg`.</span></span>

1. <span data-ttu-id="ed405-134">Klikněte pravým tlačítkem na bitovou kopii a pak klikněte na **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="ed405-134">Right-click on the image, and then click **Properties**.</span></span> <span data-ttu-id="ed405-135">Ujistěte se, že **kopírovat do výstupního adresáře** je nastaven na **vždy Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="ed405-135">Make sure that **Copy to Output Directory** is set to **Copy always**.</span></span>

    ![][1]

1. <span data-ttu-id="ed405-136">V **Program.cs** souboru, v horní části souboru přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ed405-136">In the **Program.cs** file, add the following statements at the top of the file:</span></span>

    ```csharp
    using System.IO;
    ```

1. <span data-ttu-id="ed405-137">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="ed405-137">Add the following method to the **Program** class:</span></span>

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
        Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
    }
    ```

    <span data-ttu-id="ed405-138">`UploadToBlobAsync` Metoda přebírá ve zdrojovém souboru název a datový proud souboru k odeslání a zpracovává nahrávání do úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed405-138">The `UploadToBlobAsync` method takes in the file name and stream source of the file to be uploaded and handles the upload to storage.</span></span> <span data-ttu-id="ed405-139">Konzolové aplikace zobrazí dobu potřebnou k odeslání souboru.</span><span class="sxs-lookup"><span data-stu-id="ed405-139">The console app displays the time it takes to upload the file.</span></span>

1. <span data-ttu-id="ed405-140">Přidejte následující metodu v **hlavní** metoda, těsně před `Console.ReadLine()` řádku:</span><span class="sxs-lookup"><span data-stu-id="ed405-140">Add the following method in the **Main** method, right before the `Console.ReadLine()` line:</span></span>

    ```csharp
    SendToBlobAsync();
    ```

> [!NOTE]
> <span data-ttu-id="ed405-141">Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="ed405-141">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="ed405-142">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="ed405-142">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="ed405-143">Nechte si zaslat oznámení nahrávání souborů</span><span class="sxs-lookup"><span data-stu-id="ed405-143">Receive a file upload notification</span></span>

<span data-ttu-id="ed405-144">V této části napíšete konzolovou aplikaci .NET, která přijímá zprávy oznámení nahrávání souborů ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ed405-144">In this section, you write a .NET console app that receives file upload notification messages from IoT Hub.</span></span>

1. <span data-ttu-id="ed405-145">V aktuálním řešení sady Visual Studio vytvořte projekt Visual C# Windows pomocí **konzolové aplikace** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="ed405-145">In the current Visual Studio solution, create a Visual C# Windows project by using the **Console Application** project template.</span></span> <span data-ttu-id="ed405-146">Název projektu **ReadFileUploadNotification**.</span><span class="sxs-lookup"><span data-stu-id="ed405-146">Name the project **ReadFileUploadNotification**.</span></span>

    ![Nový projekt v sadě Visual Studio][2]

1. <span data-ttu-id="ed405-148">V Průzkumníku řešení klikněte pravým tlačítkem myši **ReadFileUploadNotification** projektu a pak klikněte na tlačítko **spravovat balíčky NuGet...** .</span><span class="sxs-lookup"><span data-stu-id="ed405-148">In Solution Explorer, right-click the **ReadFileUploadNotification** project, and then click **Manage NuGet Packages...**.</span></span>

1. <span data-ttu-id="ed405-149">V **Správce balíčků NuGet** vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití.</span><span class="sxs-lookup"><span data-stu-id="ed405-149">In the **NuGet Package Manager** window, search for **Microsoft.Azure.Devices**, click **Install**, and accept the terms of use.</span></span>

    <span data-ttu-id="ed405-150">Tato akce stáhne, nainstaluje a přidá odkaz na [balíček NuGet sady SDK služby Azure IoT] v **ReadFileUploadNotification** projektu.</span><span class="sxs-lookup"><span data-stu-id="ed405-150">This action downloads, installs, and adds a reference to the [Azure IoT service SDK NuGet package] in the **ReadFileUploadNotification** project.</span></span>

1. <span data-ttu-id="ed405-151">V **Program.cs** souboru, v horní části souboru přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="ed405-151">In the **Program.cs** file, add the following statements at the top of the file:</span></span>

    ```csharp
    using Microsoft.Azure.Devices;
    ```

1. <span data-ttu-id="ed405-152">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="ed405-152">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="ed405-153">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem IoT hub z [Začínáme s centrem IoT]:</span><span class="sxs-lookup"><span data-stu-id="ed405-153">Substitute the placeholder value with the IoT hub connection string from [Get started with IoT Hub]:</span></span>

    ```csharp
    static ServiceClient serviceClient;
    static string connectionString = "{iot hub connection string}";
    ```

1. <span data-ttu-id="ed405-154">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="ed405-154">Add the following method to the **Program** class:</span></span>

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

    <span data-ttu-id="ed405-155">Všimněte si, že tento vzor receive se shoduje s klíčem pro příjem zpráv typu cloud zařízení z aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="ed405-155">Note this receive pattern is the same one used to receive cloud-to-device messages from the device app.</span></span>

1. <span data-ttu-id="ed405-156">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="ed405-156">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    Console.WriteLine("Receive file upload notifications\n");
    serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
    ReceiveFileUploadNotificationAsync();
    Console.WriteLine("Press Enter to exit\n");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a><span data-ttu-id="ed405-157">Spuštění aplikací</span><span class="sxs-lookup"><span data-stu-id="ed405-157">Run the applications</span></span>

<span data-ttu-id="ed405-158">Nyní můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="ed405-158">Now you are ready to run the applications.</span></span>

1. <span data-ttu-id="ed405-159">V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**.</span><span class="sxs-lookup"><span data-stu-id="ed405-159">In Visual Studio, right-click your solution, and select **Set StartUp projects**.</span></span> <span data-ttu-id="ed405-160">Vyberte **více projektů po spuštění**, vyberte **spustit** akce pro **ReadFileUploadNotification** a **SimulatedDevice**.</span><span class="sxs-lookup"><span data-stu-id="ed405-160">Select **Multiple startup projects**, then select the **Start** action for **ReadFileUploadNotification** and **SimulatedDevice**.</span></span>

1. <span data-ttu-id="ed405-161">Stiskněte klávesu **F5**.</span><span class="sxs-lookup"><span data-stu-id="ed405-161">Press **F5**.</span></span> <span data-ttu-id="ed405-162">Obě aplikace by se měl spustit.</span><span class="sxs-lookup"><span data-stu-id="ed405-162">Both applications should start.</span></span> <span data-ttu-id="ed405-163">Měli byste vidět nahrávání dokončit v jedné aplikaci konzoly a odesílání oznámení přijatých konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ed405-163">You should see the upload completed in one console app and the upload notification message received by the other console app.</span></span> <span data-ttu-id="ed405-164">Můžete použít [portál Azure] nebo Průzkumníka serveru Visual Studia chcete zkontrolovat přítomnost nahrávaný soubor ve vašem účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ed405-164">You can use the [Azure portal] or Visual Studio Server Explorer to check for the presence of the uploaded file in your Azure Storage account.</span></span>

    ![][50]

## <a name="next-steps"></a><span data-ttu-id="ed405-165">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed405-165">Next steps</span></span>

<span data-ttu-id="ed405-166">V tomto kurzu jste zjistili, jak používat funkce nahrávání souboru služby IoT Hub pro zjednodušení nahrávání souborů ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="ed405-166">In this tutorial, you learned how to use the file upload capabilities of IoT Hub to simplify file uploads from devices.</span></span> <span data-ttu-id="ed405-167">Můžete dál prozkoumat funkcí služby IoT hub a scénáře v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="ed405-167">You can continue to explore IoT hub features and scenarios with the following articles:</span></span>

* <span data-ttu-id="ed405-168">[Vytvoření služby IoT hub prostřednictvím kódu programu][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="ed405-168">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="ed405-169">[Úvod do jazyka C SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="ed405-169">[Introduction to C SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="ed405-170">[Sady SDK služby Azure IoT][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="ed405-170">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="ed405-171">Pokud chcete prozkoumat další možnosti IoT Hub, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="ed405-171">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="ed405-172">[Simulaci zařízení s hranou IoT][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="ed405-172">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png

<!-- Links -->

<span data-ttu-id="ed405-173">[portál Azure]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="ed405-173">[Azure portal]: https://portal.azure.com/</span></span>

<span data-ttu-id="ed405-174">[Azure střediska pro vývojáře IoT]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="ed405-174">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>

<span data-ttu-id="ed405-175">[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="ed405-175">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="ed405-176">[balíček NuGet sady SDK služby Azure IoT]: https://www.nuget.org/packages/Microsoft.Azure.Devices/</span><span class="sxs-lookup"><span data-stu-id="ed405-176">[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md
