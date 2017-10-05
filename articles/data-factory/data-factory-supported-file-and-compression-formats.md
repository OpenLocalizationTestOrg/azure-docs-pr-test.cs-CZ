---
title: "Formáty souborů a komprese v Azure Data Factory | Microsoft Docs"
description: "Další informace o formátech souborů podporovaných službou Azure Data Factory."
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
ms.openlocfilehash: f4746e0dd249e417b8077a9bc733d2886daafdf2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="file-and-compression-formats-supported-by-azure-data-factory"></a><span data-ttu-id="df446-104">Formáty souborů a komprese podporovaných službou Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="df446-104">File and compression formats supported by Azure Data Factory</span></span>
<span data-ttu-id="df446-105">*Toto téma se vztahuje na následující konektory: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [objektů Blob v Azure](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [systém souborů](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), a [SFTP](data-factory-sftp-connector.md).*</span><span class="sxs-lookup"><span data-stu-id="df446-105">*This topic applies to the following connectors: [Amazon S3](data-factory-amazon-simple-storage-service-connector.md), [Azure Blob](data-factory-azure-blob-connector.md), [Azure Data Lake Store](data-factory-azure-datalake-connector.md), [File System](data-factory-onprem-file-system-connector.md), [FTP](data-factory-ftp-connector.md), [HDFS](data-factory-hdfs-connector.md), [HTTP](data-factory-http-connector.md), and [SFTP](data-factory-sftp-connector.md).*</span></span>

<span data-ttu-id="df446-106">Azure Data Factory podporuje následující typy souboru formátu:</span><span class="sxs-lookup"><span data-stu-id="df446-106">Azure Data Factory supports the following file format types:</span></span>

* [<span data-ttu-id="df446-107">Textovém formátu</span><span class="sxs-lookup"><span data-stu-id="df446-107">Text format</span></span>](#text-format)
* [<span data-ttu-id="df446-108">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="df446-108">JSON format</span></span>](#json-format)
* [<span data-ttu-id="df446-109">Formát Avro</span><span class="sxs-lookup"><span data-stu-id="df446-109">Avro format</span></span>](#avro-format)
* [<span data-ttu-id="df446-110">Formát ORC</span><span class="sxs-lookup"><span data-stu-id="df446-110">ORC format</span></span>](#orc-format)
* [<span data-ttu-id="df446-111">Formát parquet</span><span class="sxs-lookup"><span data-stu-id="df446-111">Parquet format</span></span>](#parquet-format)

## <a name="text-format"></a><span data-ttu-id="df446-112">Textovém formátu</span><span class="sxs-lookup"><span data-stu-id="df446-112">Text format</span></span>
<span data-ttu-id="df446-113">Pokud chcete pro čtení z textového souboru nebo zápis do textového souboru, nastavte `type` vlastnost `format` části datové sady, která **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="df446-113">If you want to read from a text file or write to a text file, set the `type` property in the `format` section of the dataset to **TextFormat**.</span></span> <span data-ttu-id="df446-114">Můžete také zadat následující **nepovinné** vlastnosti v oddílu `format`.</span><span class="sxs-lookup"><span data-stu-id="df446-114">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="df446-115">Postup konfigurace najdete v části [Příklad typu TextFormat](#textformat-example).</span><span class="sxs-lookup"><span data-stu-id="df446-115">See [TextFormat example](#textformat-example) section on how to configure.</span></span>

| <span data-ttu-id="df446-116">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="df446-116">Property</span></span> | <span data-ttu-id="df446-117">Popis</span><span class="sxs-lookup"><span data-stu-id="df446-117">Description</span></span> | <span data-ttu-id="df446-118">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="df446-118">Allowed values</span></span> | <span data-ttu-id="df446-119">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="df446-119">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="df446-120">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="df446-120">columnDelimiter</span></span> |<span data-ttu-id="df446-121">Znak, který slouží k oddělení sloupců v souboru.</span><span class="sxs-lookup"><span data-stu-id="df446-121">The character used to separate columns in a file.</span></span> <span data-ttu-id="df446-122">Můžete zvážit použití výjimečných netisknutelné char, který nemusí již pravděpodobně existuje ve vašich datech.</span><span class="sxs-lookup"><span data-stu-id="df446-122">You can consider to use a rare unprintable char that may not likely exists in your data.</span></span> <span data-ttu-id="df446-123">Můžete například zadejte "\u0001", která představuje spustit z záhlaví (SOH).</span><span class="sxs-lookup"><span data-stu-id="df446-123">For example, specify "\u0001", which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="df446-124">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="df446-124">Only one character is allowed.</span></span> <span data-ttu-id="df446-125">**Výchozí** hodnota je **čárka (,)**.</span><span class="sxs-lookup"><span data-stu-id="df446-125">The **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="df446-126">Chcete-li použít znak Unicode, přečtěte [znaky znakové sady Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) získat odpovídající kód pro ni.</span><span class="sxs-lookup"><span data-stu-id="df446-126">To use a Unicode character, refer to [Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) to get the corresponding code for it.</span></span> |<span data-ttu-id="df446-127">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-127">No</span></span> |
| <span data-ttu-id="df446-128">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="df446-128">rowDelimiter</span></span> |<span data-ttu-id="df446-129">Znak, který slouží k oddělení řádků v souboru.</span><span class="sxs-lookup"><span data-stu-id="df446-129">The character used to separate rows in a file.</span></span> |<span data-ttu-id="df446-130">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="df446-130">Only one character is allowed.</span></span> <span data-ttu-id="df446-131">**Výchozí** hodnotou pro čtení může být libovolná z těchto hodnot: **[\r\n, \r, \n]** a pro zápis hodnota **\r\n**.</span><span class="sxs-lookup"><span data-stu-id="df446-131">The **default** value is any of the following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="df446-132">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-132">No</span></span> |
| <span data-ttu-id="df446-133">escapeChar</span><span class="sxs-lookup"><span data-stu-id="df446-133">escapeChar</span></span> |<span data-ttu-id="df446-134">Speciální znak, který slouží k potlačení oddělovače sloupců v obsahu vstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="df446-134">The special character used to escape a column delimiter in the content of input file.</span></span> <br/><br/><span data-ttu-id="df446-135">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="df446-135">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="df446-136">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="df446-136">Only one character is allowed.</span></span> <span data-ttu-id="df446-137">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="df446-137">No default value.</span></span> <br/><br/><span data-ttu-id="df446-138">Příklad: Pokud jako oddělovač sloupců používáte čárku (,), ale chcete znak čárky použít v textu (příklad: Hello, world), můžete jako řídicí znak definovat $ a použít ve zdroji řetězec Hello$, world.</span><span class="sxs-lookup"><span data-stu-id="df446-138">Example: if you have comma (',') as the column delimiter but you want to have the comma character in the text (example: "Hello, world"), you can define ‘$’ as the escape character and use string "Hello$, world" in the source.</span></span> |<span data-ttu-id="df446-139">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-139">No</span></span> |
| <span data-ttu-id="df446-140">quoteChar</span><span class="sxs-lookup"><span data-stu-id="df446-140">quoteChar</span></span> |<span data-ttu-id="df446-141">Znak, který slouží k uvození textového řetězce.</span><span class="sxs-lookup"><span data-stu-id="df446-141">The character used to quote a string value.</span></span> <span data-ttu-id="df446-142">Oddělovače sloupců a řádků uvnitř znaků uvozovek budou považované za součást hodnoty příslušného řetězce.</span><span class="sxs-lookup"><span data-stu-id="df446-142">The column and row delimiters inside the quote characters would be treated as part of the string value.</span></span> <span data-ttu-id="df446-143">Tato vlastnost se vztahuje na vstupní i výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="df446-143">This property is applicable to both input and output datasets.</span></span><br/><br/><span data-ttu-id="df446-144">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="df446-144">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="df446-145">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="df446-145">Only one character is allowed.</span></span> <span data-ttu-id="df446-146">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="df446-146">No default value.</span></span> <br/><br/><span data-ttu-id="df446-147">Příklad: Pokud jako oddělovač sloupců používáte čárku (,), ale chcete znak čárky použít v textu (příklad: <Hello, world>), můžete jako znak uvozovek definovat " (dvojité uvozovky) a použít ve zdroji řetězec "Hello$, world".</span><span class="sxs-lookup"><span data-stu-id="df446-147">For example, if you have comma (',') as the column delimiter but you want to have comma character in the text (example: <Hello, world>), you can define " (double quote) as the quote character and use the string "Hello, world" in the source.</span></span> |<span data-ttu-id="df446-148">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-148">No</span></span> |
| <span data-ttu-id="df446-149">nullValue</span><span class="sxs-lookup"><span data-stu-id="df446-149">nullValue</span></span> |<span data-ttu-id="df446-150">Jeden nebo několik znaků, které se používají jako reprezentace hodnoty Null.</span><span class="sxs-lookup"><span data-stu-id="df446-150">One or more characters used to represent a null value.</span></span> |<span data-ttu-id="df446-151">Jeden nebo několik znaků.</span><span class="sxs-lookup"><span data-stu-id="df446-151">One or more characters.</span></span> <span data-ttu-id="df446-152">**Výchozí** hodnoty jsou **\N a NULL** pro čtení a **\N** pro zápis.</span><span class="sxs-lookup"><span data-stu-id="df446-152">The **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="df446-153">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-153">No</span></span> |
| <span data-ttu-id="df446-154">encodingName</span><span class="sxs-lookup"><span data-stu-id="df446-154">encodingName</span></span> |<span data-ttu-id="df446-155">Zadejte název kódování.</span><span class="sxs-lookup"><span data-stu-id="df446-155">Specify the encoding name.</span></span> |<span data-ttu-id="df446-156">Platný název kódování.</span><span class="sxs-lookup"><span data-stu-id="df446-156">A valid encoding name.</span></span> <span data-ttu-id="df446-157">Další informace najdete v tématu [Vlastnost Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="df446-157">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="df446-158">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="df446-158">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="df446-159">**Výchozí** hodnota je **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="df446-159">The **default** value is **UTF-8**.</span></span> |<span data-ttu-id="df446-160">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-160">No</span></span> |
| <span data-ttu-id="df446-161">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="df446-161">firstRowAsHeader</span></span> |<span data-ttu-id="df446-162">Určuje, jestli se má první řádek považovat za záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-162">Specifies whether to consider the first row as a header.</span></span> <span data-ttu-id="df446-163">U vstupní datové sady Data Factory načítá první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-163">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="df446-164">U výstupní datové sady Data Factory zapisuje první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-164">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="df446-165">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="df446-165">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="df446-166">True</span><span class="sxs-lookup"><span data-stu-id="df446-166">True</span></span><br/><span data-ttu-id="df446-167"><b>False (výchozí)</b></span><span class="sxs-lookup"><span data-stu-id="df446-167"><b>False (default)</b></span></span> |<span data-ttu-id="df446-168">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-168">No</span></span> |
| <span data-ttu-id="df446-169">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="df446-169">skipLineCount</span></span> |<span data-ttu-id="df446-170">Určuje počet řádků, které se při čtení dat ze vstupních souborů mají přeskočit.</span><span class="sxs-lookup"><span data-stu-id="df446-170">Indicates the number of rows to skip when reading data from input files.</span></span> <span data-ttu-id="df446-171">Pokud je zadaný parametr skipLineCount i firstRowAsHeader, nejdřív se přeskočí příslušný počet řádků a potom se ze vstupního souboru načtou informace záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-171">If both skipLineCount and firstRowAsHeader are specified, the lines are skipped first and then the header information is read from the input file.</span></span> <br/><br/><span data-ttu-id="df446-172">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="df446-172">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="df446-173">Integer</span><span class="sxs-lookup"><span data-stu-id="df446-173">Integer</span></span> |<span data-ttu-id="df446-174">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-174">No</span></span> |
| <span data-ttu-id="df446-175">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="df446-175">treatEmptyAsNull</span></span> |<span data-ttu-id="df446-176">Určuje, jestli se při čtení dat ze vstupního souboru má prázdný řetězec nebo řetězec s hodnotou null považovat za hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="df446-176">Specifies whether to treat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="df446-177">**True (výchozí)**</span><span class="sxs-lookup"><span data-stu-id="df446-177">**True (default)**</span></span><br/><span data-ttu-id="df446-178">False</span><span class="sxs-lookup"><span data-stu-id="df446-178">False</span></span> |<span data-ttu-id="df446-179">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-179">No</span></span> |

### <a name="textformat-example"></a><span data-ttu-id="df446-180">Příklad typu TextFormat</span><span class="sxs-lookup"><span data-stu-id="df446-180">TextFormat example</span></span>
<span data-ttu-id="df446-181">V následující definici JSON pro datovou sadu některé volitelné vlastnosti nejsou zadány.</span><span class="sxs-lookup"><span data-stu-id="df446-181">In the following JSON definition for a dataset, some of the optional properties are specified.</span></span>

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

<span data-ttu-id="df446-182">Pokud chcete místo `quoteChar` použít `escapeChar`, nahraďte řádek s `quoteChar` touto hodnotou escapeChar:</span><span class="sxs-lookup"><span data-stu-id="df446-182">To use an `escapeChar` instead of `quoteChar`, replace the line with `quoteChar` with the following escapeChar:</span></span>

```json
"escapeChar": "$",
```

### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="df446-183">Scénáře použití firstRowAsHeader a skipLineCount</span><span class="sxs-lookup"><span data-stu-id="df446-183">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="df446-184">Kopírujete z nesouborového zdroje do textového souboru a chcete přidat řádek záhlaví obsahující metadata schématu (třeba schéma SQL).</span><span class="sxs-lookup"><span data-stu-id="df446-184">You are copying from a non-file source to a text file and would like to add a header line containing the schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="df446-185">V tomto scénáři zadejte ve vstupní sadě `firstRowAsHeader` jako true.</span><span class="sxs-lookup"><span data-stu-id="df446-185">Specify `firstRowAsHeader` as true in the output dataset for this scenario.</span></span>
* <span data-ttu-id="df446-186">Kopírujete text z textového souboru obsahujícího řádek záhlaví do nesouborové jímky a chcete tento řádek vynechat.</span><span class="sxs-lookup"><span data-stu-id="df446-186">You are copying from a text file containing a header line to a non-file sink and would like to drop that line.</span></span> <span data-ttu-id="df446-187">Zadejte ve vstupní sadě `firstRowAsHeader` jako true.</span><span class="sxs-lookup"><span data-stu-id="df446-187">Specify `firstRowAsHeader` as true in the input dataset.</span></span>
* <span data-ttu-id="df446-188">Kopírujete text z textového souboru a chcete vynechat několik prvních řádků, které neobsahují data ani informace záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-188">You are copying from a text file and want to skip a few lines at the beginning that contain no data or header information.</span></span> <span data-ttu-id="df446-189">Zadáním `skipLineCount` určete, kolik řádků se má přeskočit.</span><span class="sxs-lookup"><span data-stu-id="df446-189">Specify `skipLineCount` to indicate the number of lines to be skipped.</span></span> <span data-ttu-id="df446-190">Pokud zbytek souboru obsahuje řádek záhlaví, můžete také zadat `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="df446-190">If the rest of the file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="df446-191">Pokud je zadaný parametr `skipLineCount` i `firstRowAsHeader`, nejdřív se přeskočí příslušný počet řádků a potom se ze vstupního souboru načtou informace záhlaví.</span><span class="sxs-lookup"><span data-stu-id="df446-191">If both `skipLineCount` and `firstRowAsHeader` are specified, the lines are skipped first and then the header information is read from the input file</span></span>

## <a name="json-format"></a><span data-ttu-id="df446-192">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="df446-192">JSON format</span></span>
<span data-ttu-id="df446-193">K **importu a exportu souboru JSON jako-je do/z Azure Cosmos DB**, najdete v tématu [dokumentů JSON importu a exportu](data-factory-azure-documentdb-connector.md#importexport-json-documents) kapitoly [přesun dat do/z Azure Cosmos DB](data-factory-azure-documentdb-connector.md) článku.</span><span class="sxs-lookup"><span data-stu-id="df446-193">To **import/export a JSON file as-is into/from Azure Cosmos DB**, the see [Import/export JSON documents](data-factory-azure-documentdb-connector.md#importexport-json-documents) section in [Move data to/from Azure Cosmos DB](data-factory-azure-documentdb-connector.md) article.</span></span>

<span data-ttu-id="df446-194">Pokud chcete analyzovat JSON soubory nebo zapisovat data ve formátu JSON, nastavte `type` vlastnost `format` části k **JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="df446-194">If you want to parse the JSON files or write the data in JSON format, set the `type` property in the `format` section to **JsonFormat**.</span></span> <span data-ttu-id="df446-195">Můžete také zadat následující **nepovinné** vlastnosti v oddílu `format`.</span><span class="sxs-lookup"><span data-stu-id="df446-195">You can also specify the following **optional** properties in the `format` section.</span></span> <span data-ttu-id="df446-196">Postup konfigurace najdete v části [Příklad typu JsonFormat](#jsonformat-example).</span><span class="sxs-lookup"><span data-stu-id="df446-196">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span>

| <span data-ttu-id="df446-197">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="df446-197">Property</span></span> | <span data-ttu-id="df446-198">Popis</span><span class="sxs-lookup"><span data-stu-id="df446-198">Description</span></span> | <span data-ttu-id="df446-199">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="df446-199">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="df446-200">filePattern</span><span class="sxs-lookup"><span data-stu-id="df446-200">filePattern</span></span> |<span data-ttu-id="df446-201">Určete vzor dat uložených v jednotlivých souborech JSON.</span><span class="sxs-lookup"><span data-stu-id="df446-201">Indicate the pattern of data stored in each JSON file.</span></span> <span data-ttu-id="df446-202">Povolené hodnoty jsou **setOfObjects** a **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="df446-202">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="df446-203">**Výchozí hodnota** je **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="df446-203">The **default** value is **setOfObjects**.</span></span> <span data-ttu-id="df446-204">Podrobné informace o těchto vzorech najdete v tématu [Vzory souborů JSON](#json-file-patterns).</span><span class="sxs-lookup"><span data-stu-id="df446-204">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="df446-205">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-205">No</span></span> |
| <span data-ttu-id="df446-206">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="df446-206">jsonNodeReference</span></span> | <span data-ttu-id="df446-207">Pokud chcete iterovat a extrahovat data z objektů uvnitř pole se stejným vzorem, zadejte pro toto pole cestu JSON.</span><span class="sxs-lookup"><span data-stu-id="df446-207">If you want to iterate and extract data from the objects inside an array field with the same pattern, specify the JSON path of that array.</span></span> <span data-ttu-id="df446-208">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="df446-208">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="df446-209">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-209">No</span></span> |
| <span data-ttu-id="df446-210">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="df446-210">jsonPathDefinition</span></span> | <span data-ttu-id="df446-211">Zadejte výraz cesty JSON pro každé mapování sloupců s vlastním názvem sloupce (s počátečním malým písmenem).</span><span class="sxs-lookup"><span data-stu-id="df446-211">Specify the JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="df446-212">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON. Můžete extrahovat data z objektu nebo pole.</span><span class="sxs-lookup"><span data-stu-id="df446-212">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="df446-213">U polí v kořenovém objektu začtěte s kořenem $, u polí uvnitř pole vybraného pomocí vlastnosti `jsonNodeReference` začněte elementem pole.</span><span class="sxs-lookup"><span data-stu-id="df446-213">For fields under root object, start with root $; for fields inside the array chosen by `jsonNodeReference` property, start from the array element.</span></span> <span data-ttu-id="df446-214">Postup konfigurace najdete v části [Příklad typu JsonFormat](#jsonformat-example).</span><span class="sxs-lookup"><span data-stu-id="df446-214">See [JsonFormat example](#jsonformat-example) section on how to configure.</span></span> | <span data-ttu-id="df446-215">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-215">No</span></span> |
| <span data-ttu-id="df446-216">encodingName</span><span class="sxs-lookup"><span data-stu-id="df446-216">encodingName</span></span> |<span data-ttu-id="df446-217">Zadejte název kódování.</span><span class="sxs-lookup"><span data-stu-id="df446-217">Specify the encoding name.</span></span> <span data-ttu-id="df446-218">Seznam platných názvů kódování najdete v tématu věnovaném vlastnosti [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="df446-218">For the list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="df446-219">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="df446-219">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="df446-220">**Výchozí** hodnota je **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="df446-220">The **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="df446-221">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-221">No</span></span> |
| <span data-ttu-id="df446-222">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="df446-222">nestingSeparator</span></span> |<span data-ttu-id="df446-223">Znak, který se používá k oddělení úrovní vnoření.</span><span class="sxs-lookup"><span data-stu-id="df446-223">Character that is used to separate nesting levels.</span></span> <span data-ttu-id="df446-224">Výchozí hodnota je tečka (.).</span><span class="sxs-lookup"><span data-stu-id="df446-224">The default value is '.' (dot).</span></span> |<span data-ttu-id="df446-225">Ne</span><span class="sxs-lookup"><span data-stu-id="df446-225">No</span></span> |

### <a name="json-file-patterns"></a><span data-ttu-id="df446-226">Vzory souborů JSON</span><span class="sxs-lookup"><span data-stu-id="df446-226">JSON file patterns</span></span>

<span data-ttu-id="df446-227">Aktivita kopírování lze analyzovat následující vzorce soubory JSON:</span><span class="sxs-lookup"><span data-stu-id="df446-227">Copy activity can parse the following patterns of JSON files:</span></span>

- <span data-ttu-id="df446-228">**Typ I: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="df446-228">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="df446-229">Každý soubor obsahuje jeden objekt nebo několik objektů, které jsou zřetězené nebo oddělené řádkem.</span><span class="sxs-lookup"><span data-stu-id="df446-229">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="df446-230">Když je pro výstupní datovou sadu vybraná tato možnost, aktivita kopírování vytvoří jeden soubor JSON, ve kterém je každý objekt na jednom řádku (oddělení řádkem).</span><span class="sxs-lookup"><span data-stu-id="df446-230">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="df446-231">**Příklad JSON s jedním objektem**</span><span class="sxs-lookup"><span data-stu-id="df446-231">**single object JSON example**</span></span>

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

    * <span data-ttu-id="df446-232">**Příklad JSON s oddělením řádky**</span><span class="sxs-lookup"><span data-stu-id="df446-232">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="df446-233">**Příklad JSON se zřetězením**</span><span class="sxs-lookup"><span data-stu-id="df446-233">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="df446-234">**Typ II: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="df446-234">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="df446-235">Každý soubor obsahuje pole objektů.</span><span class="sxs-lookup"><span data-stu-id="df446-235">Each file contains an array of objects.</span></span>

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

### <a name="jsonformat-example"></a><span data-ttu-id="df446-236">Příklad typu JsonFormat</span><span class="sxs-lookup"><span data-stu-id="df446-236">JsonFormat example</span></span>

<span data-ttu-id="df446-237">**Případ 1: Kopírování dat ze souborů JSON**</span><span class="sxs-lookup"><span data-stu-id="df446-237">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="df446-238">Při kopírování dat z soubory JSON, najdete v následujících dvou vzorcích.</span><span class="sxs-lookup"><span data-stu-id="df446-238">See the following two samples when copying data from JSON files.</span></span> <span data-ttu-id="df446-239">Obecné body poznamenat:</span><span class="sxs-lookup"><span data-stu-id="df446-239">The generic points to note:</span></span>

<span data-ttu-id="df446-240">**Ukázka 1: Extrakce dat z objektu a pole**</span><span class="sxs-lookup"><span data-stu-id="df446-240">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="df446-241">V této ukázce očekáváte, že jeden kořenový objekt JSON se mapuje na jeden záznam v tabulkovém výsledku.</span><span class="sxs-lookup"><span data-stu-id="df446-241">In this sample, you expect one root JSON object maps to single record in tabular result.</span></span> <span data-ttu-id="df446-242">Pokud máte soubor JSON s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="df446-242">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="df446-243">a chcete ho zkopírovat do tabulky Azure SQL v následujícím formátu a přitom data uvnitř pole linearizovat, a to extrakcí dat z objektů i z pole:</span><span class="sxs-lookup"><span data-stu-id="df446-243">and you want to copy it into an Azure SQL table in the following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="df446-244">id</span><span class="sxs-lookup"><span data-stu-id="df446-244">id</span></span> | <span data-ttu-id="df446-245">deviceType</span><span class="sxs-lookup"><span data-stu-id="df446-245">deviceType</span></span> | <span data-ttu-id="df446-246">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="df446-246">targetResourceType</span></span> | <span data-ttu-id="df446-247">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="df446-247">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="df446-248">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="df446-248">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="df446-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="df446-249">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="df446-250">PC</span><span class="sxs-lookup"><span data-stu-id="df446-250">PC</span></span> | <span data-ttu-id="df446-251">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="df446-251">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="df446-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="df446-252">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="df446-253">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="df446-253">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="df446-254">Vstupní datová sada typu **JsonFormat** je definovaná následujícím způsobem (částečná definice obsahující jenom relevantní části).</span><span class="sxs-lookup"><span data-stu-id="df446-254">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="df446-255">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="df446-255">More specifically:</span></span>

- <span data-ttu-id="df446-256">Oddíl `structure` definuje vlastní názvy sloupců a odpovídající datový typ při převodu do tabulkového formátu.</span><span class="sxs-lookup"><span data-stu-id="df446-256">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="df446-257">Pokud mapování sloupců není potřeba, je tento oddíl **nepovinný**.</span><span class="sxs-lookup"><span data-stu-id="df446-257">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="df446-258">V tématu [mapování zdrojové sloupce datové sady na cílový datovou sadu sloupců](data-factory-map-columns.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="df446-258">See [Map source dataset columns to destination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="df446-259">`jsonPathDefinition` určuje cestu JSON pro jednotlivé sloupce a udává, odkud se mají extrahovat data.</span><span class="sxs-lookup"><span data-stu-id="df446-259">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="df446-260">Chcete-li kopírovat data z pole, můžete použít tvar **pole[x].vlastnost** a extrahovat hodnotu dané vlastnosti z x-tého objektu, nebo můžete použít tvar  **pole[*].vlastnost** a najít hodnotu v libovolném objektu, který obsahuje tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="df446-260">To copy data from array, you can use **array[x].property** to extract value of the given property from the xth object, or you can use **array[*].property** to find the value from any object containing such property.</span></span>

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

<span data-ttu-id="df446-261">**Ukázka 2: Křížové použití více objektů se stejným vzorkem z pole**</span><span class="sxs-lookup"><span data-stu-id="df446-261">**Sample 2: cross apply multiple objects with the same pattern from array**</span></span>

<span data-ttu-id="df446-262">V této ukázce očekáváte, že transformujete jeden kořenový objekt JSON do několika záznamů v tabulkovém výsledku.</span><span class="sxs-lookup"><span data-stu-id="df446-262">In this sample, you expect to transform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="df446-263">Pokud máte soubor JSON s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="df446-263">If you have a JSON file with the following content:</span></span>  

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
<span data-ttu-id="df446-264">a chcete ho zkopírovat do tabulky Azure SQL v následujícím formátu a přitom data uvnitř pole linearizovat a propojit je se společnými kořenovými informacemi:</span><span class="sxs-lookup"><span data-stu-id="df446-264">and you want to copy it into an Azure SQL table in the following format, by flattening the data inside the array and cross join with the common root info:</span></span>

| <span data-ttu-id="df446-265">ordernumber</span><span class="sxs-lookup"><span data-stu-id="df446-265">ordernumber</span></span> | <span data-ttu-id="df446-266">orderdate</span><span class="sxs-lookup"><span data-stu-id="df446-266">orderdate</span></span> | <span data-ttu-id="df446-267">order_pd</span><span class="sxs-lookup"><span data-stu-id="df446-267">order_pd</span></span> | <span data-ttu-id="df446-268">order_price</span><span class="sxs-lookup"><span data-stu-id="df446-268">order_price</span></span> | <span data-ttu-id="df446-269">city</span><span class="sxs-lookup"><span data-stu-id="df446-269">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="df446-270">01</span><span class="sxs-lookup"><span data-stu-id="df446-270">01</span></span> | <span data-ttu-id="df446-271">20170122</span><span class="sxs-lookup"><span data-stu-id="df446-271">20170122</span></span> | <span data-ttu-id="df446-272">P1</span><span class="sxs-lookup"><span data-stu-id="df446-272">P1</span></span> | <span data-ttu-id="df446-273">23</span><span class="sxs-lookup"><span data-stu-id="df446-273">23</span></span> | <span data-ttu-id="df446-274">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="df446-274">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="df446-275">01</span><span class="sxs-lookup"><span data-stu-id="df446-275">01</span></span> | <span data-ttu-id="df446-276">20170122</span><span class="sxs-lookup"><span data-stu-id="df446-276">20170122</span></span> | <span data-ttu-id="df446-277">P2</span><span class="sxs-lookup"><span data-stu-id="df446-277">P2</span></span> | <span data-ttu-id="df446-278">13</span><span class="sxs-lookup"><span data-stu-id="df446-278">13</span></span> | <span data-ttu-id="df446-279">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="df446-279">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="df446-280">01</span><span class="sxs-lookup"><span data-stu-id="df446-280">01</span></span> | <span data-ttu-id="df446-281">20170122</span><span class="sxs-lookup"><span data-stu-id="df446-281">20170122</span></span> | <span data-ttu-id="df446-282">P3</span><span class="sxs-lookup"><span data-stu-id="df446-282">P3</span></span> | <span data-ttu-id="df446-283">231</span><span class="sxs-lookup"><span data-stu-id="df446-283">231</span></span> | <span data-ttu-id="df446-284">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="df446-284">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="df446-285">Vstupní datová sada typu **JsonFormat** je definovaná následujícím způsobem (částečná definice obsahující jenom relevantní části).</span><span class="sxs-lookup"><span data-stu-id="df446-285">The input dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="df446-286">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="df446-286">More specifically:</span></span>

- <span data-ttu-id="df446-287">Oddíl `structure` definuje vlastní názvy sloupců a odpovídající datový typ při převodu do tabulkového formátu.</span><span class="sxs-lookup"><span data-stu-id="df446-287">`structure` section defines the customized column names and the corresponding data type while converting to tabular data.</span></span> <span data-ttu-id="df446-288">Pokud mapování sloupců není potřeba, je tento oddíl **nepovinný**.</span><span class="sxs-lookup"><span data-stu-id="df446-288">This section is **optional** unless you need to do column mapping.</span></span> <span data-ttu-id="df446-289">V tématu [mapování zdrojové sloupce datové sady na cílový datovou sadu sloupců](data-factory-map-columns.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="df446-289">See [Map source dataset columns to destination dataset columns](data-factory-map-columns.md) section for more details.</span></span>
- <span data-ttu-id="df446-290">`jsonNodeReference` určuje, že se budou iterovat a extrahovat data z objektů se stejným vzorem v rámci **pole** orderlines.</span><span class="sxs-lookup"><span data-stu-id="df446-290">`jsonNodeReference` indicates to iterate and extract data from the objects with the same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="df446-291">`jsonPathDefinition` určuje cestu JSON pro jednotlivé sloupce a udává, odkud se mají extrahovat data.</span><span class="sxs-lookup"><span data-stu-id="df446-291">`jsonPathDefinition` specifies the JSON path for each column indicating where to extract the data from.</span></span> <span data-ttu-id="df446-292">V tomto příkladu jsou položky ordernumber, orderdate a city v kořenovém objektu s cestou JSON, která začíná řetězcem „$.“. Oproti tomu order_pd a order_price jsou definované pomocí cesty odvozené z elementu pole bez řetězce „$.“.</span><span class="sxs-lookup"><span data-stu-id="df446-292">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from the array element without "$.".</span></span>

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

<span data-ttu-id="df446-293">**Je třeba počítat s následujícím:**</span><span class="sxs-lookup"><span data-stu-id="df446-293">**Note the following points:**</span></span>

* <span data-ttu-id="df446-294">Pokud v datové sadě Data Factory nejsou `structure` a `jsonPathDefinition` definované, aktivita kopírování detekuje schéma z prvního objektu a celý objekt linearizuje.</span><span class="sxs-lookup"><span data-stu-id="df446-294">If the `structure` and `jsonPathDefinition` are not defined in the Data Factory dataset, the Copy Activity detects the schema from the first object and flatten the whole object.</span></span>
* <span data-ttu-id="df446-295">Pokud vstup JSON obsahuje pole, aktivita kopírování ve výchozím nastavení převede celou hodnotu pole na řetězec.</span><span class="sxs-lookup"><span data-stu-id="df446-295">If the JSON input has an array, by default the Copy Activity converts the entire array value into a string.</span></span> <span data-ttu-id="df446-296">Můžete si vybrat, jestli z něj budete data extrahovat pomocí `jsonNodeReference` a/nebo `jsonPathDefinition`, nebo jestli ho přeskočíte tím, že ho nezadáte v `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="df446-296">You can choose to extract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="df446-297">Pokud je několik duplicitních názvů na stejné úrovni, aktivita kopírování vybere poslední z nich.</span><span class="sxs-lookup"><span data-stu-id="df446-297">If there are duplicate names at the same level, the Copy Activity picks the last one.</span></span>
* <span data-ttu-id="df446-298">V názvech vlastností se rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="df446-298">Property names are case-sensitive.</span></span> <span data-ttu-id="df446-299">Vlastnosti se stejným názvem, ale různým použitím velkých a malých písmen, se budou považovat za různé.</span><span class="sxs-lookup"><span data-stu-id="df446-299">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="df446-300">**Příklad 2: Zápis dat do souboru JSON**</span><span class="sxs-lookup"><span data-stu-id="df446-300">**Case 2: Writing data to JSON file**</span></span>

<span data-ttu-id="df446-301">Pokud máte v databázi SQL v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="df446-301">If you have the following table in SQL Database:</span></span>

| <span data-ttu-id="df446-302">id</span><span class="sxs-lookup"><span data-stu-id="df446-302">id</span></span> | <span data-ttu-id="df446-303">order_date</span><span class="sxs-lookup"><span data-stu-id="df446-303">order_date</span></span> | <span data-ttu-id="df446-304">order_price</span><span class="sxs-lookup"><span data-stu-id="df446-304">order_price</span></span> | <span data-ttu-id="df446-305">order_by</span><span class="sxs-lookup"><span data-stu-id="df446-305">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="df446-306">1</span><span class="sxs-lookup"><span data-stu-id="df446-306">1</span></span> | <span data-ttu-id="df446-307">20170119</span><span class="sxs-lookup"><span data-stu-id="df446-307">20170119</span></span> | <span data-ttu-id="df446-308">2000</span><span class="sxs-lookup"><span data-stu-id="df446-308">2000</span></span> | <span data-ttu-id="df446-309">David</span><span class="sxs-lookup"><span data-stu-id="df446-309">David</span></span> |
| <span data-ttu-id="df446-310">2</span><span class="sxs-lookup"><span data-stu-id="df446-310">2</span></span> | <span data-ttu-id="df446-311">20170120</span><span class="sxs-lookup"><span data-stu-id="df446-311">20170120</span></span> | <span data-ttu-id="df446-312">3500</span><span class="sxs-lookup"><span data-stu-id="df446-312">3500</span></span> | <span data-ttu-id="df446-313">Patrick</span><span class="sxs-lookup"><span data-stu-id="df446-313">Patrick</span></span> |
| <span data-ttu-id="df446-314">3</span><span class="sxs-lookup"><span data-stu-id="df446-314">3</span></span> | <span data-ttu-id="df446-315">20170121</span><span class="sxs-lookup"><span data-stu-id="df446-315">20170121</span></span> | <span data-ttu-id="df446-316">4000</span><span class="sxs-lookup"><span data-stu-id="df446-316">4000</span></span> | <span data-ttu-id="df446-317">Jason</span><span class="sxs-lookup"><span data-stu-id="df446-317">Jason</span></span> |

<span data-ttu-id="df446-318">a pro každý záznam očekáváte k zápisu do objektu JSON v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="df446-318">and for each record, you expect to write to a JSON object in the following format:</span></span>
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

<span data-ttu-id="df446-319">Výstupní datová sada typu **JsonFormat** je definovaná následujícím způsobem (částečná definice obsahující jenom relevantní části).</span><span class="sxs-lookup"><span data-stu-id="df446-319">The output dataset with **JsonFormat** type is defined as follows: (partial definition with only the relevant parts).</span></span> <span data-ttu-id="df446-320">Přesněji řečeno `structure` oddíl definuje názvy přizpůsobených vlastností, které v cílovém souboru `nestingSeparator` (výchozí hodnota je ".") se používají k identifikaci vrstvě vnoření od názvu.</span><span class="sxs-lookup"><span data-stu-id="df446-320">More specifically, `structure` section defines the customized property names in destination file, `nestingSeparator` (default is ".") are used to identify the nest layer from the name.</span></span> <span data-ttu-id="df446-321">Pokud nechcete měnit název vlastnosti v porovnání se zdrojovým názvem sloupce nebo vnořit některé z vlastností, je tento oddíl **nepovinný**.</span><span class="sxs-lookup"><span data-stu-id="df446-321">This section is **optional** unless you want to change the property name comparing with source column name, or nest some of the properties.</span></span>

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

## <a name="avro-format"></a><span data-ttu-id="df446-322">Formát AVRO</span><span class="sxs-lookup"><span data-stu-id="df446-322">AVRO format</span></span>
<span data-ttu-id="df446-323">Pokud chcete analyzovat soubory Avro nebo zapisovat data ve formátu Avro, nastavte vlastnost `format` `type` na hodnotu **AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="df446-323">If you want to parse the Avro files or write the data in Avro format, set the `format` `type` property to **AvroFormat**.</span></span> <span data-ttu-id="df446-324">V oddílu Format v části typeProperties není potřeba zadávat žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="df446-324">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="df446-325">Příklad:</span><span class="sxs-lookup"><span data-stu-id="df446-325">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="df446-326">Pokud chcete formát Avro použít v tabulce Hive, najdete potřebné informace v [kurzu k Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="df446-326">To use Avro format in a Hive table, you can refer to [Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="df446-327">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="df446-327">Note the following points:</span></span>  

* <span data-ttu-id="df446-328">[Komplexní datové typy](http://avro.apache.org/docs/current/spec.html#schema_complex) nejsou podporovány (zaznamenává výčty, pole, map, sjednocení a pevné).</span><span class="sxs-lookup"><span data-stu-id="df446-328">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions, and fixed).</span></span>

## <a name="orc-format"></a><span data-ttu-id="df446-329">Formát ORC</span><span class="sxs-lookup"><span data-stu-id="df446-329">ORC format</span></span>
<span data-ttu-id="df446-330">Pokud chcete analyzovat soubory ORC nebo zapisovat data ve formátu ORC, nastavte vlastnost `format` `type` na hodnotu **OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="df446-330">If you want to parse the ORC files or write the data in ORC format, set the `format` `type` property to **OrcFormat**.</span></span> <span data-ttu-id="df446-331">V oddílu Format v části typeProperties není potřeba zadávat žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="df446-331">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="df446-332">Příklad:</span><span class="sxs-lookup"><span data-stu-id="df446-332">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="df446-333">Pokud soubory ORC mezi místním prostředím a cloudovým úložištěm dat nekopírujete **tak, jak jsou**, musíte si na počítači brány nainstalovat prostředí JRE 8 (Java Runtime Environment).</span><span class="sxs-lookup"><span data-stu-id="df446-333">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="df446-334">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="df446-334">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="df446-335">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="df446-335">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="df446-336">Vyberte z nich tu vhodnou.</span><span class="sxs-lookup"><span data-stu-id="df446-336">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="df446-337">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="df446-337">Note the following points:</span></span>

* <span data-ttu-id="df446-338">Komplexní datové typy se nepodporují (STRUCT, MAP, LIST, UNION).</span><span class="sxs-lookup"><span data-stu-id="df446-338">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="df446-339">Soubory ORC mají tři [možnosti komprese](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB a SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="df446-339">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="df446-340">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="df446-340">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="df446-341">K načtení dat využívá kompresní kodek v metadatech.</span><span class="sxs-lookup"><span data-stu-id="df446-341">It uses the compression codec is in the metadata to read the data.</span></span> <span data-ttu-id="df446-342">Při zápisu do souboru ORC ale Data Factory využívá možnost ZLIB, která je pro formát ORC výchozí.</span><span class="sxs-lookup"><span data-stu-id="df446-342">However, when writing to an ORC file, Data Factory chooses ZLIB, which is the default for ORC.</span></span> <span data-ttu-id="df446-343">V současnosti toto chování nejde potlačit.</span><span class="sxs-lookup"><span data-stu-id="df446-343">Currently, there is no option to override this behavior.</span></span>

## <a name="parquet-format"></a><span data-ttu-id="df446-344">Formát parquet</span><span class="sxs-lookup"><span data-stu-id="df446-344">Parquet format</span></span>
<span data-ttu-id="df446-345">Pokud chcete analyzovat soubory Parquet nebo zapisovat data ve formátu Parquet, nastavte vlastnost `format` `type` na hodnotu **Format**.</span><span class="sxs-lookup"><span data-stu-id="df446-345">If you want to parse the Parquet files or write the data in Parquet format, set the `format` `type` property to **ParquetFormat**.</span></span> <span data-ttu-id="df446-346">V oddílu Format v části typeProperties není potřeba zadávat žádné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="df446-346">You do not need to specify any properties in the Format section within the typeProperties section.</span></span> <span data-ttu-id="df446-347">Příklad:</span><span class="sxs-lookup"><span data-stu-id="df446-347">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="df446-348">Pokud soubory Parquet mezi místním prostředím a cloudovým úložištěm dat nekopírujete **tak, jak jsou**, musíte si na počítači brány nainstalovat prostředí JRE 8 (Java Runtime Environment).</span><span class="sxs-lookup"><span data-stu-id="df446-348">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need to install the JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="df446-349">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="df446-349">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="df446-350">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="df446-350">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="df446-351">Vyberte z nich tu vhodnou.</span><span class="sxs-lookup"><span data-stu-id="df446-351">Choose the appropriate one.</span></span>
>
>

<span data-ttu-id="df446-352">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="df446-352">Note the following points:</span></span>

* <span data-ttu-id="df446-353">Komplexní datové typy se nepodporují (MAP, LIST).</span><span class="sxs-lookup"><span data-stu-id="df446-353">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="df446-354">Soubory Parquet mají tyto možnosti komprese: NONE, SNAPPY, GZIP a LZO.</span><span class="sxs-lookup"><span data-stu-id="df446-354">Parquet file has the following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="df446-355">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="df446-355">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="df446-356">K načtení dat využívá kompresní kodek v metadatech.</span><span class="sxs-lookup"><span data-stu-id="df446-356">It uses the compression codec in the metadata to read the data.</span></span> <span data-ttu-id="df446-357">Při zápisu do souboru Parquet ale Data Factory využívá možnost SNAPPY, která je pro formát Parquet výchozí.</span><span class="sxs-lookup"><span data-stu-id="df446-357">However, when writing to a Parquet file, Data Factory chooses SNAPPY, which is the default for Parquet format.</span></span> <span data-ttu-id="df446-358">V současnosti toto chování nejde potlačit.</span><span class="sxs-lookup"><span data-stu-id="df446-358">Currently, there is no option to override this behavior.</span></span>

## <a name="compression-support"></a><span data-ttu-id="df446-359">Podporu komprese</span><span class="sxs-lookup"><span data-stu-id="df446-359">Compression support</span></span>
<span data-ttu-id="df446-360">Zpracování velkých datových sad může způsobit vstupně-výstupních operací a sítě kritická místa.</span><span class="sxs-lookup"><span data-stu-id="df446-360">Processing large data sets can cause I/O and network bottlenecks.</span></span> <span data-ttu-id="df446-361">Proto komprimovaná data v úložištích můžete nejen urychlit přenos dat přes síť a ušetřit místo na disku, ale také uvést významné zlepšení výkonu při zpracování velkých objemů dat.</span><span class="sxs-lookup"><span data-stu-id="df446-361">Therefore, compressed data in stores can not only speed up data transfer across the network and save disk space, but also bring significant performance improvements in processing big data.</span></span> <span data-ttu-id="df446-362">Komprese je aktuálně podporované pro úložiště dat na základě souborů například objektů Blob v Azure nebo v místním systému souborů.</span><span class="sxs-lookup"><span data-stu-id="df446-362">Currently, compression is supported for file-based data stores such as Azure Blob or On-premises File System.</span></span>  

<span data-ttu-id="df446-363">Pokud chcete zadat komprese datové sady, použijte **komprese** vlastnost v datové sadě JSON jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="df446-363">To specify compression for a dataset, use the **compression** property in the dataset JSON as in the following example:</span></span>   

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

<span data-ttu-id="df446-364">Předpokládejme, že jako výstup aktivity kopírování se používá ukázkovou datovou sadu, aktivitě kopírování komprimaci dat, výstupní s GZIP kodeků pomocí optimální poměr a zapište si komprimovaná data do souboru s názvem pagecounts.csv.gz ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="df446-364">Suppose the sample dataset is used as the output of a copy activity, the copy activity compresses the output data with GZIP codec using optimal ratio and then write the compressed data into a file named pagecounts.csv.gz in the Azure Blob Storage.</span></span>

> [!NOTE]
> <span data-ttu-id="df446-365">Nastavení komprese nejsou podporovány pro data v **AvroFormat**, **OrcFormat**, nebo **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="df446-365">Compression settings are not supported for data in the **AvroFormat**, **OrcFormat**, or **ParquetFormat**.</span></span> <span data-ttu-id="df446-366">Při čtení souborů v těchto formátů, Data Factory zjišťuje a používá kompresní kodek v metadatech.</span><span class="sxs-lookup"><span data-stu-id="df446-366">When reading files in these formats, Data Factory detects and uses the compression codec in the metadata.</span></span> <span data-ttu-id="df446-367">Při zápisu do souborů v těchto formátů, Data Factory zvolí kodek komprese výchozí pro tento formát.</span><span class="sxs-lookup"><span data-stu-id="df446-367">When writing to files in these formats, Data Factory chooses the default compression codec for that format.</span></span> <span data-ttu-id="df446-368">Například ZLIB OrcFormat a SNAPPY pro ParquetFormat.</span><span class="sxs-lookup"><span data-stu-id="df446-368">For example, ZLIB for OrcFormat and SNAPPY for ParquetFormat.</span></span>   

<span data-ttu-id="df446-369">**Komprese** části má dvě vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="df446-369">The **compression** section has two properties:</span></span>  

* <span data-ttu-id="df446-370">**Typ:** kodeků kompresi, což může být **GZIP**, **Deflate**, **BZIP2**, nebo **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="df446-370">**Type:** the compression codec, which can be **GZIP**, **Deflate**, **BZIP2**, or **ZipDeflate**.</span></span>  
* <span data-ttu-id="df446-371">**Úroveň:** kompresní poměr, což může být **Optimal** nebo **nejrychlejší**.</span><span class="sxs-lookup"><span data-stu-id="df446-371">**Level:** the compression ratio, which can be **Optimal** or **Fastest**.</span></span>

  * <span data-ttu-id="df446-372">**Nejrychlejší:** komprese operace by se měla dokončit co nejrychleji, i když bude výsledný soubor není komprimována optimálně.</span><span class="sxs-lookup"><span data-stu-id="df446-372">**Fastest:** The compression operation should complete as quickly as possible, even if the resulting file is not optimally compressed.</span></span>
  * <span data-ttu-id="df446-373">**Optimální**: komprese operace by měl být optimálně komprimován, i v případě, že trvá delší dobu pro dokončení operace.</span><span class="sxs-lookup"><span data-stu-id="df446-373">**Optimal**: The compression operation should be optimally compressed, even if the operation takes a longer time to complete.</span></span>

    <span data-ttu-id="df446-374">Další informace najdete v tématu [úroveň komprese](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) tématu.</span><span class="sxs-lookup"><span data-stu-id="df446-374">For more information, see [Compression Level](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) topic.</span></span>

<span data-ttu-id="df446-375">Pokud zadáte `compression` vlastnost ve vstupní datové sady JSON kanálu může číst komprimovaná data ze zdroje; a pokud zadáte vlastnost ve výstupní datové JSON, aktivitě kopírování může zapsat komprimovaná data do cílové.</span><span class="sxs-lookup"><span data-stu-id="df446-375">When you specify `compression` property in an input dataset JSON, the pipeline can read compressed data from the source; and when you specify the property in an output dataset JSON, the copy activity can write compressed data to the destination.</span></span> <span data-ttu-id="df446-376">Tady je několik ukázkových scénářů:</span><span class="sxs-lookup"><span data-stu-id="df446-376">Here are a few sample scenarios:</span></span>

* <span data-ttu-id="df446-377">Komprimované GZIP čtení dat z objektu blob Azure dekomprimovat ho a zapsat Výsledná data do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="df446-377">Read GZIP compressed data from an Azure blob, decompress it, and write result data to an Azure SQL database.</span></span> <span data-ttu-id="df446-378">Definujete vstupní datové sady objektu Blob Azure s `compression` `type` JSON vlastnost jako GZIP.</span><span class="sxs-lookup"><span data-stu-id="df446-378">You define the input Azure Blob dataset with the `compression` `type` JSON property as GZIP.</span></span>
* <span data-ttu-id="df446-379">Čtení dat ze souboru ve formátu prostého textu z místního systému souborů, je formátu GZip komprimovat a zapíše komprimovaná data do Azure blob.</span><span class="sxs-lookup"><span data-stu-id="df446-379">Read data from a plain-text file from on-premises File System, compress it using GZip format, and write the compressed data to an Azure blob.</span></span> <span data-ttu-id="df446-380">Definujte výstupní datové Azure Blob s `compression` `type` JSON vlastnost jako GZip.</span><span class="sxs-lookup"><span data-stu-id="df446-380">You define an output Azure Blob dataset with the `compression` `type` JSON property as GZip.</span></span>
* <span data-ttu-id="df446-381">Soubor ZIP číst ze serveru FTP dekomprimovat ho k získání souborů uvnitř a zobrazovat tyto soubory do Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="df446-381">Read .zip file from FTP server, decompress it to get the files inside, and land those files into Azure Data Lake Store.</span></span> <span data-ttu-id="df446-382">Definujete vstupní datové sady FTP s `compression` `type` JSON vlastnost jako ZipDeflate.</span><span class="sxs-lookup"><span data-stu-id="df446-382">You define an input FTP dataset with the `compression` `type` JSON property as ZipDeflate.</span></span>
* <span data-ttu-id="df446-383">Číst kompresí GZIP dat z Azure blob, dekomprimovat ho, je pomocí BZIP2 komprimovat a zapsat Výsledná data do Azure blob.</span><span class="sxs-lookup"><span data-stu-id="df446-383">Read a GZIP-compressed data from an Azure blob, decompress it, compress it using BZIP2, and write result data to an Azure blob.</span></span> <span data-ttu-id="df446-384">Definujete vstupní datové sady objektu Blob Azure s `compression` `type` nastavena na GZIP a výstupní datovou sadu s `compression` `type` nastavena na BZIP2 v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="df446-384">You define the input Azure Blob dataset with `compression` `type` set to GZIP and the output dataset with `compression` `type` set to BZIP2 in this case.</span></span>   


## <a name="next-steps"></a><span data-ttu-id="df446-385">Další kroky</span><span class="sxs-lookup"><span data-stu-id="df446-385">Next steps</span></span>
<span data-ttu-id="df446-386">Najdete v následujících článcích pro úložiště dat na základě souborů podporovaných službou Azure Data Factory:</span><span class="sxs-lookup"><span data-stu-id="df446-386">See the following articles for file-based data stores supported by Azure Data Factory:</span></span>

- [<span data-ttu-id="df446-387">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="df446-387">Azure Blob Storage</span></span>](data-factory-azure-blob-connector.md)
- [<span data-ttu-id="df446-388">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="df446-388">Azure Data Lake Store</span></span>](data-factory-azure-datalake-connector.md)
- [<span data-ttu-id="df446-389">FTP</span><span class="sxs-lookup"><span data-stu-id="df446-389">FTP</span></span>](data-factory-ftp-connector.md)
- [<span data-ttu-id="df446-390">HDFS</span><span class="sxs-lookup"><span data-stu-id="df446-390">HDFS</span></span>](data-factory-hdfs-connector.md)
- [<span data-ttu-id="df446-391">Systém souborů</span><span class="sxs-lookup"><span data-stu-id="df446-391">File System</span></span>](data-factory-onprem-file-system-connector.md)
- [<span data-ttu-id="df446-392">Amazon S3</span><span class="sxs-lookup"><span data-stu-id="df446-392">Amazon S3</span></span>](data-factory-amazon-simple-storage-service-connector.md)
