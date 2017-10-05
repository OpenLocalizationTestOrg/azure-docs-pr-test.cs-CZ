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
# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Používání Azure Service Bus se sadou WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# ukázek kódu, které ukazují, jak spustit proces, při příjmu zprávy Azure Service Bus. Kód – ukázky použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md).

Fragmenty kódu ukazují jenom funkce, není kód, který vytvoří `JobHost` objektu jako v následujícím příkladu:

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

A [kompletní příklad kódu Service Bus](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) je v úložišti azure webjobs sdk ukázky na webu GitHub.com.

## <a id="prerequisites"></a>Požadavky
Pro práci s Service Bus, je třeba nainstalovat [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) balíček NuGet kromě dalších balíčků WebJobs SDK. 

Také budete muset nastavit připojovací řetězec AzureWebJobsServiceBus kromě úložiště připojovací řetězce.  To provedete `connectionStrings` v souboru App.config, jak je znázorněno v následujícím příkladu:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Ukázkový projekt, který obsahuje řetězec nastavení připojení služby Service Bus v souboru App.config, najdete v části [Service Bus příklad](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Připojovací řetězce lze nastavit i v prostředí Azure runtime, které pak přepíše nastavení App.config při spuštění vytvářené webové úlohy v Azure; Další informace najdete v tématu [začít pracovat s WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Postup aktivace funkce při příjmu zprávy fronty Service Bus
Chcete-li vytvořit funkci, která volá WebJobs SDK při příjmu zprávy fronty, použijte `ServiceBusTrigger` atribut. Konstruktoru atributu přebírá parametr, který určuje název fronty pro cyklické dotazování.

### <a name="how-servicebustrigger-works"></a>Jak funguje ServiceBusTrigger
Sada SDK přijme nějakou zprávu v `PeekLock` režimu a volání `Complete` na zprávu, pokud funkci skončí úspěšně, nebo volání `Abandon` Pokud funkce selže. Pokud je funkce spuštěná déle, než `PeekLock` automaticky obnovují vždy vypršel časový limit, zámek.

Service Bus nepodporuje svůj vlastní zpracování poškozených fronty, která nemůže být řídí nebo konfigurovat tak, že sada WebJobs SDK. 

### <a name="string-queue-message"></a>Řetězec fronty zpráv
Následující ukázka kódu přečte zprávu fronty, která obsahuje řetězec a zapisuje řetězec na řídicím panelu WebJobs SDK.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Poznámka:** při vytváření fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, nezapomeňte nastavit [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) k "text/plain".

### <a name="poco-queue-message"></a>Zpráva fronty objektů POCO
Sada SDK bude automaticky deserializovat zprávu fronty, která obsahuje JSON pro objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) typu. Následující ukázka kódu přečte zprávu fronty, který obsahuje `BlobInformation` objekt, který má `BlobName` vlastnost:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Ukázky kódu znázorňující způsob použití vlastnosti objektů POCO pro práci s objekty BLOB a tabulek ve stejné funkci, najdete v článku [úložiště fronty verzi tohoto článku](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Pokud váš kód, který vytvoří zprávu fronty nepoužívá WebJobs SDK, použijte kód podobně jako v následujícím příkladu:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Typy ServiceBusTrigger pracuje s
Kromě `string` a typy objektů POCO, můžete použít `ServiceBusTrigger` atribut s bajtové pole nebo `BrokeredMessage` objektu.

## <a id="create"></a>Postup vytvoření zprávy fronty Service Bus
Zápis funkce, která vytvoří nové použití fronty zpráv `ServiceBus` atribut a předat název fronty konstruktoru atributu. 

### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Vytvořit zprávu jedné frontě v jiných asynchronní funkce
Následující příklad kódu používá výstupní parametr k vytvoření nové zprávy ve frontě s názvem "outputqueue" se stejným obsahem jako ve frontě s názvem "inputqueue" přijaté zprávy.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Výstupní parametr pro vytvoření zpráva s jednou frontou může být libovolná z následujících typů:

* `string`
* `byte[]`
* `BrokeredMessage`
* Serializovatelné typ objektů POCO, které definujete. Automaticky serializovanou jako JSON.

Pro parametry typu objektů POCO fronty zpráv je vytvořen vždy při ukončení funkce; Pokud má parametr hodnotu null, sadu SDK vytvoří zprávu fronty, která vrátí hodnotu null, když zprávu přijme a deserializovat. Pro jiné typy Pokud má parametr hodnotu null žádná zpráva fronty se vytvoří.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Vytvoření více fronty zpráv nebo asynchronní funkce
Chcete-li vytvořit více zpráv, použijte `ServiceBus` atribut s `ICollector<T>` nebo `IAsyncCollector<T>`, jak znázorňuje následující ukázka kódu:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každou zprávu fronty je vytvořena ihned po `Add` metoda je volána.

## <a id="topics"></a>Jak pracovat s témat sběrnice Service Bus
Chcete-li vytvořit funkci, která volá sady SDK při příjmu zprávy v tématu Service Bus, použijte `ServiceBusTrigger` atribut s konstruktor, který přebírá název tématu a název odběru, jak znázorňuje následující ukázka kódu:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Chcete-li vytvořit zprávu na téma, použijte `ServiceBus` atribut s názvem tématu stejným způsobem, můžete ji použít s názvem fronty.

## <a name="features-added-in-release-11"></a>Funkce přidané do verze 1.1
Verze 1.1 byly přidány následující funkce:

* Povolit přímý přizpůsobení zpracování prostřednictvím zpráv `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`podporuje přizpůsobení Service Bus `MessagingFactory` a `NamespaceManager`.
* A `MessageProcessor` strategie vzor umožňuje určit procesor na fronta nebo téma.
* Ve výchozím nastavení je podporována souběžnosti zpracování zprávy. 
* Snadné přizpůsobení `OnMessageOptions` prostřednictvím `ServiceBusConfiguration.MessageOptions`.
* Povolit [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) ho zadat na `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (pro scénáře, kde nemusí mít spravovat práva). Upozorňujeme, že se nepodařilo automaticky zřizovat neexistující front a témat bez spravovat AccessRights Azure WebJobs.

## <a id="queues"></a>Související témata předmětem článek s postupy fronty úložiště
Informace o scénářích WebJobs SDK, které nejsou specifické pro Service Bus najdete v tématu [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Následující témata popsaná v tomto článku:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v tělo funkce
* Sada SDK připojovacích řetězců v kódu
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s Azure Service Bus. Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

