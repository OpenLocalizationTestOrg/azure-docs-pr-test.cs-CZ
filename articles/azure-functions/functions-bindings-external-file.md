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
# <a name="azure-functions-external-file-bindings-preview"></a><span data-ttu-id="87d65-103">Externí soubor funkce vazby Azure (Preview)</span><span class="sxs-lookup"><span data-stu-id="87d65-103">Azure Functions External File bindings (Preview)</span></span>
<span data-ttu-id="87d65-104">Tento článek ukazuje, jak toomanipulate souborů z různých SaaS zprostředkovatelé (například OneDrive, Dropbox) v rámci funkce využívá integrované vazby.</span><span class="sxs-lookup"><span data-stu-id="87d65-104">This article shows how toomanipulate files from different SaaS providers (e.g. OneDrive, Dropbox) within your function utilizing built-in bindings.</span></span> <span data-ttu-id="87d65-105">Azure functions podporuje aktivaci, vstup a výstup vazby pro externí soubor.</span><span class="sxs-lookup"><span data-stu-id="87d65-105">Azure functions supports trigger, input, and output bindings for external file.</span></span>

<span data-ttu-id="87d65-106">Tuto vazbu vytvoří připojení rozhraní API zprostředkovatelů tooSaaS nebo použije existující API připojení ze skupiny prostředků aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="87d65-106">This binding creates API connections tooSaaS providers, or uses existing API connections from your Function App's resource group.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a><span data-ttu-id="87d65-107">Podporované připojení souboru</span><span class="sxs-lookup"><span data-stu-id="87d65-107">Supported File connections</span></span>

