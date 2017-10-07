---
title: "vazby funkce úložiště objektů Blob aaaAzure | Microsoft Docs"
description: Pochopit, jak se aktivuje toouse Azure Storage a vazeb v Azure Functions.
services: functions
documentationcenter: na
author: lindydonna
manager: erikre
editor: 
tags: 
keywords: "Funkce Azure, funkce zpracování událostí, dynamické výpočetní architektura bez serveru"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/25/2017
ms.author: glenga
ms.openlocfilehash: cef44bd2154d0b97cca9220b6c5024a5b620c80d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Vazby úložiště Azure Blob funkce
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tento článek vysvětluje, jak tooconfigure a práce s Azure Blob storage vazeb v Azure Functions. Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure. Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> A [pouze účet úložiště blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) není podporován. Objekt BLOB úložiště triggerů a vazeb se vyžaduje účet úložiště pro obecné účely. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>Objekt BLOB úložiště triggerů a vazeb

Pomocí aktivační událost úložiště objektů Blob v Azure hello, kódu funkce je volána, když je zjištěna nová nebo aktualizovaná objektu blob. Hello obsahu objektu blob jsou uvedeny jako vstupní toohello funkce.

Zadejte aktivační události objektu blob úložiště pomocí hello **integrací** kartě hello funkce portálu. Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

