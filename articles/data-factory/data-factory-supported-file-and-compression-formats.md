---
title: "formáty aaaFile a komprese v Azure Data Factory | Microsoft Docs"
description: "Další informace o hello formáty souborů podporovaných službou Azure Data Factory."
keywords: "data objektu BLOB, kopírovat objekt blob systému azure"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 9d40517b059fc533776bcc088db8c531ee5b003d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a><span data-ttu-id="bbe37-104">Formáty souborů a komprese podporovaných službou Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="bbe37-104">File and compression formats supported by Azure Data Factory</span></span>
<span data-ttu-id="bbe37-105">*Toto téma se týká toohello následující konektory: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [objektů Blob v Azure](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [systém souborů](data-factory-onprem-file-system-connector.md), [ FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), a [SFTP](data-factory-sftp-connector.md).*</span><span class="sxs-lookup"><span data-stu-id="bbe37-105">*This topic applies toohello following connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [File System](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), and [SFTP](data-factory-sftp-connector.md).*</span></span>

<span data-ttu-id="bbe37-106">Azure Data Factory podporuje následující typy formátu souboru hello:</span><span class="sxs-lookup"><span data-stu-id="bbe37-106">Azure Data Factory supports hello following file format types:</span></span>

* [<span data-ttu-id="bbe37-107">Textovém formátu</span><span class="sxs-lookup"><span data-stu-id="bbe37-107">Text format</span></span>](#text-format)
* [<span data-ttu-id="bbe37-108">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="bbe37-108">JSON format</span></span>](#json-format)
* [<span data-ttu-id="bbe37-109">Formát Avro</span><span class="sxs-lookup"><span data-stu-id="bbe37-109">Avro format</span></span>](#avro-format)
* [<span data-ttu-id="bbe37-110">Formát ORC</span><span class="sxs-lookup"><span data-stu-id="bbe37-110">ORC format</span></span>](#orc-format)
* [<span data-ttu-id="bbe37-111">Formát parquet</span><span class="sxs-lookup"><span data-stu-id="bbe37-111">Parquet format</span></span>](#parquet-format)

## <a name="text-format"></a><span data-ttu-id="bbe37-112">Textovém formátu</span><span class="sxs-lookup"><span data-stu-id="bbe37-112">Text format</span></span>
<span data-ttu-id="bbe37-113">Chcete-li tooread z textového souboru nebo zápis tooa textového souboru, nastavte hello `type` vlastnost hello `format` části datové sady hello příliš**TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-113">If you want tooread from a text file or write tooa text file, set hello `type` property in hello `format` section of hello dataset too**TextFormat**.</span></span> <span data-ttu-id="bbe37-114">Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části.</span><span class="sxs-lookup"><span data-stu-id="bbe37-114">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="bbe37-115">V tématu [TextFormat příklad](#textformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="bbe37-115">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="bbe37-116">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bbe37-116">Property</span></span> | <span data-ttu-id="bbe37-117">Popis</span><span class="sxs-lookup"><span data-stu-id="bbe37-117">Description</span></span> | <span data-ttu-id="bbe37-118">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="bbe37-118">Allowed values</span></span> | <span data-ttu-id="bbe37-119">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bbe37-119">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bbe37-120">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="bbe37-120">columnDelimiter</span></span> |<span data-ttu-id="bbe37-121">v souboru použit znak Hello tooseparate sloupce.</span><span class="sxs-lookup"><span data-stu-id="bbe37-121">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="bbe37-122">Můžete zvážit toouse výjimečných netisknutelné char, který může není pravděpodobně existuje ve vašich datech.</span><span class="sxs-lookup"><span data-stu-id="bbe37-122">You can consider toouse a rare unprintable char that may not likely exists in your data.</span></span> <span data-ttu-id="bbe37-123">Můžete například zadejte "\u0001", která představuje spustit z záhlaví (SOH).</span><span class="sxs-lookup"><span data-stu-id="bbe37-123">For example, specify "\u0001", which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="bbe37-124">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="bbe37-124">Only one character is allowed.</span></span> <span data-ttu-id="bbe37-125">Hello **výchozí** hodnota je **čárkou (,)**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-125">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="bbe37-126">toouse znak Unicode odkazovat příliš[znaky znakové sady Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello odpovídající kód pro ni.</span><span class="sxs-lookup"><span data-stu-id="bbe37-126">toouse a Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="bbe37-127">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-127">No</span></span> |
| <span data-ttu-id="bbe37-128">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="bbe37-128">rowDelimiter</span></span> |<span data-ttu-id="bbe37-129">znak Hello používá tooseparate řádků v souboru.</span><span class="sxs-lookup"><span data-stu-id="bbe37-129">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="bbe37-130">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="bbe37-130">Only one character is allowed.</span></span> <span data-ttu-id="bbe37-131">Hello **výchozí** hodnota je všechny následující hodnoty na čtení hello: **["\r\n", "\r", "\n"]** a **"\r\n"** v zápisu.</span><span class="sxs-lookup"><span data-stu-id="bbe37-131">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="bbe37-132">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-132">No</span></span> |
| <span data-ttu-id="bbe37-133">escapeChar</span><span class="sxs-lookup"><span data-stu-id="bbe37-133">escapeChar</span></span> |<span data-ttu-id="bbe37-134">speciální znak Hello používá tooescape oddělovač sloupec v hello obsah vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="bbe37-134">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="bbe37-135">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="bbe37-135">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="bbe37-136">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="bbe37-136">Only one character is allowed.</span></span> <span data-ttu-id="bbe37-137">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="bbe37-137">No default value.</span></span> <br/><br/><span data-ttu-id="bbe37-138">Příklad: Pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave hello čárku v textu hello (Příklad: "Hello, world"), můžete definovat jako hello řídicí znak "$" a použijte řetězec "Hello$, world" v hello zdroji.</span><span class="sxs-lookup"><span data-stu-id="bbe37-138">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="bbe37-139">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-139">No</span></span> |
| <span data-ttu-id="bbe37-140">quoteChar</span><span class="sxs-lookup"><span data-stu-id="bbe37-140">quoteChar</span></span> |<span data-ttu-id="bbe37-141">tooquote řetězcovou hodnotu použít znak Hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-141">hello character used tooquote a string value.</span></span> <span data-ttu-id="bbe37-142">oddělovače sloupců a řádků Hello uvnitř znaky uvozovek za sebou hello by považovány za součást hello řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bbe37-142">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="bbe37-143">Tato vlastnost je použít tooboth vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="bbe37-143">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="bbe37-144">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="bbe37-144">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="bbe37-145">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="bbe37-145">Only one character is allowed.</span></span> <span data-ttu-id="bbe37-146">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="bbe37-146">No default value.</span></span> <br/><br/><span data-ttu-id="bbe37-147">Například, pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave čárku v textu hello (Příklad: < Hello, world >), můžete definovat "(dvojitých uvozovek) jako hello znak uvozovky a použít hello řetězec"Hello, world"v hello zdroji.</span><span class="sxs-lookup"><span data-stu-id="bbe37-147">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="bbe37-148">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-148">No</span></span> |
| <span data-ttu-id="bbe37-149">nullValue</span><span class="sxs-lookup"><span data-stu-id="bbe37-149">nullValue</span></span> |<span data-ttu-id="bbe37-150">Jeden nebo více znaků použít toorepresent hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="bbe37-150">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="bbe37-151">Jeden nebo několik znaků.</span><span class="sxs-lookup"><span data-stu-id="bbe37-151">One or more characters.</span></span> <span data-ttu-id="bbe37-152">Hello **výchozí** hodnoty jsou **"\N" a "NULL"** na čtení a **"\N"** v zápisu.</span><span class="sxs-lookup"><span data-stu-id="bbe37-152">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="bbe37-153">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-153">No</span></span> |
| <span data-ttu-id="bbe37-154">encodingName</span><span class="sxs-lookup"><span data-stu-id="bbe37-154">encodingName</span></span> |<span data-ttu-id="bbe37-155">Zadejte název kódování hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-155">Specify hello encoding name.</span></span> |<span data-ttu-id="bbe37-156">Platný název kódování.</span><span class="sxs-lookup"><span data-stu-id="bbe37-156">A valid encoding name.</span></span> <span data-ttu-id="bbe37-157">Další informace najdete v tématu [Vlastnost Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="bbe37-157">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="bbe37-158">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="bbe37-158">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="bbe37-159">Hello **výchozí** hodnota je **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-159">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="bbe37-160">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-160">No</span></span> |
| <span data-ttu-id="bbe37-161">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="bbe37-161">firstRowAsHeader</span></span> |<span data-ttu-id="bbe37-162">Určuje, zda tooconsider hello první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="bbe37-162">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="bbe37-163">U vstupní datové sady Data Factory načítá první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="bbe37-163">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="bbe37-164">U výstupní datové sady Data Factory zapisuje první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="bbe37-164">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="bbe37-165">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="bbe37-165">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="bbe37-166">True</span><span class="sxs-lookup"><span data-stu-id="bbe37-166">True</span></span><br/><span data-ttu-id="bbe37-167"><b>False (výchozí)</b></span><span class="sxs-lookup"><span data-stu-id="bbe37-167"><b>False (default)</b></span></span> |<span data-ttu-id="bbe37-168">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-168">No</span></span> |
| <span data-ttu-id="bbe37-169">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="bbe37-169">skipLineCount</span></span> |<span data-ttu-id="bbe37-170">Označuje hello počet řádků tooskip při čtení dat ze vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="bbe37-170">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="bbe37-171">Pokud jsou zadané skipLineCount i firstRowAsHeader, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-171">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="bbe37-172">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="bbe37-172">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="bbe37-173">Integer</span><span class="sxs-lookup"><span data-stu-id="bbe37-173">Integer</span></span> |<span data-ttu-id="bbe37-174">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-174">No</span></span> |
| <span data-ttu-id="bbe37-175">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="bbe37-175">treatEmptyAsNull</span></span> |<span data-ttu-id="bbe37-176">Určuje, zda hodnota tootreat hodnotu null nebo prázdný řetězec jako hodnotu null při čtení dat ze vstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="bbe37-176">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="bbe37-177">**True (výchozí)**</span><span class="sxs-lookup"><span data-stu-id="bbe37-177">**True (default)**</span></span><br/><span data-ttu-id="bbe37-178">False</span><span class="sxs-lookup"><span data-stu-id="bbe37-178">False</span></span> |<span data-ttu-id="bbe37-179">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-179">No</span></span> |

### <a name="textformat-example"></a><span data-ttu-id="bbe37-180">Příklad typu TextFormat</span><span class="sxs-lookup"><span data-stu-id="bbe37-180">TextFormat example</span></span>
<span data-ttu-id="bbe37-181">V hello následující definici JSON pro datovou sadu některé volitelné vlastnosti hello nejsou zadány.</span><span class="sxs-lookup"><span data-stu-id="bbe37-181">In hello following JSON definition for a dataset, some of hello optional properties are specified.</span></span>

```json
"typeProperties":
{
    "folderPath": "mycontainer/myfolder",
    "fileName": "myblobname",
    "format":
    {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";",
        "quoteChar": "\"",
        "NullValue": "NaN",
        "firstRowAsHeader": true,
        "skipLineCount": 0,
        "treatEmptyAsNull": true
    }
},
```

<span data-ttu-id="bbe37-182">toouse `escapeChar` místo `quoteChar`, nahraďte hello řádku s `quoteChar` s hello escapeChar následující:</span><span class="sxs-lookup"><span data-stu-id="bbe37-182">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="bbe37-183">Scénáře použití firstRowAsHeader a skipLineCount</span><span class="sxs-lookup"><span data-stu-id="bbe37-183">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="bbe37-184">Kopírování z textového souboru tooa zdroj nepocházející ze souborů a chcete tooadd řádek záhlaví, který obsahuje metadata schématu hello (například: schématu SQL).</span><span class="sxs-lookup"><span data-stu-id="bbe37-184">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="bbe37-185">Zadejte `firstRowAsHeader` jako true v hello výstupní datovou sadu pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="bbe37-185">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="bbe37-186">Kopírování z textového souboru obsahujícího jímka nepocházející ze souborů tooa řádek záhlaví a chcete toodrop, který řádek.</span><span class="sxs-lookup"><span data-stu-id="bbe37-186">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="bbe37-187">Zadejte `firstRowAsHeader` jako true v hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="bbe37-187">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="bbe37-188">Kopírování z textového souboru a mají tooskip pár řádků od začátku hello, které neobsahují žádné informace dat nebo záhlaví.</span><span class="sxs-lookup"><span data-stu-id="bbe37-188">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="bbe37-189">Zadejte `skipLineCount` tooindicate hello počet řádků toobe přeskočena.</span><span class="sxs-lookup"><span data-stu-id="bbe37-189">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="bbe37-190">Pokud hello zbytek hello soubor obsahuje řádek záhlaví, můžete také zadat `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="bbe37-190">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="bbe37-191">Pokud oba `skipLineCount` a `firstRowAsHeader` jsou zadané, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello</span><span class="sxs-lookup"><span data-stu-id="bbe37-191">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

## <a name="json-format"></a><span data-ttu-id="bbe37-192">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="bbe37-192">JSON format</span></span>
<span data-ttu-id="bbe37-193">příliš**importu a exportu souboru JSON jako-je do/z Azure Cosmos DB**, hello najdete [dokumentů JSON importu a exportu](data-factory-azure-documentdb-connector.md#importexport-json-documents) kapitoly [přesun dat do/z Azure Cosmos DB](data-factory-azure-documentdb-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="bbe37-193">too**import/export a JSON file as-is into/from Azure Cosmos DB**, hello see [Import/export JSON documents](data-factory-azure-documentdb-connector.md#importexport-json-documents) section in [Move data to/from Azure Cosmos DB](data-factory-azure-documentdb-connector.md) article.</span></span>

<span data-ttu-id="bbe37-194">Pokud chcete soubory JSON hello tooparse nebo zapsat hello data ve formátu JSON, nastavte hello `type` vlastnost hello `format` části příliš**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-194">If you want tooparse hello JSON files or write hello data in JSON format, set hello `type` property in hello `format` section too**JsonFormat**.</span></span> <span data-ttu-id="bbe37-195">Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části.</span><span class="sxs-lookup"><span data-stu-id="bbe37-195">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="bbe37-196">V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="bbe37-196">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="bbe37-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="bbe37-197">Property</span></span> | <span data-ttu-id="bbe37-198">Popis</span><span class="sxs-lookup"><span data-stu-id="bbe37-198">Description</span></span> | <span data-ttu-id="bbe37-199">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="bbe37-199">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="bbe37-200">filePattern</span><span class="sxs-lookup"><span data-stu-id="bbe37-200">filePattern</span></span> |<span data-ttu-id="bbe37-201">Uveďte hello vzor dat uložených do každého souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="bbe37-201">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="bbe37-202">Povolené hodnoty jsou **setOfObjects** a **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-202">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="bbe37-203">Hello **výchozí** hodnota je **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-203">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="bbe37-204">Podrobné informace o těchto vzorech najdete v tématu [Vzory souborů JSON](#json-file-patterns).</span><span class="sxs-lookup"><span data-stu-id="bbe37-204">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="bbe37-205">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-205">No</span></span> |
| <span data-ttu-id="bbe37-206">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="bbe37-206">jsonNodeReference</span></span> | <span data-ttu-id="bbe37-207">Pokud chcete tooiterate a extrahovat data z objektů hello uvnitř pole pole s hello stejný vzor, zadejte cestu JSON hello tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="bbe37-207">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="bbe37-208">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="bbe37-208">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="bbe37-209">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-209">No</span></span> |
| <span data-ttu-id="bbe37-210">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="bbe37-210">jsonPathDefinition</span></span> | <span data-ttu-id="bbe37-211">Zadejte výraz cesty hello JSON pro každé mapování sloupců s názvem přizpůsobené sloupce (začněte s malá).</span><span class="sxs-lookup"><span data-stu-id="bbe37-211">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="bbe37-212">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON. Můžete extrahovat data z objektu nebo pole.</span><span class="sxs-lookup"><span data-stu-id="bbe37-212">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="bbe37-213">Pro pole v části kořenový objekt začněte kořenové $; pro pole uvnitř hello pole, kterou si zvolí `jsonNodeReference` vlastnost, začátek od hello pole elementu.</span><span class="sxs-lookup"><span data-stu-id="bbe37-213">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="bbe37-214">V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="bbe37-214">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="bbe37-215">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-215">No</span></span> |
| <span data-ttu-id="bbe37-216">encodingName</span><span class="sxs-lookup"><span data-stu-id="bbe37-216">encodingName</span></span> |<span data-ttu-id="bbe37-217">Zadejte název kódování hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-217">Specify hello encoding name.</span></span> <span data-ttu-id="bbe37-218">Hello seznam platných názvů kódování, najdete v tématu: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bbe37-218">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="bbe37-219">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="bbe37-219">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="bbe37-220">Hello **výchozí** hodnota je: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-220">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="bbe37-221">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-221">No</span></span> |
| <span data-ttu-id="bbe37-222">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="bbe37-222">nestingSeparator</span></span> |<span data-ttu-id="bbe37-223">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="bbe37-223">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="bbe37-224">Hello výchozí hodnota je '. " (tečka).</span><span class="sxs-lookup"><span data-stu-id="bbe37-224">hello default value is '.' (dot).</span></span> |<span data-ttu-id="bbe37-225">Ne</span><span class="sxs-lookup"><span data-stu-id="bbe37-225">No</span></span> |

### <a name="json-file-patterns"></a><span data-ttu-id="bbe37-226">Vzory souborů JSON</span><span class="sxs-lookup"><span data-stu-id="bbe37-226">JSON file patterns</span></span>

<span data-ttu-id="bbe37-227">Aktivita kopírování můžete analyzovat hello následující vzorce soubory JSON:</span><span class="sxs-lookup"><span data-stu-id="bbe37-227">Copy activity can parse hello following patterns of JSON files:</span></span>

- <span data-ttu-id="bbe37-228">**Typ I: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="bbe37-228">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="bbe37-229">Každý soubor obsahuje jeden objekt nebo několik objektů, které jsou zřetězené nebo oddělené řádkem.</span><span class="sxs-lookup"><span data-stu-id="bbe37-229">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="bbe37-230">Když je pro výstupní datovou sadu vybraná tato možnost, aktivita kopírování vytvoří jeden soubor JSON, ve kterém je každý objekt na jednom řádku (oddělení řádkem).</span><span class="sxs-lookup"><span data-stu-id="bbe37-230">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="bbe37-231">**Příklad JSON s jedním objektem**</span><span class="sxs-lookup"><span data-stu-id="bbe37-231">**single object JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        ```

    * <span data-ttu-id="bbe37-232">**Příklad JSON s oddělením řádky**</span><span class="sxs-lookup"><span data-stu-id="bbe37-232">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="bbe37-233">**Příklad JSON se zřetězením**</span><span class="sxs-lookup"><span data-stu-id="bbe37-233">**concatenated JSON example**</span></span>

        ```json
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        }
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        }
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
        ```

- <span data-ttu-id="bbe37-234">**Typ II: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="bbe37-234">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="bbe37-235">Každý soubor obsahuje pole objektů.</span><span class="sxs-lookup"><span data-stu-id="bbe37-235">Each file contains an array of objects.</span></span>

    ```json
    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        }
    ]
    ```

### <a name="jsonformat-example"></a><span data-ttu-id="bbe37-236">Příklad typu JsonFormat</span><span class="sxs-lookup"><span data-stu-id="bbe37-236">JsonFormat example</span></span>

<span data-ttu-id="bbe37-237">**Případ 1: Kopírování dat ze souborů JSON**</span><span class="sxs-lookup"><span data-stu-id="bbe37-237">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="bbe37-238">Najdete v následujících dvou vzorcích při kopírování dat z soubory JSON hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-238">See hello following two samples when copying data from JSON files.</span></span> <span data-ttu-id="bbe37-239">Obecné Hello body toonote:</span><span class="sxs-lookup"><span data-stu-id="bbe37-239">hello generic points toonote:</span></span>

<span data-ttu-id="bbe37-240">**Ukázka 1: Extrakce dat z objektu a pole**</span><span class="sxs-lookup"><span data-stu-id="bbe37-240">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="bbe37-241">V této ukázce očekáváte, že jeden kořenový objekt JSON mapuje toosingle záznam v tabulkovém výsledek.</span><span class="sxs-lookup"><span data-stu-id="bbe37-241">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="bbe37-242">Pokud máte soubor JSON s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="bbe37-242">If you have a JSON file with hello following content:</span></span>  

```json
{
    "id": "ed0e4960-d9c5-11e6-85dc-d7996816aad3",
    "context": {
        "device": {
            "type": "PC"
        },
        "custom": {
            "dimensions": [
                {
                    "TargetResourceType": "Microsoft.Compute/virtualMachines"
                },
                {
                    "ResourceManagmentProcessRunId": "827f8aaa-ab72-437c-ba48-d8917a7336a3"
                },
                {
                    "OccurrenceTime": "1/13/2017 11:24:37 AM"
                }
            ]
        }
    }
}
```
<span data-ttu-id="bbe37-243">a chcete ji do tabulky Azure SQL v následujících hello formátu podle extrahování dat z objekty a pole toocopy:</span><span class="sxs-lookup"><span data-stu-id="bbe37-243">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="bbe37-244">id</span><span class="sxs-lookup"><span data-stu-id="bbe37-244">id</span></span> | <span data-ttu-id="bbe37-245">deviceType</span><span class="sxs-lookup"><span data-stu-id="bbe37-245">deviceType</span></span> | <span data-ttu-id="bbe37-246">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="bbe37-246">targetResourceType</span></span> | <span data-ttu-id="bbe37-247">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="bbe37-247">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="bbe37-248">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="bbe37-248">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="bbe37-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="bbe37-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="bbe37-250">PC</span><span class="sxs-lookup"><span data-stu-id="bbe37-250">PC</span></span> | <span data-ttu-id="bbe37-251">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="bbe37-251">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="bbe37-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="bbe37-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="bbe37-253">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="bbe37-253">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="bbe37-254">vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="bbe37-254">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="bbe37-255">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="bbe37-255">More specifically:</span></span>

- <span data-ttu-id="bbe37-256">`structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data.</span><span class="sxs-lookup"><span data-stu-id="bbe37-256">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="bbe37-257">Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců.</span><span class="sxs-lookup"><span data-stu-id="bbe37-257">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="bbe37-258">V tématu [mapování zdrojových datovou sadu sloupců toodestination datovou sadu sloupců](data-factory-map-columns.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bbe37-258">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="bbe37-259">`jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z.</span><span class="sxs-lookup"><span data-stu-id="bbe37-259">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="bbe37-260">toocopy data z pole, můžete použít **pole [x] .property** tooextract hodnotu hello zadána vlastnost z objektu x hello, nebo můžete použít  **pole [*] .property** toofind Hodnota Hello z libovolného objektu, který obsahuje tato vlastnost.</span><span class="sxs-lookup"><span data-stu-id="bbe37-260">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "deviceType",
            "type": "String"
        },
        {
            "name": "targetResourceType",
            "type": "String"
        },
        {
            "name": "resourceManagmentProcessRunId",
            "type": "String"
        },
        {
            "name": "occurrenceTime",
            "type": "DateTime"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonPathDefinition": {"id": "$.id", "deviceType": "$.context.device.type", "targetResourceType": "$.context.custom.dimensions[0].TargetResourceType", "resourceManagmentProcessRunId": "$.context.custom.dimensions[1].ResourceManagmentProcessRunId", "occurrenceTime": " $.context.custom.dimensions[2].OccurrenceTime"}      
        }
    }
}
```

<span data-ttu-id="bbe37-261">**Příklad 2: mezi použít více objektů se stejný vzor z pole hello**</span><span class="sxs-lookup"><span data-stu-id="bbe37-261">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="bbe37-262">V této ukázce předpokládáte tootransform jeden kořenový objekt JSON do více záznamů v tabulkovém výsledek.</span><span class="sxs-lookup"><span data-stu-id="bbe37-262">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="bbe37-263">Pokud máte soubor JSON s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="bbe37-263">If you have a JSON file with hello following content:</span></span>  

```json
{
    "ordernumber": "01",
    "orderdate": "20170122",
    "orderlines": [
        {
            "prod": "p1",
            "price": 23
        },
        {
            "prod": "p2",
            "price": 13
        },
        {
            "prod": "p3",
            "price": 231
        }
    ],
    "city": [ { "sanmateo": "No 1" } ]
}
```
<span data-ttu-id="bbe37-264">a chcete ji do tabulky Azure SQL v následujících hello naformátovat pomocí sloučení hello data uvnitř pole hello toocopy a křížové spojení s informacemi kořenové běžné hello:</span><span class="sxs-lookup"><span data-stu-id="bbe37-264">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="bbe37-265">ordernumber</span><span class="sxs-lookup"><span data-stu-id="bbe37-265">ordernumber</span></span> | <span data-ttu-id="bbe37-266">orderdate</span><span class="sxs-lookup"><span data-stu-id="bbe37-266">orderdate</span></span> | <span data-ttu-id="bbe37-267">order_pd</span><span class="sxs-lookup"><span data-stu-id="bbe37-267">order_pd</span></span> | <span data-ttu-id="bbe37-268">order_price</span><span class="sxs-lookup"><span data-stu-id="bbe37-268">order_price</span></span> | <span data-ttu-id="bbe37-269">city</span><span class="sxs-lookup"><span data-stu-id="bbe37-269">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="bbe37-270">01</span><span class="sxs-lookup"><span data-stu-id="bbe37-270">01</span></span> | <span data-ttu-id="bbe37-271">20170122</span><span class="sxs-lookup"><span data-stu-id="bbe37-271">20170122</span></span> | <span data-ttu-id="bbe37-272">P1</span><span class="sxs-lookup"><span data-stu-id="bbe37-272">P1</span></span> | <span data-ttu-id="bbe37-273">23</span><span class="sxs-lookup"><span data-stu-id="bbe37-273">23</span></span> | <span data-ttu-id="bbe37-274">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="bbe37-274">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="bbe37-275">01</span><span class="sxs-lookup"><span data-stu-id="bbe37-275">01</span></span> | <span data-ttu-id="bbe37-276">20170122</span><span class="sxs-lookup"><span data-stu-id="bbe37-276">20170122</span></span> | <span data-ttu-id="bbe37-277">P2</span><span class="sxs-lookup"><span data-stu-id="bbe37-277">P2</span></span> | <span data-ttu-id="bbe37-278">13</span><span class="sxs-lookup"><span data-stu-id="bbe37-278">13</span></span> | <span data-ttu-id="bbe37-279">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="bbe37-279">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="bbe37-280">01</span><span class="sxs-lookup"><span data-stu-id="bbe37-280">01</span></span> | <span data-ttu-id="bbe37-281">20170122</span><span class="sxs-lookup"><span data-stu-id="bbe37-281">20170122</span></span> | <span data-ttu-id="bbe37-282">P3</span><span class="sxs-lookup"><span data-stu-id="bbe37-282">P3</span></span> | <span data-ttu-id="bbe37-283">231</span><span class="sxs-lookup"><span data-stu-id="bbe37-283">231</span></span> | <span data-ttu-id="bbe37-284">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="bbe37-284">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="bbe37-285">vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="bbe37-285">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="bbe37-286">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="bbe37-286">More specifically:</span></span>

- <span data-ttu-id="bbe37-287">`structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data.</span><span class="sxs-lookup"><span data-stu-id="bbe37-287">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="bbe37-288">Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců.</span><span class="sxs-lookup"><span data-stu-id="bbe37-288">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="bbe37-289">V tématu [mapování zdrojových datovou sadu sloupců toodestination datovou sadu sloupců](data-factory-map-columns.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bbe37-289">See [Map source dataset columns toodestination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="bbe37-290">`jsonNodeReference`označuje tooiterate a extrahování dat z objektů hello se stejný vzor pod hello **pole** orderlines.</span><span class="sxs-lookup"><span data-stu-id="bbe37-290">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="bbe37-291">`jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z.</span><span class="sxs-lookup"><span data-stu-id="bbe37-291">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="bbe37-292">V tomto příkladu "ordernumber", "orderdate" a "Město, jsou ve kořenový objekt s počínaje"$.", zatímco"order_pd"a"order_price"jsou definovány s cestou odvozen z elementu pole hello bez"$". cesta JSON.</span><span class="sxs-lookup"><span data-stu-id="bbe37-292">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "ordernumber",
            "type": "String"
        },
        {
            "name": "orderdate",
            "type": "String"
        },
        {
            "name": "order_pd",
            "type": "String"
        },
        {
            "name": "order_price",
            "type": "Int64"
        },
        {
            "name": "city",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat",
            "filePattern": "setOfObjects",
            "jsonNodeReference": "$.orderlines",
            "jsonPathDefinition": {"ordernumber": "$.ordernumber", "orderdate": "$.orderdate", "order_pd": "prod", "order_price": "price", "city": " $.city"}         
        }
    }
}
```

<span data-ttu-id="bbe37-293">**Všimněte si hello následující body:**</span><span class="sxs-lookup"><span data-stu-id="bbe37-293">**Note hello following points:**</span></span>

* <span data-ttu-id="bbe37-294">Pokud hello `structure` a `jsonPathDefinition` nejsou definovány v datové sadě služby Data Factory hello, hello aktivity kopírování zjistí hello schématu z první objekt hello a vyrovnání hello celý objekt.</span><span class="sxs-lookup"><span data-stu-id="bbe37-294">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="bbe37-295">Pokud hello vstupu JSON má pole, převede hello aktivity kopírování ve výchozím nastavení hello celé pole hodnotu na řetězec.</span><span class="sxs-lookup"><span data-stu-id="bbe37-295">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="bbe37-296">Můžete zvolit tooextract dat pomocí `jsonNodeReference` nebo `jsonPathDefinition`, nebo přeskočit zadáním není v `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="bbe37-296">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="bbe37-297">Pokud existují duplicitní názvy v hello stejné úrovni, hello aktivity kopírování vybere hello naposledy.</span><span class="sxs-lookup"><span data-stu-id="bbe37-297">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="bbe37-298">V názvech vlastností se rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="bbe37-298">Property names are case-sensitive.</span></span> <span data-ttu-id="bbe37-299">Vlastnosti se stejným názvem, ale různým použitím velkých a malých písmen, se budou považovat za různé.</span><span class="sxs-lookup"><span data-stu-id="bbe37-299">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="bbe37-300">**Případ 2: Zápis dat tooJSON souboru**</span><span class="sxs-lookup"><span data-stu-id="bbe37-300">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="bbe37-301">Pokud máte hello následující tabulka v databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="bbe37-301">If you have hello following table in SQL Database:</span></span>

| <span data-ttu-id="bbe37-302">id</span><span class="sxs-lookup"><span data-stu-id="bbe37-302">id</span></span> | <span data-ttu-id="bbe37-303">order_date</span><span class="sxs-lookup"><span data-stu-id="bbe37-303">order_date</span></span> | <span data-ttu-id="bbe37-304">order_price</span><span class="sxs-lookup"><span data-stu-id="bbe37-304">order_price</span></span> | <span data-ttu-id="bbe37-305">order_by</span><span class="sxs-lookup"><span data-stu-id="bbe37-305">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="bbe37-306">1</span><span class="sxs-lookup"><span data-stu-id="bbe37-306">1</span></span> | <span data-ttu-id="bbe37-307">20170119</span><span class="sxs-lookup"><span data-stu-id="bbe37-307">20170119</span></span> | <span data-ttu-id="bbe37-308">2000</span><span class="sxs-lookup"><span data-stu-id="bbe37-308">2000</span></span> | <span data-ttu-id="bbe37-309">David</span><span class="sxs-lookup"><span data-stu-id="bbe37-309">David</span></span> |
| <span data-ttu-id="bbe37-310">2</span><span class="sxs-lookup"><span data-stu-id="bbe37-310">2</span></span> | <span data-ttu-id="bbe37-311">20170120</span><span class="sxs-lookup"><span data-stu-id="bbe37-311">20170120</span></span> | <span data-ttu-id="bbe37-312">3500</span><span class="sxs-lookup"><span data-stu-id="bbe37-312">3500</span></span> | <span data-ttu-id="bbe37-313">Patrick</span><span class="sxs-lookup"><span data-stu-id="bbe37-313">Patrick</span></span> |
| <span data-ttu-id="bbe37-314">3</span><span class="sxs-lookup"><span data-stu-id="bbe37-314">3</span></span> | <span data-ttu-id="bbe37-315">20170121</span><span class="sxs-lookup"><span data-stu-id="bbe37-315">20170121</span></span> | <span data-ttu-id="bbe37-316">4000</span><span class="sxs-lookup"><span data-stu-id="bbe37-316">4000</span></span> | <span data-ttu-id="bbe37-317">Jason</span><span class="sxs-lookup"><span data-stu-id="bbe37-317">Jason</span></span> |

<span data-ttu-id="bbe37-318">a pro každý záznam očekáváte objekt JSON tooa toowrite hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="bbe37-318">and for each record, you expect toowrite tooa JSON object in hello following format:</span></span>
```json
{
    "id": "1",
    "order": {
        "date": "20170119",
        "price": 2000,
        "customer": "David"
    }
}
```

<span data-ttu-id="bbe37-319">Hello výstupní datovou sadu s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="bbe37-319">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="bbe37-320">Přesněji řečeno `structure` oddíl definuje názvy vlastností hello přizpůsobit v cílovém souboru `nestingSeparator` (výchozí hodnota je ".") jsou použité tooidentify hello vnoření vrstvu ze hello název.</span><span class="sxs-lookup"><span data-stu-id="bbe37-320">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") are used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="bbe37-321">Tato část se **volitelné** Pokud název vlastnosti hello toochange porovnávání s název zdrojového sloupce, nebo se některé vlastnosti hello vnořit.</span><span class="sxs-lookup"><span data-stu-id="bbe37-321">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

```json
"properties": {
    "structure": [
        {
            "name": "id",
            "type": "String"
        },
        {
            "name": "order.date",
            "type": "String"
        },
        {
            "name": "order.price",
            "type": "Int64"
        },
        {
            "name": "order.customer",
            "type": "String"
        }
    ],
    "typeProperties": {
        "folderPath": "mycontainer/myfolder",
        "format": {
            "type": "JsonFormat"
        }
    }
}
```

## <a name="avro-format"></a><span data-ttu-id="bbe37-322">Formát AVRO</span><span class="sxs-lookup"><span data-stu-id="bbe37-322">AVRO format</span></span>
<span data-ttu-id="bbe37-323">Chcete-li tooparse hello Avro soubory nebo zapisovat hello data ve formátu Avro, nastavte hello `format` `type` vlastnost příliš**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-323">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="bbe37-324">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-324">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="bbe37-325">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bbe37-325">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="bbe37-326">Formát Avro toouse v tabulce Hive, naleznete v části příliš[kurz Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="bbe37-326">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="bbe37-327">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="bbe37-327">Note hello following points:</span></span>  

* <span data-ttu-id="bbe37-328">[Komplexní datové typy](http://avro.apache.org/docs/current/spec.html#schema_complex) nejsou podporovány (zaznamenává výčty, pole, map, sjednocení a pevné).</span><span class="sxs-lookup"><span data-stu-id="bbe37-328">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions, and fixed).</span></span>

## <a name="orc-format"></a><span data-ttu-id="bbe37-329">Formát ORC</span><span class="sxs-lookup"><span data-stu-id="bbe37-329">ORC format</span></span>
<span data-ttu-id="bbe37-330">Chcete-li tooparse hello ORC soubory nebo zapisovat hello data ve formátu ORC, nastavte hello `format` `type` vlastnost příliš**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-330">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="bbe37-331">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-331">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="bbe37-332">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bbe37-332">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="bbe37-333">Pokud nejsou kopírování souborů ORC **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime).</span><span class="sxs-lookup"><span data-stu-id="bbe37-333">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="bbe37-334">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="bbe37-334">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="bbe37-335">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="bbe37-335">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="bbe37-336">Vyberte příslušné hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-336">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="bbe37-337">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="bbe37-337">Note hello following points:</span></span>

* <span data-ttu-id="bbe37-338">Komplexní datové typy se nepodporují (STRUCT, MAP, LIST, UNION).</span><span class="sxs-lookup"><span data-stu-id="bbe37-338">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="bbe37-339">Soubory ORC mají tři [možnosti komprese](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB a SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="bbe37-339">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="bbe37-340">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="bbe37-340">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="bbe37-341">Používá kompresi hello kodeků je v datech hello tooread metadata hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-341">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="bbe37-342">Ale při zápisu souboru ORC tooan, Data Factory zvolí ZLIB, což je výchozí hodnotou hello ORC.</span><span class="sxs-lookup"><span data-stu-id="bbe37-342">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="bbe37-343">V současné době není žádná možnost toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="bbe37-343">Currently, there is no option toooverride this behavior.</span></span>

## <a name="parquet-format"></a><span data-ttu-id="bbe37-344">Formát parquet</span><span class="sxs-lookup"><span data-stu-id="bbe37-344">Parquet format</span></span>
<span data-ttu-id="bbe37-345">Chcete-li tooparse hello Parquet soubory nebo zapisovat hello data ve formátu Parquet, nastavte hello `format` `type` vlastnost příliš**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-345">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="bbe37-346">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-346">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="bbe37-347">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bbe37-347">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="bbe37-348">Pokud nejsou kopírování souborů Parquet **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime).</span><span class="sxs-lookup"><span data-stu-id="bbe37-348">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="bbe37-349">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="bbe37-349">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="bbe37-350">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="bbe37-350">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="bbe37-351">Vyberte příslušné hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-351">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="bbe37-352">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="bbe37-352">Note hello following points:</span></span>

* <span data-ttu-id="bbe37-353">Komplexní datové typy se nepodporují (MAP, LIST).</span><span class="sxs-lookup"><span data-stu-id="bbe37-353">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="bbe37-354">Soubor parquet má následující možnosti týkající se komprese hello: NONE, SNAPPY, GZIP a LZO.</span><span class="sxs-lookup"><span data-stu-id="bbe37-354">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="bbe37-355">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="bbe37-355">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="bbe37-356">Kompresní kodek hello používá v hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="bbe37-356">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="bbe37-357">Ale při zápisu souboru Parquet tooa, Data Factory zvolí SNAPPY, což je výchozí hello Parquet formát.</span><span class="sxs-lookup"><span data-stu-id="bbe37-357">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="bbe37-358">V současné době není žádná možnost toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="bbe37-358">Currently, there is no option toooverride this behavior.</span></span>

## <a name="compression-support"></a><span data-ttu-id="bbe37-359">Podporu komprese</span><span class="sxs-lookup"><span data-stu-id="bbe37-359">Compression support</span></span>
<span data-ttu-id="bbe37-360">Zpracování velkých datových sad může způsobit vstupně-výstupních operací a sítě kritická místa.</span><span class="sxs-lookup"><span data-stu-id="bbe37-360">Processing large data sets can cause I/O and network bottlenecks.</span></span> <span data-ttu-id="bbe37-361">Proto komprimovaná data v úložištích můžete nejen urychlit přenos dat v síti hello a ušetřit místo na disku, ale také uvést významné zlepšení výkonu při zpracování velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="bbe37-361">Therefore, compressed data in stores can not only speed up data transfer across hello network and save disk space, but also bring significant performance improvements in processing big data.</span></span> <span data-ttu-id="bbe37-362">Komprese je aktuálně podporované pro úložiště dat na základě souborů například objektů Blob v Azure nebo v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="bbe37-362">Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.</span></span>  

<span data-ttu-id="bbe37-363">toospecify komprese datové sady, použijte hello **komprese** vlastnost v datové sadě hello JSON jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="bbe37-363">toospecify compression for a dataset, use hello **compression** property in hello dataset JSON as in hello following example:</span></span>   

```json
{  
    "name": "AzureBlobDataSet",  
    "properties": {  
        "availability": {  
            "frequency": "Day",  
              "interval": 1  
        },  
        "type": "AzureBlob",  
        "linkedServiceName": "StorageLinkedService",  
        "typeProperties": {  
            "fileName": "pagecounts.csv.gz",  
            "folderPath": "compression/file/",  
            "compression": {  
                "type": "GZip",  
                "level": "Optimal"  
            }  
        }  
    }  
}  
```

<span data-ttu-id="bbe37-364">Předpokládejme, že hello ukázkovou datovou sadu se používá jako výstup hello aktivitu kopírování, hello kopie aktivity zkomprimuje hello výstupní data s GZIP kodeků pomocí optimální poměr a zapište si hello komprimována data do souboru s názvem pagecounts.csv.gz v hello Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="bbe37-364">Suppose hello sample dataset is used as hello output of a copy activity, hello copy activity compresses hello output data with GZIP codec using optimal ratio and then write hello compressed data into a file named pagecounts.csv.gz in hello Azure Blob Storage.</span></span>

> [!NOTE]
> <span data-ttu-id="bbe37-365">Nastavení komprese nejsou podporovány pro data v hello **AvroFormat**, **OrcFormat**, nebo **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-365">Compression settings are not supported for data in hello **AvroFormat**, **OrcFormat**, or **ParquetFormat**.</span></span> <span data-ttu-id="bbe37-366">Při čtení souborů v těchto formátů, Data Factory zjišťuje a používá hello kompresní kodek v metadatech hello.</span><span class="sxs-lookup"><span data-stu-id="bbe37-366">When reading files in these formats, Data Factory detects and uses hello compression codec in hello metadata.</span></span> <span data-ttu-id="bbe37-367">Při zápisu toofiles v těchto formátů, Data Factory zvolí hello výchozí kompresní kodek pro tento formát.</span><span class="sxs-lookup"><span data-stu-id="bbe37-367">When writing toofiles in these formats, Data Factory chooses hello default compression codec for that format.</span></span> <span data-ttu-id="bbe37-368">Například ZLIB OrcFormat a SNAPPY pro ParquetFormat.</span><span class="sxs-lookup"><span data-stu-id="bbe37-368">For example, ZLIB for OrcFormat and SNAPPY for ParquetFormat.</span></span>   

<span data-ttu-id="bbe37-369">Hello **komprese** části má dvě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="bbe37-369">hello **compression** section has two properties:</span></span>  

* <span data-ttu-id="bbe37-370">**Typ:** kodeků hello kompresi, což může být **GZIP**, **Deflate**, **BZIP2**, nebo **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-370">**Type:** hello compression codec, which can be **GZIP**, **Deflate**, **BZIP2**, or **ZipDeflate**.</span></span>  
* <span data-ttu-id="bbe37-371">**Úroveň:** hello kompresní poměr, což může být **Optimal** nebo **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="bbe37-371">**Level:** hello compression ratio, which can be **Optimal** or **Fastest**.</span></span>

  * <span data-ttu-id="bbe37-372">**Nejrychlejší:** hello komprese operace by se měla dokončit co nejrychleji, i když hello výsledný soubor není komprimována optimálně.</span><span class="sxs-lookup"><span data-stu-id="bbe37-372">**Fastest:** hello compression operation should complete as quickly as possible, even if hello resulting file is not optimally compressed.</span></span>
  * <span data-ttu-id="bbe37-373">**Optimální**: hello komprese operace by měla být optimálně komprimován, i v případě, že operace hello trvá delší dobu toocomplete.</span><span class="sxs-lookup"><span data-stu-id="bbe37-373">**Optimal**: hello compression operation should be optimally compressed, even if hello operation takes a longer time toocomplete.</span></span>

    <span data-ttu-id="bbe37-374">Další informace najdete v tématu [úroveň komprese](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="bbe37-374">For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic.</span></span>

<span data-ttu-id="bbe37-375">Pokud zadáte `compression` vlastnost ve vstupní datové sady JSON, hello kanálu může číst komprimovaná data ze zdroje hello; a po zadání vlastnosti hello v datové sadě služby výstup JSON, aktivity kopírování hello může zapsat cílový toohello komprimovaná data.</span><span class="sxs-lookup"><span data-stu-id="bbe37-375">When you specify `compression` property in an input dataset JSON, hello pipeline can read compressed data from hello source; and when you specify hello property in an output dataset JSON, hello copy activity can write compressed data toohello destination.</span></span> <span data-ttu-id="bbe37-376">Tady je několik ukázkových scénářů:</span><span class="sxs-lookup"><span data-stu-id="bbe37-376">Here are a few sample scenarios:</span></span>

* <span data-ttu-id="bbe37-377">Čtení GZIP komprimované dat z Azure blob, dekomprimovat ho a zapíše výsledek data tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="bbe37-377">Read GZIP compressed data from an Azure blob, decompress it, and write result data tooan Azure SQL database.</span></span> <span data-ttu-id="bbe37-378">Můžete definovat hello vstupní Azure datovou sadu objektu Blob s hello `compression` `type` JSON vlastnost jako GZIP.</span><span class="sxs-lookup"><span data-stu-id="bbe37-378">You define hello input Azure Blob dataset with hello `compression` `type` JSON property as GZIP.</span></span>
* <span data-ttu-id="bbe37-379">Čtení dat ze souboru ve formátu prostého textu z místního systému souborů, je pomocí formátu GZip komprimovat a zapíše tooan hello komprimována data objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe37-379">Read data from a plain-text file from on-premises File System, compress it using GZip format, and write hello compressed data tooan Azure blob.</span></span> <span data-ttu-id="bbe37-380">Definujte výstupní datové Azure Blob s hello `compression` `type` JSON vlastnost jako GZip.</span><span class="sxs-lookup"><span data-stu-id="bbe37-380">You define an output Azure Blob dataset with hello `compression` `type` JSON property as GZip.</span></span>
* <span data-ttu-id="bbe37-381">Soubor ZIP číst ze serveru FTP, dekomprimovat ho tooget hello soubory uvnitř a zobrazovat tyto soubory do Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="bbe37-381">Read .zip file from FTP server, decompress it tooget hello files inside, and land those files into Azure Data Lake Store.</span></span> <span data-ttu-id="bbe37-382">Definujete vstupní datové sady FTP s hello `compression` `type` JSON vlastnost jako ZipDeflate.</span><span class="sxs-lookup"><span data-stu-id="bbe37-382">You define an input FTP dataset with hello `compression` `type` JSON property as ZipDeflate.</span></span>
* <span data-ttu-id="bbe37-383">Číst kompresí GZIP dat z Azure blob, dekomprimovat ho, je pomocí BZIP2 komprimovat a zapíše výsledek tooan data objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="bbe37-383">Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data tooan Azure blob.</span></span> <span data-ttu-id="bbe37-384">Můžete definovat hello vstupní Azure datovou sadu objektu Blob s `compression` `type` nastavte tooGZIP a hello výstupní datovou sadu s `compression` `type` v takovém případě nastavte tooBZIP2.</span><span class="sxs-lookup"><span data-stu-id="bbe37-384">You define hello input Azure Blob dataset with `compression` `type` set tooGZIP and hello output dataset with `compression` `type` set tooBZIP2 in this case.</span></span>   


## <a name="next-steps"></a><span data-ttu-id="bbe37-385">Další kroky</span><span class="sxs-lookup"><span data-stu-id="bbe37-385">Next steps</span></span>
<span data-ttu-id="bbe37-386">Viz následující články pro úložiště dat na základě souborů podporovaných službou Azure Data Factory hello:</span><span class="sxs-lookup"><span data-stu-id="bbe37-386">See hello following articles for file-based data stores supported by Azure Data Factory:</span></span>

- [<span data-ttu-id="bbe37-387">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="bbe37-387">Azure Blob Storage</span></span>](data-factory-azure-blob-connector.md)
- [<span data-ttu-id="bbe37-388">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="bbe37-388">Azure Data Lake Store</span></span>](data-factory-azure-datalake-connector.md)
- [<span data-ttu-id="bbe37-389">FTP</span><span class="sxs-lookup"><span data-stu-id="bbe37-389">FTP</span></span>](data-factory-ftp-connector.md)
- [<span data-ttu-id="bbe37-390">HDFS</span><span class="sxs-lookup"><span data-stu-id="bbe37-390">HDFS</span></span>](data-factory-hdfs-connector.md)
- [<span data-ttu-id="bbe37-391">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="bbe37-391">File System</span></span>](data-factory-onprem-file-system-connector.md)
- [<span data-ttu-id="bbe37-392">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="bbe37-392">Amazon S3</span></span>](data-factory-amazon-simple-storage-service-connector.md)
