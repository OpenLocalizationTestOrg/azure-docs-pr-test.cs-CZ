---
title: "aaaGetting začít s fronty úložiště a Visual Studio připojených služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooa účet úložiště pomocí sady Visual Studio připojené služby pomocí Azure Queue storage v projektu webové úlohy."
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 47a446aa5c6bbf25526339823db4952ac1a8802f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Začínáme s Azure Queue storage a Visual Studio připojené služeb (webové úlohy projekty)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Přehled
Tento článek popisuje, jak získat spuštění pomocí Azure Queue storage v projektu Visual Studio Azure Webjobs po vytvoření a odkazuje pomocí účtu úložiště Azure hello Visual Studio **přidat připojení služby** dialogové okno. Když přidáte projektu úlohy WebJob tooa účet úložiště pomocí hello Visual Studio **přidat připojení služby** dialogové okno, jsou nainstalovány hello odpovídající balíčky NuGet úložiště Azure, jsou přidané toohello hello příslušné odkazy na rozhraní .NET projekt a připojovací řetězce pro účet úložiště hello se aktualizují v souboru App.config hello.  

Tento článek obsahuje C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello služby Azure Queue storage.

Azure Queue storage je služba pro ukládání velkého počtu zpráv, které lze přistupovat z libovolné místo v hello, world prostřednictvím ověřených volání s využitím protokolu HTTP nebo HTTPS. Zpráva s jednou frontou může být až velikost too64 KB a jedna fronta můžete obsahovat miliony zpráv, až toohello limit celkové kapacity účtu úložiště. V tématu [Začínáme s Azure Queue Storage pomocí rozhraní .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) Další informace. Další informace o technologii ASP.NET najdete v tématu [ASP.NET](http://www.asp.net).

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a>Jak tootrigger funkce při příjmu zprávy fronty
volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello **QueueTrigger** atribut. konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello toopoll fronty hello. toosee jak tooset hello název fronty dynamicky, podívejte se na [jak tooset možnosti konfigurace](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
V následujícím příkladu hello, fronty hello obsahuje řetězec zprávu, takže **QueueTrigger** je použité tooa řetězec parametr s názvem **logMessage** obsahující hello obsah zprávy fronty hello. Hello funkce [zapíše protokolu zpráv toohello řídicí panel](#how-to-write-logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Kromě **řetězec**, hello parametr může být bajtové pole, **CloudQueueMessage** objekt nebo objektů POCO, které definujete.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
V následujícím příkladu hello, obsahuje zprávu fronty hello JSON pro **BlobInformation** objekt, který zahrnuje **BlobName** vlastnost. Hello SDK automaticky deserializuje objekt hello.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy. Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynchronní funkce
Následující funkce asynchronní Hello [zapíše protokolu toohello řídicí panel](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak ukazuje následující příklad, který kopíruje objekt blob hello. (Další informace o hello **queueTrigger** zástupného symbolu, najdete v části hello [objekty BLOB](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) části.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a>Typy hello QueueTrigger atribut pracuje s
Můžete použít **QueueTrigger** s hello následující typy:

* **řetězec**
* Typ objektů POCO serializovanou jako JSON
* **Byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Algoritmus dotazování
Hello SDK implementuje efekt náhodných exponenciální back vypnout algoritmus tooreduce hello nečinnosti-fronty dotazování na transakce náklady na úložiště.  Když se najde zprávu, hello SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy. Po následné neúspěšných pokusů tooget zprávu fronty, doba čekání hello pokračovat tooincrease, dokud nebude dosaženo hello maximální doba čekání, které minutu tooone výchozí hodnoty. [Hello maximální doba čekání je možné konfigurovat](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Více instancí
Pokud vaše webová aplikace běží na několik instancí, nepřetržité webové úlohy se spustí na každém počítači, a každý počítač bude čekat na aktivační události a toorun funkce. V některých případech to může vést toosome funkce zpracování hello dvakrát stejná data, takže funkce by mělo být idempotent (zapsání, který volání opakovaně s hello stejnými vstupními daty neobsahuje duplicitní výsledky).  

## <a name="parallel-execution"></a>Paralelní provádění
Pokud máte víc funkcí naslouchá na různých front, hello SDK je zavolá paralelně přijetí zpráv současně.

Hello totéž platí při přijetí více zpráv pro jednu frontu. Ve výchozím nastavení hello SDK získá dávku 16 fronty zpráv v čase a provede hello funkce, která je zpracuje paralelně. [velikost dávky Hello je možné konfigurovat](#how-to-set-configuration-options). Když hello počet zpracovávaných získá dolů toohalf hello velikost dávky, hello SDK získá další dávku a spustí zpracování těchto zpráv. Maximální počet souběžných zpráv, které jsou zpracovány za funkce hello je proto velikost dávky hello jeden a půl krát. Toto omezení se vztahuje samostatně tooeach funkce, který má **QueueTrigger** atribut. Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, nastavte too1 velikost dávky hello.

## <a name="get-queue-or-queue-message-metadata"></a>Získání fronty nebo fronty zpráv metadat
Můžete získat hello následující vlastnosti zprávy přidáním podpis metody toohello parametry:

* **Datový typ DateTimeOffset** expirationTime
* **Datový typ DateTimeOffset** insertionTime
* **Datový typ DateTimeOffset** nextVisibleTime
* **řetězec** queueTrigger (obsahuje text zprávy)
* **řetězec** id
* **řetězec** vlastností popReceipt
* **int** dequeueCount

Pokud chcete toowork přímo s hello úložiště Azure API, můžete také přidat **CloudStorageAccount** parametr.

Hello následující příklad zapíše všechny tento protokol metadata tooan informace o aplikaci. V příkladu hello logMessage a queueTrigger obsahovat hello obsah zprávy fronty hello.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Tady je příklad protokolu o zapsaných správcem hello ukázkový kód:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Řádné vypnutí
Funkci, která běží v nepřetržitá webová úloha může přijmout **CancellationToken** parametr, který umožňuje hello operačního systému toonotify hello funkce při hello webové úlohy je o toobe byla ukončena. Můžete vytvořit toto oznámení toomake se, že funkce hello není neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.

Následující příklad ukazuje, jak Hello toocheck pro předcházení ukončení webové úlohy ve funkci.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Poznámka:** hello řídicí panel nemusí zobrazit správně hello stav a výstup funkcí, které byly vypnuty.

Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a>Jak toocreate fronty zpráv při zpracování zprávy fronty
toowrite funkci, která vytvoří novou zprávu fronty, použijte hello **fronty** atribut. Jako **QueueTrigger**, kterou předáte název fronty hello jako řetězec, nebo můžete [nastavit název fronty hello dynamicky](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
Následující ukázka kódu bez asynchronní Hello vytvoří novou zprávu fronty ve frontě hello s názvem "outputqueue" se stejný obsah jako zprávu fronty hello dostali hello frontu s názvem "inputqueue" hello. (Pro asynchronní použít funkce **IAsyncCollector<T>**  znázorněné později v této části.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
toocreate zprávu fronty, která obsahuje objektů POCO spíše než řetězec průchodu hello objektů POCO, zadejte jako parametr toohello výstupu **fronty** konstruktoru atributu.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Hello SDK automaticky serializuje tooJSON objekt hello. Zprávu fronty je vytvořena vždy, i když hello objekt má hodnotu null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Vytvoření více zpráv nebo asynchronní funkce
toocreate více zpráv, zkontrolujte typ parametru hello pro hello výstupní fronty **ICollector<T>**  nebo **IAsyncCollector<T>**, jak ukazuje následující příklad hello.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každou zprávu fronty se vytvoří okamžitě při hello **přidat** metoda je volána.

### <a name="types-that-hello-queue-attribute-works-with"></a>Typy tento atribut fronty hello funguje s
Můžete použít hello **fronty** atribut hello následující typy parametrů:

* **na řetězce** (vytvoří zprávu fronty, pokud je hodnota parametru jinou hodnotu než null při ukončení hello funkce)
* **out byte []** (funguje jako **řetězec**)
* **out CloudQueueMessage** (funguje jako **řetězec**)
* **out objektů POCO** (typu serializable vytvoří zprávu s objektem hodnotu null. Pokud hello parametru má hodnotu null při ukončení hello funkce)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (pro vytváření zpráv ručně pomocí hello API pro Azure Storage přímo)

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a>Použití atributů WebJobs SDK v hello tělo funkce
Pokud potřebujete toodo některé fungovat ve vaší funkci před použitím atribut WebJobs SDK, jako **fronty**, **Blob**, nebo **tabulky**, můžete použít hello **IBinder** rozhraní.

Následující ukázka Hello přebírá zprávu vstupní fronty a vytvoří novou zprávu s hello stejný obsah v výstupní fronty. Název fronty výstup Hello je nastavena podle kódu v textu hello hello funkce.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Hello **IBinder** rozhraní lze také s hello **tabulky** a **Blob** atributy.

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Jak objekty BLOB tooread a zápis a tabulky při zpracování zprávy fronty
Hello **Blob** a **tabulky** atributů umožňují tooread a zapisovat objekty BLOB a tabulek. Ukázky Hello v této části platí tooblobs. Ukázky kódu, které ukazují, jak tootrigger zpracovává, když se objekty BLOB jsou vytvořeny nebo aktualizovány, najdete v části [jak toouse Azure blob storage s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [jak toouse tabulky Azure úložiště s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Zprávy fronty řetězec spuštění operace objektů blob
Pro zprávu fronty, který obsahuje řetězec **queueTrigger** je zástupný symbol můžete použít v hello **Blob** atributu **blobPath** parametr, který obsahuje obsah hello uvítací zprávu.

Hello následující příklad používá **datového proudu** objekty tooread a zápisu objektů BLOB. uvítací zprávu fronty je hello název objektu blob umístěna v kontejneru textblobs hello. Kopie objektu blob hello s "-nové" připojením toohello jméno se vytvoří v hello stejné kontejneru.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hello **Blob** atributu konstruktoru trvá **blobPath** parametr, který určuje hello kontejneru a název objektu blob. Další informace o tento zástupný text najdete v tématu [jak toouse Azure blob storage s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).

Pokud atribut hello upraví **datového proudu** objektu jiný parametr konstruktoru určuje hello **FileAccess** režimu pro čtení, zápisu nebo čtení a zápis.

Hello následující příklad používá **CloudBlockBlob** objektu toodelete objekt blob. uvítací zprávu fronty je hello název objektu hello blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
Pro objektů POCO uloží jako dokumenty JSON v uvítací zprávu fronty, můžete použít zástupné znaky, které název vlastnosti objektu hello v hello **fronty** atributu **blobPath** parametr. Můžete taky názvy vlastností fronty metadata zástupnými symboly. V tématu [získat fronty nebo fronty zpráv metadata](#get-queue-or-queue-message-metadata).

Hello následujícím příkladu se zkopíruje objekt blob tooa nové blob se jiné rozšíření. zpráva fronty Hello **BlobInformation** objekt, který zahrnuje **BlobName** a **BlobNameWithoutExtension** vlastnosti. názvy vlastností Hello jsou použity jako zástupné symboly v cestě objektu blob hello pro hello **Blob** atributy.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy. Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Pokud potřebujete toodo některé fungovat ve vaší funkci před vazby tooan objekt blob, můžete použít atribut hello v textu hello hello funkce, jak je znázorněno v [pomocí WebJobs SDK atributy v textu hello funkce](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-hello-blob-attribute-with"></a>Můžete použít hello typy objektů Blob atribut s
Hello **Blob** atribut lze použít s hello následující typy:

* **Datový proud** (pro čtení nebo zápisu, zadat pomocí parametru konstruktoru FileAccess hello)
* **TextReader**
* **TextWriter**
* **řetězec** (přečíst)
* **na řetězce** (zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce hello má jinou hodnotu než null při návratu hello funkce)
* Objektů POCO (čtení)
* out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, když se vrátí hello funkce)
* **CloudBlobStream** (zápisu)
* **ICloudBlob** (čtení nebo zápisu)
* **CloudBlockBlob** (čtení nebo zápisu)
* **CloudPageBlob** (čtení nebo zápisu)

## <a name="how-toohandle-poison-messages"></a>Jak toohandle škodlivých zpráv
Zprávy, jejichž obsah způsobí, že funkce toofail se nazývají *škodlivých zpráv*. Pokud funkce hello nezdaří, hello fronty zpráv není odstraněna a nakonec je převzata znovu, což způsobilo toobe cyklus hello opakuje. Hello SDK můžete automaticky přerušení hello cyklus po omezený počet iterací, nebo můžete provést ručně.

### <a name="automatic-poison-message-handling"></a>Zpracování automatické poškozených zpráv
Hello SDK bude volat funkci až too5 časy tooprocess zprávu fronty. V případě selhání páté zkuste hello je uvítací zprávu přesunutý tooa poškozených fronty. Uvidíte, jak tooconfigure hello maximální počet opakovaných pokusů v [jak možnosti konfigurace tooset](#how-to-set-configuration-options).

Název fronty poškozených Hello *{originalqueuename}*-poškozených. Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost.

V následujícím příkladu hello hello **CopyBlob** funkce se nezdaří, pokud obsahuje zprávu fronty hello název objektu blob, který neexistuje. Pokud k tomu dojde, hello zpráva bude přesunuta z hello copyblobqueue fronty toohello copyblobqueue poison fronty. Hello **ProcessPoisonMessage** pak protokoly hello poškozená zpráva.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

Hello následující obrázek ukazuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.

![Výstup konzoly pro zpracování poškozených zpráv](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Zpracování ručních poškozených zpráv
Hello počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte **int** parametr s názvem **dequeueCount** tooyour funkce. Můžete pak zkontrolujte hello dequeue – počet v kódu funkce a provádět vlastní zpracování poškozených zpráv Pokud hello číslo překročí prahovou hodnotu, jak ukazuje následující příklad hello.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-tooset-configuration-options"></a>Jak tooset možnosti konfigurace
Můžete použít hello **JobHostConfiguration** hello tooset typ následující možnosti konfigurace:

* Nastavit hello SDK připojovacích řetězců v kódu.
* Konfigurace **QueueTrigger** nastavení, jako například maximální počet vyřazení z fronty.
* Získáte názvy fronty z konfigurace.

### <a name="set-sdk-connection-strings-in-code"></a>Sada SDK připojovacích řetězců v kódu
Nastavení hello SDK připojovacích řetězců v kódu umožňuje vám toouse vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak ukazuje následující příklad hello.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>Konfigurace nastavení QueueTrigger
Můžete nakonfigurovat následující nastavení, které se vztahují zpracování zprávy fronty toohello hello:

* maximální počet zpráv fronty, které jsou zachyceny současně toobe paralelně Hello (výchozí hodnota je 16).
* Hello maximální počet opakovaných pokusů, než je odeslána zpráva fronty tooa poškozených fronty (výchozí hodnota je 5).
* Hello maximální doba čekání před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).

Následující příklad ukazuje, jak Hello tooconfigure tato nastavení:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
Občas můžete chtít toospecify název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu. Například můžete název fronty hello toospecify **QueueTrigger** do proměnné prostředí nebo soubor konfigurace.

Můžete to udělat pomocí předávání **NameResolver** objektu toohello **JobHostConfiguration** typu. Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a **NameResolver** kódu určuje toobe hello skutečné hodnoty, použijí se místo tyto zástupné symboly.

Například předpokládejme, že chcete toouse frontu s názvem logqueuetest v testovacím prostředí hello a jednu s názvem logqueueprod v produkčním prostředí. Místo názvu fronty pevně chcete toospecify hello název záznamu v hello **appSettings** kolekce, která by měla mít název skutečné fronty hello. Pokud hello **appSettings** klíč je logqueue, funkce by měl vypadat jako následující příklad hello.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Vaše **NameResolver** třídy, může získat název fronty hello z **appSettings** jak ukazuje následující příklad hello:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Předat hello **NameResolver** třídy v toohello **JobHost** objektu, jak ukazuje následující příklad hello.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale jenom v případě, že spuštěním aplikace hello překladu názvů kontejner objektů blob. Název kontejneru objektu blob nelze změnit, když je spuštěná úloha hello.

## <a name="how-tootrigger-a-function-manually"></a>Jak tootrigger funkce ručně
tootrigger funkce ručně, použijte hello **volání** nebo **CallAsync** metodu hello **JobHost** objekt a hello **NoAutomaticTrigger** atribut hello funkce, jak ukazuje následující příklad hello.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-toowrite-logs"></a>Jak toowrite protokoly
Hello řídicí panel zobrazuje protokoly na dvou místech: hello stránky pro webové úlohy hello a hello stránky pro konkrétní vyvolání webové úlohy.

![Protokoly v webová stránka](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Výstup z konzoly metod, které volají ve funkci nebo v hello **Main()** metoda se zobrazí v hello stránka řídicího panelu pro hello webové úlohy, nikoli na stránku hello volání konkrétní metody. Výstup hello objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí v hello stránka řídicího panelu pro volání metody.

Výstup konzoly nemůže být volání konkrétní metody propojené tooa, protože hello konzoly je jedním podprocesem, zatímco mnoho funkcí úlohy může běžet v hello stejnou dobu. To je důvod, proč hello SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.

toowrite [protokoly trasování aplikací](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte **Console.Out** (vytvoří protokoly, které jsou označeny jako údaje) a **Console.Error** (vytvoří protokoly, které jsou označeny jako chyba). Alternativou je toouse [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně v přidání tooInfo a chyby. Protokoly trasování aplikací se zobrazí v souborech protokolů hello webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure. Jak platí všechny výstup konzoly hello nejnovější 100 aplikační protokoly se také zobrazí v hello stránka řídicího panelu pro hello webové úlohy, ne hello stránku pro volání funkce.

Výstup konzoly se zobrazí v hello řídicí panel pouze v případě, že při spuštění programu hello v webové úlohy Azure, není-li hello program spuštěn místně nebo v některých dalších prostředí.

Protokolování můžete zakázat nastavením hello řídicí panel připojovací řetězec toonull. Další informace najdete v tématu [jak tooset možnosti konfigurace](#how-to-set-configuration-options).

Hello následující příklad znázorňuje několik způsobů toowrite protokoly:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

V řídicím panelu WebJobs SDK hello, hello výstup hello **TextWriter** objektů, zobrazí se při návratu toohello stránky pro konkrétní funkce volání a vyberte **přepnutí výstup**:

![Vyvolání odkazu](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

V řídicím panelu WebJobs SDK hello, hello nejnovější 100 řádků konzoly výstup zobrazit si při přejděte toohello stránku hello webové úlohy (není pro volání funkce hello) a vyberte možnost **přepnutí výstup**.

![Přepnutí výstup](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů aplikace hello webové aplikace.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

V aplikaci Azure blob hello protokoly vypadat například takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13, chyba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,

A v hello tabulky Azure **Console.Out** a **Console.Error** protokoly vypadat takto:

![Informace o protokolu v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Protokol chyb v tabulce](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Další kroky
Tento článek poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure fronty. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

