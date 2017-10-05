---
title: "Používání Azure Queue Storage se sadou WebJobs SDK"
description: "Další informace o použití úložiště fronty Azure pomocí WebJobs SDK. Vytváření a odstraňování front; Vložit, prohlížet, získání a odstranění zprávy fronty a další."
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
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Používání Azure Queue Storage se sadou WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# ukázek kódu, které ukazují, jak používat Azure WebJobs SDK verze 1.x pomocí služby Azure queue storage.

V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Většina fragmenty kódu zobrazit pouze funkce, není kód, který vytvoří `JobHost` objektu jako v následujícím příkladu:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

V Průvodci obsahuje následující témata:

* [Postup aktivace funkce při příjmu zprávy fronty](#trigger)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Asynchronní funkce
  * Typy atribut QueueTrigger pracuje s
  * Algoritmus dotazování
  * Více instancí
  * Paralelní provádění
  * Získání fronty nebo fronty zpráv metadat
  * Řádné vypnutí
* [Postup vytvoření fronty zpráv při zpracování zprávy fronty](#createqueue)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Vytvoření více zpráv nebo asynchronní funkce
  * Typy atribut fronty pracuje s
  * Použití atributů WebJobs SDK v tělo funkce
* [Postup pro čtení a zápis objektů BLOB při zpracování zprávy fronty](#blobs)
  * Řetězec fronty zpráv
  * Zprávy fronty objektů POCO
  * Typy objektů Blob atribut funguje s
* [Postupy: zpracování poškozených zpráv](#poison)
  * Zpracování automatické poškozených zpráv
  * Zpracování ručních poškozených zpráv
* [Jak nastavit možnosti konfigurace](#config)
  * Sada SDK připojovacích řetězců v kódu
  * Konfigurace nastavení QueueTrigger
  * Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* [Postup ruční aktivaci funkce](#manual)
* [Jak napsat protokoly](#logs)
* [Zpracování chyb a konfiguraci časových limitů](#errors)
* [Další kroky](#nextsteps)

## <a id="trigger"></a>Postup aktivace funkce při příjmu zprávy fronty
Chcete-li vytvořit funkci, která volá WebJobs SDK při příjmu zprávy fronty, použijte `QueueTrigger` atribut. Konstruktoru atributu přijímá řetězcový parametr, který určuje název fronty pro cyklické dotazování. Můžete také [nastavit název fronty dynamicky](#config).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
V následujícím příkladu fronty obsahuje řetězec zprávu, takže `QueueTrigger` se použije pro parametr řetězec s názvem `logMessage` obsahující obsah zprávy ve frontě. Funkce [zapíše zprávu protokolu na řídicí panel](#logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Kromě `string`, tento parametr může být bajtové pole, `CloudQueueMessage` objekt nebo objektů POCO, které definujete.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
V následujícím příkladu obsahuje zprávy ve frontě JSON pro `BlobInformation` objekt, který zahrnuje `BlobName` vlastnost. Sada SDK automaticky deserializuje objekt.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy. Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynchronní funkce
Následující funkce asynchronní [zapíše do protokolu na řídicí panel](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynchronní funkce může trvat [token zrušení](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), jak je znázorněno v následujícím příkladu, který se zkopíruje do objektu blob. (Další informace o `queueTrigger` zástupného symbolu, najdete v článku [objekty BLOB](#blobs) části.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Typy atribut QueueTrigger pracuje s
Můžete použít `QueueTrigger` s následujícími typy:

* `string`
* Typ objektů POCO serializovanou jako JSON
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Algoritmus dotazování
Sada SDK implementuje náhodných exponenciální back vypnout algoritmus, aby se snížil dopad nečinnosti-fronty dotazování na transakce náklady na úložiště.  Když se najde zprávu, sadu SDK vyčká dvou sekund a poté zkontroluje další zprávu; Pokud je nalezena žádná zpráva čeká před dalším pokusem o 4 sekundy. Po následné neúspěšných pokusech o získání zprávu fronty doba čekání nadále zvýšit, dokud nedosáhne maximální čekací doba, výchozí nastavení je na jednu minutu. [Maximální čekací doba je konfigurovatelná](#config).

### <a id="instances"></a>Více instancí
Pokud vaše webová aplikace běží na několik instancí, nepřetržitá webová úloha běží na každý počítač, a každý počítač bude čekat na aktivační události a pokus o spuštění funkce. Aktivace fronty WebJobs SDK automaticky zabrání funkci zpracování zpráv fronty vícekrát; má být proveden zápis idempotent nemají funkce. Ale pokud chcete zajistit, že pouze jedna instance funkce spustí i v případě, že existuje více instancí hostitele webové aplikace, můžete použít `Singleton` atribut.

### <a id="parallel"></a>Paralelní provádění
Pokud máte víc funkcí naslouchá na různých front, sadu SDK je zavolá paralelně přijetí zpráv současně.

Totéž platí při přijetí více zpráv pro jednu frontu. Ve výchozím nastavení sada SDK získá dávku 16 fronty zpráv v čase a provádí funkce, která je zpracuje paralelně. [Velikost dávky je možné konfigurovat](#config). Když počet zpracovávaných získá na polovinu velikost dávky, sadu SDK získá další dávku a spustí zpracování těchto zpráv. Proto je maximální počet souběžných zpráv, které jsou zpracovány za funkce jeden a půl krát velikost dávky. Toto omezení se vztahuje samostatně pro každou funkci, která má `QueueTrigger` atribut.

Pokud nechcete, aby paralelní zpracování zprávy přijaté v jedné frontě, můžete nastavit velikost dávky na 1. Viz také **větší kontrolu nad fronty zpracování** v [RTM Azure WebJobs SDK 1.1.0](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Získání fronty nebo fronty zpráv metadat
Následující vlastnosti zprávy můžete získat tak, že přidáte parametry pro podpis metody:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (obsahuje text zprávy)
* `string`ID
* `string`Vlastnosti popReceipt
* `int`dequeueCount

Pokud chcete pracovat přímo s úložištěm Azure API, můžete také přidat `CloudStorageAccount` parametr.

Následující příklad všechny tato metadata zapíše do protokolu informace o aplikaci. V příkladu logMessage a queueTrigger obsahovat obsah zprávy ve frontě.

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

Tady je příklad protokolu o zapsaných správcem ukázkový kód:

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
Funkci, která běží v nepřetržitá webová úloha může přijmout `CancellationToken` parametr, který umožňuje operačního systému a upozorňovaly vytvářené webové úlohy je možné ukončit funkce. Toto oznámení můžete Ujistěte se, že funkce nepodporuje neočekávaně ukončí tak, aby data ponechá v nekonzistentním stavu.

Následující příklad ukazuje, jak zkontrolovat pro předcházení ukončení webové úlohy ve funkci.

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

**Poznámka:** řídicí panel nemusí zobrazit správně, stav a výstup funkcí, které byly vypnuty.

Další informace najdete v tématu [WebJobs řádné vypnutí](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Postup vytvoření fronty zpráv při zpracování zprávy fronty
Chcete-li vytvořit funkci, která vytvoří novou zprávu fronty, použijte `Queue` atribut. Jako `QueueTrigger`, kterou předáte název fronty jako řetězec, nebo můžete [nastavit název fronty dynamicky](#config).

### <a name="string-queue-messages"></a>Řetězec fronty zpráv
Následující ukázka kódu bez asynchronní vytvoří novou zprávu fronty ve frontě s názvem "outputqueue" se stejným obsahem jako zprávy ve frontě dostali frontu s názvem "inputqueue". (Pro asynchronní použít funkce `IAsyncCollector<T>` znázorněné později v této části.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
Vytvoření fronty zprávu, která obsahuje objektů POCO spíše než řetězec, předá typ objektů POCO jako výstupní parametr k `Queue` konstruktoru atributu.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Sada SDK automaticky serializuje objekt do formátu JSON. Zprávu fronty je vytvořena vždy, i v případě, že objekt má hodnotu null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Vytvoření více zpráv nebo asynchronní funkce
Pokud chcete vytvořit více zpráv, ujistěte se, typ parametru pro výstupní fronty `ICollector<T>` nebo `IAsyncCollector<T>`, jak je znázorněno v následujícím příkladu.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Každou zprávu fronty je vytvořena ihned po `Add` metoda je volána.

### <a name="types-that-the-queue-attribute-works-with"></a>Typy, které atribut fronty pracuje s
Můžete použít `Queue` atribut na následující typy parametrů:

* `out string`(Pokud je hodnota parametru jinou hodnotu než null při ukončení funkce vytvoří zprávu fronty)
* `out byte[]`(funguje jako `string`)
* `out CloudQueueMessage`(funguje jako `string`)
* `out POCO`(typu serializable vytvoří zprávu s objektem hodnotu null. Pokud má parametr hodnotu null při ukončení funkce)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(pro vytváření zpráv ručně přímo pomocí rozhraní API služby Azure Storage)

### <a id="ibinder"></a>Použití atributů WebJobs SDK v tělo funkce
Pokud potřebujete nějakou práci ve vašem funkci před použitím atribut WebJobs SDK, jako `Queue`, `Blob`, nebo `Table`, můžete použít `IBinder` rozhraní.

Následující příklad přebírá zprávu vstupní fronty a vytvoří novou zprávu se stejným obsahem v výstupní fronty. Název fronty výstupu je nastavena podle kódu v těle funkce.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

`IBinder` Rozhraní lze také s `Table` a `Blob` atributy.

## <a id="blobs"></a>Tom, jak číst a zapisovat objekty BLOB a tabulek při zpracování zprávy fronty
`Blob` a `Table` atributů umožňují čtení a zápisu objektů BLOB a tabulek. Ukázky v této části se vztahují na objekty BLOB. Ukázky kódu, které ukazují, jak aktivovat procesy při objekty BLOB jsou vytvoření nebo aktualizaci, najdete v části [používání úložiště objektů blob v Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)a ukázky kódu, které pro čtení a zápis tabulky, najdete v části [použití úložiště tabulek Azure s WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Zprávy fronty řetězec spuštění operace objektů blob
Pro zprávu fronty, který obsahuje řetězec `queueTrigger` je zástupný symbol v můžete použít `Blob` atributu `blobPath` parametr, který obsahuje obsah zprávy.

Následující příklad používá `Stream` objekty ke čtení a zápisu objektů BLOB. Zprávy ve frontě je název objektu blob v kontejneru textblobs. Kopie objektu blob s "-nové" připojenou k názvu se vytvoří ve stejném kontejneru.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

`Blob` Atributu konstruktoru trvá `blobPath` parametr, který určuje název kontejneru a objektů blob. Další informace o tento zástupný text najdete v tématu [používání úložiště objektů blob v Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),

Pokud atribut upraví `Stream` objektu určuje jiný parametr konstruktoru `FileAccess` režimu pro čtení, zápisu nebo čtení a zápis.

Následující příklad používá `CloudBlockBlob` objekt, který chcete odstranit objekt blob. Zprávy ve frontě je název objektu blob.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>Objektů POCO [(prostý původního objektu CLR](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) fronty zpráv
Pro objektů POCO uloží jako dokumenty JSON v zprávy ve frontě, můžete použít zástupné znaky, které název vlastnosti v objektu `Queue` atributu `blobPath` parametr. Můžete také použít [fronty názvy vlastností metadat](#queuemetadata) zástupnými symboly.

Následující příklad zkopíruje objekt blob do nového objektu blob s jiné rozšíření. Zprávy ve frontě je `BlobInformation` objekt, který zahrnuje `BlobName` a `BlobNameWithoutExtension` vlastnosti. Názvy vlastností, které jsou použity jako zástupné symboly v cestě objektu blob pro `Blob` atributy.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Sada SDK používá [balíček Newtonsoft.Json NuGet](http://www.nuget.org/packages/Newtonsoft.Json) k serializaci a deserializaci zprávy. Pokud vytvoříte fronty zpráv v aplikaci, která nepoužívá WebJobs SDK, můžete napsat kód jako v následujícím příkladu pro vytvoření zprávy fronty objektů POCO, které mohou analyzovat sady SDK.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Pokud potřebujete nějakou práci ve vašem funkci před vazby objektu blob na objekt, můžete použít atribut v těle funkce, [jak je uvedeno výše pro atribut fronty](#ibinder).

### <a id="blobattributetypes"></a>Typy, které můžete použít atribut objektů Blob s
`Blob` Atribut lze použít s následujícími typy:

* `Stream`(pro čtení nebo zápisu, zadán pomocí parametru FileAccess konstruktor)
* `TextReader`
* `TextWriter`
* `string`(pro čtení)
* `out string`(zapisovat; vytvoří objekt blob jenom v případě, že parametr řetězce je jinou hodnotu než null, pokud funkce vrátí hodnotu)
* Objektů POCO (čtení)
* out objektů POCO (zápisu; vždycky vytvoří objekt blob, vytvoří jako objektu null, pokud parametr objektů POCO má hodnotu null, pokud funkce vrátí hodnotu)
* `CloudBlobStream`(zápisu)
* `ICloudBlob`(pro čtení nebo zápisu)
* `CloudBlockBlob`(pro čtení nebo zápisu)
* `CloudPageBlob`(pro čtení nebo zápisu)

## <a id="poison"></a>Postupy: zpracování poškozených zpráv
Zprávy, jejichž obsah způsobí, že funkce, která se nezdaří se nazývají *škodlivých zpráv*. Pokud se funkce nezdaří, zprávy ve frontě není odstraněna a nakonec je převzata znovu způsobuje cyklus opakovat. Sada SDK může automaticky přerušení na cyklus po omezený počet iterací, nebo můžete provést ručně.

### <a name="automatic-poison-message-handling"></a>Zpracování automatické poškozených zpráv
Sada SDK bude volat funkci až 5 x zpracovat zprávu fronty. V případě selhání páté zkuste zpráva bude přesunuta do fronty poškozených. [Maximální počet opakovaných pokusů se dá konfigurovat](#config).

Poškozených fronty jmenuje *{originalqueuename}*-poškozených. Můžete napsat, že je funkce, která se zpracování zpráv z fronty poškozených jejich protokolování nebo odesílání oznámení, že ruční pozornost.

V následujícím příkladu `CopyBlob` funkce se nezdaří, pokud zpráva fronty obsahuje název objektu blob, který neexistuje. Pokud k tomu dojde, je zpráva přesunuta do fronty copyblobqueue poison z fronty copyblobqueue. `ProcessPoisonMessage` Pak protokoly poškozená zpráva.

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
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Následující obrázek znázorňuje výstup konzoly z těchto funkcí při zpracování poškozená zpráva.

![Výstup konzoly pro zpracování poškozených zpráv](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Zpracování ručních poškozených zpráv
Počet, kolikrát byla převzata zprávu pro zpracování můžete získat tak, že přidáte `int` parametr s názvem `dequeueCount` na funkce. Potom můžete zkontrolovat počet dequeue v kódu funkce a provádět vlastní nezpracovatelná zpráva zpracování Pokud počet překročí prahovou hodnotu, jak je znázorněno v následujícím příkladu.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Jak nastavit možnosti konfigurace
Můžete použít `JobHostConfiguration` typ a nastavte následující možnosti konfigurace:

* Nastavte SDK připojovacích řetězců v kódu.
* Konfigurace `QueueTrigger` nastavení, jako například maximální počet vyřazení z fronty.
* Získáte názvy fronty z konfigurace.

### <a id="setconnstr"></a>Sada SDK připojovacích řetězců v kódu
Nastavení SDK připojovacích řetězců v kódu můžete použít vlastní názvy řetězec připojení v konfiguračních souborů nebo proměnné prostředí, jak je znázorněno v následujícím příkladu.

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
Můžete nakonfigurovat následující nastavení, které platí pro zpracování zprávy fronty:

* Maximální počet zpráv fronty, které jsou zachyceny současně spouštění paralelní (výchozí hodnota je 16).
* Maximální počet opakovaných pokusů, než odešle zprávu fronty poškozených fronty (výchozí hodnota je 5).
* Maximální čekací dobu před znovu dotazování, když se fronta je prázdný (výchozí hodnota je 1 minuta).

Následující příklad ukazuje, jak nakonfigurovat tato nastavení:

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
Někdy chcete zadat název fronty, název objektu blob nebo kontejner, nebo tabulku název v kódu než pevný kódu. Například můžete chtít zadat název fronty pro `QueueTrigger` do proměnné prostředí nebo soubor konfigurace.

Můžete to udělat pomocí předávání `NameResolver` do objektu `JobHostConfiguration` typu. Zahrnout speciální zástupné symboly obklopená znaky procenta (%) v parametrech konstruktoru atributu WebJobs SDK a `NameResolver` kódu určuje skutečné hodnoty má být použit místo tyto zástupné symboly.

Předpokládejme například, že chcete použít frontu s názvem logqueuetest v testovacím prostředí a jednu s názvem logqueueprod v produkčním prostředí. Místo názvu pevně fronty, které chcete určit název záznamu v `appSettings` kolekce, která by měla mít název skutečné fronty. Pokud `appSettings` klíč je logqueue, funkce může vypadat podobně jako v následujícím příkladu.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Vaše `NameResolver` třídy, může získat název fronty z `appSettings` jak je znázorněno v následujícím příkladu:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Předáte `NameResolver` třídy v do `JobHost` objektu, jak je znázorněno v následujícím příkladu.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Poznámka:** Queue, table a názvy objektů blob jsou vyřešeny pokaždé, když je volána funkce, ale překladu názvů kontejner objektů blob pouze při spuštění aplikace. Název kontejneru objektu blob nelze změnit, když úloha běží.

## <a id="manual"></a>Postup ruční aktivaci funkce
Chcete-li aktivovat funkci ručně, použijte `Call` nebo `CallAsync` metodu `JobHost` objektu a `NoAutomaticTrigger` atribut na funkci, jak je znázorněno v následujícím příkladu.

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

## <a id="logs"></a>Jak napsat protokoly
Řídicí panel zobrazuje protokoly na dvou místech: této webové stránky a stránky pro konkrétní vyvolání webové úlohy.

![Protokoly v webová stránka](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Výstup konzoly metody, kterou je možné volat ve funkci nebo v `Main()` metoda se zobrazí na stránce řídicího panelu pro vytvářené webové úlohy, není na stránce pro volání konkrétní metody. Výstup z objekt TextWriter, kterou můžete získat z parametr ve vašem podpis metody se zobrazí na stránce řídicího panelu pro volání metody.

Výstup konzoly, nelze ho propojit s volání konkrétní metody, protože konzole je jedním podprocesem, spuštěného mnoho funkcí úloha může být ve stejnou dobu. To je důvod, proč sada SDK poskytuje každé vyvolání funkce s objekt zapisovače svůj vlastní jedinečný protokolu.

Zápis [protokoly trasování aplikací](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), použijte `Console.Out` (vytvoří protokoly, které jsou označeny jako údaje) a `Console.Error` (vytvoří protokoly, které jsou označeny jako chyba). Další možností je použít [trasování nebo TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), který poskytuje podrobné, upozornění, a kritická úrovně kromě údaje a informace o chybě. Protokoly trasování aplikací se zobrazí v souborech protokolů webové aplikace, tabulky Azure, nebo v závislosti na tom, jak nakonfigurujete vaší webové aplikace Azure objektů BLOB Azure. Platí všechny výstup konzoly nejnovější protokoly 100 aplikací také se zobrazí na stránce řídicího panelu pro webové úlohy, není na stránce pro volání funkce.

Výstup konzoly se zobrazí v řídicím panelu pouze v případě, že je aplikace spuštěna v webové úlohy Azure není v případě, že program spuštěn místně nebo v některých dalších prostředí.

Zakázání protokolování řídicí panel pro scénáře vysoké propustnosti. Ve výchozím nastavení sadu SDK zapisuje protokoly do úložiště a tato aktivita může snížit výkon při zpracování velkého počtu zpráv. Chcete-li zakázat protokolování, nastavte řídicí panel připojovací řetězec na hodnotu null, jak je znázorněno v následujícím příkladu.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

Následující příklad ukazuje několik způsobů, jak zápis protokolů:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

V řídicím WebJobs SDK na výstup `TextWriter` objekt zobrazí až když přejdete na stránku pro konkrétní funkce volání a klikněte na tlačítko **přepnutí výstup**:

![Klikněte na odkaz volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Protokoly stránce volání funkce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Na řídicím panelu WebJobs SDK nejnovější 100 řádků konzoly výstup zobrazit až když přejdete na stránku pro webové úlohy (není pro volání funkce) a klikněte na tlačítko **přepnutí výstup**.

![Klikněte na přepínač výstup](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

V nepřetržitá webová úloha, aplikační protokoly objeví v/data/úlohy/průběžné/*{webjobname}*/job_log.txt v systému souborů webové aplikace.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

V Azure blob vzhledu protokoly aplikace takto: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello, world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello, world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello, world!,

A v tabulce Azure `Console.Out` a `Console.Error` protokoly vypadat takto:

![Informace o protokolu v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Protokol chyb v tabulce](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Pokud chcete zařadit vlastní protokoly, přečtěte si téma [v tomto příkladu](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Zpracování chyb a konfiguraci časových limitů
Také zahrnuje WebJobs SDK [časový limit](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) atribut, který můžete použít, aby funkce, která se zruší, pokud se nedokončí ve stanoveném intervalu. A pokud chcete vygenerovat výstrahu v případě příliš mnoho chyb dojít v zadaném časovém období, můžete použít `ErrorTrigger` atribut. Tady je [ErrorTrigger příklad](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Můžete také dynamicky zakázat a povolit funkce řízení, zda se můžete spustit, a to pomocí konfigurace přepínače, které by mohly být nastavení aplikace nebo název proměnné prostředí. Ukázkový kód, najdete `Disable` atribut [WebJobs SDK ukázky úložiště](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s Azure fronty. Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).
