---
title: "vazby funkce externí soubor aaaAzure (Preview) | Microsoft Docs"
description: "Používání vazeb externí soubor v Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 583d9c0b871dc68a79614749ba6ac6711fa820fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Externí soubor funkce vazby Azure (Preview)
Tento článek ukazuje, jak toomanipulate souborů z různých SaaS zprostředkovatelé (například OneDrive, Dropbox) v rámci funkce využívá integrované vazby. Azure functions podporuje aktivaci, vstup a výstup vazby pro externí soubor.

Tuto vazbu vytvoří připojení rozhraní API zprostředkovatelů tooSaaS nebo použije existující API připojení ze skupiny prostředků aplikaci funkce.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Podporované připojení souboru

|konektor|Aktivační události|Vstup|Výstup|
|:-----|:---:|:---:|:---:|
|[Pole](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive pro firmy](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Externí připojení souboru lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Externí soubor aktivovat vazby

Aktivace Azure externí soubor Hello umožňuje monitorovat to vzdálená složka a svůj kód funkce spustit při zjištění změny.

aktivační událost externí soubor Hello používá následující objekty JSON v hello hello `bindings` pole function.json

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder toomonitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of hello following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Vzory názvů
Můžete zadat vzor názvů souborů v hello `path` vlastnost. Složka Hello odkazuje musí existovat v hello SaaS zprostředkovatele.
Příklady:

```json
"path": "input/original-{name}",
```

Tato cesta by vyhledat soubor s názvem *původní File1.txt* v hello *vstupní* složky a hello hodnoty hello `name` proměnné v kódu funkce by `File1.txt`.

Další příklad:

```json
"path": "input/{filename}.{fileextension}",
```

Tato cesta by také vyhledat soubor s názvem *původní File1.txt*a hodnota hello hello `filename` a `fileextension` proměnných v kódu funkce by *původní File1* a  *txt*.

Typ souboru hello souborů můžete omezit pomocí pevná hodnota pro příponu souboru hello. Například:

```json
"path": "samples/{name}.png",
```

V tomto případě pouze *.png* soubory v hello *ukázky* složky Aktivační hello funkce.

Složené závorky jsou speciální znaky v vzory názvů. toospecify názvů souborů, které mají složené závorky v názvu text hello, double hello složené závorky.
Například:

```json
"path": "images/{{20140101}}-{name}",
```

Tato cesta by vyhledat soubor s názvem *{20140101}-soundfile.mp3* v hello *bitové kopie* složku a hello `name` hodnotu proměnné v kódu funkce hello by *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
hello Azure Functions runtime makes sure that no external file trigger function gets called more than once for hello same new or updated file.
It does so by maintaining *file receipts* toodetermine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in hello Azure storage account for your function app
(specified by hello `AzureWebJobsStorage` app setting). A file receipt has hello following information:

* hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* hello folder name
* hello file type ("BlockFile" or "PageFile")
* hello file name
* hello ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

tooforce reprocessing of a file, delete hello file receipt for that file from hello *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Zpracování poškozených souborů
Pokud se aktivace funkce externí soubor nezdaří, Azure Functions opakuje této funkce too5 časů ve výchozím nastavení (včetně prvního pokusu hello) pro daný soubor.
Pokud selžou všechny 5 pokusů, přidá funkce fronta zpráv tooa úložiště s názvem *webjobs. apihubtrigger poison*. uvítací zprávu fronty poškozených souborů je objekt JSON, který obsahuje hello následující vlastnosti:

* FunctionId (ve formátu hello  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)
* Typ souboru
* Název složky
* Název souboru
* Značka ETag (identifikátor verze souboru, například: "0x8D1DC6E70A277EF")


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Aktivační události využití
V jazyce C# funkce, vazby dat toohello vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v [aktivovat JSON](#trigger). Ve funkcích Node.js přistupujete hello vstupní soubor dat pomocí `context.bindings.<name>`.

soubor Hello lze deserializovat do jakéhokoli hello následující typy:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.
  Pokud je deklarovat vlastní vstupní typ (například `FooType`), Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.
* String – vhodné pro data textového souboru.

V jazyce C# funkce můžete také navázat tooany hello následující typy a hello Functions runtime pokusí rekonstruovat hello souborová data pomocí typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Ukázka aktivační události
Předpokládejme, že máte hello následující function.json, který definuje aktivační procedury externí soubor:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

V tématu vzorku hello konkrétní jazyk, který protokoly hello obsah každý soubor, který se přidá složka monitorované toohello.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>Aktivační události využití v jazyce C# #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Aktivační události využití v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Externí soubor vstupní vazby
Vstupní vazba Azure externí soubor Hello umožňuje toouse soubor z externí složky ve vaší funkci.

Hello externí soubor vstupní tooa funkce využívá hello následující objekty JSON v hello `bindings` pole function.json:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Vezměte na vědomí následující hello:

* `path`musí obsahovat název složky hello a název souboru hello. Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` toopoint tooa souboru v hello `samples-workitems` složku s názvem, který odpovídá názvu souboru hello zadaný ve zprávě hello aktivační událost.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Vstupní využití
V jazyce C# funkce, vazby dat toohello vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.
Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v [vstupní vazby](#input). Ve funkcích Node.js přistupujete hello vstupní soubor dat pomocí `context.bindings.<name>`.

soubor Hello lze deserializovat do jakéhokoli hello následující typy:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.
  Pokud je deklarovat vlastní vstupní typ (například `InputType`), Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.
* String – vhodné pro data textového souboru.

V jazyce C# funkce můžete také navázat tooany hello následující typy a hello Functions runtime pokusí rekonstruovat hello souborová data pomocí typu:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Externí soubor výstup vazby
Hello Azure externí soubor výstupu vazby vám umožní externí složka tooan toowrite souborů ve vaší funkci.

externí soubor Hello výstup hello následující objekty JSON v hello používá funkci `bindings` pole function.json:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Vezměte na vědomí následující hello:

* `path`musí obsahovat název složky hello a toowrite název souboru hello k. Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` toopoint tooa souboru v hello `samples-workitems` složku s názvem, který odpovídá názvu souboru hello zadaný ve zprávě hello aktivační událost.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Využití výstupní
V jazyce C# funkce, vazby toohello výstupní soubor s použitím hello s názvem `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je hello datový typ, který má tooserialize hello data do, a `paramName` je hello název můžete zadaný v [výstup vazby](#output). V Node.js funkce získáte přístup pomocí souboru výstup hello `context.bindings.<name>`.

Můžete napsat toohello výstupní soubor pomocí kteréhokoli hello následující typy:

* Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – užitečné pro serializaci JSON.
  Pokud je deklarovat vlastní výstupní typ (například `out OutputType paramName`), Azure Functions pokusí tooserialize objekt do formátu JSON. Pokud hello výstupní parametr hodnotu null při ukončení hello funkce, hello Functions runtime vytvoří soubor jako objekt s hodnotou null.
* String – (`out string paramName`) vhodné pro data textového souboru. Hello Functions runtime vytvoří soubor pouze v případě, že parametr řetězce má jinou hodnotu než null, při ukončení hello funkce.

V jazyce C# funkce můžete také výstup tooany hello následující typy:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Vstup + ukázkový výstup
Předpokládejme, že máte hello následující function.json, který definuje [aktivace fronty úložiště](functions-bindings-storage-queue.md), externí soubor vstup a výstup externí soubor:

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
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

V tématu vzorku hello konkrétní jazyk, který zkopíruje hello vstupní soubor toohello výstupní soubor.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>Použití v jazyce C# #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Použití v Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Další kroky
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
