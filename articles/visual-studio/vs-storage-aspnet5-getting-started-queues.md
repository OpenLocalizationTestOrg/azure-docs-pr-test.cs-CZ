---
title: "aaaGet začít s fronty úložiště a Visual Studio připojených služeb (ASP.NET Core) | Microsoft Docs"
description: "Způsob tooget spuštění pomocí fronty Azure storage v projektu ASP.NET Core v sadě Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 90c1ebcb6a2eac6bc4f6b69133bdf3135d359a72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Začínáme s fronty úložiště a Visual Studio připojené služby (ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooget spuštění pomocí Azure Queue storage v sadě Visual Studio, po vytvoření a odkazuje pomocí sady Visual Studio hello účet úložiště Azure v projektu ASP.NET Core **přidat připojení služby** dialogové okno. Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.

Azure queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až too64 kilobajtů (KB) velikost a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.

tooget spustit, musíte nejprve toocreate fronty Azure ve vašem účtu úložiště. Ukážeme vám jak toocreate fronty v kódu. Také ukážeme jak tooperform basic fronty operací, jako je přidání, úprava, čtení a odebrání zprávy do fronty. Hello ukázky jsou napsané v jazyce C\# kód a použít hello Klientská knihovna pro úložiště Azure pro .NET. Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).

**Poznámka:** některé hello rozhraní API, která provádět volání tooAzure úložiště v ASP.NET Core jsou asynchronní. V tématu [asynchronní programování s Async a Await](http://msdn.microsoft.com/library/hh191443.aspx) Další informace. Následující kód Hello předpokládá, že asynchronní programování metody jsou používány.

* V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) Další informace o programu manipulace s fronty.
* V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.
* V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.
* V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.

## <a name="access-queues-in-code"></a>Přístup front v kódu
fronty tooaccess projektů ASP.NET Core, potřebujete následující hello tooinclude položky tooany C# zdrojový soubor, který přistupuje k Azure queue storage.

1. Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Získání **CloudQueueClient** objektu tooreference hello queue – objekty v účtu úložiště.  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Získání **CloudQueue** objektu tooreference konkrétní fronty.
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Poznámka:** používat všechny hello výše kódu před hello kódu v hello následující ukázky.

### <a name="create-a-queue-in-code"></a>Vytvoření fronty v kódu
toocreate hello fronty Azure v kódu, stačí přidat volání příliš**CreateIfNotExistsAsync**.

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a>Přidat tooa fronty zpráv
tooinsert zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání hello **AddMessageAsync** metoda.

A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.

Tady je příklad, který se vloží uvítací zprávu "Hello, World".

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Čtení zprávy ve frontě
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessageAsync** metoda.

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Přečtěte si a odebrat zprávu ve frontě
Váš kód můžete odebrat (dequeue –) zprávu z fronty ve dvou krocích.

1. Volání **GetMessageAsync** tooget hello další zprávu ve frontě. Zpráva vrácená metodou **GetMessageAsync** stane neviditelnou tooany další kód, který čte zprávy z této fronty. Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.
2. odebrání uvítací zprávu z fronty hello, volání toofinish **DeleteMessageAsync**.

Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu. Hello následující kód volání **DeleteMessageAsync** hned po zpracování zprávy hello.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Využívání dalších možností pro vyřazení zprávy
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.
Nejprve můžete načíst dávku zpráv (až too32). Druhý můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání. Následně se každá zpráva zpracuje pomocí smyčky **foreach**. Nastaví taky hello neviditelnosti časový limit too5 minut pro každou zprávu. Všimněte si, že počáteční hello 5 minut pro všechny zprávy v hello stejný čas, takže po 5 minut po volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny opět viditelné.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a>Získání délky fronty hello
Můžete získat odhad hello počet zpráv ve frontě. **FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello. Hello **ApproximateMethodCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a>Použití vzoru Async-Await hello s obecné frontě rozhraní API
Tento příklad ukazuje, jak toouse hello vzoru Async-Await s obecné frontě rozhraní API. Hello Ukázka volání hello asynchronní verzi každé z hello zadané metody. To může zobrazit hello asynchronní po opravy jednotlivých metod. Při použití asynchronní metody pozastaví hello vzoru Async-Await místní provádění až do dokončení volání hello. Toto chování umožňuje hello aktuální vlákno toodo jinou práci, která pomáhá zabránit kritická místa výkonu a zlepšuje celkovou rychlost reakce aplikace hello. Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET, najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message tooadd toohello queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue hello message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Odstranění fronty
toodelete frontu a všechny zprávy hello obsažené v něm, volejte **odstranit** metoda na objekt fronty hello.

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

