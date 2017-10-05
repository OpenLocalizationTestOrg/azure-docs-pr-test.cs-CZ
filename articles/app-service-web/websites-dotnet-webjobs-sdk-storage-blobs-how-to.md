---
title: "Jak používat Úložiště objektů blob v Azure pomocí WebJobs SDK"
description: "Naučte se používat úložiště objektů blob v Azure pomocí WebJobs SDK. Spustit proces, jakmile se zobrazí nový objekt blob v kontejneru a zpracovat \"poškozených objekty BLOB\"."
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
ms.openlocfilehash: e0a792ccdf8097d5cde254d6d4690a64838378ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Jak používat Úložiště objektů blob v Azure pomocí WebJobs SDK
## <a name="overview"></a>Přehled
Tato příručka obsahuje C# ukázek kódu, které ukazují, jak spustit proces, při vytvoření nebo aktualizaci objektu blob Azure. Kód – ukázky použití [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verze 1.x.

Ukázky kódu, které ukazují, jak vytvořit objekty BLOB, najdete v části [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

V Průvodci se předpokládá, víte, [postup vytvoření projektu úlohy WebJob v sadě Visual Studio s připojením řetězce, které odkazují na účtu úložiště](websites-dotnet-webjobs-sdk-get-started.md) nebo [více účtů úložiště](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Jak aktivovat funkci, když se vytvoří nebo aktualizuje objekt blob
Tato část ukazuje způsob použití `BlobTrigger` atribut. 

> [!NOTE]
> Sada WebJobs SDK kontroluje soubory protokolů, které chcete sledovat pro nové nebo změněné objekty BLOB. Tento proces není v reálném čase; funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob. Kromě toho [protokol úložiště jsou vytvořené na "maximální úsilí"](https://msdn.microsoft.com/library/azure/hh343262.aspx) základ; není zaručeno, že bude možné zaznamenat všechny události. Za určitých podmínek může být načteni protokoly. Pokud omezení rychlosti a spolehlivost služby aktivačních událostí objekt blob nejsou přijatelné pro vaši aplikaci, doporučuje se vytvořit zprávu fronty, při vytváření objektu blob a použít [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) atribut místo `BlobTrigger` atribut na funkce, která zpracovává objektu blob.
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a>Jeden zástupný symbol pro název objektu blob s příponou
Následující ukázka kódu zkopíruje text objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Konstruktoru atributu přijímá řetězcový parametr, který určuje název kontejneru a zástupný symbol pro název objektu blob. V tomto příkladu, pokud objekt blob s názvem *Blob1.txt* je vytvořen v *vstupní* kontejneru, funkce vytvoří objekt blob s názvem *Blob1.txt* v *výstup* kontejneru. 

Vzor názvů s zástupný symbol název objektu blob, můžete určit, jak znázorňuje následující ukázka kódu:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Tento kód zkopíruje pouze objekty BLOB, které mají názvy počínaje "původní-". Například *původní Blob1.txt* v *vstupní* kontejneru se zkopíruje do *kopie Blob1.txt* v *výstup* kontejneru.

Pokud budete muset zadat vzor názvu pro názvy objektů blob, které mají v názvu složené závorky, dvakrát složené závorky. Například, pokud chcete najít objektů BLOB v *bitové kopie* kontejner, který mají názvy takto:

        {20140101}-soundfile.mp3

To lze používejte pro vaše vzor:

        images/{{20140101}}-{name}

V příkladu *název* hodnotu zástupného symbolu by *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Zástupné symboly název a příponu samostatných objektů blob
Následující ukázka kódu změní přípona souboru, protože kopíruje objekty BLOB, které se zobrazují v *vstupní* kontejner, aby *výstup* kontejneru. Kód protokoly rozšíření *vstupní* objektů blob a nastaví rozšíření *výstup* objektu blob do *.txt*.

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

## <a id="types"></a>Typy, které můžete vázat na objekty BLOB
Můžete použít `BlobTrigger` atribut u následujících typů:

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

Pokud chcete pracovat přímo s účtu úložiště Azure, můžete také přidat `CloudStorageAccount` parametru podpis metody.

Příklady najdete v tématu [blob vazby kódu v úložišti azure. webjobs sdk na webu GitHub.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Získávání obsahu objektu blob text vazby na řetězec
Pokud se očekává text objekty BLOB, `BlobTrigger` lze použít pro `string` parametr. Následující ukázka kódu váže objekt blob text `string` parametr s názvem `logMessage`. Funkce pomocí tohoto parametru zapsat obsah objektu blob na řídicím panelu WebJobs SDK. 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Získávání serializovat obsah objektu blob pomocí ICloudBlobStreamBinder
Následující příklad kódu používá třídu, která implementuje `ICloudBlobStreamBinder` povolit `BlobTrigger` atributu bude vazba provedena objekt blob `WebImage` typu.

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

`WebImage` Vazby kódu je součástí `WebImageBinder` třídu odvozenou od `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Získání cesty objektu blob pro spouštěcí objektů blob
Chcete-li získat název kontejneru a název objektu blob objektu blob, který má aktivoval funkce, zahrnují `blobTrigger` řetězcový parametr v podpis funkce.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Postupy: zpracování poškozených objektů BLOB
Když `BlobTrigger` funkce selže, sadu SDK nazve je znovu, v případě selhání bylo způsobeno přechodné chybě. Pokud selhání je způsobena obsah objektu blob, funkce selže pokaždé, když se pokusí zpracovat objektu blob. Ve výchozím nastavení sadu SDK volá funkci až 5 výskyty pro daný objekt blob. Pokud pátý zkuste selže, sadu SDK přidá zprávu do fronty s názvem *webjobs. blobtrigger poison*.

Maximální počet opakovaných pokusů je možné konfigurovat. Stejné [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) nastavení se používá pro zpracování poškozených objektů blob a zpracování poškozených fronty zpráv. 

Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:

* FunctionId (ve formátu *{název webové úlohy}*. Funkce. *{Název funkce}*, například: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

V následující ukázce kódu `CopyBlob` funkce obsahuje kód, který způsobuje, že selžou pokaždé, když je volána. Po nazve je sada SDK pro maximální počet opakovaných pokusů, vytvoření zprávu ve frontě poškozených objektů blob a zpracovává zprávy `LogPoisonBlob` funkce. 

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

Sada SDK automaticky deserializuje zpráva JSON. Tady je `PoisonBlobMessage` třídy: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Algoritmus dotazování objektů BLOB
Sada WebJobs SDK kontroluje všechny kontejnery určeného `BlobTrigger` atributy při spuštění aplikace. V účtu úložiště velké tato kontrola může trvat nějakou dobu, proto chvíli může být před nebyly nalezeny nové objekty BLOB a `BlobTrigger` provedení funkce.

Ke zjištění nové nebo změněné objekty BLOB po spuštění aplikace, sady SDK pravidelně čte z protokolů úložiště objektů blob. Objekt blob protokoly jsou do vyrovnávací paměti a pouze získat fyzicky zapisují každých 10 minut, nebo Ano, tedy s sebou může být důležité zpoždění po objekt blob se vytvoří nebo aktualizuje před odpovídající `BlobTrigger` provádí funkce. 

Dojde k výjimce pro objekty BLOB, které vytvoříte pomocí `Blob` atribut. Když WebJobs SDK vytvoří nový objekt blob, pak předá nový objekt blob okamžitě žádné odpovídající `BlobTrigger` funkce. Proto pokud máte řetěz blob vstupy a výstupy, sadu SDK dokáže zpracovat efektivně. Pokud chcete s nízkou latencí systémem objektu blob služby zpracování funkce pro objekty BLOB, které jsou vytvořeny nebo aktualizovány jiným způsobem, doporučujeme používat, ale `QueueTrigger` místo `BlobTrigger`.

### <a id="receipts"></a>Potvrzení objektů BLOB
Sada WebJobs SDK je zajištěno, že žádné `BlobTrigger` funkce pro stejný objekt blob nové nebo aktualizované volala více než jednou. Dělá to pomocí zachování *blob potvrzení* aby bylo možné zjistit, pokud byla zpracována na verzi daného objektu blob.

Potvrzení BLOB jsou uloženy v kontejneru nazvaném *azure. webové úlohy hostitelů* v účtu úložiště Azure určeného AzureWebJobsStorage připojovací řetězec. Potvrzení o objektu blob obsahuje následující informace:

* Funkce, která byla volána pro tento objekt blob ("*{název webové úlohy}*. Funkce. *{Název funkce}*", například:"WebJob1.Functions.CopyBlob")
* Název kontejneru
* Typ objektu blob ("BlockBlob" nebo "PageBlob")
* Název objektu blob
* Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

Pokud chcete vynutit opětovné zpracování objektu blob, můžete ručně odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru.

## <a id="queues"></a>Související témata uvedených v článku fronty
Informace o způsobu zpracování objektů blob zpracování aktivovány zprávu fronty, nebo WebJobs SDK scénáře, které nejsou specifické do objektu blob zpracování, projděte si téma [používání úložiště fronty Azure pomocí WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Související témata popsaná v tomto článku zahrnují následující:

* Asynchronní funkce
* Více instancí
* Řádné vypnutí
* Použití atributů WebJobs SDK v tělo funkce
* Nastavte SDK připojovacích řetězců v kódu.
* Nastavení hodnot pro WebJobs SDK parametry konstruktor v kódu
* Konfigurace `MaxDequeueCount` pro zpracování poškozených objektů blob.
* Ruční aktivaci funkce
* Zápis protokolů

## <a id="nextsteps"></a> Další kroky
Tato příručka poskytl ukázek kódu, které ukazují, jak zpracovat běžné scénáře pro práci s objekty BLOB Azure. Další informace o tom, jak používat Azure WebJobs a WebJobs SDK najdete v tématu [Azure WebJobs doporučené prostředky](http://go.microsoft.com/fwlink/?linkid=390226).

