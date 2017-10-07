---
title: "aaaGet začít s fronty úložiště a Visual Studio připojených služeb (cloudové služby) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Queue storage v projektu cloudové služby v sadě Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 8ba3d830cb83e3d75102b8a09363f1dc200ff1c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Začínáme s Azure Queue storage a Visual Studio připojené služby (projekty cloudových služeb)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak tooget spuštění pomocí Azure Queue storage v sadě Visual Studio, po vytvoření a účet úložiště Azure v projektu cloudové služby odkazuje pomocí sady Visual Studio hello **přidat připojení služby** dialogové okno .

Ukážeme vám jak toocreate fronty v kódu. Také ukážeme jak tooperform basic fronty operací, jako je přidání, úprava, čtení a odebrání zprávy fronty. Hello ukázky jsou napsané v kódu jazyka C# a používají hello [Microsoft Azure Storage Client Library pro .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Hello **přidat připojení služby** operaci nainstaluje hello odpovídající NuGet balíčky tooaccess úložiště Azure ve vašem projektu a přidá hello připojovací řetězec pro hello úložiště účet tooyour projektu konfigurační soubory.

* V tématu [Začínáme s Azure Queue storage pomocí rozhraní .NET](storage-dotnet-how-to-use-queues.md) Další informace o práci s front v kódu.
* V tématu [úložiště dokumentace](https://azure.microsoft.com/documentation/services/storage/) obecné informace o službě Azure Storage.
* V tématu [cloudové služby dokumentaci](https://azure.microsoft.com/documentation/services/cloud-services/) obecné informace o cloudových služeb Azure.
* V tématu [ASP.NET](http://www.asp.net) Další informace o programování aplikací ASP.NET.

Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště.

## <a name="access-queues-in-code"></a>Přístup front v kódu
tooaccess fronty v projektech Visual Studio cloudové služby, je nutné tooinclude hello následující položky tooany C# zdrojový soubor, který přístup k Azure Queue storage.

1. Deklarace oborů názvů hello hello horní části souboru hello C# zahrnout tyto **pomocí** příkazy.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Získání **CloudStorageAccount** objekt, který reprezentuje informace o účtu úložiště. Následující kód tooget hello použití hello připojovacího řetězce úložiště a informace o účtu úložiště z konfigurace služby Azure hello.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Získání **CloudQueueClient** objektu tooreference hello queue – objekty v účtu úložiště.  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Získání **CloudQueue** objektu tooreference konkrétní fronty.
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Poznámka:** používat všechny hello výše kódu před hello kódu v hello následující ukázky.

## <a name="create-a-queue-in-code"></a>Vytvoření fronty v kódu
toocreate hello fronty v kódu, stačí přidat volání příliš**CreateIfNotExists**.

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a>Přidat tooa fronty zpráv
tooinsert zprávu do existující fronty, vytvořte novou **CloudQueueMessage** objekt a potom volání hello **AddMessage** metoda.

A **CloudQueueMessage** můžete vytvořit objekt z řetězce (ve formátu UTF-8) nebo pole bajtů.

Tady je příklad, který se vloží uvítací zprávu "Hello, World".

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Čtení zprávy ve frontě
Můžete prohlížet zprávy hello v popředí hello fronty bez odebere ji z fronty hello pomocí volání hello **PeekMessage** metoda.

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Přečtěte si a odebrat zprávu ve frontě
Můžete odebrat kódu (zrušte fronty) zprávu z fronty ve dvou krocích.

1. Volání **GetMessage** tooget hello další zprávu ve frontě. Zpráva vrácená metodou **GetMessage** stane neviditelnou tooany další kód, který čte zprávy z této fronty. Ve výchozím nastavení tato zpráva zůstává neviditelná po dobu 30 sekund.
2. odebrání uvítací zprávu z fronty hello, volání toofinish **DeleteMessage**.

Tento dvoukrokový proces odebrání zprávy zaručuje, pokud se váš kód nezdaří tooprocess, které můžete získat zprávu z důvodu selhání toohardware nebo softwaru, jiná instance vašeho kódu hello stejnou zprávu a zkuste to znovu. Hello následující kód volání **DeleteMessage** hned po zpracování zprávy hello.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a>Použít další možnosti tooprocess a odebrání zprávy fronty
Načítání zpráv z fronty si můžete přizpůsobit dvěma způsoby.

* Můžete načíst dávku zpráv (až too32).
* Můžete nastavit časový limit neviditelnosti delší nebo kratší, povolení váš kód více nebo méně času toofully zpracování jednotlivých zpráv. Hello následující příklad kódu používá **GetMessages** metoda tooget 20 zpráv v jednom volání. Následně se každá zpráva zpracuje pomocí smyčky **foreach**. Nastaví taky hello neviditelnosti časový limit toofive minut pro každou zprávu. Všimněte si, že hello 5 minut spustí pro všechny zprávy s hello stejný čas, takže po 5 minut od volání hello příliš předané**GetMessages**, všechny zprávy, které nebyly odstraněny, opět viditelné.

Tady je příklad:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a>Získání délky fronty hello
Můžete získat odhad hello počet zpráv ve frontě. **FetchAttributes** metoda požádá hello službu front o načtení atributů fronty hello, včetně počtu zpráv hello. Hello **ApproximateMethodCount** vlastnost vrací hello poslední hodnotu načtenou **FetchAttributes** metoda bez volání služby front hello.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a>Hello použití vzoru Async-Await s běžné fronty rozhraní API služby Azure
Tento příklad ukazuje, jak vzor Async-Await hello toouse společné rozhraní API fronty Azure. Ukázka Hello volá hello asynchronní verzi každé z hello zadané metody, to může zobrazit hello **asynchronní** po opravy jednotlivých metod. Při použití asynchronní metody je použité hello async-await vzor pozastaví místní spuštění, dokud se nedokončí volání hello. Toto chování umožňuje hello aktuální vlákno toodo jinou práci, která pomáhá zabránit kritická místa výkonu a zlepšuje celkovou rychlost reakce aplikace hello. Další podrobnosti o použití hello vzoru Async-Await v rozhraní .NET najdete v části [Async a Await (C# a Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message tooput in hello queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add hello message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete hello message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Odstranění fronty
fronty a všechny zprávy hello obsažené v něm volání hello toodelete **odstranit** metoda na objekt fronty hello.

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Další kroky
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

