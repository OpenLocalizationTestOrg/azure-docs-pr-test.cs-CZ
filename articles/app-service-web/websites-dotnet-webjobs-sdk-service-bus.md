---
title: aaaHow toouse Azure Service Bus s hello WebJobs SDK
description: "Zjistěte, jak toouse fronty Azure Service Bus a témat hello WebJobs SDK."
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
ms.openlocfilehash: cb801a9320a20c276da4f48c8941c09d3f09bb1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a><span data-ttu-id="c61f8-103">Jak toouse služba Azure Service Bus s hello WebJobs SDK</span><span class="sxs-lookup"><span data-stu-id="c61f8-103">How toouse Azure Service Bus with hello WebJobs SDK</span></span>
## <a name="overview"></a><span data-ttu-id="c61f8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="c61f8-104">Overview</span></span>
<span data-ttu-id="c61f8-105">Tato příručka obsahuje C# kódu ukázky, zobrazující jak tootrigger procesu, když je obdržena zprávu Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c61f8-105">This guide provides C# code samples that show how tootrigger a process when an Azure Service Bus message is received.</span></span> <span data-ttu-id="c61f8-106">Ukázky kódu Hello použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.</span><span class="sxs-lookup"><span data-stu-id="c61f8-106">hello code samples use [WebJobs SDK](websites-dotnet-webjobs-sdk.md) version 1.x.</span></span>

<span data-ttu-id="c61f8-107">Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c61f8-107">hello guide assumes you know [how toocreate a WebJob project in Visual Studio with connection strings that point tooyour storage account](websites-dotnet-webjobs-sdk-get-started.md).</span></span>

<span data-ttu-id="c61f8-108">fragmenty kódu Hello zobrazit pouze funkce, není hello kód, který vytvoří hello `JobHost` objektu jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c61f8-108">hello code snippets only show functions, not hello code that creates hello `JobHost` object as in this example:</span></span>

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