|<span data-ttu-id="87d65-108">konektor</span><span class="sxs-lookup"><span data-stu-id="87d65-108">Connector</span></span>|<span data-ttu-id="87d65-109">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="87d65-109">Trigger</span></span>|<span data-ttu-id="87d65-110">Vstup</span><span class="sxs-lookup"><span data-stu-id="87d65-110">Input</span></span>|<span data-ttu-id="87d65-111">Výstup</span><span class="sxs-lookup"><span data-stu-id="87d65-111">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="87d65-112">Pole</span><span class="sxs-lookup"><span data-stu-id="87d65-112">Box</span></span>](https://www.box.com)|<span data-ttu-id="87d65-113">x</span><span class="sxs-lookup"><span data-stu-id="87d65-113">x</span></span>|<span data-ttu-id="87d65-114">x</span><span class="sxs-lookup"><span data-stu-id="87d65-114">x</span></span>|<span data-ttu-id="87d65-115">x</span><span class="sxs-lookup"><span data-stu-id="87d65-115">x</span></span>
|[<span data-ttu-id="87d65-116">Dropbox</span><span class="sxs-lookup"><span data-stu-id="87d65-116">Dropbox</span></span>](https://www.dropbox.com)|<span data-ttu-id="87d65-117">x</span><span class="sxs-lookup"><span data-stu-id="87d65-117">x</span></span>|<span data-ttu-id="87d65-118">x</span><span class="sxs-lookup"><span data-stu-id="87d65-118">x</span></span>|<span data-ttu-id="87d65-119">x</span><span class="sxs-lookup"><span data-stu-id="87d65-119">x</span></span>
|[<span data-ttu-id="87d65-120">FTP</span><span class="sxs-lookup"><span data-stu-id="87d65-120">FTP</span></span>](https://docs.microsoft.com/azure/app-service-web/app-service-deploy-ftp)|<span data-ttu-id="87d65-121">x</span><span class="sxs-lookup"><span data-stu-id="87d65-121">x</span></span>|<span data-ttu-id="87d65-122">x</span><span class="sxs-lookup"><span data-stu-id="87d65-122">x</span></span>|<span data-ttu-id="87d65-123">x</span><span class="sxs-lookup"><span data-stu-id="87d65-123">x</span></span>
|[<span data-ttu-id="87d65-124">OneDrive</span><span class="sxs-lookup"><span data-stu-id="87d65-124">OneDrive</span></span>](https://onedrive.live.com)|<span data-ttu-id="87d65-125">x</span><span class="sxs-lookup"><span data-stu-id="87d65-125">x</span></span>|<span data-ttu-id="87d65-126">x</span><span class="sxs-lookup"><span data-stu-id="87d65-126">x</span></span>|<span data-ttu-id="87d65-127">x</span><span class="sxs-lookup"><span data-stu-id="87d65-127">x</span></span>
|[<span data-ttu-id="87d65-128">OneDrive pro firmy</span><span class="sxs-lookup"><span data-stu-id="87d65-128">OneDrive for Business</span></span>](https://onedrive.live.com/about/business/)|<span data-ttu-id="87d65-129">x</span><span class="sxs-lookup"><span data-stu-id="87d65-129">x</span></span>|<span data-ttu-id="87d65-130">x</span><span class="sxs-lookup"><span data-stu-id="87d65-130">x</span></span>|<span data-ttu-id="87d65-131">x</span><span class="sxs-lookup"><span data-stu-id="87d65-131">x</span></span>
|[<span data-ttu-id="87d65-132">SFTP</span><span class="sxs-lookup"><span data-stu-id="87d65-132">SFTP</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|<span data-ttu-id="87d65-133">x</span><span class="sxs-lookup"><span data-stu-id="87d65-133">x</span></span>|<span data-ttu-id="87d65-134">x</span><span class="sxs-lookup"><span data-stu-id="87d65-134">x</span></span>|<span data-ttu-id="87d65-135">x</span><span class="sxs-lookup"><span data-stu-id="87d65-135">x</span></span>
|[<span data-ttu-id="87d65-136">Google Drive</span><span class="sxs-lookup"><span data-stu-id="87d65-136">Google Drive</span></span>](https://www.google.com/drive/)||<span data-ttu-id="87d65-137">x</span><span class="sxs-lookup"><span data-stu-id="87d65-137">x</span></span>|<span data-ttu-id="87d65-138">x</span><span class="sxs-lookup"><span data-stu-id="87d65-138">x</span></span>|

> [!NOTE]
> <span data-ttu-id="87d65-139">Externí připojení souboru lze použít také v [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="87d65-139">External File connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

## <a name="external-file-trigger-binding"></a><span data-ttu-id="87d65-140">Externí soubor aktivovat vazby</span><span class="sxs-lookup"><span data-stu-id="87d65-140">External File trigger binding</span></span>

<span data-ttu-id="87d65-141">Aktivace Azure externí soubor Hello umožňuje monitorovat to vzdálená složka a svůj kód funkce spustit při zjištění změny.</span><span class="sxs-lookup"><span data-stu-id="87d65-141">hello Azure external file trigger lets you monitor a remote folder and run your function code when changes are detected.</span></span>

<span data-ttu-id="87d65-142">aktivační událost externí soubor Hello používá následující objekty JSON v hello hello `bindings` pole function.json</span><span class="sxs-lookup"><span data-stu-id="87d65-142">hello external file trigger uses hello following JSON objects in hello `bindings` array of function.json</span></span>

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

### <a name="name-patterns"></a><span data-ttu-id="87d65-143">Vzory názvů</span><span class="sxs-lookup"><span data-stu-id="87d65-143">Name patterns</span></span>
<span data-ttu-id="87d65-144">Můžete zadat vzor názvů souborů v hello `path` vlastnost.</span><span class="sxs-lookup"><span data-stu-id="87d65-144">You can specify a file name pattern in hello `path` property.</span></span> <span data-ttu-id="87d65-145">Složka Hello odkazuje musí existovat v hello SaaS zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="87d65-145">hello folder referenced must exist in hello SaaS provider.</span></span>
<span data-ttu-id="87d65-146">Příklady:</span><span class="sxs-lookup"><span data-stu-id="87d65-146">Examples:</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="87d65-147">Tato cesta by vyhledat soubor s názvem *původní File1.txt* v hello *vstupní* složky a hello hodnoty hello `name` proměnné v kódu funkce by `File1.txt`.</span><span class="sxs-lookup"><span data-stu-id="87d65-147">This path would find a file named *original-File1.txt* in hello *input* folder, and hello value of hello `name` variable in function code would be `File1.txt`.</span></span>

<span data-ttu-id="87d65-148">Další příklad:</span><span class="sxs-lookup"><span data-stu-id="87d65-148">Another example:</span></span>

```json
"path": "input/{filename}.{fileextension}",
```

<span data-ttu-id="87d65-149">Tato cesta by také vyhledat soubor s názvem *původní File1.txt*a hodnota hello hello `filename` a `fileextension` proměnných v kódu funkce by *původní File1* a  *txt*.</span><span class="sxs-lookup"><span data-stu-id="87d65-149">This path would also find a file named *original-File1.txt*, and hello value of hello `filename` and `fileextension` variables in function code would be *original-File1* and *txt*.</span></span>

<span data-ttu-id="87d65-150">Typ souboru hello souborů můžete omezit pomocí pevná hodnota pro příponu souboru hello.</span><span class="sxs-lookup"><span data-stu-id="87d65-150">You can restrict hello file type of files by using a fixed value for hello file extension.</span></span> <span data-ttu-id="87d65-151">Například:</span><span class="sxs-lookup"><span data-stu-id="87d65-151">For example:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="87d65-152">V tomto případě pouze *.png* soubory v hello *ukázky* složky Aktivační hello funkce.</span><span class="sxs-lookup"><span data-stu-id="87d65-152">In this case, only *.png* files in hello *samples* folder trigger hello function.</span></span>

<span data-ttu-id="87d65-153">Složené závorky jsou speciální znaky v vzory názvů.</span><span class="sxs-lookup"><span data-stu-id="87d65-153">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="87d65-154">toospecify názvů souborů, které mají složené závorky v názvu text hello, double hello složené závorky.</span><span class="sxs-lookup"><span data-stu-id="87d65-154">toospecify file names that have curly braces in hello name, double hello curly braces.</span></span>
<span data-ttu-id="87d65-155">Například:</span><span class="sxs-lookup"><span data-stu-id="87d65-155">For example:</span></span>

```json
"path": "images/{{20140101}}-{name}",
```

<span data-ttu-id="87d65-156">Tato cesta by vyhledat soubor s názvem *{20140101}-soundfile.mp3* v hello *bitové kopie* složku a hello `name` hodnotu proměnné v kódu funkce hello by *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="87d65-156">This path would find a file named *{20140101}-soundfile.mp3* in hello *images* folder, and hello `name` variable value in hello function code would be *soundfile.mp3*.</span></span>

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

### <a name="handling-poison-files"></a><span data-ttu-id="87d65-157">Zpracování poškozených souborů</span><span class="sxs-lookup"><span data-stu-id="87d65-157">Handling poison files</span></span>
<span data-ttu-id="87d65-158">Pokud se aktivace funkce externí soubor nezdaří, Azure Functions opakuje této funkce too5 časů ve výchozím nastavení (včetně prvního pokusu hello) pro daný soubor.</span><span class="sxs-lookup"><span data-stu-id="87d65-158">When an external file trigger function fails, Azure Functions retries that function up too5 times by default (including hello first try) for a given file.</span></span>
<span data-ttu-id="87d65-159">Pokud selžou všechny 5 pokusů, přidá funkce fronta zpráv tooa úložiště s názvem *webjobs. apihubtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="87d65-159">If all 5 tries fail, Functions adds a message tooa Storage queue named *webjobs-apihubtrigger-poison*.</span></span> <span data-ttu-id="87d65-160">uvítací zprávu fronty poškozených souborů je objekt JSON, který obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="87d65-160">hello queue message for poison files is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="87d65-161">FunctionId (ve formátu hello  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)</span><span class="sxs-lookup"><span data-stu-id="87d65-161">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="87d65-162">Typ souboru</span><span class="sxs-lookup"><span data-stu-id="87d65-162">FileType</span></span>
* <span data-ttu-id="87d65-163">Název složky</span><span class="sxs-lookup"><span data-stu-id="87d65-163">FolderName</span></span>
* <span data-ttu-id="87d65-164">Název souboru</span><span class="sxs-lookup"><span data-stu-id="87d65-164">FileName</span></span>
* <span data-ttu-id="87d65-165">Značka ETag (identifikátor verze souboru, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="87d65-165">ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")</span></span>


<a name="triggerusage"></a>

## <a name="trigger-usage"></a><span data-ttu-id="87d65-166">Aktivační události využití</span><span class="sxs-lookup"><span data-stu-id="87d65-166">Trigger usage</span></span>
<span data-ttu-id="87d65-167">V jazyce C# funkce, vazby dat toohello vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="87d65-167">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="87d65-168">Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v [aktivovat JSON](#trigger).</span><span class="sxs-lookup"><span data-stu-id="87d65-168">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [trigger JSON](#trigger).</span></span> <span data-ttu-id="87d65-169">Ve funkcích Node.js přistupujete hello vstupní soubor dat pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="87d65-169">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="87d65-170">soubor Hello lze deserializovat do jakéhokoli hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="87d65-170">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="87d65-171">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.</span><span class="sxs-lookup"><span data-stu-id="87d65-171">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="87d65-172">Pokud je deklarovat vlastní vstupní typ (například `FooType`), Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="87d65-172">If you declare a custom input type (e.g. `FooType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="87d65-173">String – vhodné pro data textového souboru.</span><span class="sxs-lookup"><span data-stu-id="87d65-173">String - useful for text file data.</span></span>

<span data-ttu-id="87d65-174">V jazyce C# funkce můžete také navázat tooany hello následující typy a hello Functions runtime pokusí rekonstruovat hello souborová data pomocí typu:</span><span class="sxs-lookup"><span data-stu-id="87d65-174">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a><span data-ttu-id="87d65-175">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="87d65-175">Trigger sample</span></span>
<span data-ttu-id="87d65-176">Předpokládejme, že máte hello následující function.json, který definuje aktivační procedury externí soubor:</span><span class="sxs-lookup"><span data-stu-id="87d65-176">Suppose you have hello following function.json, that defines an external file trigger:</span></span>

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

<span data-ttu-id="87d65-177">V tématu vzorku hello konkrétní jazyk, který protokoly hello obsah každý soubor, který se přidá složka monitorované toohello.</span><span class="sxs-lookup"><span data-stu-id="87d65-177">See hello language-specific sample that logs hello contents of each file that is added toohello monitored folder.</span></span>

* [<span data-ttu-id="87d65-178">C#</span><span class="sxs-lookup"><span data-stu-id="87d65-178">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="87d65-179">Node.js</span><span class="sxs-lookup"><span data-stu-id="87d65-179">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a><span data-ttu-id="87d65-180">Aktivační události využití v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="87d65-180">Trigger usage in C#</span></span> #

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

### <a name="trigger-usage-in-nodejs"></a><span data-ttu-id="87d65-181">Aktivační události využití v Node.js</span><span class="sxs-lookup"><span data-stu-id="87d65-181">Trigger usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a><span data-ttu-id="87d65-182">Externí soubor vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="87d65-182">External File input binding</span></span>
<span data-ttu-id="87d65-183">Vstupní vazba Azure externí soubor Hello umožňuje toouse soubor z externí složky ve vaší funkci.</span><span class="sxs-lookup"><span data-stu-id="87d65-183">hello Azure external file input binding enables you toouse a file from an external folder in your function.</span></span>

<span data-ttu-id="87d65-184">Hello externí soubor vstupní tooa funkce využívá hello následující objekty JSON v hello `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="87d65-184">hello external file input tooa function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

<span data-ttu-id="87d65-185">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="87d65-185">Note hello following:</span></span>

* <span data-ttu-id="87d65-186">`path`musí obsahovat název složky hello a název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="87d65-186">`path` must contain hello folder name and hello file name.</span></span> <span data-ttu-id="87d65-187">Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` toopoint tooa souboru v hello `samples-workitems` složku s názvem, který odpovídá názvu souboru hello zadaný ve zprávě hello aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="87d65-187">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="inputusage"></a>

## <a name="input-usage"></a><span data-ttu-id="87d65-188">Vstupní využití</span><span class="sxs-lookup"><span data-stu-id="87d65-188">Input usage</span></span>
<span data-ttu-id="87d65-189">V jazyce C# funkce, vazby dat toohello vstupní soubor pomocí parametr s názvem v podpisu funkce, jako je třeba `<T> <name>`.</span><span class="sxs-lookup"><span data-stu-id="87d65-189">In C# functions, you bind toohello input file data by using a named parameter in your function signature, like `<T> <name>`.</span></span>
<span data-ttu-id="87d65-190">Kde `T` je hello datový typ, který má toodeserialize hello data do, a `paramName` je název hello jste zadali v [vstupní vazby](#input).</span><span class="sxs-lookup"><span data-stu-id="87d65-190">Where `T` is hello data type that you want toodeserialize hello data into, and `paramName` is hello name you specified in the [input binding](#input).</span></span> <span data-ttu-id="87d65-191">Ve funkcích Node.js přistupujete hello vstupní soubor dat pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="87d65-191">In Node.js functions, you access hello input file data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="87d65-192">soubor Hello lze deserializovat do jakéhokoli hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="87d65-192">hello file can be deserialized into any of hello following types:</span></span>

* <span data-ttu-id="87d65-193">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – vhodné pro data souboru serializací JSON.</span><span class="sxs-lookup"><span data-stu-id="87d65-193">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialized file data.</span></span>
  <span data-ttu-id="87d65-194">Pokud je deklarovat vlastní vstupní typ (například `InputType`), Azure Functions pokusí toodeserialize hello JSON data do zadaného typu.</span><span class="sxs-lookup"><span data-stu-id="87d65-194">If you declare a custom input type (e.g. `InputType`), Azure Functions attempts toodeserialize hello JSON data into your specified type.</span></span>
* <span data-ttu-id="87d65-195">String – vhodné pro data textového souboru.</span><span class="sxs-lookup"><span data-stu-id="87d65-195">String - useful for text file data.</span></span>

<span data-ttu-id="87d65-196">V jazyce C# funkce můžete také navázat tooany hello následující typy a hello Functions runtime pokusí rekonstruovat hello souborová data pomocí typu:</span><span class="sxs-lookup"><span data-stu-id="87d65-196">In C# functions, you can also bind tooany of hello following types, and hello Functions runtime attempts to deserialize hello file data using that type:</span></span>

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a><span data-ttu-id="87d65-197">Externí soubor výstup vazby</span><span class="sxs-lookup"><span data-stu-id="87d65-197">External File output binding</span></span>
<span data-ttu-id="87d65-198">Hello Azure externí soubor výstupu vazby vám umožní externí složka tooan toowrite souborů ve vaší funkci.</span><span class="sxs-lookup"><span data-stu-id="87d65-198">hello Azure external file output binding enables you toowrite files tooan external folder in your function.</span></span>

<span data-ttu-id="87d65-199">externí soubor Hello výstup hello následující objekty JSON v hello používá funkci `bindings` pole function.json:</span><span class="sxs-lookup"><span data-stu-id="87d65-199">hello external file output for a function uses hello following JSON objects in hello `bindings` array of function.json:</span></span>

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

<span data-ttu-id="87d65-200">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="87d65-200">Note hello following:</span></span>

* <span data-ttu-id="87d65-201">`path`musí obsahovat název složky hello a toowrite název souboru hello k.</span><span class="sxs-lookup"><span data-stu-id="87d65-201">`path` must contain hello folder name and hello file name toowrite to.</span></span> <span data-ttu-id="87d65-202">Pokud máte například [aktivační událost fronty](functions-bindings-storage-queue.md) ve funkci, můžete použít `"path": "samples-workitems/{queueTrigger}"` toopoint tooa souboru v hello `samples-workitems` složku s názvem, který odpovídá názvu souboru hello zadaný ve zprávě hello aktivační událost.</span><span class="sxs-lookup"><span data-stu-id="87d65-202">For example, if you have a [queue trigger](functions-bindings-storage-queue.md) in your function, you can use `"path": "samples-workitems/{queueTrigger}"` toopoint tooa file in hello `samples-workitems` folder with a name that matches hello file name specified in hello trigger message.</span></span>   

<a name="outputusage"></a>

## <a name="output-usage"></a><span data-ttu-id="87d65-203">Využití výstupní</span><span class="sxs-lookup"><span data-stu-id="87d65-203">Output usage</span></span>
<span data-ttu-id="87d65-204">V jazyce C# funkce, vazby toohello výstupní soubor s použitím hello s názvem `out` jako parametr ve vaší podpis funkce `out <T> <name>`, kde `T` je hello datový typ, který má tooserialize hello data do, a `paramName` je hello název můžete zadaný v [výstup vazby](#output).</span><span class="sxs-lookup"><span data-stu-id="87d65-204">In C# functions, you bind toohello output file by using hello named `out` parameter in your function signature, like `out <T> <name>`, where `T` is hello data type that you want tooserialize hello data into, and `paramName` is hello name you specified in the [output binding](#output).</span></span> <span data-ttu-id="87d65-205">V Node.js funkce získáte přístup pomocí souboru výstup hello `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="87d65-205">In Node.js functions, you access hello output file using `context.bindings.<name>`.</span></span>

<span data-ttu-id="87d65-206">Můžete napsat toohello výstupní soubor pomocí kteréhokoli hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="87d65-206">You can write toohello output file using any of hello following types:</span></span>

* <span data-ttu-id="87d65-207">Všechny [objekt](https://msdn.microsoft.com/library/system.object.aspx) – užitečné pro serializaci JSON.</span><span class="sxs-lookup"><span data-stu-id="87d65-207">Any [Object](https://msdn.microsoft.com/library/system.object.aspx) - useful for JSON-serialization.</span></span>
  <span data-ttu-id="87d65-208">Pokud je deklarovat vlastní výstupní typ (například `out OutputType paramName`), Azure Functions pokusí tooserialize objekt do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="87d65-208">If you declare a custom output type (e.g. `out OutputType paramName`), Azure Functions attempts tooserialize object into JSON.</span></span> <span data-ttu-id="87d65-209">Pokud hello výstupní parametr hodnotu null při ukončení hello funkce, hello Functions runtime vytvoří soubor jako objekt s hodnotou null.</span><span class="sxs-lookup"><span data-stu-id="87d65-209">If hello output parameter is null when hello function exits, hello Functions runtime creates a file as a null object.</span></span>
* <span data-ttu-id="87d65-210">String – (`out string paramName`) vhodné pro data textového souboru.</span><span class="sxs-lookup"><span data-stu-id="87d65-210">String - (`out string paramName`) useful for text file data.</span></span> <span data-ttu-id="87d65-211">Hello Functions runtime vytvoří soubor pouze v případě, že parametr řetězce má jinou hodnotu než null, při ukončení hello funkce.</span><span class="sxs-lookup"><span data-stu-id="87d65-211">hello Functions runtime creates a file only if the string parameter is non-null when hello function exits.</span></span>

<span data-ttu-id="87d65-212">V jazyce C# funkce můžete také výstup tooany hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="87d65-212">In C# functions you can also output tooany of hello following types:</span></span>

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a><span data-ttu-id="87d65-213">Vstup + ukázkový výstup</span><span class="sxs-lookup"><span data-stu-id="87d65-213">Input + Output sample</span></span>
<span data-ttu-id="87d65-214">Předpokládejme, že máte hello následující function.json, který definuje [aktivace fronty úložiště](functions-bindings-storage-queue.md), externí soubor vstup a výstup externí soubor:</span><span class="sxs-lookup"><span data-stu-id="87d65-214">Suppose you have hello following function.json, that defines a [Storage queue trigger](functions-bindings-storage-queue.md), an external file input, and an external file output:</span></span>

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

<span data-ttu-id="87d65-215">V tématu vzorku hello konkrétní jazyk, který zkopíruje hello vstupní soubor toohello výstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="87d65-215">See hello language-specific sample that copies hello input file toohello output file.</span></span>

* [<span data-ttu-id="87d65-216">C#</span><span class="sxs-lookup"><span data-stu-id="87d65-216">C#</span></span>](#incsharp)
* [<span data-ttu-id="87d65-217">Node.js</span><span class="sxs-lookup"><span data-stu-id="87d65-217">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="87d65-218">Použití v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="87d65-218">Usage in C#</span></span> #

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

### <a name="usage-in-nodejs"></a><span data-ttu-id="87d65-219">Použití v Node.js</span><span class="sxs-lookup"><span data-stu-id="87d65-219">Usage in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="87d65-220">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87d65-220">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
