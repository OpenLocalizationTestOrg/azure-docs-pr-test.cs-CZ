---
title: "aaaHow toouse s hello WebJobs SDK služby Azure blob storage"
description: "Zjistěte, jak toouse Azure blob storage s hello WebJobs SDK. Spustit proces, jakmile se zobrazí nový objekt blob v kontejneru a zpracovat \"poškozených objekty BLOB\"."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.assetid: bf32f919-f7bc-4aaa-916e-461c02f2e26c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: b34ea8cffee7c0475641886150dee521130a3132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-with-hello-webjobs-sdk"></a>Jak toouse Azure blob storage s hello WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# kódu ukázky, zobrazující jak tootrigger proces při vytvoření nebo aktualizaci objektu blob Azure. Ukázky kódu Hello použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

Ukázky kódu, které ukazují, jak objekty BLOB toocreate, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Hello Příručka předpokládá znáte [jak toocreate projekt webové úlohy v sadě Visual Studio se připojení řetězce daný účet úložiště bodu tooyour](websites-dotnet-webjobs-sdk-get-started.md) nebo příliš[více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Jak tootrigger funkce při vytvoření nebo aktualizaci objektu blob
Tato část uvádí, jak toouse hello `BlobTrigger` atribut. 

> [!NOTE]
> Hello WebJobs SDK kontroly protokolu soubory toowatch pro nové nebo změněné objekty BLOB. Tento proces není v reálném čase; funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello. Kromě toho [protokol úložiště jsou vytvořené na "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základ; není zaručeno, že bude možné zaznamenat všechny události. Za určitých podmínek může být načteni protokoly. Pokud hello rychlost a spolehlivost omezení aktivační události objektu blob nejsou přijatelné pro vaši aplikaci, hello doporučená metoda je toocreate zprávu fronty, při vytváření objektu blob hello a použijte hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo Hello `BlobTrigger` atribut hello funkce, která zpracovává hello objektů blob.
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a>Jeden zástupný symbol pro název objektu blob s příponou
Hello následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

konstruktoru atributu Hello přijímá řetězcový parametr, který určuje název kontejneru hello a zástupný symbol pro název objektu blob hello. V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v hello *vstupní* kontejneru hello funkce vytvoří objekt blob s názvem *Blob1.txt* v hello *výstup*  kontejneru. 

Vzor názvů s hello zástupný symbol název objektu blob, můžete určit, jak je znázorněno v hello následující ukázka kódu:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-". Například *původní Blob1.txt* v hello *vstupní* kontejneru se zkopíruje příliš*kopie Blob1.txt* v hello *výstup* kontejneru.

Pokud potřebujete toospecify vzor názvu pro názvy objektů blob, které mají v názvu hello složené závorky, dvakrát hello složené závorky. Například, pokud chcete, aby toofind objektů BLOB v hello *bitové kopie* kontejner, který mají názvy takto:

        {20140101}-soundfile.mp3

To lze používejte pro vaše vzor:

        images/{{20140101}}-{name}

V příkladu hello hello *název* hodnotu zástupného symbolu by *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Zástupné symboly název a příponu samostatných objektů blob
Hello následující změny ukázkový kód hello příponu souboru jako kopíruje objekty BLOB, které se zobrazují v hello *vstupní* kontejneru toohello *výstup* kontejneru. Kód Hello protokoly hello rozšíření hello *vstupní* objektů blob a nastaví hello rozšíření hello *výstup* blob příliš*.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Typy tooblobs můžete vytvořit vazbu
Můžete použít hello `BlobTrigger` atribut hello následující typy:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Jiné typy deserializoval [ICloudBlobStreamBinder](#icbsb) 

Pokud chcete toowork přímo s hello účtu úložiště Azure, můžete také přidat `CloudStorageAccount` podpis metody toohello parametr.

Příklady najdete v tématu hello [blob vazby kódu v úložišti azure. webjobs sdk hello na webu GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Získávání obsahu objektu blob text toostring vazby
Pokud se očekává text objekty BLOB, `BlobTrigger` může být použité tooa `string` parametr. Hello následující ukázka kódu váže tooa objektů blob text `string` parametr s názvem `logMessage`. Funkce Hello používá tento parametr toowrite hello obsah objektu blob toohello hello řídicím panelu WebJobs SDK. 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder
Hello následující příklad kódu používá třídu, která implementuje `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` atribut toobind blob toohello `WebImage` typu.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Hello `WebImage` vazby kódu je součástí `WebImageBinder` třídu odvozenou od `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-hello-blob-path-for-hello-triggering-blob"></a>Získávání hello blob cestu pro hello aktivován objektů blob
název kontejneru hello tooget a název objektu blob objektu blob hello, který má aktivoval hello funkce zahrnují `blobTrigger` řetězcový parametr v podpis funkce hello.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Jak toohandle poison objektů BLOB
Když `BlobTrigger` funkce selže, hello SDK nazve je znovu, v případě, že hello selhání bylo způsobeno přechodné chybě. Pokud hello selhání je způsobena hello obsahu objektu hello blob, funkce hello selže pokaždé, když se pokusí o tooprocess hello blob. Ve výchozím nastavení hello SDK volá funkci too5 časů pro daný objekt blob. V případě selhání páté zkuste hello hello SDK přidá zprávu tooa frontu s názvem *webjobs. blobtrigger poison*.

maximální počet opakovaných pokusů Hello je možné konfigurovat. Hello stejné [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv. 

uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:

* FunctionId (ve formátu hello *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

V hello následující ukázka kódu, hello `CopyBlob` funkce obsahuje kód, který způsobuje, že toofail pokaždé, když je volána. Po hello SDK nazve je pro hello maximální počet opakovaných pokusů, vytvoření zprávu ve frontě hello poškozených objektů blob a zpracovává zprávy hello `LogPoisonBlob` funkce. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

Hello SDK automaticky deserializuje uvítací zprávu JSON. Tady je hello `PoisonBlobMessage` třídy: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Algoritmus dotazování objektů BLOB
Hello WebJobs SDK kontroluje všechny kontejnery určeného `BlobTrigger` atributy při spuštění aplikace. V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a `BlobTrigger` provedení funkce.

toodetect nové nebo změněné objekty BLOB po spuštění aplikace, zaznamená hello SDK pravidelně čte z úložiště objektů blob hello. Hello blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo tak, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před hello odpovídající `BlobTrigger` provádí funkce. 

Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí hello `Blob` atribut. Když hello WebJobs SDK vytvoří nový objekt blob, předá nový objekt blob hello okamžitě odpovídající tooany `BlobTrigger` funkce. Proto pokud máte řetěz blob vstupy a výstupy, hello SDK dokáže zpracovat efektivně. Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale `QueueTrigger` místo `BlobTrigger`.

### <a id="receipts"></a>Potvrzení objektů BLOB
Hello WebJobs SDK je zajištěno, že žádné `BlobTrigger` funkce získá volaná víc než jednou pro hello stejné nové nebo aktualizované objektů blob. Dělá to pomocí zachování *blob potvrzení* v toodetermine pořadí, pokud byla zpracována na verzi daného objektu blob.

Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure hello určeného hello AzureWebJobsStorage připojovací řetězec. Potvrzení o objekt blob má hello následující informace:

* Hello funkce, která byla volána pro objekt blob hello ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")
* název kontejneru Hello
* Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")
* Název objektu blob Hello
* Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

Pokud chcete tooforce opětovné zpracování objektu blob, můžete ručně odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru.

## <a id="queues"></a>Související témata uvedených v článku fronty hello
Informace o tom, jak zpracování objektů blob toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není konkrétní tooblob zpracování, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Související témata popsaná v tomto článku hello následující:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v hello tělo funkce
* Nastavit hello SDK připojovacích řetězců v kódu.
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Konfigurace `MaxDequeueCount` pro zpracování poškozených objektů blob.
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl ukázky kódu, které zobrazují jak objekty BLOB toohandle běžné scénáře pro práci s Azure. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

