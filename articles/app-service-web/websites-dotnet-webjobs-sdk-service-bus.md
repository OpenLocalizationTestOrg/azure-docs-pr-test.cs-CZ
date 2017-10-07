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
# <a name="how-toouse-azure-service-bus-with-hello-webjobs-sdk"></a>Jak toouse služba Azure Service Bus s hello WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# kódu ukázky, zobrazující jak tootrigger procesu, když je obdržena zprávu Azure Service Bus. Ukázky kódu Hello použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md).

fragmenty kódu Hello zobrazit pouze funkce, není hello kód, který vytvoří hello `JobHost` objektu jako v následujícím příkladu:

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

A [kompletní příklad kódu Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je v úložišti azure webjobs sdk ukázky hello na webu GitHub.com.

## <a id="prerequisites"></a>Požadavky
toowork službou Service Bus máte tooinstall hello [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) NuGet balíček kromě toohello dalších balíčků WebJobs SDK. 

Máte také tooset hello AzureWebJobsServiceBus připojovací řetězec v přidání toohello úložiště připojovací řetězce.  To provedete v hello `connectionStrings` části souboru App.config hello, jak je znázorněno v hello následující ukázka:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ukázkový projekt, který obsahuje nastavení hello Service Bus připojovacího řetězce v souboru App.config hello, najdete v části [Service Bus příklad](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

připojovací řetězce Hello lze nastavit i v prostředí Azure runtime hello, které pak přepíše nastavení App.config hello při spuštění hello webové úlohy v Azure; Další informace najdete v tématu [začít pracovat s hello WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Jak je obdržena tootrigger funkce když Service Bus fronty zpráv
volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello `ServiceBusTrigger` atribut. konstruktoru atributu Hello přebírá parametr, který určuje název hello toopoll fronty hello.

### <a name="how-servicebustrigger-works"></a>Jak funguje ServiceBusTrigger
Hello SDK přijme nějakou zprávu v `PeekLock` režimu a volání `Complete` na uvítací zprávu, pokud funkce hello skončí úspěšně, nebo volání `Abandon` Pokud hello funkce selže. Pokud poběží déle než hello hello funkce `PeekLock` automaticky obnovují vždy vypršel časový limit, hello zámku.

Service Bus nepodporuje svůj vlastní zpracování poškozených fronty, která nemůže být řídí nebo konfigurovat tak, že hello WebJobs SDK. 

### <a name="string-queue-message"></a>Řetězec fronty zpráv
Hello následující ukázka kódu přečte zprávu fronty, která obsahuje řetězec a zapíše hello řetězec toohello řídicím panelu WebJobs SDK.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Poznámka:** při vytváření hello fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, ujistěte se, že tooset [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) příliš "text/plain".

### <a name="poco-queue-message"></a>Zpráva fronty objektů POCO
Hello SDK bude automaticky deserializovat zprávu fronty, která obsahuje JSON pro objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu. Hello následující ukázka kódu přečte zprávu fronty, který obsahuje `BlobInformation` objekt, který má `BlobName` vlastnost:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

Ukázky kódu znázorňující, jak toouse vlastnosti objektů POCO toowork hello s objekty BLOB a tabulek v hello stejné funkce naleznete v tématu hello [úložiště fronty verzi tohoto článku](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Pokud váš kód, který vytvoří zprávu fronty hello nepoužívá hello WebJobs SDK, použijte následující příklad podobné toohello kódu:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typy ServiceBusTrigger pracuje s
Kromě `string` a typy objektů POCO, můžete použít hello `ServiceBusTrigger` atribut s bajtové pole nebo `BrokeredMessage` objektu.

## <a id="create"></a>Jak toocreate Service Bus fronty zpráv
toowrite funkci, která vytvoří novou zprávu fronty použít hello `ServiceBus` atribut a předejte v konstruktoru atributu toohello název fronty hello. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Vytvořit zprávu jedné frontě v jiných asynchronní funkce
Následující ukázka kódu Hello používá výstupní parametr toocreate novou zprávu ve frontě hello s názvem "outputqueue" s hello stejný obsah jako hello zprávu dostali hello frontu s názvem "inputqueue".

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Hello výstupní parametr pro vytvoření zpráva s jednou frontou může být libovolná z hello následující typy:

* `string`
* `byte[]`
* `BrokeredMessage`
* Serializovatelné typ objektů POCO, které definujete. Automaticky serializovanou jako JSON.

Pro parametry typu objektů POCO fronty zpráv je vytvořen vždy při ukončení funkce hello; Pokud parametr hello má hodnotu null, hello SDK vytvoří zprávu fronty, která vrátí hodnotu null, když hello zprávu přijme a deserializovat. Pro hello jiné typy, pokud parametr hello má hodnotu null vytvoří se žádná zpráva fronty.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Vytvoření více fronty zpráv nebo asynchronní funkce
toocreate více zpráv, použijte hello `ServiceBus` atribut s `ICollector<T>` nebo `IAsyncCollector<T>`, jak je znázorněno v hello následující ukázka kódu:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každou zprávu fronty se vytvoří okamžitě při hello `Add` metoda je volána.

## <a id="topics"></a>Jak toowork s témat sběrnice Service Bus
volá funkci, která hello SDK toowrite při příjmu zprávy v tématu Service Bus, použijte hello `ServiceBusTrigger` atribut s hello konstruktor, který přebírá název tématu a název odběru, jak je znázorněno v hello následující ukázka kódu:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

toocreate zprávu na téma, použijte hello `ServiceBus` atribut s hello název tématu stejné způsobu, jakým používáte s názvem fronty.

## <a name="features-added-in-release-11"></a>Funkce přidané do verze 1.1
verze 1.1 byly přidány Hello následující funkce:

* Povolit přímý přizpůsobení zpracování prostřednictvím zpráv `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`podporuje přizpůsobení hello Service Bus `MessagingFactory` a `NamespaceManager`.
* A `MessageProcessor` strategie vzor vám umožní toospecify procesor na fronta nebo téma.
* Ve výchozím nastavení je podporována souběžnosti zpracování zprávy. 
* Snadné přizpůsobení `OnMessageOptions` prostřednictvím `ServiceBusConfiguration.MessageOptions`.
* Povolit [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) toobe zadaný na `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (pro scénáře, kde nemusí mít spravovat práva). Upozorňujeme, že se Azure WebJobs tooautomatically nelze zřídit neexistující front a témat bez AccessRights spravovat.

## <a id="queues"></a>Související témata předmětem fronty úložiště hello postupy tooarticle
Informace o scénářích WebJobs SDK nejsou specifické tooService Bus, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Obsahuje následující témata v tomto článku hello následující:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v hello tělo funkce
* Nastavení hello SDK připojovacích řetězců v kódu
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure Service Bus. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

