---
title: "Používání Azure Service Bus se sadou WebJobs SDK"
description: "Naučte se používat témata a fronty Azure Service Bus pomocí WebJobs SDK."
services: app-service\web, service-bus
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: 2114a934-135b-42b8-871c-6cc040214e76
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 7cec03cae5d20d1ead9eb24e99415c33d8b76f05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a><span data-ttu-id="b2421-103">Používání Azure Service Bus se sadou WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="b2421-103">How to use Azure Service Bus with the WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="b2421-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b2421-104">Overview</span></span>
<span data-ttu-id="b2421-105">Tato příručka obsahuje C# ukázek kódu, které ukazují, jak spustit proces, při příjmu zprávy Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2421-105">This guide provides C# code samples that show how to trigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="b2421-106">Kód – ukázky použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="b2421-106">The code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="b2421-107">V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b2421-107">The guide assumes you know [how to create a WebJob project in Visual Studio with connection strings that point to your storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="b2421-108">Fragmenty kódu ukazují jenom funkce, není kód, který vytvoří `JobHost` objektu jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b2421-108">The code snippets only show functions, not the code that creates the `JobHost` object as in this example:</span></span>

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

<span data-ttu-id="b2421-109">A [kompletní příklad kódu Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je v úložišti azure webjobs sdk ukázky na webu GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="b2421-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in the azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="b2421-110"><a id="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="b2421-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="b2421-111">Pro práci s Service Bus, je třeba nainstalovat [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) balíček NuGet kromě dalších balíčků WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="b2421-111">To work with Service Bus you have to install the [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition to the other WebJobs SDK packages.</span></span> 

<span data-ttu-id="b2421-112">Také budete muset nastavit připojovací řetězec AzureWebJobsServiceBus kromě úložiště připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="b2421-112">You also have to set the AzureWebJobsServiceBus connection string in addition to the storage connection strings.</span></span>  <span data-ttu-id="b2421-113">To provedete `connectionStrings` v souboru App.config, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b2421-113">You can do this in the `connectionStrings` section of the App.config file, as shown in the following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="b2421-114">Ukázkový projekt, který obsahuje řetězec nastavení připojení služby Service Bus v souboru App.config, najdete v části [Service Bus příklad](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="b2421-114">For a sample project that includes the Service Bus connection string setting in the App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="b2421-115">Připojovací řetězce lze nastavit i v prostředí Azure runtime, které pak přepíše nastavení App.config při spuštění vytvářené webové úlohy v Azure; Další informace najdete v tématu [začít pracovat s WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="b2421-115">The connection strings can also be set in the Azure runtime environment, which then overrides the App.config settings when the WebJob runs in Azure; for more information, see [Get Started with the WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="b2421-116"><a id="trigger"></a>Postup aktivace funkce při příjmu zprávy fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="b2421-116"><a id="trigger"></a> How to trigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="b2421-117">Chcete-li vytvořit funkci, která volá WebJobs SDK při příjmu zprávy fronty, použijte `ServiceBusTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="b2421-117">To write a function that the WebJobs SDK calls when a queue message is received, use the `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="b2421-118">Konstruktoru atributu přebírá parametr, který určuje název fronty pro cyklické dotazování.</span><span class="sxs-lookup"><span data-stu-id="b2421-118">The attribute constructor takes a parameter that specifies the name of the queue to poll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="b2421-119">Jak funguje ServiceBusTrigger</span><span class="sxs-lookup"><span data-stu-id="b2421-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="b2421-120">Sada SDK přijme nějakou zprávu v `PeekLock` režimu a volání `Complete` na zprávu, pokud funkci skončí úspěšně, nebo volání `Abandon` Pokud funkce selže.</span><span class="sxs-lookup"><span data-stu-id="b2421-120">The SDK receives a message in `PeekLock` mode and calls `Complete` on the message if the function finishes successfully, or calls `Abandon` if the function fails.</span></span> <span data-ttu-id="b2421-121">Pokud je funkce spuštěná déle, než `PeekLock` automaticky obnovují vždy vypršel časový limit, zámek.</span><span class="sxs-lookup"><span data-stu-id="b2421-121">If the function runs longer than the `PeekLock` timeout, the lock is automatically renewed.</span></span>

<span data-ttu-id="b2421-122">Service Bus nepodporuje svůj vlastní zpracování poškozených fronty, která nemůže být řídí nebo konfigurovat tak, že sada WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="b2421-122">Service Bus does its own poison queue handling which cannot be controlled or configured by the WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="b2421-123">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="b2421-123">String queue message</span></span>
<span data-ttu-id="b2421-124">Následující ukázka kódu přečte zprávu fronty, která obsahuje řetězec a zapisuje řetězec na řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="b2421-124">The following code sample reads a queue message that contains a string and writes the string to the WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="b2421-125">**Poznámka:** při vytváření fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, nezapomeňte nastavit [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) k "text/plain".</span><span class="sxs-lookup"><span data-stu-id="b2421-125">**Note:** If you are creating the queue messages in an application that doesn't use the WebJobs SDK, make sure to set [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) to "text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="b2421-126">Zpráva fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="b2421-126">POCO queue message</span></span>
<span data-ttu-id="b2421-127">Sada SDK bude automaticky deserializovat zprávu fronty, která obsahuje JSON pro objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu.</span><span class="sxs-lookup"><span data-stu-id="b2421-127">The SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="b2421-128">Následující ukázka kódu přečte zprávu fronty, který obsahuje `BlobInformation` objekt, který má `BlobName` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="b2421-128">The following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

<span data-ttu-id="b2421-129">Ukázky kódu znázorňující způsob použití vlastnosti objektů POCO pro práci s objekty BLOB a tabulek ve stejné funkci, najdete v článku [úložiště fronty verzi tohoto článku](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="b2421-129">For code samples showing how to use properties of the POCO to work with blobs and tables in the same function, see the [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="b2421-130">Pokud váš kód, který vytvoří zprávu fronty nepoužívá WebJobs SDK, použijte kód podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="b2421-130">If your code that creates the queue message doesn't use the WebJobs SDK, use code similar to the following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="b2421-131">Typy ServiceBusTrigger pracuje s</span><span class="sxs-lookup"><span data-stu-id="b2421-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="b2421-132">Kromě `string` a typy objektů POCO, můžete použít `ServiceBusTrigger` atribut s bajtové pole nebo `BrokeredMessage` objektu.</span><span class="sxs-lookup"><span data-stu-id="b2421-132">Besides `string` and POCO  types, you can use the `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="b2421-133"><a id="create"></a>Postup vytvoření zprávy fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="b2421-133"><a id="create"></a> How to create Service Bus queue messages</span></span>
<span data-ttu-id="b2421-134">Zápis funkce, která vytvoří nové použití fronty zpráv `ServiceBus` atribut a předat název fronty konstruktoru atributu.</span><span class="sxs-lookup"><span data-stu-id="b2421-134">To write a function that creates a new queue message use the `ServiceBus` attribute and pass in the queue name to the attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="b2421-135">Vytvořit zprávu jedné frontě v jiných asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="b2421-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="b2421-136">Následující příklad kódu používá výstupní parametr k vytvoření nové zprávy ve frontě s názvem "outputqueue" se stejným obsahem jako ve frontě s názvem "inputqueue" přijaté zprávy.</span><span class="sxs-lookup"><span data-stu-id="b2421-136">The following code sample uses an output parameter to create a new message in the queue named "outputqueue" with the same content as the message received in the queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="b2421-137">Výstupní parametr pro vytvoření zpráva s jednou frontou může být libovolná z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="b2421-137">The output parameter for creating a single queue message can be any of the following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="b2421-138">Serializovatelné typ objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="b2421-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="b2421-139">Automaticky serializovanou jako JSON.</span><span class="sxs-lookup"><span data-stu-id="b2421-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="b2421-140">Pro parametry typu objektů POCO fronty zpráv je vytvořen vždy při ukončení funkce; Pokud má parametr hodnotu null, sadu SDK vytvoří zprávu fronty, která vrátí hodnotu null, když zprávu přijme a deserializovat.</span><span class="sxs-lookup"><span data-stu-id="b2421-140">For POCO type parameters, a queue message is always created when the function ends; if the parameter is null, the SDK creates a queue message that will return null when the message is received and deserialized.</span></span> <span data-ttu-id="b2421-141">Pro jiné typy Pokud má parametr hodnotu null žádná zpráva fronty se vytvoří.</span><span class="sxs-lookup"><span data-stu-id="b2421-141">For the other types, if the parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="b2421-142">Vytvoření více fronty zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="b2421-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="b2421-143">Chcete-li vytvořit více zpráv, použijte `ServiceBus` atribut s `ICollector<T>` nebo `IAsyncCollector<T>`, jak znázorňuje následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="b2421-143">To create multiple messages, use  the `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in the following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="b2421-144">Každou zprávu fronty je vytvořena ihned po `Add` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="b2421-144">Each queue message is created immediately when the `Add` method is called.</span></span>

## <span data-ttu-id="b2421-145"><a id="topics"></a>Jak pracovat s témat sběrnice Service Bus</span><span class="sxs-lookup"><span data-stu-id="b2421-145"><a id="topics"></a>How to work with Service Bus topics</span></span>
<span data-ttu-id="b2421-146">Chcete-li vytvořit funkci, která volá sady SDK při příjmu zprávy v tématu Service Bus, použijte `ServiceBusTrigger` atribut s konstruktor, který přebírá název tématu a název odběru, jak znázorňuje následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="b2421-146">To write a function that the SDK calls when a message is received on a Service Bus topic, use the `ServiceBusTrigger` attribute with the constructor that takes topic name and subscription name, as shown in the following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="b2421-147">Chcete-li vytvořit zprávu na téma, použijte `ServiceBus` atribut s názvem tématu stejným způsobem, můžete ji použít s názvem fronty.</span><span class="sxs-lookup"><span data-stu-id="b2421-147">To create a message on a topic, use the `ServiceBus` attribute with a topic name the same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="b2421-148">Funkce přidané do verze 1.1</span><span class="sxs-lookup"><span data-stu-id="b2421-148">Features added in release 1.1</span></span>
<span data-ttu-id="b2421-149">Verze 1.1 byly přidány následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b2421-149">The following features were added in release 1.1:</span></span>

* <span data-ttu-id="b2421-150">Povolit přímý přizpůsobení zpracování prostřednictvím zpráv `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="b2421-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="b2421-151">`MessagingProvider`podporuje přizpůsobení Service Bus `MessagingFactory` a `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="b2421-151">`MessagingProvider` supports customization of the Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="b2421-152">A `MessageProcessor` strategie vzor umožňuje určit procesor na fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="b2421-152">A `MessageProcessor` strategy pattern allows you to specify a processor per queue/topic.</span></span>
* <span data-ttu-id="b2421-153">Ve výchozím nastavení je podporována souběžnosti zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="b2421-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="b2421-154">Snadné přizpůsobení `OnMessageOptions` prostřednictvím `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="b2421-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="b2421-155">Povolit [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) ho zadat na `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (pro scénáře, kde nemusí mít spravovat práva).</span><span class="sxs-lookup"><span data-stu-id="b2421-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) to be specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="b2421-156">Upozorňujeme, že se nepodařilo automaticky zřizovat neexistující front a témat bez spravovat AccessRights Azure WebJobs.</span><span class="sxs-lookup"><span data-stu-id="b2421-156">Note that Azure WebJobs is unable to automatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="b2421-157"><a id="queues"></a>Související témata předmětem článek s postupy fronty úložiště</span><span class="sxs-lookup"><span data-stu-id="b2421-157"><a id="queues"></a>Related topics covered by the storage queues how-to article</span></span>
<span data-ttu-id="b2421-158">Informace o scénářích WebJobs SDK, které nejsou specifické pro Service Bus najdete v tématu [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="b2421-158">For information about WebJobs SDK scenarios not specific to Service Bus, see [How to use Azure queue storage with the WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="b2421-159">Následující témata popsaná v tomto článku:</span><span class="sxs-lookup"><span data-stu-id="b2421-159">Topics covered in that article include the following:</span></span>

* <span data-ttu-id="b2421-160">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="b2421-160">Async functions</span></span>
* <span data-ttu-id="b2421-161">Více instancí</span><span class="sxs-lookup"><span data-stu-id="b2421-161">Multiple instances</span></span>
* <span data-ttu-id="b2421-162">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="b2421-162">Graceful shutdown</span></span>
* <span data-ttu-id="b2421-163">Použití atributů WebJobs SDK v tělo funkce</span><span class="sxs-lookup"><span data-stu-id="b2421-163">Use WebJobs SDK attributes in the body of a function</span></span>
* <span data-ttu-id="b2421-164">Sada SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="b2421-164">Set the SDK connection strings in code</span></span>
* <span data-ttu-id="b2421-165">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="b2421-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="b2421-166">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="b2421-166">Trigger a function manually</span></span>
* <span data-ttu-id="b2421-167">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="b2421-167">Write logs</span></span>

## <span data-ttu-id="b2421-168"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2421-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="b2421-169">Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="b2421-169">This guide has provided code samples that show how to handle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="b2421-170">Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="b2421-170">For more information about how to use Azure WebJobs and the WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

