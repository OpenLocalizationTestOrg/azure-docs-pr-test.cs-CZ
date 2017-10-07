---
title: aaaHow toouse fronty Azure storage s hello WebJobs SDK
description: "Zjistěte, jak toouse Azure fronty úložiště s hello WebJobs SDK. Vytváření a odstraňování front; Vložit, prohlížet, získání a odstranění zprávy fronty a další."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 49f844436b0453489800b2762a5c7dc30b9db805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-queue-storage-with-hello-webjobs-sdk"></a>Jak toouse Azure fronty úložiště s hello WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# ukázek kódu, které ukazují, jak toouse hello Azure WebJobs SDK verze 1.x s hello fronty Azure storage service.

Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Většina fragmenty kódu hello zobrazit pouze funkce, není hello kód, který vytvoří hello `JobHost` objektu jako v následujícím příkladu:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

Příručka Hello obsahuje hello následující témata:

* [Jak tootrigger funkce při příjmu zprávy fronty](#trigger)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Asynchronní funkce
  * Typy hello QueueTrigger atribut pracuje s
  * Algoritmus dotazování
  * Více instancí
  * Paralelní provádění
  * Získání fronty nebo fronty zpráv metadat
  * Řádné vypnutí
* [Jak toocreate fronty zpráv při zpracování zprávy fronty](#createqueue)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Vytvoření více zpráv nebo asynchronní funkce
  * Typy hello fronty atribut pracuje s
  * Použití atributů WebJobs SDK v hello tělo funkce
* [Jak objekty při zpracování zprávy fronty BLOB tooread a zápis](#blobs)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Typy hello Blob atribut pracuje s
* [Jak toohandle škodlivých zpráv](#poison)
  * Zpracování automatické poškozených zpráv
  * Zpracování ručních poškozených zpráv
* [Jak tooset možnosti konfigurace](#config)
  * Sada SDK připojovacích řetězců v kódu
  * Konfigurace nastavení QueueTrigger
  * Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* [Jak tootrigger funkce ručně](#manual)
* [Jak toowrite protokoly](#logs)
* [Jak toohandle chyby a nakonfigurujte časové limity](#errors)
* [Další kroky](#nextsteps)

## <a id="trigger"></a>Jak tootrigger funkce při příjmu zprávy fronty
volá funkci, která hello WebJobs SDK toowrite při příjmu zprávy fronty, použijte hello `QueueTrigger` atribut. konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název hello toopoll fronty hello. Můžete také [nastavit název fronty hello dynamicky](#config).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
V následujícím příkladu hello, fronty hello obsahuje řetězec zprávu, takže `QueueTrigger` je použité tooa řetězec parametr s názvem `logMessage` obsahující hello obsah zprávy fronty hello. Hello funkce [zapíše protokolu zpráv toohello řídicí panel](#logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Kromě `string`, hello parametr může být bajtové pole, `CloudQueueMessage` objekt nebo objektů POCO, které definujete.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
V následujícím příkladu hello, obsahuje zprávu fronty hello JSON pro `BlobInformation` objekt, který zahrnuje `BlobName` vlastnost. Hello SDK automaticky deserializuje objekt hello.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

Hello SDK používá hello [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize a deserializaci zprávy. Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá hello WebJobs SDK, můžete napsat kód jako následující příklad toocreate zprávu fronty objektů POCO hello této hello, které mohou analyzovat SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynchronní funkce
Následující funkce asynchronní Hello [zapíše protokolu toohello řídicí panel](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak ukazuje následující příklad, který kopíruje objekt blob hello. (Další informace o hello `queueTrigger` zástupného symbolu, najdete v části hello [objekty BLOB](#blobs) části.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Typy hello QueueTrigger atribut pracuje s
Můžete použít `QueueTrigger` s hello následující typy:

* `string`
* Typ objektů POCO serializovanou jako JSON
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Algoritmus dotazování
Hello SDK implementuje efekt náhodných exponenciální back vypnout algoritmus tooreduce hello nečinnosti-fronty dotazování na transakce náklady na úložiště.  Když se najde zprávu, hello SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy. Po následné neúspěšných pokusů tooget zprávu fronty, doba čekání hello pokračovat tooincrease, dokud nebude dosaženo hello maximální doba čekání, které minutu tooone výchozí hodnoty. [Hello maximální doba čekání je možné konfigurovat](#config).

### <a id="instances"></a>Více instancí
Pokud vaše webová aplikace běží na několik instancí, nepřetržitá webová úloha běží na každý počítač, a každý počítač bude čekat na aktivační události a toorun funkce. Hello aktivace fronty WebJobs SDK automaticky zabrání funkci zpracování zpráv fronty vícekrát; Funkce nemají toobe zapsána toobe idempotent. Ale pokud chcete, aby tooensure že pouze jedna instance funkce spustí i v případě, že existuje více instancí hello hostitele webové aplikace, můžete použít hello `Singleton` atribut.

### <a id="parallel"></a>Paralelní provádění
Pokud máte víc funkcí naslouchá na různých front, hello SDK je zavolá paralelně přijetí zpráv současně.

Hello totéž platí při přijetí více zpráv pro jednu frontu. Ve výchozím nastavení hello SDK získá dávku 16 fronty zpráv v čase a provede hello funkce, která je zpracuje paralelně. [velikost dávky Hello je možné konfigurovat](#config). Když hello počet zpracovávaných získá dolů toohalf hello velikost dávky, hello SDK získá další dávku a spustí zpracování těchto zpráv. Maximální počet souběžných zpráv, které jsou zpracovány za funkce hello je proto velikost dávky hello jeden a půl krát. Toto omezení se vztahuje samostatně tooeach funkce, který má `QueueTrigger` atribut.

Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, můžete nastavit too1 velikost dávky hello. Viz také **větší kontrolu nad fronty zpracování** v [RTM Azure WebJobs SDK 1.1.0](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Získání fronty nebo fronty zpráv metadat
Můžete získat hello následující vlastnosti zprávy přidáním podpis metody toohello parametry:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (obsahuje text zprávy)
* `string`ID
* `string`Vlastnosti popReceipt
* `int`dequeueCount

Pokud chcete toowork přímo s hello úložiště Azure API, můžete také přidat `CloudStorageAccount` parametr.

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

### <a id="graceful"></a>Řádné vypnutí
Funkci, která běží v nepřetržitá webová úloha může přijmout `CancellationToken` parametr, který umožňuje hello operačního systému toonotify hello funkce při hello webové úlohy je o toobe byla ukončena. Můžete vytvořit toto oznámení toomake se, že funkce hello není neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.

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

## <a id="createqueue"></a>Jak toocreate fronty zpráv při zpracování zprávy fronty
toowrite funkci, která vytvoří novou zprávu fronty, použijte hello `Queue` atribut. Jako `QueueTrigger`, kterou předáte název fronty hello jako řetězec, nebo můžete [nastavit název fronty hello dynamicky](#config).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
Následující ukázka kódu bez asynchronní Hello vytvoří novou zprávu fronty ve frontě hello s názvem "outputqueue" se stejný obsah jako zprávu fronty hello dostali hello frontu s názvem "inputqueue" hello. (Pro asynchronní použít funkce `IAsyncCollector<T>` znázorněné později v této části.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
toocreate zprávu fronty, která obsahuje objektů POCO spíše než řetězec průchodu hello objektů POCO, zadejte jako parametr toohello výstupu `Queue` konstruktoru atributu.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Hello SDK automaticky serializuje tooJSON objekt hello. Zprávu fronty je vytvořena vždy, i když hello objekt má hodnotu null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Vytvoření více zpráv nebo asynchronní funkce
toocreate více zpráv, zkontrolujte typ parametru hello pro hello výstupní fronty `ICollector<T>` nebo `IAsyncCollector<T>`, jak ukazuje následující příklad hello.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každou zprávu fronty se vytvoří okamžitě při hello `Add` metoda je volána.

### <a name="types-that-hello-queue-attribute-works-with"></a>Typy tento atribut fronty hello funguje s
Můžete použít hello `Queue` atribut hello následující typy parametrů:

* `out string`(Pokud je hodnota parametru jinou hodnotu než null při ukončení hello funkce vytvoří zprávu fronty)
* `out byte[]`(funguje jako `string`)
* `out CloudQueueMessage`(funguje jako `string`)
* `out POCO`(typu serializable vytvoří zprávu s objektem hodnotu null. Pokud hello parametru má hodnotu null při ukončení hello funkce)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(pro vytváření zpráv ručně přímo pomocí hello API pro Azure Storage)

### <a id="ibinder"></a>Použití atributů WebJobs SDK v hello tělo funkce
Pokud potřebujete toodo některé fungovat ve vaší funkci před použitím atribut WebJobs SDK, jako `Queue`, `Blob`, nebo `Table`, můžete použít hello `IBinder` rozhraní.

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

Hello `IBinder` rozhraní lze také s hello `Table` a `Blob` atributy.

## <a id="blobs"></a>Jak objekty BLOB tooread a zápis a tabulky při zpracování zprávy fronty
Hello `Blob` a `Table` atributů umožňují tooread a zapisovat objekty BLOB a tabulek. Ukázky Hello v této části platí tooblobs. Ukázky kódu, které ukazují, jak tootrigger zpracovává, když se objekty BLOB jsou vytvořeny nebo aktualizovány, najdete v části [jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [jak toouse tabulky Azure úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Zprávy fronty řetězec spuštění operace objektů blob
Pro zprávu fronty, který obsahuje řetězec `queueTrigger` je zástupný symbol můžete použít v hello `Blob` atributu `blobPath` parametr, který obsahuje obsah hello uvítací zprávu.

Hello následující příklad používá `Stream` objekty tooread a zápisu objektů BLOB. uvítací zprávu fronty je hello název objektu blob umístěna v kontejneru textblobs hello. Kopie objektu blob hello s "-nové" připojením toohello jméno se vytvoří v hello stejné kontejneru.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hello `Blob` atributu konstruktoru trvá `blobPath` parametr, který určuje hello kontejneru a název objektu blob. Další informace o tento zástupný text najdete v tématu [jak toouse Azure blob storage s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),

Pokud atribut hello upraví `Stream` objektu jiný parametr konstruktoru určuje hello `FileAccess` režimu pro čtení, zápisu nebo čtení a zápis.

Hello následující příklad používá `CloudBlockBlob` objektu toodelete objekt blob. uvítací zprávu fronty je hello název objektu hello blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
Pro objektů POCO uloží jako dokumenty JSON v uvítací zprávu fronty, můžete použít zástupné znaky, které název vlastnosti objektu hello v hello `Queue` atributu `blobPath` parametr. Můžete také použít [fronty názvy vlastností metadat](#queuemetadata) zástupnými symboly.

Hello následujícím příkladu se zkopíruje objekt blob tooa nové blob se jiné rozšíření. zpráva fronty Hello `BlobInformation` objekt, který zahrnuje `BlobName` a `BlobNameWithoutExtension` vlastnosti. názvy vlastností Hello jsou použity jako zástupné symboly v cestě objektu blob hello pro hello `Blob` atributy.

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

Pokud potřebujete toodo některé fungovat ve vaší funkci před vazby tooan objekt blob, můžete použít atribut hello v textu hello hello funkce [jak je uvedeno výše pro atribut fronty hello](#ibinder).

### <a id="blobattributetypes"></a>Můžete použít hello typy objektů Blob atribut s
Hello `Blob` atribut lze použít s hello následující typy:

* `Stream`(pro čtení nebo zápisu, zadat pomocí parametru konstruktoru FileAccess hello)
* `TextReader`
* `TextWriter`
* `string`(pro čtení)
* `out string`(zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce hello má jinou hodnotu než null při návratu hello funkce)
* Objektů POCO (čtení)
* out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, když se vrátí hello funkce)
* `CloudBlobStream`(zápisu)
* `ICloudBlob`(pro čtení nebo zápisu)
* `CloudBlockBlob`(pro čtení nebo zápisu)
* `CloudPageBlob`(pro čtení nebo zápisu)

## <a id="poison"></a>Jak toohandle škodlivých zpráv
Zprávy, jejichž obsah způsobí, že funkce toofail se nazývají *škodlivých zpráv*. Pokud funkce hello nezdaří, hello fronty zpráv není odstraněna a nakonec je převzata znovu, což způsobilo toobe cyklus hello opakuje. Hello SDK můžete automaticky přerušení hello cyklus po omezený počet iterací, nebo můžete provést ručně.

### <a name="automatic-poison-message-handling"></a>Zpracování automatické poškozených zpráv
Hello SDK bude volat funkci až too5 časy tooprocess zprávu fronty. V případě selhání páté zkuste hello je uvítací zprávu přesunutý tooa poškozených fronty. [maximální počet opakovaných pokusů Hello je možné konfigurovat](#config).

Název fronty poškozených Hello *{originalqueuename}*-poškozených. Můžete napsat tooprocess funkce zprávy z fronty poškozených hello jejich protokolování nebo odeslání oznámení, že je potřeba ruční pozornost.

V následujícím příkladu hello hello `CopyBlob` funkce se nezdaří, pokud obsahuje zprávu fronty hello název objektu blob, který neexistuje. Pokud k tomu dojde, hello zpráva bude přesunuta z hello copyblobqueue fronty toohello copyblobqueue poison fronty. Hello `ProcessPoisonMessage` pak protokoly hello poškozená zpráva.

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

![Výstup konzoly pro zpracování poškozených zpráv](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Zpracování ručních poškozených zpráv
Hello počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte `int` parametr s názvem `dequeueCount` tooyour funkce. Můžete pak zkontrolujte hello dequeue – počet v kódu funkce a provádět vlastní zpracování poškozených zpráv Pokud hello číslo překročí prahovou hodnotu, jak ukazuje následující příklad hello.

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

## <a id="config"></a>Jak tooset možnosti konfigurace
Můžete použít hello `JobHostConfiguration` hello tooset typ následující možnosti konfigurace:

* Nastavit hello SDK připojovacích řetězců v kódu.
* Konfigurace `QueueTrigger` nastavení, jako například maximální počet vyřazení z fronty.
* Získáte názvy fronty z konfigurace.

### <a id="setconnstr"></a>Sada SDK připojovacích řetězců v kódu
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

### <a id="configqueue"></a>Konfigurace nastavení QueueTrigger
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

### <a id="setnamesincode"></a>Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
Občas můžete chtít toospecify název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu. Například můžete název fronty hello toospecify `QueueTrigger` do proměnné prostředí nebo soubor konfigurace.

Můžete to udělat pomocí předávání `NameResolver` objektu toohello `JobHostConfiguration` typu. Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a `NameResolver` kódu určuje toobe hello skutečné hodnoty, použijí se místo tyto zástupné symboly.

Například předpokládejme, že chcete toouse frontu s názvem logqueuetest v testovacím prostředí hello a jednu s názvem logqueueprod v produkčním prostředí. Místo názvu fronty pevně chcete toospecify hello název záznamu v hello `appSettings` kolekce, která by měla mít název skutečné fronty hello. Pokud hello `appSettings` klíč je logqueue, funkce by měl vypadat jako následující příklad hello.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Vaše `NameResolver` třídy, může získat název fronty hello z `appSettings` jak ukazuje následující příklad hello:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Předat hello `NameResolver` třídy v toohello `JobHost` objektu, jak ukazuje následující příklad hello.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale jenom v případě, že spuštěním aplikace hello překladu názvů kontejner objektů blob. Název kontejneru objektu blob nelze změnit, když je spuštěná úloha hello.

## <a id="manual"></a>Jak tootrigger funkce ručně
tootrigger funkce ručně, použijte hello `Call` nebo `CallAsync` metodu hello `JobHost` objekt a hello `NoAutomaticTrigger` atributu u hello funkce, jak ukazuje následující příklad hello.

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

## <a id="logs"></a>Jak toowrite protokoly
Hello řídicí panel zobrazuje protokoly na dvou místech: hello stránky pro webové úlohy hello a hello stránky pro konkrétní vyvolání webové úlohy.

![Protokoly v webová stránka](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Výstup z konzoly metod, které volají ve funkci nebo v hello `Main()` metoda se zobrazí v hello stránka řídicího panelu pro hello webové úlohy, nikoli na stránku hello volání konkrétní metody. Výstup hello objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí v hello stránka řídicího panelu pro volání metody.

Výstup konzoly nemůže být volání konkrétní metody propojené tooa, protože hello konzoly je jedním podprocesem, zatímco mnoho funkcí úlohy může běžet v hello stejnou dobu. To je důvod, proč hello SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.

toowrite [protokoly trasování aplikací](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte `Console.Out` (vytvoří protokoly, které jsou označeny jako údaje) a `Console.Error` (vytvoří protokoly, které jsou označeny jako chyba). Alternativou je toouse [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně v přidání tooInfo a chyby. Protokoly trasování aplikací se zobrazí v souborech protokolů hello webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure. Jak platí všechny výstup konzoly hello nejnovější 100 aplikační protokoly se také zobrazí v hello stránka řídicího panelu pro hello webové úlohy, ne hello stránku pro volání funkce.

Výstup konzoly se zobrazí v hello řídicí panel pouze v případě, že při spuštění programu hello v webové úlohy Azure, není-li hello program spuštěn místně nebo v některých dalších prostředí.

Zakázání protokolování řídicí panel pro scénáře vysoké propustnosti. Ve výchozím nastavení hello SDK zapíše toostorage protokoly a tato aktivita může snížit výkon při zpracování velkého počtu zpráv. toodisable protokolování, nastavte hello řídicí panel připojovací řetězec toonull, jak je znázorněno v následující ukázka hello.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

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

V řídicím panelu WebJobs SDK hello, hello výstup hello `TextWriter` objektů, zobrazí se při návratu toohello stránky pro konkrétní funkce volání a klikněte na tlačítko **přepnutí výstup**:

![Klikněte na odkaz volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

V řídicím panelu WebJobs SDK hello, hello nejnovější 100 řádků konzoly výstup zobrazit až když přejdete toohello stránku hello webové úlohy (není pro volání funkce hello) a klikněte na tlačítko **přepnutí výstup**.

![Klikněte na přepínač výstup](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů aplikace hello webové aplikace.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

V aplikaci Azure blob hello protokoly vypadat například takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13, chyba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,

A v hello tabulky Azure `Console.Out` a `Console.Error` protokoly vypadat takto:

![Informace o protokolu v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Protokol chyb v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Pokud chcete tooplug ve vlastní protokoly, najdete v části [v tomto příkladu](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Jak toohandle chyby a nakonfigurujte časové limity
Hello WebJobs SDK také zahrnuje [časový limit](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) atribut, který můžete použít toocause toobe funkce zruší, pokud se nedokončí ve stanoveném intervalu. A pokud chcete tooraise výstrahu, když příliš mnoho chyb dojít v zadaném časovém období, můžete použít hello `ErrorTrigger` atribut. Tady je [ErrorTrigger příklad](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    too= "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors toohello Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Můžete také dynamicky zakázat a povolit funkce toocontrol, zda se můžete spustit, a to pomocí konfigurace přepínače, které by mohly být nastavení aplikace nebo název proměnné prostředí. Ukázkový kód, najdete v části hello `Disable` atribut [hello WebJobs SDK ukázky úložiště](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl kódu ukázky, zobrazující jak toohandle běžné scénáře pro práci s Azure fronty. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).
