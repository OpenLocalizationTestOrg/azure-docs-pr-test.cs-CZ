---
title: Azure Blob Storage funkce vazby | Microsoft Docs
description: "Pochopit, jak používat Azure Storage triggerů a vazeb v Azure Functions."
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
ms.openlocfilehash: 8d8f510ec906c0e0420ec48d45d88b93c144658a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-blob-storage-bindings"></a><span data-ttu-id="1d3a3-104">Vazby úložiště Azure Blob funkce</span><span class="sxs-lookup"><span data-stu-id="1d3a3-104">Azure Functions Blob storage bindings</span></span>
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

<span data-ttu-id="1d3a3-105">Tento článek vysvětluje postup konfigurace a práce s Azure Blob storage vazeb v Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-105">This article explains how to configure and work with Azure Blob storage bindings in Azure Functions.</span></span> <span data-ttu-id="1d3a3-106">Azure podporuje aktivaci funkce vstup a výstup vazby pro úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-106">Azure Functions supports trigger, input, and output bindings for Azure Blob storage.</span></span> <span data-ttu-id="1d3a3-107">Funkce, které jsou k dispozici v všechny vazby, najdete v části [Azure Functions triggerů a vazeb koncepty](functions-triggers-bindings.md).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-107">For features that are available in all bindings, see [Azure Functions triggers and bindings concepts](functions-triggers-bindings.md).</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> <span data-ttu-id="1d3a3-108">A [pouze účet úložiště blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) není podporován.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-108">A [blob only storage account](../storage/common/storage-create-storage-account.md#blob-storage-accounts) is not supported.</span></span> <span data-ttu-id="1d3a3-109">Objekt BLOB úložiště triggerů a vazeb se vyžaduje účet úložiště pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-109">Blob storage triggers and bindings require a general-purpose storage account.</span></span> 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a><span data-ttu-id="1d3a3-110">Objekt BLOB úložiště triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="1d3a3-110">Blob storage triggers and bindings</span></span>

<span data-ttu-id="1d3a3-111">Pomocí aktivační událost úložiště objektů Blob v Azure, kódu funkce je volána, když je zjištěna nová nebo aktualizovaná objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-111">Using the Azure Blob storage trigger, your function code is called when a new or updated blob is detected.</span></span> <span data-ttu-id="1d3a3-112">Obsahu objektu blob jsou uvedeny jako vstup do funkce.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-112">The blob contents are provided as input to the function.</span></span>

<span data-ttu-id="1d3a3-113">Definovat pomocí aktivační události objektu blob úložiště **integrací** na portálu funkce.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-113">Define a blob storage trigger using the **Integrate** tab in the Functions portal.</span></span> <span data-ttu-id="1d3a3-114">Vytvoří následující definice v portálu **vazby** části *function.json*:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-114">The portal creates the following definition in the  **bindings** section of *function.json*:</span></span>

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

<span data-ttu-id="1d3a3-115">Objekt BLOB vstup a výstup vazby jsou definovány pomocí `blob` jako typ vazby:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-115">Blob input and output bindings are defined using `blob` as the binding type:</span></span>

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* <span data-ttu-id="1d3a3-116">`path` Podporuje vlastnost vazby výrazy a parametry filtrování.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-116">The `path` property supports binding expressions and filter parameters.</span></span> <span data-ttu-id="1d3a3-117">V tématu [název vzory](#pattern).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-117">See [Name patterns](#pattern).</span></span>
* <span data-ttu-id="1d3a3-118">`connection` Vlastnost musí obsahovat název nastavení aplikace, který obsahuje připojovací řetězec úložiště.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-118">The `connection` property must contain the name of an app setting that contains a storage connection string.</span></span> <span data-ttu-id="1d3a3-119">Na portálu Azure, standardního editoru v **integrací** kartě nakonfiguruje toto nastavení aplikace pro vás, když vyberete účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-119">In the Azure portal, the standard editor in the **Integrate** tab configures this app setting for you when you select a storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="1d3a3-120">Pokud používáte aktivační události objektu blob na plánu spotřeby, může být až 10 minut zpoždění při zpracování nové objekty BLOB po aplikaci funkce přešel nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-120">When you're using a blob trigger on a Consumption plan, there can be up to a 10-minute delay in processing new blobs after a function app has gone idle.</span></span> <span data-ttu-id="1d3a3-121">Po aplikaci funkce běží, objekty BLOB jsou zpracovávány okamžitě.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-121">After the function app is running, blobs are processed immediately.</span></span> <span data-ttu-id="1d3a3-122">Abyste se vyhnuli Tato počáteční prodleva, zvažte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-122">To avoid this initial delay, consider one of the following options:</span></span>
> - <span data-ttu-id="1d3a3-123">Plán služby App Service použijte s povolenou funkci Always On.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-123">Use an App Service plan with Always On enabled.</span></span>
> - <span data-ttu-id="1d3a3-124">Použijte jiný mechanismus pro aktivaci objektu blob zpracování, např. zprávu fronty, který obsahuje název objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-124">Use another mechanism to trigger the blob processing, such as a queue message that contains the blob name.</span></span> <span data-ttu-id="1d3a3-125">Příklad, naleznete v části [vstupní frontě aktivační události s objektem blob vazby](#input-sample).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-125">For an example, see [Queue trigger with blob input binding](#input-sample).</span></span>

<a name="pattern"></a>

### <a name="name-patterns"></a><span data-ttu-id="1d3a3-126">Vzory názvů</span><span class="sxs-lookup"><span data-stu-id="1d3a3-126">Name patterns</span></span>
<span data-ttu-id="1d3a3-127">Můžete zadat vzor názvu objektu blob v `path` vlastnosti, která může být výraz filtru nebo vazby.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-127">You can specify a blob name pattern in the `path` property, which can be a filter or binding expression.</span></span> <span data-ttu-id="1d3a3-128">V tématu [vazby výrazy a vzory](functions-triggers-bindings.md#binding-expressions-and-patterns).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-128">See [Binding expressions and patterns](functions-triggers-bindings.md#binding-expressions-and-patterns).</span></span>

<span data-ttu-id="1d3a3-129">Například pokud chcete filtrovat, aby objekty BLOB, které začínají řetězcem "původní", použijte následující definice.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-129">For example, to filter to blobs that start with the string "original," use the following definition.</span></span> <span data-ttu-id="1d3a3-130">Tuto cestu vyhledá objekt blob s názvem *původní Blob1.txt* v *vstupní* kontejneru a hodnota `name` proměnná v kódu funkce `Blob1`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-130">This path finds a blob named *original-Blob1.txt* in the *input* container, and the value of the `name` variable in function code is `Blob1`.</span></span>

```json
"path": "input/original-{name}",
```

<span data-ttu-id="1d3a3-131">Pokud chcete vytvořit vazbu samostatně na název souboru objektů blob a příponu, použijte dva vzory.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-131">To bind to the blob file name and extension separately, use two patterns.</span></span> <span data-ttu-id="1d3a3-132">Tato cesta také vyhledá objekt blob s názvem *původní Blob1.txt*a hodnota `blobname` a `blobextension` proměnné v kódu funkce jsou *původní Blob1* a *txt*.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-132">This path also finds a blob named *original-Blob1.txt*, and the value of the `blobname` and `blobextension` variables in function code are *original-Blob1* and *txt*.</span></span>

```json
"path": "input/{blobname}.{blobextension}",
```

<span data-ttu-id="1d3a3-133">Typ souboru objektů BLOB můžete omezit pomocí příkazu pevnou hodnotu pro tuto příponu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-133">You can restrict the file type of blobs by using a fixed value for the file extension.</span></span> <span data-ttu-id="1d3a3-134">Například aktivovat pouze u souborů, PNG, používají následující vzorec:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-134">For instance, to trigger only on .png files, use the following pattern:</span></span>

```json
"path": "samples/{name}.png",
```

<span data-ttu-id="1d3a3-135">Složené závorky jsou speciální znaky v vzory názvů.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-135">Curly braces are special characters in name patterns.</span></span> <span data-ttu-id="1d3a3-136">Pokud chcete zadat názvy objektů blob, které mají v názvu složené závorky, můžete vyhnuli složené závorky pomocí dvou složené závorky.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-136">To specify blob names that have curly braces in the name, you can escape the braces using two braces.</span></span> <span data-ttu-id="1d3a3-137">Následující příklad vyhledá objekt blob s názvem *{20140101}-soundfile.mp3* v *bitové kopie* kontejner a `name` hodnotu proměnné v kód funkce je *soundfile.mp3*.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-137">The following example finds a blob named *{20140101}-soundfile.mp3* in the *images* container, and the `name` variable value in the function code is *soundfile.mp3*.</span></span> 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a><span data-ttu-id="1d3a3-138">Aktivační událost metadat</span><span class="sxs-lookup"><span data-stu-id="1d3a3-138">Trigger metadata</span></span>

<span data-ttu-id="1d3a3-139">Aktivační události objektu blob nabízí několik vlastností metadat.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-139">The blob trigger provides several metadata properties.</span></span> <span data-ttu-id="1d3a3-140">Tyto vlastnosti lze použít jako součást výrazů vazby v jiných vazby nebo jako parametry v kódu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-140">These properties can be used as part of bindings expressions in other bindings or as parameters in your code.</span></span> <span data-ttu-id="1d3a3-141">Tyto hodnoty mají stejnou sémantiku jako [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-141">These values have the same semantics as [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).</span></span>

- <span data-ttu-id="1d3a3-142">**BlobTrigger**.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-142">**BlobTrigger**.</span></span> <span data-ttu-id="1d3a3-143">Zadejte `string`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-143">Type `string`.</span></span> <span data-ttu-id="1d3a3-144">Spouštěcí objekt blob cesta</span><span class="sxs-lookup"><span data-stu-id="1d3a3-144">The triggering blob path</span></span>
- <span data-ttu-id="1d3a3-145">**Identifikátor URI**.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-145">**Uri**.</span></span> <span data-ttu-id="1d3a3-146">Zadejte `System.Uri`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-146">Type `System.Uri`.</span></span> <span data-ttu-id="1d3a3-147">Identifikátor URI objektu blob pro primární umístění.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-147">The blob's URI for the primary location.</span></span>
- <span data-ttu-id="1d3a3-148">**Vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-148">**Properties**.</span></span> <span data-ttu-id="1d3a3-149">Zadejte `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-149">Type `Microsoft.WindowsAzure.Storage.Blob.BlobProperties`.</span></span> <span data-ttu-id="1d3a3-150">Vlastnosti systému objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-150">The blob's system properties.</span></span>
- <span data-ttu-id="1d3a3-151">**Metadata**.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-151">**Metadata**.</span></span> <span data-ttu-id="1d3a3-152">Zadejte `IDictionary<string,string>`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-152">Type `IDictionary<string,string>`.</span></span> <span data-ttu-id="1d3a3-153">Metadata definovaná uživatelem pro tento objekt blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-153">The user-defined metadata for the blob.</span></span>

<a name="receipts"></a>

### <a name="blob-receipts"></a><span data-ttu-id="1d3a3-154">Potvrzení objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="1d3a3-154">Blob receipts</span></span>
<span data-ttu-id="1d3a3-155">Modulu runtime Azure Functions zajistí, že žádná funkce aktivační události objektu blob volala více než jednou pro stejný objekt blob nové nebo aktualizované.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-155">The Azure Functions runtime ensures that no blob trigger function gets called more than once for the same new or updated blob.</span></span> <span data-ttu-id="1d3a3-156">Chcete-li zjistit, pokud byla zpracována na verzi daného objektu blob, udržuje *blob potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-156">To determine if a given blob version has been processed, it maintains *blob receipts*.</span></span>

<span data-ttu-id="1d3a3-157">Azure funkce úložiště objektů blob v kontejneru nazvaném potvrzení *azure. webové úlohy hostitelů* v účtu úložiště Azure pro aplikaci funkce (definované nastavení aplikace `AzureWebJobsStorage`).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-157">Azure Functions stores blob receipts in a container named *azure-webjobs-hosts* in the Azure storage account for your function app (defined by the app setting `AzureWebJobsStorage`).</span></span> <span data-ttu-id="1d3a3-158">Potvrzení o objektu blob obsahuje následující informace:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-158">A blob receipt has the following information:</span></span>

* <span data-ttu-id="1d3a3-159">Funkci spouštěná ("*&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*", například:"MyFunctionApp.Functions.CopyBlob")</span><span class="sxs-lookup"><span data-stu-id="1d3a3-159">The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "MyFunctionApp.Functions.CopyBlob")</span></span>
* <span data-ttu-id="1d3a3-160">Název kontejneru</span><span class="sxs-lookup"><span data-stu-id="1d3a3-160">The container name</span></span>
* <span data-ttu-id="1d3a3-161">Typ objektu blob ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="1d3a3-161">The blob type ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="1d3a3-162">Název objektu blob</span><span class="sxs-lookup"><span data-stu-id="1d3a3-162">The blob name</span></span>
* <span data-ttu-id="1d3a3-163">Značky ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="1d3a3-163">The ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

<span data-ttu-id="1d3a3-164">Pokud chcete vynutit opětovné zpracování objektu blob, odstranit objekt blob příjmu pro tento objekt blob z *azure. webové úlohy hostitelů* kontejneru ručně.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-164">To force reprocessing of a blob, delete the blob receipt for that blob from the *azure-webjobs-hosts* container manually.</span></span>

<a name="poison"></a>

### <a name="handling-poison-blobs"></a><span data-ttu-id="1d3a3-165">Zpracování poškozených objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="1d3a3-165">Handling poison blobs</span></span>
<span data-ttu-id="1d3a3-166">Pokud funkci aktivační události objektu blob se nezdaří pro daný objekt blob Azure Functions opakovaných pokusů, které funkce celkem 5krát ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-166">When a blob trigger function fails for a given blob, Azure Functions retries that function a total of 5 times by default.</span></span> 

<span data-ttu-id="1d3a3-167">Pokud selžou všechny 5 pokusů, Azure Functions přidá zprávu do fronty úložiště s názvem *webjobs. blobtrigger poison*.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-167">If all 5 tries fail, Azure Functions adds a message to a Storage queue named *webjobs-blobtrigger-poison*.</span></span> <span data-ttu-id="1d3a3-168">Zprávy ve frontě pro poškozených objekty BLOB je objekt JSON, který obsahuje následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-168">The queue message for poison blobs is a JSON object that contains the following properties:</span></span>

* <span data-ttu-id="1d3a3-169">FunctionId (ve formátu  *&lt;funkce název aplikace >*. Funkce.  *&lt;název funkce >*)</span><span class="sxs-lookup"><span data-stu-id="1d3a3-169">FunctionId (in the format *&lt;function app name>*.Functions.*&lt;function name>*)</span></span>
* <span data-ttu-id="1d3a3-170">BlobType ("BlockBlob" nebo "PageBlob")</span><span class="sxs-lookup"><span data-stu-id="1d3a3-170">BlobType ("BlockBlob" or "PageBlob")</span></span>
* <span data-ttu-id="1d3a3-171">ContainerName</span><span class="sxs-lookup"><span data-stu-id="1d3a3-171">ContainerName</span></span>
* <span data-ttu-id="1d3a3-172">BlobName</span><span class="sxs-lookup"><span data-stu-id="1d3a3-172">BlobName</span></span>
* <span data-ttu-id="1d3a3-173">Značka ETag (identifikátor objektu blob verze, například: "0x8D1DC6E70A277EF")</span><span class="sxs-lookup"><span data-stu-id="1d3a3-173">ETag (a blob version identifier, for example: "0x8D1DC6E70A277EF")</span></span>

### <a name="blob-polling-for-large-containers"></a><span data-ttu-id="1d3a3-174">Dotazování pro velké kontejnery objektů BLOB</span><span class="sxs-lookup"><span data-stu-id="1d3a3-174">Blob polling for large containers</span></span>
<span data-ttu-id="1d3a3-175">Pokud kontejner objektů blob, který je monitorován obsahuje více než 10 000 objektů BLOB, přihlaste se kontroly runtime funkce soubory, které chcete sledovat pro nové nebo změněné objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-175">If the blob container being monitored contains more than 10,000 blobs, the Functions runtime scans log files to watch for new or changed blobs.</span></span> <span data-ttu-id="1d3a3-176">Tento proces není v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-176">This process is not real time.</span></span> <span data-ttu-id="1d3a3-177">Funkce nemusí získat aktivuje až několik minut nebo déle po vytvoření objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-177">A function might not get triggered until several minutes or longer after the blob is created.</span></span> <span data-ttu-id="1d3a3-178">Kromě toho [protokol úložiště jsou vytvořené na "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) intervalech.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-178">In addition, [storage logs are created on a "best effort"](/rest/api/storageservices/About-Storage-Analytics-Logging) basis.</span></span> <span data-ttu-id="1d3a3-179">Není zaručeno, že jsou zachyceny všechny události.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-179">There is no guarantee that all events are captured.</span></span> <span data-ttu-id="1d3a3-180">Za určitých podmínek může být načteni protokoly.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-180">Under some conditions, logs may be missed.</span></span> <span data-ttu-id="1d3a3-181">Pokud budete potřebovat rychlejší a spolehlivější blob zpracování, zvažte vytvoření [zprávu fronty](../storage/queues/storage-dotnet-how-to-use-queues.md) při vytváření objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-181">If you require faster or more reliable blob processing, consider creating a [queue message](../storage/queues/storage-dotnet-how-to-use-queues.md) when you create the blob.</span></span> <span data-ttu-id="1d3a3-182">Poté použijte [aktivační událost fronty](functions-bindings-storage-queue.md) místo aktivační událost objektu blob ke zpracování objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-182">Then, use a [queue trigger](functions-bindings-storage-queue.md) instead of a blob trigger to process the blob.</span></span>

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a><span data-ttu-id="1d3a3-183">Pomocí aktivační události objektu blob a vstupní vazby</span><span class="sxs-lookup"><span data-stu-id="1d3a3-183">Using a blob trigger and input binding</span></span>
<span data-ttu-id="1d3a3-184">V rozhraní .NET funkce, přístup k datům objektu blob pomocí parametru metody, jako třeba `Stream paramName`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-184">In .NET functions, access the blob data using a method parameter such as `Stream paramName`.</span></span> <span data-ttu-id="1d3a3-185">Zde `paramName` je hodnota zadaná v [konfigurace aktivační události](#trigger).</span><span class="sxs-lookup"><span data-stu-id="1d3a3-185">Here, `paramName` is the value you specified in the [trigger configuration](#trigger).</span></span> <span data-ttu-id="1d3a3-186">Ve funkcích Node.js, přístup k datům vstupního objektu blob pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-186">In Node.js functions, access the input blob data using `context.bindings.<name>`.</span></span>

<span data-ttu-id="1d3a3-187">V rozhraní .NET můžete vázat na jakýkoli z typů v níže uvedeném seznamu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-187">In .NET, you can bind to any of the types in the list below.</span></span> <span data-ttu-id="1d3a3-188">Pokud slouží jako vstupní vazby, přičemž některé z těchto typů vyžadují `inout` vazby směr v *function.json*.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-188">If used as an input binding, some of these types require an `inout` binding direction in *function.json*.</span></span> <span data-ttu-id="1d3a3-189">Tomto směru nepodporuje standardního editoru, je nutné použít rozšířené editoru.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-189">This direction is not supported by the standard editor, so you must use the advanced editor.</span></span>

* `TextReader`
* `Stream`
* <span data-ttu-id="1d3a3-190">`ICloudBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="1d3a3-190">`ICloudBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="1d3a3-191">`CloudBlockBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="1d3a3-191">`CloudBlockBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="1d3a3-192">`CloudPageBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="1d3a3-192">`CloudPageBlob` (requires "inout" binding direction)</span></span>
* <span data-ttu-id="1d3a3-193">`CloudAppendBlob`(vyžaduje "inout" vazba směr)</span><span class="sxs-lookup"><span data-stu-id="1d3a3-193">`CloudAppendBlob` (requires "inout" binding direction)</span></span>

<span data-ttu-id="1d3a3-194">Pokud se očekává text objekty BLOB, můžete také vázat na .NET `string` typu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-194">If text blobs are expected, you can also bind to a .NET `string` type.</span></span> <span data-ttu-id="1d3a3-195">Toto nastavení se doporučuje jenom Pokud velikost objektu blob je malá, protože obsah celý objekt blob jsou načtena do paměti.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-195">This is only recommended if the blob size is small, as the entire blob contents are loaded into memory.</span></span> <span data-ttu-id="1d3a3-196">Obecně platí, je vhodnější použít `Stream` nebo `CloudBlockBlob` typu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-196">Generally, it is preferable to use a `Stream` or `CloudBlockBlob` type.</span></span>

## <a name="trigger-sample"></a><span data-ttu-id="1d3a3-197">Ukázka aktivační události</span><span class="sxs-lookup"><span data-stu-id="1d3a3-197">Trigger sample</span></span>
<span data-ttu-id="1d3a3-198">Předpokládejme, že máte následující function.json, která definuje aktivační událost úložiště objektů blob:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-198">Suppose you have the following function.json that defines a blob storage trigger:</span></span>

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

<span data-ttu-id="1d3a3-199">Naleznete v ukázce pro specifický jazyk, který zaznamenává obsah jednotlivých objektů blob, který je přidán do kontejneru monitorovaných.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-199">See the language-specific sample that logs the contents of each blob that is added to the monitored container.</span></span>

* [<span data-ttu-id="1d3a3-200">C#</span><span class="sxs-lookup"><span data-stu-id="1d3a3-200">C#</span></span>](#triggercsharp)
* [<span data-ttu-id="1d3a3-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3a3-201">Node.js</span></span>](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a><span data-ttu-id="1d3a3-202">Příklady aktivační události objektu BLOB v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="1d3a3-202">Blob trigger examples in C#</span></span> #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a><span data-ttu-id="1d3a3-203">Příklad aktivační události v Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3a3-203">Trigger example in Node.js</span></span>

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a><span data-ttu-id="1d3a3-204">Pomocí objektu blob výstup vazby</span><span class="sxs-lookup"><span data-stu-id="1d3a3-204">Using a blob output binding</span></span>

<span data-ttu-id="1d3a3-205">V rozhraní .NET funkce, měli byste buď použijte `out string` parametr v podpis funkce nebo použijte jeden z typů v následujícím seznamu.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-205">In .NET functions, you should either use a `out string` parameter in your function signature or use one of the types in the following list.</span></span> <span data-ttu-id="1d3a3-206">V Node.js funkce, přístup k výstupu blob pomocí `context.bindings.<name>`.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-206">In Node.js functions, you access the output blob using `context.bindings.<name>`.</span></span>

<span data-ttu-id="1d3a3-207">V rozhraní .NET funkce výstup můžete na některý z následujících typů:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-207">In .NET functions you can output to any of the following types:</span></span>

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a><span data-ttu-id="1d3a3-208">Aktivační událost fronty s blob vstupní a výstupní ukázka</span><span class="sxs-lookup"><span data-stu-id="1d3a3-208">Queue trigger with blob input and output sample</span></span>
<span data-ttu-id="1d3a3-209">Předpokládejme, že máte následující function.json, který definuje [aktivační událost Queue Storage](functions-bindings-storage-queue.md)úložiště objektů blob vstup a výstup úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-209">Suppose you have the following function.json, that defines a [Queue Storage trigger](functions-bindings-storage-queue.md), a blob storage input, and a blob storage output.</span></span> <span data-ttu-id="1d3a3-210">Všimněte si použití `queueTrigger` vlastnost metadat.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-210">Notice the use of the `queueTrigger` metadata property.</span></span> <span data-ttu-id="1d3a3-211">v objektu blob vstupní a výstupní `path` vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="1d3a3-211">in the blob input and output `path` properties:</span></span>

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

<span data-ttu-id="1d3a3-212">Naleznete v ukázce pro specifický jazyk, který kopíruje vstupního objektu blob do výstupního objektu blob.</span><span class="sxs-lookup"><span data-stu-id="1d3a3-212">See the language-specific sample that copies the input blob to the output blob.</span></span>

* [<span data-ttu-id="1d3a3-213">C#</span><span class="sxs-lookup"><span data-stu-id="1d3a3-213">C#</span></span>](#incsharp)
* [<span data-ttu-id="1d3a3-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3a3-214">Node.js</span></span>](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a><span data-ttu-id="1d3a3-215">Příklad vazby objektů BLOB v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="1d3a3-215">Blob binding example in C#</span></span> #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a><span data-ttu-id="1d3a3-216">Příklad vazby objektů BLOB v Node.js</span><span class="sxs-lookup"><span data-stu-id="1d3a3-216">Blob binding example in Node.js</span></span>

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a><span data-ttu-id="1d3a3-217">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d3a3-217">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

