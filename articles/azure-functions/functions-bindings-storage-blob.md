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
# <a name="azure-functions-blob-storage-bindings"></a><span data-ttu-id="78326-104">Vazby úložiště Azure Blob funkce</span><span class="sxs-lookup"><span data-stu-id="78326-104">Azure Functions Blob storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="78326-105">Tento článek vysvětluje, jak tooconfigure a práce s Azure Blob storage vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="78326-105">This article explains how tooconfigure and work with Azure Blob storage bindings in Azure Functions.</span></span> <span data-ttu-id="78326-106">Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="78326-106">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="78326-107">Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="78326-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> <span data-ttu-id="78326-108">A [pouze účet úložiště blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) není podporován.</span><span class="sxs-lookup"><span data-stu-id="78326-108">A [blob only storage account](../storage/common/storage-create-storage-account.md#blob-storage-accounts) is not supported.</span></span> <span data-ttu-id="78326-109">Objekt BLOB úložiště triggerů a vazeb se vyžaduje účet úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="78326-109">Blob storage triggers and bindings require a general-purpose storage account.</span></span> 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a><span data-ttu-id="78326-110">Objekt BLOB úložiště triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="78326-110">Blob storage triggers and bindings</span></span>

<span data-ttu-id="78326-111">Pomocí aktivační událost úložiště objektů Blob v Azure hello, kódu funkce je volána, když je zjištěna nová nebo aktualizovaná objektu blob.</span><span class="sxs-lookup"><span data-stu-id="78326-111">Using hello Azure Blob storage trigger, your function code is called when a new or updated blob is detected.</span></span> <span data-ttu-id="78326-112">Hello obsahu objektu blob jsou uvedeny jako vstupní toohello funkce.</span><span class="sxs-lookup"><span data-stu-id="78326-112">hello blob contents are provided as input toohello function.</span></span>

<span data-ttu-id="78326-113">Zadejte aktivační události objektu blob úložiště pomocí hello **integrací** kartě hello funkce portálu.</span><span class="sxs-lookup"><span data-stu-id="78326-113">Define a blob storage trigger using hello **Integrate** tab in hello Functions portal.</span></span> <span data-ttu-id="78326-114">Hello portál vytvoří hello následující definice v hello **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="78326-114">hello portal creates hello following definition in hello  **bindings** section of *function.json*:</span></span>