<span data-ttu-id="c61f8-109">A [kompletní příklad kódu Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je v úložišti azure webjobs sdk ukázky hello na webu GitHub.com.</span><span class="sxs-lookup"><span data-stu-id="c61f8-109">A [complete Service Bus code example](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) is in hello azure-webjobs-sdk-samples repository on GitHub.com.</span></span>

## <span data-ttu-id="c61f8-110"><a id="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="c61f8-110"><a id="prerequisites"></a> Prerequisites</span></span>
<span data-ttu-id="c61f8-111">toowork službou Service Bus máte tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet balíček kromě toohello dalších balíčků WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c61f8-111">toowork with Service Bus you have tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet package in addition toohello other WebJobs SDK packages.</span></span> 

<span data-ttu-id="c61f8-112">Máte také tooset hello AzureWebJobsServiceBus připojovací řetězec v přidání toohello úložiště připojovací řetězce.</span><span class="sxs-lookup"><span data-stu-id="c61f8-112">You also have tooset hello AzureWebJobsServiceBus connection string in addition toohello storage connection strings.</span></span>  <span data-ttu-id="c61f8-113">To provedete v hello `connectionStrings` části souboru App.config hello, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c61f8-113">You can do this in hello `connectionStrings` section of hello App.config file, as shown in hello following example:</span></span>

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

<span data-ttu-id="c61f8-114">Ukázkový projekt, který obsahuje nastavení hello Service Bus připojovacího řetězce v souboru App.config hello, najdete v části [Service Bus příklad](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span><span class="sxs-lookup"><span data-stu-id="c61f8-114">For a sample project that includes hello Service Bus connection string setting in hello App.config file, see [Service Bus example](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus).</span></span> 

<span data-ttu-id="c61f8-115">připojovací řetězce Hello lze nastavit i v prostředí Azure runtime hello, které pak přepíše nastavení App.config hello při spuštění hello webové úlohy v Azure; Další informace najdete v tématu [začít pracovat s hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c61f8-115">hello connection strings can also be set in hello Azure runtime environment, which then overrides hello App.config settings when hello WebJob runs in Azure; for more information, see [Get Started with hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).</span></span>

## <span data-ttu-id="c61f8-116"><a id="trigger"></a>Jak je obdržena tootrigger funkce když Service Bus fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="c61f8-116"><a id="trigger"></a> How tootrigger a function when a Service Bus queue message is received</span></span>
<span data-ttu-id="c61f8-117">volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello `ServiceBusTrigger` atribut.</span><span class="sxs-lookup"><span data-stu-id="c61f8-117">toowrite a function that hello WebJobs SDK calls when a queue message is received, use hello `ServiceBusTrigger` attribute.</span></span> <span data-ttu-id="c61f8-118">konstruktoru atributu Hello přebírá parametr, který určuje název hello toopoll fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c61f8-118">hello attribute constructor takes a parameter that specifies hello name of hello queue toopoll.</span></span>

### <a name="how-servicebustrigger-works"></a><span data-ttu-id="c61f8-119">Jak funguje ServiceBusTrigger</span><span class="sxs-lookup"><span data-stu-id="c61f8-119">How ServiceBusTrigger works</span></span>
<span data-ttu-id="c61f8-120">Hello SDK přijme nějakou zprávu v `PeekLock` režimu a volání `Complete` na uvítací zprávu, pokud funkce hello skončí úspěšně, nebo volání `Abandon` Pokud hello funkce selže.</span><span class="sxs-lookup"><span data-stu-id="c61f8-120">hello SDK receives a message in `PeekLock` mode and calls `Complete` on hello message if hello function finishes successfully, or calls `Abandon` if hello function fails.</span></span> <span data-ttu-id="c61f8-121">Pokud poběží déle než hello hello funkce `PeekLock` automaticky obnovují vždy vypršel časový limit, hello zámku.</span><span class="sxs-lookup"><span data-stu-id="c61f8-121">If hello function runs longer than hello `PeekLock` timeout, hello lock is automatically renewed.</span></span>

<span data-ttu-id="c61f8-122">Service Bus nepodporuje svůj vlastní zpracování poškozených fronty, která nemůže být řídí nebo konfigurovat tak, že hello WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c61f8-122">Service Bus does its own poison queue handling which cannot be controlled or configured by hello WebJobs SDK.</span></span> 

### <a name="string-queue-message"></a><span data-ttu-id="c61f8-123">Řetězec fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="c61f8-123">String queue message</span></span>
<span data-ttu-id="c61f8-124">Hello následující ukázka kódu přečte zprávu fronty, která obsahuje řetězec a zapíše hello řetězec toohello řídicím panelu WebJobs SDK.</span><span class="sxs-lookup"><span data-stu-id="c61f8-124">hello following code sample reads a queue message that contains a string and writes hello string toohello WebJobs SDK dashboard.</span></span>

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

<span data-ttu-id="c61f8-125">**Poznámka:** při vytváření hello fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, ujistěte se, že tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) příliš "text/plain".</span><span class="sxs-lookup"><span data-stu-id="c61f8-125">**Note:** If you are creating hello queue messages in an application that doesn't use hello WebJobs SDK, make sure tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) too"text/plain".</span></span>

### <a name="poco-queue-message"></a><span data-ttu-id="c61f8-126">Zpráva fronty objektů POCO</span><span class="sxs-lookup"><span data-stu-id="c61f8-126">POCO queue message</span></span>
<span data-ttu-id="c61f8-127">Hello SDK bude automaticky deserializovat zprávu fronty, která obsahuje JSON pro objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu.</span><span class="sxs-lookup"><span data-stu-id="c61f8-127">hello SDK will automatically deserialize a queue message that contains JSON for a POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) type.</span></span> <span data-ttu-id="c61f8-128">Hello následující ukázka kódu přečte zprávu fronty, který obsahuje `BlobInformation` objekt, který má `BlobName` vlastnost:</span><span class="sxs-lookup"><span data-stu-id="c61f8-128">hello following code sample reads a queue message that contains a `BlobInformation` object which has a `BlobName` property:</span></span>

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

<span data-ttu-id="c61f8-129">Ukázky kódu znázorňující, jak toouse vlastnosti objektů POCO toowork hello s objekty BLOB a tabulek v hello stejné funkce naleznete v tématu hello [úložiště fronty verzi tohoto článku](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span><span class="sxs-lookup"><span data-stu-id="c61f8-129">For code samples showing how toouse properties of hello POCO toowork with blobs and tables in hello same function, see hello [storage queues version of this article](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).</span></span>

<span data-ttu-id="c61f8-130">Pokud váš kód, který vytvoří zprávu fronty hello nepoužívá hello WebJobs SDK, použijte následující příklad podobné toohello kódu:</span><span class="sxs-lookup"><span data-stu-id="c61f8-130">If your code that creates hello queue message doesn't use hello WebJobs SDK, use code similar toohello following example:</span></span>

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a><span data-ttu-id="c61f8-131">Typy ServiceBusTrigger pracuje s</span><span class="sxs-lookup"><span data-stu-id="c61f8-131">Types ServiceBusTrigger works with</span></span>
<span data-ttu-id="c61f8-132">Kromě `string` a typy objektů POCO, můžete použít hello `ServiceBusTrigger` atribut s bajtové pole nebo `BrokeredMessage` objektu.</span><span class="sxs-lookup"><span data-stu-id="c61f8-132">Besides `string` and POCO  types, you can use hello `ServiceBusTrigger` attribute with a byte array or a `BrokeredMessage` object.</span></span>

## <span data-ttu-id="c61f8-133"><a id="create"></a>Jak toocreate Service Bus fronty zpráv</span><span class="sxs-lookup"><span data-stu-id="c61f8-133"><a id="create"></a> How toocreate Service Bus queue messages</span></span>
<span data-ttu-id="c61f8-134">toowrite funkci, která vytvoří novou zprávu fronty použít hello `ServiceBus` atribut a předejte v konstruktoru atributu toohello název fronty hello.</span><span class="sxs-lookup"><span data-stu-id="c61f8-134">toowrite a function that creates a new queue message use hello `ServiceBus` attribute and pass in hello queue name toohello attribute constructor.</span></span> 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a><span data-ttu-id="c61f8-135">Vytvořit zprávu jedné frontě v jiných asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="c61f8-135">Create a single queue message in a non-async function</span></span>
<span data-ttu-id="c61f8-136">Následující ukázka kódu Hello používá výstupní parametr toocreate novou zprávu ve frontě hello s názvem "outputqueue" s hello stejný obsah jako hello zprávu dostali hello frontu s názvem "inputqueue".</span><span class="sxs-lookup"><span data-stu-id="c61f8-136">hello following code sample uses an output parameter toocreate a new message in hello queue named "outputqueue" with hello same content as hello message received in hello queue named "inputqueue".</span></span>

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

<span data-ttu-id="c61f8-137">Hello výstupní parametr pro vytvoření zpráva s jednou frontou může být libovolná z hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="c61f8-137">hello output parameter for creating a single queue message can be any of hello following types:</span></span>

* `string`
* `byte[]`
* `BrokeredMessage`
* <span data-ttu-id="c61f8-138">Serializovatelné typ objektů POCO, které definujete.</span><span class="sxs-lookup"><span data-stu-id="c61f8-138">A serializable POCO type that you define.</span></span> <span data-ttu-id="c61f8-139">Automaticky serializovanou jako JSON.</span><span class="sxs-lookup"><span data-stu-id="c61f8-139">Automatically serialized as JSON.</span></span>

<span data-ttu-id="c61f8-140">Pro parametry typu objektů POCO fronty zpráv je vytvořen vždy při ukončení funkce hello; Pokud parametr hello má hodnotu null, hello SDK vytvoří zprávu fronty, která vrátí hodnotu null, když hello zprávu přijme a deserializovat.</span><span class="sxs-lookup"><span data-stu-id="c61f8-140">For POCO type parameters, a queue message is always created when hello function ends; if hello parameter is null, hello SDK creates a queue message that will return null when hello message is received and deserialized.</span></span> <span data-ttu-id="c61f8-141">Pro hello jiné typy, pokud parametr hello má hodnotu null vytvoří se žádná zpráva fronty.</span><span class="sxs-lookup"><span data-stu-id="c61f8-141">For hello other types, if hello parameter is null no queue message is created.</span></span>

### <a name="create-multiple-queue-messages-or-in-async-functions"></a><span data-ttu-id="c61f8-142">Vytvoření více fronty zpráv nebo asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="c61f8-142">Create multiple queue messages or in async functions</span></span>
<span data-ttu-id="c61f8-143">toocreate více zpráv, použijte hello `ServiceBus` atribut s `ICollector<T>` nebo `IAsyncCollector<T>`, jak je znázorněno v hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="c61f8-143">toocreate multiple messages, use  hello `ServiceBus` attribute with `ICollector<T>` or `IAsyncCollector<T>`, as shown in hello following code sample:</span></span>

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

<span data-ttu-id="c61f8-144">Každou zprávu fronty se vytvoří okamžitě při hello `Add` metoda je volána.</span><span class="sxs-lookup"><span data-stu-id="c61f8-144">Each queue message is created immediately when hello `Add` method is called.</span></span>

## <span data-ttu-id="c61f8-145"><a id="topics"></a>Jak toowork s témat sběrnice Service Bus</span><span class="sxs-lookup"><span data-stu-id="c61f8-145"><a id="topics"></a>How toowork with Service Bus topics</span></span>
<span data-ttu-id="c61f8-146">volá funkci, která hello SDK toowrite při příjmu zprávy v tématu Service Bus, použijte hello `ServiceBusTrigger` atribut s hello konstruktor, který přebírá název tématu a název odběru, jak je znázorněno v hello následující ukázka kódu:</span><span class="sxs-lookup"><span data-stu-id="c61f8-146">toowrite a function that hello SDK calls when a message is received on a Service Bus topic, use hello `ServiceBusTrigger` attribute with hello constructor that takes topic name and subscription name, as shown in hello following code sample:</span></span>

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

<span data-ttu-id="c61f8-147">toocreate zprávu na téma, použijte hello `ServiceBus` atribut s hello název tématu stejné způsobu, jakým používáte s názvem fronty.</span><span class="sxs-lookup"><span data-stu-id="c61f8-147">toocreate a message on a topic, use hello `ServiceBus` attribute with a topic name hello same way you use it with a queue name.</span></span>

## <a name="features-added-in-release-11"></a><span data-ttu-id="c61f8-148">Funkce přidané do verze 1.1</span><span class="sxs-lookup"><span data-stu-id="c61f8-148">Features added in release 1.1</span></span>
<span data-ttu-id="c61f8-149">verze 1.1 byly přidány Hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="c61f8-149">hello following features were added in release 1.1:</span></span>

* <span data-ttu-id="c61f8-150">Povolit přímý přizpůsobení zpracování prostřednictvím zpráv `ServiceBusConfiguration.MessagingProvider`.</span><span class="sxs-lookup"><span data-stu-id="c61f8-150">Allow deep customization of message processing via `ServiceBusConfiguration.MessagingProvider`.</span></span>
* <span data-ttu-id="c61f8-151">`MessagingProvider`podporuje přizpůsobení hello Service Bus `MessagingFactory` a `NamespaceManager`.</span><span class="sxs-lookup"><span data-stu-id="c61f8-151">`MessagingProvider` supports customization of hello Service Bus `MessagingFactory` and `NamespaceManager`.</span></span>
* <span data-ttu-id="c61f8-152">A `MessageProcessor` strategie vzor vám umožní toospecify procesor na fronta nebo téma.</span><span class="sxs-lookup"><span data-stu-id="c61f8-152">A `MessageProcessor` strategy pattern allows you toospecify a processor per queue/topic.</span></span>
* <span data-ttu-id="c61f8-153">Ve výchozím nastavení je podporována souběžnosti zpracování zprávy.</span><span class="sxs-lookup"><span data-stu-id="c61f8-153">Message processing concurrency is supported by default.</span></span> 
* <span data-ttu-id="c61f8-154">Snadné přizpůsobení `OnMessageOptions` prostřednictvím `ServiceBusConfiguration.MessageOptions`.</span><span class="sxs-lookup"><span data-stu-id="c61f8-154">Easy customization of `OnMessageOptions` via `ServiceBusConfiguration.MessageOptions`.</span></span>
* <span data-ttu-id="c61f8-155">Povolit [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe zadaný na `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (pro scénáře, kde nemusí mít spravovat práva).</span><span class="sxs-lookup"><span data-stu-id="c61f8-155">Allow [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe specified on `ServiceBusTriggerAttribute`/`ServiceBusAttribute` (for scenarios where you might not have Manage rights).</span></span> <span data-ttu-id="c61f8-156">Upozorňujeme, že se Azure WebJobs tooautomatically nelze zřídit neexistující front a témat bez AccessRights spravovat.</span><span class="sxs-lookup"><span data-stu-id="c61f8-156">Note that Azure WebJobs is unable tooautomatically provision non-existent queues and topics without Manage AccessRights.</span></span>

## <span data-ttu-id="c61f8-157"><a id="queues"></a>Související témata předmětem fronty úložiště hello postupy tooarticle</span><span class="sxs-lookup"><span data-stu-id="c61f8-157"><a id="queues"></a>Related topics covered by hello storage queues how-tooarticle</span></span>
<span data-ttu-id="c61f8-158">Informace o scénářích WebJobs SDK nejsou specifické tooService Bus, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="c61f8-158">For information about WebJobs SDK scenarios not specific tooService Bus, see [How toouse Azure queue storage with hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md).</span></span> 

<span data-ttu-id="c61f8-159">Obsahuje následující témata v tomto článku hello následující:</span><span class="sxs-lookup"><span data-stu-id="c61f8-159">Topics covered in that article include hello following:</span></span>

* <span data-ttu-id="c61f8-160">Asynchronní funkce</span><span class="sxs-lookup"><span data-stu-id="c61f8-160">Async functions</span></span>
* <span data-ttu-id="c61f8-161">Více instancí</span><span class="sxs-lookup"><span data-stu-id="c61f8-161">Multiple instances</span></span>
* <span data-ttu-id="c61f8-162">Řádné vypnutí</span><span class="sxs-lookup"><span data-stu-id="c61f8-162">Graceful shutdown</span></span>
* <span data-ttu-id="c61f8-163">Použití atributů WebJobs SDK v hello tělo funkce</span><span class="sxs-lookup"><span data-stu-id="c61f8-163">Use WebJobs SDK attributes in hello body of a function</span></span>
* <span data-ttu-id="c61f8-164">Nastavení hello SDK připojovacích řetězců v kódu</span><span class="sxs-lookup"><span data-stu-id="c61f8-164">Set hello SDK connection strings in code</span></span>
* <span data-ttu-id="c61f8-165">Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu</span><span class="sxs-lookup"><span data-stu-id="c61f8-165">Set values for WebJobs SDK constructor parameters in code</span></span>
* <span data-ttu-id="c61f8-166">Ruční aktivaci funkce</span><span class="sxs-lookup"><span data-stu-id="c61f8-166">Trigger a function manually</span></span>
* <span data-ttu-id="c61f8-167">Zápis protokolů</span><span class="sxs-lookup"><span data-stu-id="c61f8-167">Write logs</span></span>

## <span data-ttu-id="c61f8-168"><a id="nextsteps"></a> Další kroky</span><span class="sxs-lookup"><span data-stu-id="c61f8-168"><a id="nextsteps"></a> Next steps</span></span>
<span data-ttu-id="c61f8-169">Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="c61f8-169">This guide has provided code samples that show how toohandle common scenarios for working with Azure Service Bus.</span></span> <span data-ttu-id="c61f8-170">Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).</span><span class="sxs-lookup"><span data-stu-id="c61f8-170">For more information about how toouse Azure WebJobs and hello WebJobs SDK, see [Azure WebJobs Recommended Resources](http://go.microsoft.com/fwlink/?linkid=390226).</span></span>

