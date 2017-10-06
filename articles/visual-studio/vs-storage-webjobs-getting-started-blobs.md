---
title: "aaaGet začít s úložiště objektů blob a Visual Studio připojených služeb (webové úlohy projekty) | Microsoft Docs"
description: "Jak tooget spustit po připojení tooan úložiště Azure pomocí sady Visual Studio pomocí úložiště objektů Blob v projektu webové úlohy připojený služby."
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 324c9376-0225-4092-9825-5d1bd5550058
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 29f2d5e19426d37d815cdf9a1e00abfb1e07ccf6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Začínáme s Azure Blob storage a Visual Studio připojené služeb (webové úlohy projekty)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Přehled
Tento článek obsahuje C# kódu ukázky, zobrazující jak tootrigger proces při vytvoření nebo aktualizaci objektu blob Azure. Ukázky kódu Hello použít hello [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) verze 1.x. Když přidáte projektu úlohy WebJob tooa účet úložiště pomocí hello Visual Studio **přidat připojení služby** dialogu hello odpovídající balíček NuGet úložiště Azure je nainstalovaný a hello příslušné odkazy na rozhraní .NET jsou přidané toohello projekt a připojovací řetězce pro účet úložiště hello se aktualizují v souboru App.config hello.

## <a name="how-tootrigger-a-function-when-a-blob-is-created-or-updated"></a>Jak tootrigger funkce při vytvoření nebo aktualizaci objektu blob
Tato část uvádí, jak toouse hello **BlobTrigger** atribut.

 **Poznámka:** hello WebJobs SDK kontroly protokolu soubory toowatch pro nové nebo změněné objekty BLOB. Tento proces je ze své podstaty pomalé. funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello.  Pokud aplikace potřebuje objekty BLOB tooprocess okamžitě, hello doporučená metoda je toocreate zprávu fronty, při vytváření objektu blob hello a použijte hello [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo hello **BlobTrigger** atribut hello funkce, která zpracovává hello objektů blob.

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

## <a name="types-that-you-can-bind-tooblobs"></a>Typy tooblobs můžete vytvořit vazbu
Můžete použít hello **BlobTrigger** atribut hello následující typy:

* **řetězec**
* **TextReader**
* **Datový proud**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Jiné typy deserializoval [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder)

Pokud chcete toowork přímo s hello účtu úložiště Azure, můžete také přidat **CloudStorageAccount** podpis metody toohello parametr.

## <a name="getting-text-blob-content-by-binding-toostring"></a>Získávání obsahu objektu blob text toostring vazby
Pokud se očekává text objekty BLOB, **BlobTrigger** může být použité tooa **řetězec** parametr. Hello následující ukázka kódu váže tooa objektů blob text **řetězec** parametr s názvem **logMessage**. Funkce Hello používá tento parametr toowrite hello obsah objektu blob toohello hello řídicím panelu WebJobs SDK.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder
Hello následující příklad kódu používá třídu, která implementuje **ICloudBlobStreamBinder** tooenable hello **BlobTrigger** atribut toobind blob toohello **WebImage** typu.

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

Hello **WebImage** vazby kódu je součástí **WebImageBinder** třídu odvozenou od **ICloudBlobStreamBinder**.

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

## <a name="how-toohandle-poison-blobs"></a>Jak toohandle poison objektů BLOB
Když **BlobTrigger** funkce selže, hello SDK nazve je znovu, v případě, že hello selhání bylo způsobeno přechodné chybě. Pokud hello selhání je způsobena hello obsahu objektu hello blob, funkce hello selže pokaždé, když se pokusí o tooprocess hello blob. Ve výchozím nastavení hello SDK volá funkci too5 časů pro daný objekt blob. V případě selhání páté zkuste hello hello SDK přidá zprávu tooa frontu s názvem *webjobs. blobtrigger poison*.

maximální počet opakovaných pokusů Hello je možné konfigurovat. Hello stejné [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv.

uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:

* FunctionId (ve formátu hello *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

V hello následující ukázka kódu, hello **CopyBlob** funkce obsahuje kód, který způsobuje, že toofail pokaždé, když je volána. Po hello SDK nazve je pro hello maximální počet opakovaných pokusů, vytvoření zprávu ve frontě hello poškozených objektů blob a zpracovává zprávy hello **LogPoisonBlob** funkce.

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

Hello SDK automaticky deserializuje uvítací zprávu JSON. Tady je hello **PoisonBlobMessage** třídy:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Algoritmus dotazování objektů BLOB
Hello WebJobs SDK kontroluje všechny kontejnery určeného **BlobTrigger** atributy při spuštění aplikace. V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a **BlobTrigger** provedení funkce.

toodetect nové nebo změněné objekty BLOB po spuštění aplikace, zaznamená hello SDK pravidelně čte z úložiště objektů blob hello. Hello blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo tak, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před hello odpovídající **BlobTrigger** provádí funkce.

Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí hello **Blob** atribut. Když hello WebJobs SDK vytvoří nový objekt blob, předá nový objekt blob hello okamžitě odpovídající tooany **BlobTrigger** funkce. Proto pokud máte řetěz blob vstupy a výstupy, hello SDK dokáže zpracovat efektivně. Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale **QueueTrigger** místo **BlobTrigger**.

### <a name="blob-receipts"></a>Potvrzení objektů BLOB
Hello WebJobs SDK je zajištěno, že žádné **BlobTrigger** funkce získá volaná víc než jednou pro hello stejné nové nebo aktualizované objektů blob. Dělá to pomocí zachování *blob potvrzení* v toodetermine pořadí, pokud byla zpracována na verzi daného objektu blob.

Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure hello určeného hello AzureWebJobsStorage připojovací řetězec. Potvrzení o objekt blob má hello následující informace:

* Hello funkce, která byla volána pro objekt blob hello ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")
* název kontejneru Hello
* Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")
* Název objektu blob Hello
* Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

Pokud chcete tooforce opětovné zpracování objektu blob, můžete ručně odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru.

## <a name="related-topics-covered-by-hello-queues-article"></a>Související témata uvedených v článku fronty hello
Informace o tom, jak zpracování objektů blob toohandle aktivaci zprávu fronty, a to pro webové úlohy scénáře SDK není konkrétní tooblob zpracování, najdete v části [jak toouse Azure fronty úložiště s hello WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Související témata popsaná v tomto článku hello následující:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v hello tělo funkce
* Nastavit hello SDK připojovacích řetězců v kódu.
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Konfigurace **MaxDequeueCount** pro zpracování poškozených objektů blob.
* Ruční aktivaci funkce
* Zápis protokolů

## <a name="next-steps"></a>Další kroky
Tento článek poskytl ukázky kódu, které zobrazují jak objekty BLOB toohandle běžné scénáře pro práci s Azure. Další informace o tom, jak toouse Azure WebJobs a hello WebJobs SDK najdete v části [zdrojů dokumentace Azure WebJobs](http://go.microsoft.com/fwlink/?linkid=390226).