Objekt BLOB vstup a výstup vazby jsou definovány pomocí `blob` jako typ vazby hello:

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* Hello `path` podporuje vlastnost vazby výrazy a parametry filtrování. V tématu [název vzory](#pattern).
* Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště. V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.

> [!NOTE]
> Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění tooa při zpracování nové objekty BLOB po aplikaci funkce přešel nečinnosti. Po hello funkce aplikace běží, objekty BLOB jsou zpracovávány okamžitě. Tento počáteční tooavoid zpoždění, zvažte jednu hello následující možnosti:
> - Plán služby App Service použijte s povolenou funkci Always On.
> - Použijte jiný mechanismus tootrigger hello blob zpracování, např. zprávu fronty, který obsahuje název objektu blob hello. Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Vzory názvů
Můžete zadat vzor názvu objektu blob v hello `path` vlastnosti, která může být výraz filtru nebo vazby. V tématu [vazby výrazy a vzory](functions-triggers-bindings.md#binding-expressions-and-patterns).

Tooblobs toofilter, které začínají řetězcem hello "původní", například použít následující definice hello. Tato cesta vyhledá objekt blob s názvem *původní Blob1.txt* v hello *vstupní* kontejneru a hodnota hello hello `name` proměnná v kódu funkce `Blob1`.

```json
"path": "input/original-{name}",
```

Název souboru objektů blob toohello toobind a příponu samostatně, použijte dva vzory. Tato cesta také vyhledá objekt blob s názvem *původní Blob1.txt*a hodnota hello hello `blobname` a `blobextension` proměnné v kódu funkce jsou *původní Blob1* a *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Typ souboru hello objektů BLOB můžete omezit pomocí pevná hodnota pro příponu souboru hello. Například tootrigger pouze u souborů, PNG, hello použijte následující vzoru:

```json
"path": "samples/{name}.png",
```

Složené závorky jsou speciální znaky v vzory názvů. názvy objektů blob toospecify, které mají v názvu hello složené závorky, můžete vyhnuli složené závorky hello pomocí dvou složené závorky. Hello následující příklad vyhledá objekt blob s názvem *{20140101}-soundfile.mp3* v hello *bitové kopie* kontejneru a hello `name` hodnotu proměnné v kódu funkce hello je  *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Aktivační událost metadat

aktivační události objektu blob Hello nabízí několik vlastností metadat. Tyto vlastnosti lze použít jako součást výrazů vazby v jiných vazby nebo jako parametry v kódu. Tyto hodnoty mají hello stejnou sémantiku jako [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. Zadejte `string`. Cesta blobu spouštěcí Hello
- **Identifikátor URI**. Zadejte `System.Uri`. identifikátor URI objektu Hello blob pro primární umístění hello.
- **Vlastnosti**. Zadejte `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`. Hello vlastnosti systému objektu blob.
- **Metadata**. Zadejte `IDictionary<string,string>`. Hello metadata definovaná uživatelem pro objekt blob hello.

<a name="receipts"></a>

### <a name="blob-receipts"></a>Potvrzení objektů BLOB
Azure Functions Hello runtime zajistí, že žádná funkce aktivační události objektu blob volala více než jednou pro hello stejné nové nebo aktualizované objektů blob. toodetermine, pokud byla zpracována na verzi daného objektu blob, udržuje *blob potvrzení*.

Azure funkce úložiště objektů blob v kontejneru nazvaném potvrzení *azure. webové úlohy hostitelů* v účtu úložiště Azure pro aplikaci funkce hello (definované nastavení aplikace hello `AzureWebJobsStorage`). Potvrzení o objekt blob má hello následující informace:

* Hello aktivuje funkce ("*&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*", například:"MyFunctionApp.Functions.CopyBlob")
* název kontejneru Hello
* Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")
* Název objektu blob Hello
* Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

tooforce opětovné zpracování objektu blob, odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru ručně.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Zpracování poškozených objektů BLOB
Pokud funkci aktivační události objektu blob se nezdaří pro daný objekt blob Azure Functions opakovaných pokusů, které funkce celkem 5krát ve výchozím nastavení. 

Pokud selžou všechny 5 pokusů, Azure Functions přidá fronta zpráv tooa úložiště s názvem *webjobs. blobtrigger poison*. uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:

* FunctionId (ve formátu hello  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)
* BlobType ("BlockBlob" nebo "PageBlob")
* ContainerName
* BlobName
* Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")

### <a name="blob-polling-for-large-containers"></a>Dotazování pro velké kontejnery objektů BLOB
Pokud kontejner objektů blob hello monitorovaných obsahuje více než 10 000 objektů BLOB, hello toowatch soubory funkcí runtime kontroly protokolu pro nové nebo změněné objekty BLOB. Tento proces není v reálném čase. Funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello. Kromě toho [protokol úložiště jsou vytvořené na "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) intervalech. Není zaručeno, že jsou zachyceny všechny události. Za určitých podmínek může být načteni protokoly. Pokud budete potřebovat rychlejší a spolehlivější blob zpracování, zvažte vytvoření [zprávu fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) při vytváření objektu blob hello. Poté použijte [aktivační událost fronty](functions-bindings-storage-queue.md) namísto objektu blob hello tooprocess aktivační události objektu blob.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Pomocí aktivační události objektu blob a vstupní vazby
V rozhraní .NET funkce, přístup k datům objektu blob hello pomocí parametru metody, jako třeba `Stream paramName`. Zde `paramName` je hodnota hello jste zadali v hello [konfigurace aktivační události](#trigger). Ve funkcích Node.js hello přístup vstup, objektů blob dat pomocí `context.bindings.<name>`.

V rozhraní .NET můžete vázat tooany typů hello hello níže uvedeného seznamu. Pokud slouží jako vstupní vazby, přičemž některé z těchto typů vyžadují `inout` vazby směr v *function.json*. Tomto směru nepodporuje hello standardního editoru, je nutné použít hello advanced editor.

* `TextReader`
* `Stream`
* `ICloudBlob`(vyžaduje "inout" vazba směr)
* `CloudBlockBlob`(vyžaduje "inout" vazba směr)
* `CloudPageBlob`(vyžaduje "inout" vazba směr)
* `CloudAppendBlob`(vyžaduje "inout" vazba směr)

Pokud se očekává text objekty BLOB, můžete také navázat tooa .NET `string` typu. Toto nastavení se doporučuje jenom Pokud velikost objektu blob hello malé, jako jsou obsahu objektu blob celý hello načten do paměti. Obecně je vhodnější toouse `Stream` nebo `CloudBlockBlob` typu.

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte hello následující function.json, který definuje aktivační událost úložiště objektů blob:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

V tématu vzorku hello konkrétní jazyk, který je protokoly hello obsah každý objekt blob, který se přidá toohello monitorovaných kontejneru.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>Příklady aktivační události objektu BLOB v jazyce C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding tooa CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Příklad aktivační události v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Pomocí objektu blob výstup vazby

V rozhraní .NET funkce, měli byste buď použijte `out string` parametr v podpis funkce nebo používají jeden z typů hello v následujícím seznamu hello. V Node.js funkce získáte přístup pomocí objektu blob výstup hello `context.bindings.<name>`.

V rozhraní .NET funkce výstup můžete tooany hello následující typy:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Aktivační událost fronty s blob vstupní a výstupní ukázka
Předpokládejme, že máte hello následující function.json, který definuje [aktivační událost Queue Storage](functions-bindings-storage-queue.md)úložiště objektů blob vstup a výstup úložiště objektů blob. Všimněte si použití hello hello `queueTrigger` vlastnost metadat. v objektu blob hello vstup a výstup `path` vlastnosti:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

V tématu vzorku hello konkrétní jazyk, který zkopíruje hello vstupního objektu blob toohello výstupního objektu blob.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>Příklad vazby objektů BLOB v jazyce C# #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>Příklad vazby objektů BLOB v Node.js

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