```json
{
    "name": "<hello name used tooidentify hello trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container toomonitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

<span data-ttu-id="78326-115">Objekt BLOB vstup a výstup vazby jsou definovány pomocí `blob` jako typ vazby hello:</span><span class="sxs-lookup"><span data-stu-id="78326-115">Blob input and output bindings are defined using `blob` as hello binding type:</span></span>

```json
{
  "name": "<hello name used tooidentify hello blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* <span data-ttu-id="78326-116">Hello `path` podporuje vlastnost vazby výrazy a parametry filtrování.</span><span class="sxs-lookup"><span data-stu-id="78326-116">hello `path` property supports binding expressions and filter parameters.</span></span> <span data-ttu-id="78326-117">V tématu [název vzory](#pattern).</span><span class="sxs-lookup"><span data-stu-id="78326-117">See [Name patterns](#pattern).</span></span>
* <span data-ttu-id="78326-118">Hello `connection` vlastnost musí obsahovat název hello nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="78326-118">hello `connection` property must contain hello name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="78326-119">V hello portálu Azure, hello standardního editoru v hello **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="78326-119">In hello Azure portal, hello standard editor in hello **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="78326-120">Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění tooa při zpracování nové objekty BLOB po aplikaci funkce přešel nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="78326-120">When you're using a blob trigger on a Consumption plan, there can be up tooa 10-minute delay in processing new blobs after a function app has gone idle.</span></span> <span data-ttu-id="78326-121">Po hello funkce aplikace běží, objekty BLOB jsou zpracovávány okamžitě.</span><span class="sxs-lookup"><span data-stu-id="78326-121">After hello function app is running, blobs are processed immediately.</span></span> <span data-ttu-id="78326-122">Tento počáteční tooavoid zpoždění, zvažte jednu hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="78326-122">tooavoid this initial delay, consider one of hello following options:</span></span>
> - <span data-ttu-id="78326-123">Plán služby App Service použijte s povolenou funkci Always On.</span><span class="sxs-lookup"><span data-stu-id="78326-123">Use an App Service plan with Always On enabled.</span></span>
> - <span data-ttu-id="78326-124">Použijte jiný mechanismus tootrigger hello blob zpracování, např. zprávu fronty, který obsahuje název objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="78326-124">Use another mechanism tootrigger hello blob processing, such as a queue message that contains hello blob name.</span></span> <span data-ttu-id="78326-125">Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](#input-sample).</span><span class="sxs-lookup"><span data-stu-id="78326-125">For an example, see [Queue trigger with blob input binding](#input-sample).</span></span>

<a name="pattern"></a>

### <a name="name-patterns"></a><span data-ttu-id="78326-126">Vzory názvů</span><span class="sxs-lookup"><span data-stu-id="78326-126">Name patterns</span></span>
<span data-ttu-id="78326-127">Můžete zadat vzor názvu objektu blob v hello `path` vlastnosti, která může být výraz filtru nebo vazby.</span><span class="sxs-lookup"><span data-stu-id="78326-127">You can specify a blob name pattern in hello `path` property, which can be a filter or binding expression.</span></span> <span data-ttu-id="78326-128">V tématu [vazby výrazy a vzory](functions-triggers-bindings.md#binding-expressions-and-patterns).</span><span class="sxs-lookup"><span data-stu-id="78326-128">See [Binding expressions and patterns](functions-triggers-bindings.md#binding-expressions-and-patterns).</span></span>

<span data-ttu-id="78326-129">Tooblobs toofilter, které začínají řetězcem hello "původní", například použít následující definice hello.</span><span class="sxs-lookup"><span data-stu-id="78326-129">For example, toofilter tooblobs that start with hello string "original," use hello following definition.</span></span> <span data-ttu-id="78326-130">Tato cesta vyhledá objekt blob s názvem *původní Blob1.txt* v hello *vstupní* kontejneru a hodnota hello hello `name` proměnná v kódu funkce `Blob1`.</span><span class="sxs-lookup"><span data-stu-id="78326-130">This path finds a blob named *original-Blob1.txt* in hello *input* container, and hello value of hello `name` variable in function code is `Blob1`.</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="78326-131">Název souboru objektů blob toohello toobind a příponu samostatně, použijte dva vzory.</span><span class="sxs-lookup"><span data-stu-id="78326-131">toobind toohello blob file name and extension separately, use two patterns.</span></span> <span data-ttu-id="78326-132">Tato cesta také vyhledá objekt blob s názvem *původní Blob1.txt*a hodnota hello hello `blobname` a `blobextension` proměnné v kódu funkce jsou *původní Blob1* a *txt*.</span><span class="sxs-lookup"><span data-stu-id="78326-132">This path also finds a blob named *original-Blob1.txt*, and hello value of hello `blobname` and `blobextension` variables in function code are *original-Blob1* and *txt*.</span></span>

```json
"path": "input/{blobname}.{blobextension}",
```

<span data-ttu-id="78326-133">Typ souboru hello objektů BLOB můžete omezit pomocí pevná hodnota pro příponu souboru hello.</span><span class="sxs-lookup"><span data-stu-id="78326-133">You can restrict hello file type of blobs by using a fixed value for hello file extension.</span></span> <span data-ttu-id="78326-134">Například tootrigger pouze u souborů, PNG, hello použijte následující vzoru:</span><span class="sxs-lookup"><span data-stu-id="78326-134">For instance, tootrigger only on .png files, use hello following pattern:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="78326-135">Složené závorky jsou speciální znaky v vzory názvů.</span><span class="sxs-lookup"><span data-stu-id="78326-135">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="78326-136">názvy objektů blob toospecify, které mají v názvu hello složené závorky, můžete vyhnuli složené závorky hello pomocí dvou složené závorky.</span><span class="sxs-lookup"><span data-stu-id="78326-136">toospecify blob names that have curly braces in hello name, you can escape hello braces using two braces.</span></span> <span data-ttu-id="78326-137">Hello následující příklad vyhledá objekt blob s názvem *{20140101}-soundfile.mp3* v hello *bitové kopie* kontejneru a hello `name` hodnotu proměnné v kódu funkce hello je  *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="78326-137">hello following example finds a blob named *{20140101}-soundfile.mp3* in hello *images* container, and hello `name` variable value in hello function code is *soundfile.mp3*.</span></span> 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a><span data-ttu-id="78326-138">Aktivační událost metadat</span><span class="sxs-lookup"><span data-stu-id="78326-138">Trigger metadata</span></span>

<span data-ttu-id="78326-139">aktivační události objektu blob Hello nabízí několik vlastností metadat.</span><span class="sxs-lookup"><span data-stu-id="78326-139">hello blob trigger provides several metadata properties.</span></span> <span data-ttu-id="78326-140">Tyto vlastnosti lze použít jako součást výrazů vazby v jiných vazby nebo jako parametry v kódu.</span><span class="sxs-lookup"><span data-stu-id="78326-140">These properties can be used as part of bindings expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="78326-141">Tyto hodnoty mají hello stejnou sémantiku jako [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="78326-141">These values have hello same semantics as [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span></span>

- <span data-ttu-id="78326-142">**BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="78326-142">**BlobTrigger**.</span></span> <span data-ttu-id="78326-143">Zadejte `string`.</span><span class="sxs-lookup"><span data-stu-id="78326-143">Type `string`.</span></span> <span data-ttu-id="78326-144">Cesta blobu spouštěcí Hello</span><span class="sxs-lookup"><span data-stu-id="78326-144">hello triggering blob path</span></span>
- <span data-ttu-id="78326-145">**Identifikátor URI**.</span><span class="sxs-lookup"><span data-stu-id="78326-145">**Uri**.</span></span> <span data-ttu-id="78326-146">Zadejte `System.Uri`.</span><span class="sxs-lookup"><span data-stu-id="78326-146">Type `System.Uri`.</span></span> <span data-ttu-id="78326-147">identifikátor URI objektu Hello blob pro primární umístění hello.</span><span class="sxs-lookup"><span data-stu-id="78326-147">hello blob's URI for hello primary location.</span></span>
- <span data-ttu-id="78326-148">**Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="78326-148">**Properties**.</span></span> <span data-ttu-id="78326-149">Zadejte `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span><span class="sxs-lookup"><span data-stu-id="78326-149">Type `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span></span> <span data-ttu-id="78326-150">Hello vlastnosti systému objektu blob.</span><span class="sxs-lookup"><span data-stu-id="78326-150">hello blob's system properties.</span></span>
- <span data-ttu-id="78326-151">**Metadata**.</span><span class="sxs-lookup"><span data-stu-id="78326-151">**Metadata**.</span></span> <span data-ttu-id="78326-152">Zadejte `IDictionary<string,string>`.</span><span class="sxs-lookup"><span data-stu-id="78326-152">Type `IDictionary<string,string>`.</span></span> <span data-ttu-id="78326-153">Hello metadata definovaná uživatelem pro objekt blob hello.</span><span class="sxs-lookup"><span data-stu-id="78326-153">hello user-defined metadata for hello blob.</span></span>

<a name="receipts"></a>

### <a name="blob-receipts"></a><span data-ttu-id="78326-154">Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="78326-154">Blob receipts</span></span>
<span data-ttu-id="78326-155">Azure Functions Hello runtime zajistí, že žádná funkce aktivační události objektu blob volala více než jednou pro hello stejné nové nebo aktualizované objektů blob.</span><span class="sxs-lookup"><span data-stu-id="78326-155">hello Azure Functions runtime ensures that no blob trigger function gets called more than once for hello same new or updated blob.</span></span> <span data-ttu-id="78326-156">toodetermine, pokud byla zpracována na verzi daného objektu blob, udržuje *blob potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="78326-156">toodetermine if a given blob version has been processed, it maintains *blob receipts*.</span></span>

<span data-ttu-id="78326-157">Azure funkce úložiště objektů blob v kontejneru nazvaném potvrzení *azure. webové úlohy hostitelů* v účtu úložiště Azure pro aplikaci funkce hello (definované nastavení aplikace hello `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="78326-157">Azure Functions stores blob receipts in a container named *azure-webjobs-hosts* in hello Azure storage account for your function app (defined by hello app setting `AzureWebJobsStorage`).</span></span> <span data-ttu-id="78326-158">Potvrzení o objekt blob má hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="78326-158">A blob receipt has hello following information:</span></span>

* <span data-ttu-id="78326-159">Hello aktivuje funkce ("*&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*", například:"MyFunctionApp.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="78326-159">hello triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "MyFunctionApp.Functions.CopyBlob")</span></span>
* <span data-ttu-id="78326-160">název kontejneru Hello</span><span class="sxs-lookup"><span data-stu-id="78326-160">hello container name</span></span>
* <span data-ttu-id="78326-161">Typ objektu blob Hello ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="78326-161">hello blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="78326-162">Název objektu blob Hello</span><span class="sxs-lookup"><span data-stu-id="78326-162">hello blob name</span></span>
* <span data-ttu-id="78326-163">Hello ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="78326-163">hello ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="78326-164">tooforce opětovné zpracování objektu blob, odstranit hello přijetí objektů blob pro tento objekt blob z hello *azure. webové úlohy hostitelů* kontejneru ručně.</span><span class="sxs-lookup"><span data-stu-id="78326-164">tooforce reprocessing of a blob, delete hello blob receipt for that blob from hello *azure-webjobs-hosts* container manually.</span></span>

<a name="poison"></a>

### <a name="handling-poison-blobs"></a><span data-ttu-id="78326-165">Zpracování poškozených objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="78326-165">Handling poison blobs</span></span>
<span data-ttu-id="78326-166">Pokud funkci aktivační události objektu blob se nezdaří pro daný objekt blob Azure Functions opakovaných pokusů, které funkce celkem 5krát ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="78326-166">When a blob trigger function fails for a given blob, Azure Functions retries that function a total of 5 times by default.</span></span> 

<span data-ttu-id="78326-167">Pokud selžou všechny 5 pokusů, Azure Functions přidá fronta zpráv tooa úložiště s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="78326-167">If all 5 tries fail, Azure Functions adds a message tooa Storage queue named *webjobs-blobtrigger-poison*.</span></span> <span data-ttu-id="78326-168">uvítací zprávu fronty pro poškozených objekty BLOB je objekt JSON, který obsahuje hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="78326-168">hello queue message for poison blobs is a JSON object that contains hello following properties:</span></span>

* <span data-ttu-id="78326-169">FunctionId (ve formátu hello  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)</span><span class="sxs-lookup"><span data-stu-id="78326-169">FunctionId (in hello format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="78326-170">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="78326-170">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="78326-171">ContainerName</span><span class="sxs-lookup"><span data-stu-id="78326-171">ContainerName</span></span>
* <span data-ttu-id="78326-172">BlobName</span><span class="sxs-lookup"><span data-stu-id="78326-172">BlobName</span></span>
* <span data-ttu-id="78326-173">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="78326-173">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

### <a name="blob-polling-for-large-containers"></a><span data-ttu-id="78326-174">Dotazování pro velké kontejnery objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="78326-174">Blob polling for large containers</span></span>
<span data-ttu-id="78326-175">Pokud kontejner objektů blob hello monitorovaných obsahuje více než 10 000 objektů BLOB, hello toowatch soubory funkcí runtime kontroly protokolu pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="78326-175">If hello blob container being monitored contains more than 10,000 blobs, hello Functions runtime scans log files toowatch for new or changed blobs.</span></span> <span data-ttu-id="78326-176">Tento proces není v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="78326-176">This process is not real time.</span></span> <span data-ttu-id="78326-177">Funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="78326-177">A function might not get triggered until several minutes or longer after hello blob is created.</span></span> <span data-ttu-id="78326-178">Kromě toho [protokol úložiště jsou vytvořené na "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) intervalech.</span><span class="sxs-lookup"><span data-stu-id="78326-178">In addition, [storage logs are created on a "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) basis.</span></span> <span data-ttu-id="78326-179">Není zaručeno, že jsou zachyceny všechny události.</span><span class="sxs-lookup"><span data-stu-id="78326-179">There is no guarantee that all events are captured.</span></span> <span data-ttu-id="78326-180">Za určitých podmínek může být načteni protokoly.</span><span class="sxs-lookup"><span data-stu-id="78326-180">Under some conditions, logs may be missed.</span></span> <span data-ttu-id="78326-181">Pokud budete potřebovat rychlejší a spolehlivější blob zpracování, zvažte vytvoření [zprávu fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) při vytváření objektu blob hello.</span><span class="sxs-lookup"><span data-stu-id="78326-181">If you require faster or more reliable blob processing, consider creating a [queue message](../storage/queues/storage-dotnet-how-to-use-queues.md) when you create hello blob.</span></span> <span data-ttu-id="78326-182">Poté použijte [aktivační událost fronty](functions-bindings-storage-queue.md) namísto objektu blob hello tooprocess aktivační události objektu blob.</span><span class="sxs-lookup"><span data-stu-id="78326-182">Then, use a [queue trigger](functions-bindings-storage-queue.md) instead of a blob trigger tooprocess hello blob.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a><span data-ttu-id="78326-183">Pomocí aktivační události objektu blob a vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="78326-183">Using a blob trigger and input binding</span></span>
<span data-ttu-id="78326-184">V rozhraní .NET funkce, přístup k datům objektu blob hello pomocí parametru metody, jako třeba `Stream paramName`.</span><span class="sxs-lookup"><span data-stu-id="78326-184">In .NET functions, access hello blob data using a method parameter such as `Stream paramName`.</span></span> <span data-ttu-id="78326-185">Zde `paramName` je hodnota hello jste zadali v hello [konfigurace aktivační události](#trigger).</span><span class="sxs-lookup"><span data-stu-id="78326-185">Here, `paramName` is hello value you specified in hello [trigger configuration](#trigger).</span></span> <span data-ttu-id="78326-186">Ve funkcích Node.js hello přístup vstup, objektů blob dat pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="78326-186">In Node.js functions, access hello input blob data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="78326-187">V rozhraní .NET můžete vázat tooany typů hello hello níže uvedeného seznamu.</span><span class="sxs-lookup"><span data-stu-id="78326-187">In .NET, you can bind tooany of hello types in hello list below.</span></span> <span data-ttu-id="78326-188">Pokud slouží jako vstupní vazby, přičemž některé z těchto typů vyžadují `inout` vazby směr v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="78326-188">If used as an input binding, some of these types require an `inout` binding direction in *function.json*.</span></span> <span data-ttu-id="78326-189">Tomto směru nepodporuje hello standardního editoru, je nutné použít hello advanced editor.</span><span class="sxs-lookup"><span data-stu-id="78326-189">This direction is not supported by hello standard editor, so you must use hello advanced editor.</span></span>

* `TextReader`
* `Stream`
* <span data-ttu-id="78326-190">`ICloudBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="78326-190">`ICloudBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="78326-191">`CloudBlockBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="78326-191">`CloudBlockBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="78326-192">`CloudPageBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="78326-192">`CloudPageBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="78326-193">`CloudAppendBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="78326-193">`CloudAppendBlob` (requires "inout" binding direction)</span></span>

<span data-ttu-id="78326-194">Pokud se očekává text objekty BLOB, můžete také navázat tooa .NET `string` typu.</span><span class="sxs-lookup"><span data-stu-id="78326-194">If text blobs are expected, you can also bind tooa .NET `string` type.</span></span> <span data-ttu-id="78326-195">Toto nastavení se doporučuje jenom Pokud velikost objektu blob hello malé, jako jsou obsahu objektu blob celý hello načten do paměti.</span><span class="sxs-lookup"><span data-stu-id="78326-195">This is only recommended if hello blob size is small, as hello entire blob contents are loaded into memory.</span></span> <span data-ttu-id="78326-196">Obecně je vhodnější toouse `Stream` nebo `CloudBlockBlob` typu.</span><span class="sxs-lookup"><span data-stu-id="78326-196">Generally, it is preferable toouse a `Stream` or `CloudBlockBlob` type.</span></span>

## <a name="trigger-sample"></a><span data-ttu-id="78326-197">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="78326-197">Trigger sample</span></span>
<span data-ttu-id="78326-198">Předpokládejme, že máte hello následující function.json, který definuje aktivační událost úložiště objektů blob:</span><span class="sxs-lookup"><span data-stu-id="78326-198">Suppose you have hello following function.json that defines a blob storage trigger:</span></span>

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

<span data-ttu-id="78326-199">V tématu vzorku hello konkrétní jazyk, který je protokoly hello obsah každý objekt blob, který se přidá toohello monitorovaných kontejneru.</span><span class="sxs-lookup"><span data-stu-id="78326-199">See hello language-specific sample that logs hello contents of each blob that is added toohello monitored container.</span></span>

* [<span data-ttu-id="78326-200">C#</span><span class="sxs-lookup"><span data-stu-id="78326-200">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="78326-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="78326-201">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a><span data-ttu-id="78326-202">Příklady aktivační události objektu BLOB v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="78326-202">Blob trigger examples in C#</span></span> #

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

### <a name="trigger-example-in-nodejs"></a><span data-ttu-id="78326-203">Příklad aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="78326-203">Trigger example in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a><span data-ttu-id="78326-204">Pomocí objektu blob výstup vazby</span><span class="sxs-lookup"><span data-stu-id="78326-204">Using a blob output binding</span></span>

<span data-ttu-id="78326-205">V rozhraní .NET funkce, měli byste buď použijte `out string` parametr v podpis funkce nebo používají jeden z typů hello v následujícím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="78326-205">In .NET functions, you should either use a `out string` parameter in your function signature or use one of hello types in hello following list.</span></span> <span data-ttu-id="78326-206">V Node.js funkce získáte přístup pomocí objektu blob výstup hello `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="78326-206">In Node.js functions, you access hello output blob using `context.bindings.<name>`.</span></span>

<span data-ttu-id="78326-207">V rozhraní .NET funkce výstup můžete tooany hello následující typy:</span><span class="sxs-lookup"><span data-stu-id="78326-207">In .NET functions you can output tooany of hello following types:</span></span>

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a><span data-ttu-id="78326-208">Aktivační událost fronty s blob vstupní a výstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="78326-208">Queue trigger with blob input and output sample</span></span>
<span data-ttu-id="78326-209">Předpokládejme, že máte hello následující function.json, který definuje [aktivační událost Queue Storage](functions-bindings-storage-queue.md)úložiště objektů blob vstup a výstup úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="78326-209">Suppose you have hello following function.json, that defines a [Queue Storage trigger](functions-bindings-storage-queue.md), a blob storage input, and a blob storage output.</span></span> <span data-ttu-id="78326-210">Všimněte si použití hello hello `queueTrigger` vlastnost metadat.</span><span class="sxs-lookup"><span data-stu-id="78326-210">Notice hello use of hello `queueTrigger` metadata property.</span></span> <span data-ttu-id="78326-211">v objektu blob hello vstup a výstup `path` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="78326-211">in hello blob input and output `path` properties:</span></span>

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

<span data-ttu-id="78326-212">V tématu vzorku hello konkrétní jazyk, který zkopíruje hello vstupního objektu blob toohello výstupního objektu blob.</span><span class="sxs-lookup"><span data-stu-id="78326-212">See hello language-specific sample that copies hello input blob toohello output blob.</span></span>

* [<span data-ttu-id="78326-213">C#</span><span class="sxs-lookup"><span data-stu-id="78326-213">C#</span></span>](#incsharp)
* [<span data-ttu-id="78326-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="78326-214">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a><span data-ttu-id="78326-215">Příklad vazby objektů BLOB v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="78326-215">Blob binding example in C#</span></span> #

```cs
// Copy blob from input toooutput, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a><span data-ttu-id="78326-216">Příklad vazby objektů BLOB v Node.js</span><span class="sxs-lookup"><span data-stu-id="78326-216">Blob binding example in Node.js</span></span>

```javascript
// Copy blob from input toooutput, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="78326-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="78326-217">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

