---
title: "Knihovny tříd rozhraní .NET pomocí Azure Functions | Microsoft Docs"
description: "Naučte se vytvářet knihovny tříd rozhraní .NET pro použití s Azure Functions"
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/09/2017
ms.author: donnam
ms.openlocfilehash: 0613bb96d3afb85ff7e684246b128e4eef518d23
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a><span data-ttu-id="68523-104">Knihovny tříd rozhraní .NET pomocí Azure Functions</span><span class="sxs-lookup"><span data-stu-id="68523-104">Using .NET class libraries with Azure Functions</span></span>

<span data-ttu-id="68523-105">Kromě souborů skriptů Azure Functions podporuje publikování knihovny tříd jako implementace pro jednu nebo více funkcí.</span><span class="sxs-lookup"><span data-stu-id="68523-105">In addition to script files, Azure Functions supports publishing a class library as the implementation for one or more functions.</span></span> <span data-ttu-id="68523-106">Doporučujeme vám, že používáte [Azure funkce 2017 nástroje sady Visual Studio](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span><span class="sxs-lookup"><span data-stu-id="68523-106">We recommend that you use the [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68523-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="68523-107">Prerequisites</span></span> 

<span data-ttu-id="68523-108">Tento článek má následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="68523-108">This article has the following prerequisites:</span></span>

- <span data-ttu-id="68523-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span><span class="sxs-lookup"><span data-stu-id="68523-109">[Visual Studio 2017 15.3 Preview](https://www.visualstudio.com/vs/preview/).</span></span> <span data-ttu-id="68523-110">Nainstalujte úlohy **ASP.NET a webové vývoj** a **Azure development**.</span><span class="sxs-lookup"><span data-stu-id="68523-110">Install the workloads **ASP.NET and web development** and **Azure development**.</span></span>
- [<span data-ttu-id="68523-111">Funkce Azure nástrojů pro Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="68523-111">Azure Function Tools for Visual Studio 2017</span></span>](https://marketplace.visualstudio.com/items?itemName=AndrewBHall-MSFT.AzureFunctionToolsforVisualStudio2017)

## <a name="functions-class-library-project"></a><span data-ttu-id="68523-112">Funkce projektu knihovny tříd</span><span class="sxs-lookup"><span data-stu-id="68523-112">Functions class library project</span></span>

<span data-ttu-id="68523-113">Ze sady Visual Studio vytvořte nový projekt Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="68523-113">From Visual Studio, create a new Azure Functions project.</span></span> <span data-ttu-id="68523-114">Nová šablona projektu vytvoří soubory *host.json* a *local.settings.json*.</span><span class="sxs-lookup"><span data-stu-id="68523-114">The new project template creates the files *host.json* and *local.settings.json*.</span></span> <span data-ttu-id="68523-115">Můžete [přizpůsobit nastavení modulu runtime Azure Functions v host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span><span class="sxs-lookup"><span data-stu-id="68523-115">You can [customize Azure Functions runtime settings in host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).</span></span> 

<span data-ttu-id="68523-116">Soubor *local.settings.json* ukládá nastavení aplikace, řetězce připojení a nastavení nástroje základní funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="68523-116">The file *local.settings.json* stores app settings, connection strings, and settings for Azure Functions Core Tools.</span></span> <span data-ttu-id="68523-117">Další informace o jeho strukturu, najdete v části [kód a testovat místně na Azure functions](functions-run-local.md#local-settings).</span><span class="sxs-lookup"><span data-stu-id="68523-117">To learn more about its structure, see [Code and test Azure functions locally](functions-run-local.md#local-settings).</span></span>

### <a name="functionname-attribute"></a><span data-ttu-id="68523-118">Atribut %{FunctionName/</span><span class="sxs-lookup"><span data-stu-id="68523-118">FunctionName attribute</span></span>

<span data-ttu-id="68523-119">Atribut [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) určí metodu jako vstupní bod funkce.</span><span class="sxs-lookup"><span data-stu-id="68523-119">The attribute [`FunctionNameAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) marks a method as a function entry point.</span></span> <span data-ttu-id="68523-120">Musíte ho použít se přesně jedna aktivační událost a 0 nebo více vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="68523-120">It must be used with exactly one trigger and 0 or more input and output bindings.</span></span>

### <a name="conversion-to-functionjson"></a><span data-ttu-id="68523-121">Převod na function.json</span><span class="sxs-lookup"><span data-stu-id="68523-121">Conversion to function.json</span></span>

<span data-ttu-id="68523-122">Během vytváření projektu na Azure Functions se vytvoří soubor `function.json` v adresáři odpovídající název funkce definované `[FunctionName]`.</span><span class="sxs-lookup"><span data-stu-id="68523-122">When an Azure Functions project is built, it produces a file `function.json` in the directory matching the function name defined by `[FunctionName]`.</span></span> <span data-ttu-id="68523-123">Určuje triggerů a vazeb a odkazuje na sestavení souboru projektu.</span><span class="sxs-lookup"><span data-stu-id="68523-123">It specifies triggers and bindings and points to the project assembly file.</span></span>

<span data-ttu-id="68523-124">Tento převod provádí balíček NuGet [Microsoft\.NET\.Sdk\.funkce](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span><span class="sxs-lookup"><span data-stu-id="68523-124">This conversion is performed by the NuGet package [Microsoft\.NET\.Sdk\.Functions](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions).</span></span> <span data-ttu-id="68523-125">Zdroj je k dispozici v úložišti GitHub [azure\-funkce\-vs\-sestavení\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span><span class="sxs-lookup"><span data-stu-id="68523-125">The source is available in the GitHub repo [azure\-functions\-vs\-build\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).</span></span>

## <a name="triggers-and-bindings"></a><span data-ttu-id="68523-126">Triggery a vazby</span><span class="sxs-lookup"><span data-stu-id="68523-126">Triggers and bindings</span></span>

<span data-ttu-id="68523-127">Následující tabulka uvádí triggerů a vazeb, které jsou k dispozici v projektu knihovny tříd Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="68523-127">The following table lists the triggers and bindings that are available in an Azure Functions class library project.</span></span> <span data-ttu-id="68523-128">Všechny atributy jsou v oboru názvů `Microsoft.Azure.WebJobs`.</span><span class="sxs-lookup"><span data-stu-id="68523-128">All attributes are in the namespace `Microsoft.Azure.WebJobs`.</span></span>

| <span data-ttu-id="68523-129">Vazba</span><span class="sxs-lookup"><span data-stu-id="68523-129">Binding</span></span> | <span data-ttu-id="68523-130">Atribut</span><span class="sxs-lookup"><span data-stu-id="68523-130">Attribute</span></span> | <span data-ttu-id="68523-131">Balíček NuGet</span><span class="sxs-lookup"><span data-stu-id="68523-131">NuGet package</span></span> |
|------   | ------    | ------        |
| [<span data-ttu-id="68523-132">Aktivační událost úložiště objektů BLOB, vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-132">Blob storage trigger, input, output</span></span>](#blob-storage) | <span data-ttu-id="68523-133">[BlobAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-133">[BlobAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="68523-134">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="68523-134">[Microsoft.Azure.WebJobs]</span></span> | <span data-ttu-id="68523-135">[Úložiště objektů blob]</span><span class="sxs-lookup"><span data-stu-id="68523-135">[Blob storage]</span></span> |
| [<span data-ttu-id="68523-136">Cosmos DB vstup a výstup vazby</span><span class="sxs-lookup"><span data-stu-id="68523-136">Cosmos DB input and output binding</span></span>](#cosmos-db) | <span data-ttu-id="68523-137">[DocumentDBAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-137">[DocumentDBAttribute]</span></span> | <span data-ttu-id="68523-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span><span class="sxs-lookup"><span data-stu-id="68523-138">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]</span></span> | 
| [<span data-ttu-id="68523-139">Aktivační událost rozbočovače a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-139">Event Hubs trigger and output</span></span>](#event-hub) | <span data-ttu-id="68523-140">[EventHubTriggerAttribute], [EventHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-140">[EventHubTriggerAttribute], [EventHubAttribute]</span></span> | <span data-ttu-id="68523-141">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="68523-141">[Microsoft.Azure.WebJobs.ServiceBus]</span></span> |
| [<span data-ttu-id="68523-142">Externí soubor vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-142">External file input and output</span></span>](#api-hub) | <span data-ttu-id="68523-143">[ApiHubFileAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-143">[ApiHubFileAttribute]</span></span> | <span data-ttu-id="68523-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span><span class="sxs-lookup"><span data-stu-id="68523-144">[Microsoft.Azure.WebJobs.Extensions.ApiHub]</span></span> |
| [<span data-ttu-id="68523-145">Aktivace protokolu HTTP a webhooku</span><span class="sxs-lookup"><span data-stu-id="68523-145">HTTP and webhook trigger</span></span>](#http) | <span data-ttu-id="68523-146">[HttpTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-146">[HttpTriggerAttribute]</span></span> | <span data-ttu-id="68523-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span><span class="sxs-lookup"><span data-stu-id="68523-147">[Microsoft.Azure.WebJobs.Extensions.Http]</span></span> |
| [<span data-ttu-id="68523-148">Mobile Apps vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-148">Mobile Apps input and output</span></span>](#mobile-apps) | <span data-ttu-id="68523-149">[MobileTableAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-149">[MobileTableAttribute]</span></span> | <span data-ttu-id="68523-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span><span class="sxs-lookup"><span data-stu-id="68523-150">[Microsoft.Azure.WebJobs.Extensions.MobileApps]</span></span> | 
| [<span data-ttu-id="68523-151">Výstup centra oznámení</span><span class="sxs-lookup"><span data-stu-id="68523-151">Notification Hubs output</span></span>](#nh) | <span data-ttu-id="68523-152">[NotificationHubAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-152">[NotificationHubAttribute]</span></span> | <span data-ttu-id="68523-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span><span class="sxs-lookup"><span data-stu-id="68523-153">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]</span></span> | 
| [<span data-ttu-id="68523-154">Aktivace fronty úložiště a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-154">Queue storage trigger and output</span></span>](#queue) | <span data-ttu-id="68523-155">[QueueAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-155">[QueueAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="68523-156">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="68523-156">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="68523-157">Sendgrid vám umožňuje výstup</span><span class="sxs-lookup"><span data-stu-id="68523-157">SendGrid output</span></span>](#sendgrid) | <span data-ttu-id="68523-158">[SendGridAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-158">[SendGridAttribute]</span></span> | <span data-ttu-id="68523-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span><span class="sxs-lookup"><span data-stu-id="68523-159">[Microsoft.Azure.WebJobs.Extensions.SendGrid]</span></span> | 
| [<span data-ttu-id="68523-160">Aktivace služby Service Bus a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-160">Service Bus trigger and output</span></span>](#service-bus) | <span data-ttu-id="68523-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-161">[ServiceBusAttribute], [ServiceBusAccountAttribute]</span></span> | <span data-ttu-id="68523-162">[Microsoft.Azure.WebJobs.ServiceBus]</span><span class="sxs-lookup"><span data-stu-id="68523-162">[Microsoft.Azure.WebJobs.ServiceBus]</span></span>
| [<span data-ttu-id="68523-163">Tabulka úložiště vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-163">Table storage input and output</span></span>](#table) | <span data-ttu-id="68523-164">[TableAttribute], [StorageAccountAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-164">[TableAttribute], [StorageAccountAttribute]</span></span> | <span data-ttu-id="68523-165">[Microsoft.Azure.WebJobs]</span><span class="sxs-lookup"><span data-stu-id="68523-165">[Microsoft.Azure.WebJobs]</span></span> | 
| [<span data-ttu-id="68523-166">Trigger časovače</span><span class="sxs-lookup"><span data-stu-id="68523-166">Timer trigger</span></span>](#timer) | <span data-ttu-id="68523-167">[TimerTriggerAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-167">[TimerTriggerAttribute]</span></span> | <span data-ttu-id="68523-168">[Microsoft.Azure.WebJobs.Extensions]</span><span class="sxs-lookup"><span data-stu-id="68523-168">[Microsoft.Azure.WebJobs.Extensions]</span></span> | 
| [<span data-ttu-id="68523-169">Výstup Twilio</span><span class="sxs-lookup"><span data-stu-id="68523-169">Twilio output</span></span>](#twilio) | <span data-ttu-id="68523-170">[TwilioSmsAttribute]</span><span class="sxs-lookup"><span data-stu-id="68523-170">[TwilioSmsAttribute]</span></span> | <span data-ttu-id="68523-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span><span class="sxs-lookup"><span data-stu-id="68523-171">[Microsoft.Azure.WebJobs.Extensions.Twilio]</span></span> | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-and-output-bindings"></a><span data-ttu-id="68523-172">Aktivační událost úložiště objektů BLOB, vstup a výstup vazby</span><span class="sxs-lookup"><span data-stu-id="68523-172">Blob storage trigger, input, and output bindings</span></span>

<span data-ttu-id="68523-173">Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="68523-173">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="68523-174">Další informace o výrazy vazby a metadata, najdete v části [vazby úložiště objektů Blob v Azure funkce](functions-bindings-storage-blob.md).</span><span class="sxs-lookup"><span data-stu-id="68523-174">For more information on binding expressions and metadata, see [Azure Functions Blob storage bindings](functions-bindings-storage-blob.md).</span></span>

<span data-ttu-id="68523-175">Aktivační události objektu blob je definovaná pomocí `[BlobTrigger]` atribut.</span><span class="sxs-lookup"><span data-stu-id="68523-175">A blob trigger is defined with the `[BlobTrigger]` attribute.</span></span> <span data-ttu-id="68523-176">Můžete použít atribut `[StorageAccount]` zadat účet úložiště, který se používá celé funkce nebo třídy.</span><span class="sxs-lookup"><span data-stu-id="68523-176">You can use the attribute `[StorageAccount]` to define the storage account that is used by an entire function or class.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

<span data-ttu-id="68523-177">Objekt BLOB vstup a výstup jsou definovány pomocí `[Blob]` atribut spolu s `FileAccess` parametr označující číst nebo zapisovat.</span><span class="sxs-lookup"><span data-stu-id="68523-177">Blob input and output are defined using the `[Blob]` attribute, along with a `FileAccess` parameter indicating read or write.</span></span> <span data-ttu-id="68523-178">Následující příklad používá objekt blob aktivační události a objektů blob výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="68523-178">The following example uses a blob trigger and blob output binding.</span></span>

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-input-and-output-bindings"></a><span data-ttu-id="68523-179">Cosmos DB vstup a výstup vazby</span><span class="sxs-lookup"><span data-stu-id="68523-179">Cosmos DB input and output bindings</span></span>

<span data-ttu-id="68523-180">Azure Functions podporuje vstup a výstup vazby pro Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="68523-180">Azure Functions supports input and output bindings for Cosmos DB.</span></span> <span data-ttu-id="68523-181">Další informace o funkcích vazby Cosmos DB najdete v tématu [Azure funkce Cosmos DB vazby](functions-bindings-documentdb.md).</span><span class="sxs-lookup"><span data-stu-id="68523-181">To learn more about the features of the Cosmos DB binding, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>

<span data-ttu-id="68523-182">Chcete-li vytvořit vazbu na dokument Cosmos DB, použijte atribut `[DocumentDB]` v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span><span class="sxs-lookup"><span data-stu-id="68523-182">To bind to a Cosmos DB document, use the attribute `[DocumentDB]` in the NuGet package [Microsoft.Azure.WebJobs.Extensions.DocumentDB].</span></span> <span data-ttu-id="68523-183">V následujícím příkladu má aktivační procedury fronty a DocumentDB API, výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="68523-183">The following example has a queue trigger and a DocumentDB API output binding:</span></span>

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "DocDBConnection")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a><span data-ttu-id="68523-184">Aktivační událost rozbočovače a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-184">Event Hubs trigger and output</span></span>

<span data-ttu-id="68523-185">Azure Functions podporuje aktivaci a výstupní vazeb pro služby Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="68523-185">Azure Functions supports trigger and output bindings for Event Hubs.</span></span> <span data-ttu-id="68523-186">Další informace najdete v tématu [centra událostí Azure funkce vazby](functions-bindings-event-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="68523-186">For more information, see  [Azure Functions Event Hub bindings](functions-bindings-event-hubs.md).</span></span>

<span data-ttu-id="68523-187">Typy `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` a `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="68523-187">The types `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` and `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="68523-188">Následující příklad používá aktivační procedury Centrum událostí:</span><span class="sxs-lookup"><span data-stu-id="68523-188">The following example uses an Event Hub trigger:</span></span>

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

<span data-ttu-id="68523-189">V následujícím příkladu má centra událostí výstupu pomocí návratovou hodnotu metody jako výstup:</span><span class="sxs-lookup"><span data-stu-id="68523-189">The following example has an Event Hub output, using the method return value as the output:</span></span>

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a><span data-ttu-id="68523-190">Externí soubor vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-190">External file input and output</span></span>

<span data-ttu-id="68523-191">Azure Functions podporuje aktivační událost, vstup a výstup vazby pro externích souborů, jako je například Google Drive, Dropbox a OneDrive.</span><span class="sxs-lookup"><span data-stu-id="68523-191">Azure Functions supports trigger, input, and output bindings for external files, such as Google Drive, Dropbox, and OneDrive.</span></span> <span data-ttu-id="68523-192">Další informace najdete v tématu [externí soubor funkce Azure vazby](functions-bindings-external-file.md).</span><span class="sxs-lookup"><span data-stu-id="68523-192">To learn more, see [Azure Functions External File bindings](functions-bindings-external-file.md).</span></span> <span data-ttu-id="68523-193">Atributy `[ExternalFileTrigger]` a `[ExternalFile]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span><span class="sxs-lookup"><span data-stu-id="68523-193">The attributes `[ExternalFileTrigger]` and `[ExternalFile]` are defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.ApiHub].</span></span>

<span data-ttu-id="68523-194">Následující příklad jazyka C# ukazuje externí soubor vstup a výstup vazby.</span><span class="sxs-lookup"><span data-stu-id="68523-194">The following C# example demonstrates an external file input and output binding.</span></span> <span data-ttu-id="68523-195">Kód zkopíruje vstupní soubor do výstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="68523-195">The code copies the input file to the output file.</span></span>

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a><span data-ttu-id="68523-196">HTTP a webhooky</span><span class="sxs-lookup"><span data-stu-id="68523-196">HTTP and webhooks</span></span>

<span data-ttu-id="68523-197">Použití `HttpTrigger` definici na triggeru protokolu HTTP nebo webhooku atributu.</span><span class="sxs-lookup"><span data-stu-id="68523-197">Use the `HttpTrigger` attribute to define an HTTP trigger or webhook.</span></span> <span data-ttu-id="68523-198">Tento atribut je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.Http].</span><span class="sxs-lookup"><span data-stu-id="68523-198">This attribute is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.Http].</span></span> <span data-ttu-id="68523-199">Můžete přizpůsobit oprávnění na úrovni, typ webhooku, trasy a metody.</span><span class="sxs-lookup"><span data-stu-id="68523-199">You can customize the authorization level, webhook type, route, and methods.</span></span> <span data-ttu-id="68523-200">V následujícím příkladu definuje aktivační procedury HTTP s anonymní ověřování a _genericJson_ webhooku typu.</span><span class="sxs-lookup"><span data-stu-id="68523-200">The following example defines an HTTP trigger with anonymous authentication and _genericJson_ webhook type.</span></span>

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a><span data-ttu-id="68523-201">Mobile Apps vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-201">Mobile Apps input and output</span></span>

<span data-ttu-id="68523-202">Azure Functions podporuje vstup a výstup vazby pro Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="68523-202">Azure Functions supports input and output bindings for Mobile Apps.</span></span> <span data-ttu-id="68523-203">Další informace najdete v tématu [Azure funkce Mobile Apps vazby](functions-bindings-mobile-apps.md).</span><span class="sxs-lookup"><span data-stu-id="68523-203">To learn more, see [Azure Functions Mobile Apps bindings](functions-bindings-mobile-apps.md).</span></span> <span data-ttu-id="68523-204">Atribut `[MobileTable]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span><span class="sxs-lookup"><span data-stu-id="68523-204">The attribute `[MobileTable]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.MobileApps].</span></span>

<span data-ttu-id="68523-205">Následující příklad ukazuje Mobile Apps výstup vazby:</span><span class="sxs-lookup"><span data-stu-id="68523-205">The following example demonstrates a Mobile Apps output binding:</span></span>

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a><span data-ttu-id="68523-206">Výstup centra oznámení</span><span class="sxs-lookup"><span data-stu-id="68523-206">Notification Hubs output</span></span>

<span data-ttu-id="68523-207">Azure Functions podporuje vazbu výstup pro centra oznámení.</span><span class="sxs-lookup"><span data-stu-id="68523-207">Azure Functions supports an output binding for Notification Hubs.</span></span> <span data-ttu-id="68523-208">Další informace najdete v tématu [centra oznámení Azure funkce výstup vazby](functions-bindings-notification-hubs.md).</span><span class="sxs-lookup"><span data-stu-id="68523-208">To learn more, see [Azure Functions Notification Hub output binding](functions-bindings-notification-hubs.md).</span></span> <span data-ttu-id="68523-209">Atribut `[NotificationHub]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span><span class="sxs-lookup"><span data-stu-id="68523-209">The attribute `[NotificationHub]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].</span></span>

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a><span data-ttu-id="68523-210">Aktivace fronty úložiště a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-210">Queue storage trigger and output</span></span>

<span data-ttu-id="68523-211">Azure Functions podporuje aktivaci a výstupní vazby pro Azure fronty.</span><span class="sxs-lookup"><span data-stu-id="68523-211">Azure Functions supports trigger and output bindings for Azure queues.</span></span> <span data-ttu-id="68523-212">Další informace najdete v tématu [Azure funkce Queue Storage vazby](functions-bindings-storage-queue.md).</span><span class="sxs-lookup"><span data-stu-id="68523-212">For more information, see [Azure Functions Queue Storage bindings](functions-bindings-storage-queue.md).</span></span>

<span data-ttu-id="68523-213">Následující příklad ukazuje, jak používat návratový typ funkce s výstupní fronty, vazbu, pomocí `[Queue]` atribut.</span><span class="sxs-lookup"><span data-stu-id="68523-213">The following example shows how to use the function return type with a queue output binding, using the `[Queue]` attribute.</span></span> <span data-ttu-id="68523-214">Chcete-li definovat aktivační procedury fronty, použijte `[QueueTrigger]` atribut.</span><span class="sxs-lookup"><span data-stu-id="68523-214">To define a queue trigger, use the `[QueueTrigger]` attribute.</span></span>

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }

    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```

<a name="sendgrid"></a>

### <a name="sendgrid-output"></a><span data-ttu-id="68523-215">Sendgrid vám umožňuje výstup</span><span class="sxs-lookup"><span data-stu-id="68523-215">SendGrid output</span></span>

<span data-ttu-id="68523-216">Azure Functions podporuje sendgrid vám umožňuje výstup, vazba pro odesílání e-mailu prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="68523-216">Azure Functions supports a SendGrid output binding for sending email programmatically.</span></span> <span data-ttu-id="68523-217">Další informace najdete v tématu [sendgrid vám umožňuje funkce Azure vazby](functions-bindings-sendgrid.md).</span><span class="sxs-lookup"><span data-stu-id="68523-217">To learn more, see [Azure Functions SendGrid bindings](functions-bindings-sendgrid.md).</span></span>

<span data-ttu-id="68523-218">Atribut `[SendGrid]` je definována v balíček NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span><span class="sxs-lookup"><span data-stu-id="68523-218">The attribute `[SendGrid]` is defined in the NuGet package [Microsoft.Azure.WebJobs.Extensions.SendGrid].</span></span>

<span data-ttu-id="68523-219">Následuje příklad použití aktivační procedury fronty Service Bus a pomocí vazby sendgrid vám umožňuje výstup `SendGridMessage`:</span><span class="sxs-lookup"><span data-stu-id="68523-219">The following is an example of using a Service Bus queue trigger and a SendGrid output binding using `SendGridMessage`:</span></span>

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a><span data-ttu-id="68523-220">Aktivace služby Service Bus a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-220">Service Bus trigger and output</span></span>

<span data-ttu-id="68523-221">Azure Functions podporuje aktivaci a výstupní vazby pro fronty sběrnice a témata.</span><span class="sxs-lookup"><span data-stu-id="68523-221">Azure Functions supports trigger and output bindings for Service Bus queues and topics.</span></span> <span data-ttu-id="68523-222">Další informace o konfiguraci vazby najdete v tématu [Azure funkce Service Bus vazby](functions-bindings-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="68523-222">For more information on configuring bindings, see [Azure Functions Service Bus bindings](functions-bindings-service-bus.md).</span></span>

<span data-ttu-id="68523-223">Atributy `[ServiceBusTrigger]` a `[ServiceBus]` jsou definovány v balíčku NuGet [Microsoft.Azure.WebJobs.ServiceBus].</span><span class="sxs-lookup"><span data-stu-id="68523-223">The attributes `[ServiceBusTrigger]` and `[ServiceBus]` are defined in the NuGet package [Microsoft.Azure.WebJobs.ServiceBus].</span></span> 

<span data-ttu-id="68523-224">Následuje příklad aktivační procedury fronty sběrnice:</span><span class="sxs-lookup"><span data-stu-id="68523-224">The following is an example of a Service Bus queue trigger:</span></span>

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<span data-ttu-id="68523-225">Následuje příklad výstupu Service Bus, vazbu, pomocí návratový typ metody jako výstup:</span><span class="sxs-lookup"><span data-stu-id="68523-225">The following is an example of a Service Bus output binding, using the method return type as the output:</span></span>

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a><span data-ttu-id="68523-226">Tabulka úložiště vstup a výstup</span><span class="sxs-lookup"><span data-stu-id="68523-226">Table storage input and output</span></span>

<span data-ttu-id="68523-227">Azure Functions podporuje vstup a výstup vazby pro Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="68523-227">Azure Functions supports input and output bindings for Azure Table storage.</span></span> <span data-ttu-id="68523-228">Další informace najdete v tématu [vazby úložiště Azure Table funkce](functions-bindings-storage-table.md).</span><span class="sxs-lookup"><span data-stu-id="68523-228">To learn more, see [Azure Functions Table storage bindings](functions-bindings-storage-table.md).</span></span>

<span data-ttu-id="68523-229">V následujícím příkladu je třída s dvě funkce, ukázka tabulky úložiště výstupní a vstupní vazby.</span><span class="sxs-lookup"><span data-stu-id="68523-229">The following example is a class with two functions, demonstrating table storage output and input bindings.</span></span> 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a><span data-ttu-id="68523-230">Trigger časovače</span><span class="sxs-lookup"><span data-stu-id="68523-230">Timer trigger</span></span>

<span data-ttu-id="68523-231">Azure Functions nabízí vazbu aktivační událost časovače, který vám umožní spustit funkce kódu podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="68523-231">Azure Functions has a timer trigger binding that lets you run your function code based on a defined schedule.</span></span> <span data-ttu-id="68523-232">Další informace o funkcích vazby naleznete v tématu [naplánovat provádění kódu s Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="68523-232">To learn more about the features of the binding, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>

<span data-ttu-id="68523-233">Spotřeba plánu, můžete definovat plány se [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span><span class="sxs-lookup"><span data-stu-id="68523-233">On the Consumption plan, you can define schedules with a [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression).</span></span> <span data-ttu-id="68523-234">Pokud používáte plánu služby App Service, můžete taky řetězec časový interval.</span><span class="sxs-lookup"><span data-stu-id="68523-234">If you're using an App Service Plan, you can also use a TimeSpan string.</span></span> 

<span data-ttu-id="68523-235">V následujícím příkladu definuje aktivační událost časovače, která se spouští každých 5 minut:</span><span class="sxs-lookup"><span data-stu-id="68523-235">The following example defines a timer trigger that runs every 5 minutes:</span></span>

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a><span data-ttu-id="68523-236">Výstup Twilio</span><span class="sxs-lookup"><span data-stu-id="68523-236">Twilio output</span></span>

<span data-ttu-id="68523-237">Azure Functions podporuje Twilio výstupní vazby k povolení funkcí k posílání textových zpráv SMS.</span><span class="sxs-lookup"><span data-stu-id="68523-237">Azure Functions supports Twilio output bindings to enable your functions to send SMS text messages.</span></span> <span data-ttu-id="68523-238">Další informace najdete v tématu [poslat SMS zprávy z Azure Functions pomocí Twilio výstup vazby](functions-bindings-twilio.md).</span><span class="sxs-lookup"><span data-stu-id="68523-238">To learn more, see [Send SMS messages from Azure Functions using the Twilio output binding](functions-bindings-twilio.md).</span></span> 

<span data-ttu-id="68523-239">Atribut `[TwilioSms]` je definována v balíčku [Microsoft.Azure.WebJobs.Extensions.Twilio].</span><span class="sxs-lookup"><span data-stu-id="68523-239">The attribute `[TwilioSms]` is defined in the package [Microsoft.Azure.WebJobs.Extensions.Twilio].</span></span>

<span data-ttu-id="68523-240">Následující příklad jazyka C# používá fronty aktivační události a Twilio výstupní vazby:</span><span class="sxs-lookup"><span data-stu-id="68523-240">The following C# example uses a queue trigger and a Twilio output binding:</span></span>

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a><span data-ttu-id="68523-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="68523-241">Next steps</span></span>

<span data-ttu-id="68523-242">Další informace o používání Azure Functions v C# skriptování najdete v tématu [C funkce Azure\# skript referenční informace pro vývojáře](functions-reference-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="68523-242">For more information on using Azure Functions in C# scripting, see [Azure Functions C\# script developer reference](functions-reference-csharp.md).</span></span>

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
<span data-ttu-id="68523-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-243">[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1</span></span>
<span data-ttu-id="68523-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-244">[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta1</span></span>
<span data-ttu-id="68523-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-245">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="68523-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-246">[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1</span></span>
<span data-ttu-id="68523-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-247">[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1</span></span>
<span data-ttu-id="68523-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-248">[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1</span></span>
<span data-ttu-id="68523-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-249">[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1</span></span>
<span data-ttu-id="68523-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-250">[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1</span></span>
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
<span data-ttu-id="68523-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span><span class="sxs-lookup"><span data-stu-id="68523-251">[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4</span></span>
<span data-ttu-id="68523-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-252">[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1</span></span>
<span data-ttu-id="68523-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span><span class="sxs-lookup"><span data-stu-id="68523-253">[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1</span></span>


<!-- Links to source --> 
<span data-ttu-id="68523-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-254">[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs</span></span>
<span data-ttu-id="68523-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-255">[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs</span></span>
<span data-ttu-id="68523-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-256">[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs</span></span>
<span data-ttu-id="68523-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-257">[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs</span></span>
<span data-ttu-id="68523-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span><span class="sxs-lookup"><span data-stu-id="68523-258">[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs </span></span>
<span data-ttu-id="68523-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-259">[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs</span></span>
<span data-ttu-id="68523-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-260">[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs</span></span>
<span data-ttu-id="68523-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-261">[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs</span></span>
<span data-ttu-id="68523-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-262">[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs</span></span>
<span data-ttu-id="68523-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-263">[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs</span></span>
<span data-ttu-id="68523-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-264">[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs</span></span>
<span data-ttu-id="68523-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-265">[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs</span></span>
<span data-ttu-id="68523-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-266">[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs</span></span>
<span data-ttu-id="68523-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-267">[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs</span></span>
<span data-ttu-id="68523-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-268">[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs</span></span>
<span data-ttu-id="68523-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span><span class="sxs-lookup"><span data-stu-id="68523-269">[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs</span></span>
