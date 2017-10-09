## <a name="specifying-formats"></a><span data-ttu-id="53875-101">Zadávání formátů</span><span class="sxs-lookup"><span data-stu-id="53875-101">Specifying formats</span></span>
<span data-ttu-id="53875-102">Azure Data Factory podporuje následující typy formátu hello:</span><span class="sxs-lookup"><span data-stu-id="53875-102">Azure Data Factory supports hello following format types:</span></span>

* [<span data-ttu-id="53875-103">Formát Text</span><span class="sxs-lookup"><span data-stu-id="53875-103">Text Format</span></span>](#specifying-textformat)
* [<span data-ttu-id="53875-104">Formát JSON</span><span class="sxs-lookup"><span data-stu-id="53875-104">JSON Format</span></span>](#specifying-jsonformat)
* [<span data-ttu-id="53875-105">Formát Avro</span><span class="sxs-lookup"><span data-stu-id="53875-105">Avro Format</span></span>](#specifying-avroformat)
* [<span data-ttu-id="53875-106">Formát ORC</span><span class="sxs-lookup"><span data-stu-id="53875-106">ORC Format</span></span>](#specifying-orcformat)
* [<span data-ttu-id="53875-107">Formát Parquet</span><span class="sxs-lookup"><span data-stu-id="53875-107">Parquet Format</span></span>](#specifying-parquetformat)

### <a name="specifying-textformat"></a><span data-ttu-id="53875-108">Zadávání formátu TextFormat</span><span class="sxs-lookup"><span data-stu-id="53875-108">Specifying TextFormat</span></span>
<span data-ttu-id="53875-109">Chcete-li tooparse hello textové soubory nebo zapisovat hello data v textovém formátu, nastavte hello `format` `type` vlastnost příliš**TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="53875-109">If you want tooparse hello text files or write hello data in text format, set hello `format` `type` property too**TextFormat**.</span></span> <span data-ttu-id="53875-110">Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části.</span><span class="sxs-lookup"><span data-stu-id="53875-110">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="53875-111">V tématu [TextFormat příklad](#textformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="53875-111">See [TextFormat example](#textformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="53875-112">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="53875-112">Property</span></span> | <span data-ttu-id="53875-113">Popis</span><span class="sxs-lookup"><span data-stu-id="53875-113">Description</span></span> | <span data-ttu-id="53875-114">Povolené hodnoty</span><span class="sxs-lookup"><span data-stu-id="53875-114">Allowed values</span></span> | <span data-ttu-id="53875-115">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="53875-115">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="53875-116">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="53875-116">columnDelimiter</span></span> |<span data-ttu-id="53875-117">v souboru použit znak Hello tooseparate sloupce.</span><span class="sxs-lookup"><span data-stu-id="53875-117">hello character used tooseparate columns in a file.</span></span> <span data-ttu-id="53875-118">Můžete zvážit toouse výjimečných netisknutelné char, která již není pravděpodobně existuje ve vašich datech: například zadejte "\u0001" představující spustit z záhlaví (SOH).</span><span class="sxs-lookup"><span data-stu-id="53875-118">You can consider toouse a rare unprintable char which not likely exists in your data: e.g. specify "\u0001" which represents Start of Heading (SOH).</span></span> |<span data-ttu-id="53875-119">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="53875-119">Only one character is allowed.</span></span> <span data-ttu-id="53875-120">Hello **výchozí** hodnota je **čárkou (,)**.</span><span class="sxs-lookup"><span data-stu-id="53875-120">hello **default** value is **comma (',')**.</span></span> <br/><br/><span data-ttu-id="53875-121">toouse znak Unicode odkazovat příliš[znaky znakové sady Unicode](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello odpovídající kód pro ni.</span><span class="sxs-lookup"><span data-stu-id="53875-121">toouse an Unicode character, refer too[Unicode Characters](https://en.wikipedia.org/wiki/List_of_Unicode_characters) tooget hello corresponding code for it.</span></span> |<span data-ttu-id="53875-122">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-122">No</span></span> |
| <span data-ttu-id="53875-123">rowDelimiter</span><span class="sxs-lookup"><span data-stu-id="53875-123">rowDelimiter</span></span> |<span data-ttu-id="53875-124">znak Hello používá tooseparate řádků v souboru.</span><span class="sxs-lookup"><span data-stu-id="53875-124">hello character used tooseparate rows in a file.</span></span> |<span data-ttu-id="53875-125">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="53875-125">Only one character is allowed.</span></span> <span data-ttu-id="53875-126">Hello **výchozí** hodnota je všechny následující hodnoty na čtení hello: **["\r\n", "\r", "\n"]** a **"\r\n"** v zápisu.</span><span class="sxs-lookup"><span data-stu-id="53875-126">hello **default** value is any of hello following values on read: **["\r\n", "\r", "\n"]** and **"\r\n"** on write.</span></span> |<span data-ttu-id="53875-127">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-127">No</span></span> |
| <span data-ttu-id="53875-128">escapeChar</span><span class="sxs-lookup"><span data-stu-id="53875-128">escapeChar</span></span> |<span data-ttu-id="53875-129">speciální znak Hello používá tooescape oddělovač sloupec v hello obsah vstupní soubor.</span><span class="sxs-lookup"><span data-stu-id="53875-129">hello special character used tooescape a column delimiter in hello content of input file.</span></span> <br/><br/><span data-ttu-id="53875-130">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="53875-130">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="53875-131">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="53875-131">Only one character is allowed.</span></span> <span data-ttu-id="53875-132">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="53875-132">No default value.</span></span> <br/><br/><span data-ttu-id="53875-133">Příklad: Pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave hello čárku v textu hello (Příklad: "Hello, world"), můžete definovat jako hello řídicí znak "$" a použijte řetězec "Hello$, world" v hello zdroji.</span><span class="sxs-lookup"><span data-stu-id="53875-133">Example: if you have comma (',') as hello column delimiter but you want toohave hello comma character in hello text (example: "Hello, world"), you can define ‘$’ as hello escape character and use string "Hello$, world" in hello source.</span></span> |<span data-ttu-id="53875-134">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-134">No</span></span> |
| <span data-ttu-id="53875-135">quoteChar</span><span class="sxs-lookup"><span data-stu-id="53875-135">quoteChar</span></span> |<span data-ttu-id="53875-136">tooquote řetězcovou hodnotu použít znak Hello.</span><span class="sxs-lookup"><span data-stu-id="53875-136">hello character used tooquote a string value.</span></span> <span data-ttu-id="53875-137">oddělovače sloupců a řádků Hello uvnitř znaky uvozovek za sebou hello by považovány za součást hello řetězcovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="53875-137">hello column and row delimiters inside hello quote characters would be treated as part of hello string value.</span></span> <span data-ttu-id="53875-138">Tato vlastnost je použít tooboth vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="53875-138">This property is applicable tooboth input and output datasets.</span></span><br/><br/><span data-ttu-id="53875-139">Pro tabulku nejde zadat escapeChar a quoteChar současně.</span><span class="sxs-lookup"><span data-stu-id="53875-139">You cannot specify both escapeChar and quoteChar for a table.</span></span> |<span data-ttu-id="53875-140">Je povolený jenom jeden znak.</span><span class="sxs-lookup"><span data-stu-id="53875-140">Only one character is allowed.</span></span> <span data-ttu-id="53875-141">Žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="53875-141">No default value.</span></span> <br/><br/><span data-ttu-id="53875-142">Například, pokud máte čárkou (', ') jako oddělovač sloupec hello ale chcete toohave čárku v textu hello (Příklad: < Hello, world >), můžete definovat "(dvojitých uvozovek) jako hello znak uvozovky a použít hello řetězec"Hello, world"v hello zdroji.</span><span class="sxs-lookup"><span data-stu-id="53875-142">For example, if you have comma (',') as hello column delimiter but you want toohave comma character in hello text (example: <Hello, world>), you can define " (double quote) as hello quote character and use hello string "Hello, world" in hello source.</span></span> |<span data-ttu-id="53875-143">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-143">No</span></span> |
| <span data-ttu-id="53875-144">nullValue</span><span class="sxs-lookup"><span data-stu-id="53875-144">nullValue</span></span> |<span data-ttu-id="53875-145">Jeden nebo více znaků použít toorepresent hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="53875-145">One or more characters used toorepresent a null value.</span></span> |<span data-ttu-id="53875-146">Jeden nebo několik znaků.</span><span class="sxs-lookup"><span data-stu-id="53875-146">One or more characters.</span></span> <span data-ttu-id="53875-147">Hello **výchozí** hodnoty jsou **"\N" a "NULL"** na čtení a **"\N"** v zápisu.</span><span class="sxs-lookup"><span data-stu-id="53875-147">hello **default** values are **"\N" and "NULL"** on read and **"\N"** on write.</span></span> |<span data-ttu-id="53875-148">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-148">No</span></span> |
| <span data-ttu-id="53875-149">encodingName</span><span class="sxs-lookup"><span data-stu-id="53875-149">encodingName</span></span> |<span data-ttu-id="53875-150">Zadejte název kódování hello.</span><span class="sxs-lookup"><span data-stu-id="53875-150">Specify hello encoding name.</span></span> |<span data-ttu-id="53875-151">Platný název kódování.</span><span class="sxs-lookup"><span data-stu-id="53875-151">A valid encoding name.</span></span> <span data-ttu-id="53875-152">Další informace najdete v tématu [Vlastnost Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span><span class="sxs-lookup"><span data-stu-id="53875-152">see [Encoding.EncodingName Property](https://msdn.microsoft.com/library/system.text.encoding.aspx).</span></span> <span data-ttu-id="53875-153">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="53875-153">Example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="53875-154">Hello **výchozí** hodnota je **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="53875-154">hello **default** value is **UTF-8**.</span></span> |<span data-ttu-id="53875-155">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-155">No</span></span> |
| <span data-ttu-id="53875-156">firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="53875-156">firstRowAsHeader</span></span> |<span data-ttu-id="53875-157">Určuje, zda tooconsider hello první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="53875-157">Specifies whether tooconsider hello first row as a header.</span></span> <span data-ttu-id="53875-158">U vstupní datové sady Data Factory načítá první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="53875-158">For an input dataset, Data Factory reads first row as a header.</span></span> <span data-ttu-id="53875-159">U výstupní datové sady Data Factory zapisuje první řádek jako záhlaví.</span><span class="sxs-lookup"><span data-stu-id="53875-159">For an output dataset, Data Factory writes first row as a header.</span></span> <br/><br/><span data-ttu-id="53875-160">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="53875-160">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="53875-161">True</span><span class="sxs-lookup"><span data-stu-id="53875-161">True</span></span><br/><span data-ttu-id="53875-162">**False (výchozí)**</span><span class="sxs-lookup"><span data-stu-id="53875-162">**False (default)**</span></span> |<span data-ttu-id="53875-163">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-163">No</span></span> |
| <span data-ttu-id="53875-164">skipLineCount</span><span class="sxs-lookup"><span data-stu-id="53875-164">skipLineCount</span></span> |<span data-ttu-id="53875-165">Označuje hello počet řádků tooskip při čtení dat ze vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="53875-165">Indicates hello number of rows tooskip when reading data from input files.</span></span> <span data-ttu-id="53875-166">Pokud jsou zadané skipLineCount i firstRowAsHeader, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello.</span><span class="sxs-lookup"><span data-stu-id="53875-166">If both skipLineCount and firstRowAsHeader are specified, hello lines are skipped first and then hello header information is read from hello input file.</span></span> <br/><br/><span data-ttu-id="53875-167">Vzorové scénáře najdete v tématu [Scénáře použití `firstRowAsHeader` a `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount).</span><span class="sxs-lookup"><span data-stu-id="53875-167">See [Scenarios for using `firstRowAsHeader` and `skipLineCount`](#scenarios-for-using-firstrowasheader-and-skiplinecount) for sample scenarios.</span></span> |<span data-ttu-id="53875-168">Integer</span><span class="sxs-lookup"><span data-stu-id="53875-168">Integer</span></span> |<span data-ttu-id="53875-169">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-169">No</span></span> |
| <span data-ttu-id="53875-170">treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="53875-170">treatEmptyAsNull</span></span> |<span data-ttu-id="53875-171">Určuje, zda hodnota tootreat hodnotu null nebo prázdný řetězec jako hodnotu null při čtení dat ze vstupního souboru.</span><span class="sxs-lookup"><span data-stu-id="53875-171">Specifies whether tootreat null or empty string as a null value when reading data from an input file.</span></span> |<span data-ttu-id="53875-172">**True (výchozí)**</span><span class="sxs-lookup"><span data-stu-id="53875-172">**True (default)**</span></span><br/><span data-ttu-id="53875-173">False</span><span class="sxs-lookup"><span data-stu-id="53875-173">False</span></span> |<span data-ttu-id="53875-174">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-174">No</span></span> |

#### <a name="textformat-example"></a><span data-ttu-id="53875-175">Příklad typu TextFormat</span><span class="sxs-lookup"><span data-stu-id="53875-175">TextFormat example</span></span>
<span data-ttu-id="53875-176">Hello následující příklad ukazuje některé vlastnosti hello formát pro TextFormat.</span><span class="sxs-lookup"><span data-stu-id="53875-176">hello following sample shows some of hello format properties for TextFormat.</span></span>

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

<span data-ttu-id="53875-177">toouse `escapeChar` místo `quoteChar`, nahraďte hello řádku s `quoteChar` s hello escapeChar následující:</span><span class="sxs-lookup"><span data-stu-id="53875-177">toouse an `escapeChar` instead of `quoteChar`, replace hello line with `quoteChar` with hello following escapeChar:</span></span>

```json
"escapeChar": "$",
```

#### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a><span data-ttu-id="53875-178">Scénáře použití firstRowAsHeader a skipLineCount</span><span class="sxs-lookup"><span data-stu-id="53875-178">Scenarios for using firstRowAsHeader and skipLineCount</span></span>
* <span data-ttu-id="53875-179">Kopírování z textového souboru tooa zdroj nepocházející ze souborů a chcete tooadd řádek záhlaví, který obsahuje metadata schématu hello (například: schématu SQL).</span><span class="sxs-lookup"><span data-stu-id="53875-179">You are copying from a non-file source tooa text file and would like tooadd a header line containing hello schema metadata (for example: SQL schema).</span></span> <span data-ttu-id="53875-180">Zadejte `firstRowAsHeader` jako true v hello výstupní datovou sadu pro tento scénář.</span><span class="sxs-lookup"><span data-stu-id="53875-180">Specify `firstRowAsHeader` as true in hello output dataset for this scenario.</span></span>
* <span data-ttu-id="53875-181">Kopírování z textového souboru obsahujícího jímka nepocházející ze souborů tooa řádek záhlaví a chcete toodrop, který řádek.</span><span class="sxs-lookup"><span data-stu-id="53875-181">You are copying from a text file containing a header line tooa non-file sink and would like toodrop that line.</span></span> <span data-ttu-id="53875-182">Zadejte `firstRowAsHeader` jako true v hello vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="53875-182">Specify `firstRowAsHeader` as true in hello input dataset.</span></span>
* <span data-ttu-id="53875-183">Kopírování z textového souboru a mají tooskip pár řádků od začátku hello, které neobsahují žádné informace dat nebo záhlaví.</span><span class="sxs-lookup"><span data-stu-id="53875-183">You are copying from a text file and want tooskip a few lines at hello beginning that contain no data or header information.</span></span> <span data-ttu-id="53875-184">Zadejte `skipLineCount` tooindicate hello počet řádků toobe přeskočena.</span><span class="sxs-lookup"><span data-stu-id="53875-184">Specify `skipLineCount` tooindicate hello number of lines toobe skipped.</span></span> <span data-ttu-id="53875-185">Pokud hello zbytek hello soubor obsahuje řádek záhlaví, můžete také zadat `firstRowAsHeader`.</span><span class="sxs-lookup"><span data-stu-id="53875-185">If hello rest of hello file contains a header line, you can also specify `firstRowAsHeader`.</span></span> <span data-ttu-id="53875-186">Pokud oba `skipLineCount` a `firstRowAsHeader` jsou zadané, hello řádky jsou přeskočeny první a pak je informace hlavičky hello číst ze vstupního souboru hello</span><span class="sxs-lookup"><span data-stu-id="53875-186">If both `skipLineCount` and `firstRowAsHeader` are specified, hello lines are skipped first and then hello header information is read from hello input file</span></span>

### <a name="specifying-jsonformat"></a><span data-ttu-id="53875-187">Zadávání formátu JsonFormat</span><span class="sxs-lookup"><span data-stu-id="53875-187">Specifying JsonFormat</span></span>
<span data-ttu-id="53875-188">příliš**importu a exportu soubory JSON jako-je do/z Azure Cosmos DB**, najdete v části [dokumentů JSON importu a exportu](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) část v konektoru Azure Cosmos DB hello s podrobnostmi.</span><span class="sxs-lookup"><span data-stu-id="53875-188">too**import/export JSON files as-is into/from Azure Cosmos DB**, see [Import/export JSON documents](../articles/data-factory/data-factory-azure-documentdb-connector.md#importexport-json-documents) section in hello Azure Cosmos DB connector with details.</span></span>

<span data-ttu-id="53875-189">Pokud chcete soubory JSON hello tooparse nebo zapsat hello data ve formátu JSON, nastavte hello `format` `type` vlastnost příliš**JsonFormat**.</span><span class="sxs-lookup"><span data-stu-id="53875-189">If you want tooparse hello JSON files or write hello data in JSON format, set hello `format` `type` property too**JsonFormat**.</span></span> <span data-ttu-id="53875-190">Můžete také zadat následující hello **volitelné** vlastnosti v hello `format` části.</span><span class="sxs-lookup"><span data-stu-id="53875-190">You can also specify hello following **optional** properties in hello `format` section.</span></span> <span data-ttu-id="53875-191">V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="53875-191">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span>

| <span data-ttu-id="53875-192">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="53875-192">Property</span></span> | <span data-ttu-id="53875-193">Popis</span><span class="sxs-lookup"><span data-stu-id="53875-193">Description</span></span> | <span data-ttu-id="53875-194">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="53875-194">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="53875-195">filePattern</span><span class="sxs-lookup"><span data-stu-id="53875-195">filePattern</span></span> |<span data-ttu-id="53875-196">Uveďte hello vzor dat uložených do každého souboru JSON.</span><span class="sxs-lookup"><span data-stu-id="53875-196">Indicate hello pattern of data stored in each JSON file.</span></span> <span data-ttu-id="53875-197">Povolené hodnoty jsou **setOfObjects** a **arrayOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="53875-197">Allowed values are: **setOfObjects** and **arrayOfObjects**.</span></span> <span data-ttu-id="53875-198">Hello **výchozí** hodnota je **setOfObjects**.</span><span class="sxs-lookup"><span data-stu-id="53875-198">hello **default** value is **setOfObjects**.</span></span> <span data-ttu-id="53875-199">Podrobné informace o těchto vzorech najdete v tématu [Vzory souborů JSON](#json-file-patterns).</span><span class="sxs-lookup"><span data-stu-id="53875-199">See [JSON file patterns](#json-file-patterns) section for details about these patterns.</span></span> |<span data-ttu-id="53875-200">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-200">No</span></span> |
| <span data-ttu-id="53875-201">jsonNodeReference</span><span class="sxs-lookup"><span data-stu-id="53875-201">jsonNodeReference</span></span> | <span data-ttu-id="53875-202">Pokud chcete tooiterate a extrahovat data z objektů hello uvnitř pole pole s hello stejný vzor, zadejte cestu JSON hello tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="53875-202">If you want tooiterate and extract data from hello objects inside an array field with hello same pattern, specify hello JSON path of that array.</span></span> <span data-ttu-id="53875-203">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON.</span><span class="sxs-lookup"><span data-stu-id="53875-203">This property is supported only when copying data from JSON files.</span></span> | <span data-ttu-id="53875-204">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-204">No</span></span> |
| <span data-ttu-id="53875-205">jsonPathDefinition</span><span class="sxs-lookup"><span data-stu-id="53875-205">jsonPathDefinition</span></span> | <span data-ttu-id="53875-206">Zadejte výraz cesty hello JSON pro každé mapování sloupců s názvem přizpůsobené sloupce (začněte s malá).</span><span class="sxs-lookup"><span data-stu-id="53875-206">Specify hello JSON path expression for each column mapping with a customized column name (start with lowercase).</span></span> <span data-ttu-id="53875-207">Tato vlastnost se podporuje jenom pro kopírování dat ze souborů JSON. Můžete extrahovat data z objektu nebo pole.</span><span class="sxs-lookup"><span data-stu-id="53875-207">This property is supported only when copying data from JSON files, and you can extract data from object or array.</span></span> <br/><br/> <span data-ttu-id="53875-208">Pro pole v části kořenový objekt začněte kořenové $; pro pole uvnitř hello pole, kterou si zvolí `jsonNodeReference` vlastnost, začátek od hello pole elementu.</span><span class="sxs-lookup"><span data-stu-id="53875-208">For fields under root object, start with root $; for fields inside hello array chosen by `jsonNodeReference` property, start from hello array element.</span></span> <span data-ttu-id="53875-209">V tématu [JsonFormat příklad](#jsonformat-example) část o tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="53875-209">See [JsonFormat example](#jsonformat-example) section on how tooconfigure.</span></span> | <span data-ttu-id="53875-210">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-210">No</span></span> |
| <span data-ttu-id="53875-211">encodingName</span><span class="sxs-lookup"><span data-stu-id="53875-211">encodingName</span></span> |<span data-ttu-id="53875-212">Zadejte název kódování hello.</span><span class="sxs-lookup"><span data-stu-id="53875-212">Specify hello encoding name.</span></span> <span data-ttu-id="53875-213">Hello seznam platných názvů kódování, najdete v tématu: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) vlastnost.</span><span class="sxs-lookup"><span data-stu-id="53875-213">For hello list of valid encoding names, see: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) Property.</span></span> <span data-ttu-id="53875-214">Příklad: windows-1250 nebo shift_jis.</span><span class="sxs-lookup"><span data-stu-id="53875-214">For example: windows-1250 or shift_jis.</span></span> <span data-ttu-id="53875-215">Hello **výchozí** hodnota je: **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="53875-215">hello **default** value is: **UTF-8**.</span></span> |<span data-ttu-id="53875-216">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-216">No</span></span> |
| <span data-ttu-id="53875-217">nestingSeparator</span><span class="sxs-lookup"><span data-stu-id="53875-217">nestingSeparator</span></span> |<span data-ttu-id="53875-218">Znak, který je použité tooseparate vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="53875-218">Character that is used tooseparate nesting levels.</span></span> <span data-ttu-id="53875-219">Hello výchozí hodnota je '. " (tečka).</span><span class="sxs-lookup"><span data-stu-id="53875-219">hello default value is '.' (dot).</span></span> |<span data-ttu-id="53875-220">Ne</span><span class="sxs-lookup"><span data-stu-id="53875-220">No</span></span> |

#### <a name="json-file-patterns"></a><span data-ttu-id="53875-221">Vzory souborů JSON</span><span class="sxs-lookup"><span data-stu-id="53875-221">JSON file patterns</span></span>

<span data-ttu-id="53875-222">Aktivita kopírování může analyzovat tyto vzory souborů JSON:</span><span class="sxs-lookup"><span data-stu-id="53875-222">Copy activity can parse below patterns of JSON files:</span></span>

- <span data-ttu-id="53875-223">**Typ I: setOfObjects**</span><span class="sxs-lookup"><span data-stu-id="53875-223">**Type I: setOfObjects**</span></span>

    <span data-ttu-id="53875-224">Každý soubor obsahuje jeden objekt nebo několik objektů, které jsou zřetězené nebo oddělené řádkem.</span><span class="sxs-lookup"><span data-stu-id="53875-224">Each file contains single object, or line-delimited/concatenated multiple objects.</span></span> <span data-ttu-id="53875-225">Když je pro výstupní datovou sadu vybraná tato možnost, aktivita kopírování vytvoří jeden soubor JSON, ve kterém je každý objekt na jednom řádku (oddělení řádkem).</span><span class="sxs-lookup"><span data-stu-id="53875-225">When this option is chosen in an output dataset, copy activity produces a single JSON file with each object per line (line-delimited).</span></span>

    * <span data-ttu-id="53875-226">**Příklad JSON s jedním objektem**</span><span class="sxs-lookup"><span data-stu-id="53875-226">**single object JSON example**</span></span>

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

    * <span data-ttu-id="53875-227">**Příklad JSON s oddělením řádky**</span><span class="sxs-lookup"><span data-stu-id="53875-227">**line-delimited JSON example**</span></span>

        ```json
        {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
        {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
        {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
        ```

    * <span data-ttu-id="53875-228">**Příklad JSON se zřetězením**</span><span class="sxs-lookup"><span data-stu-id="53875-228">**concatenated JSON example**</span></span>

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

- <span data-ttu-id="53875-229">**Typ II: arrayOfObjects**</span><span class="sxs-lookup"><span data-stu-id="53875-229">**Type II: arrayOfObjects**</span></span>

    <span data-ttu-id="53875-230">Každý soubor obsahuje pole objektů.</span><span class="sxs-lookup"><span data-stu-id="53875-230">Each file contains an array of objects.</span></span>

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

#### <a name="jsonformat-example"></a><span data-ttu-id="53875-231">Příklad typu JsonFormat</span><span class="sxs-lookup"><span data-stu-id="53875-231">JsonFormat example</span></span>

<span data-ttu-id="53875-232">**Případ 1: Kopírování dat ze souborů JSON**</span><span class="sxs-lookup"><span data-stu-id="53875-232">**Case 1: Copying data from JSON files**</span></span>

<span data-ttu-id="53875-233">Najdete níže dva typy vzorků, které se při kopírování dat z soubory JSON a hello toonote obecné body:</span><span class="sxs-lookup"><span data-stu-id="53875-233">See below two types of samples when copying data from JSON files, and hello generic points toonote:</span></span>

<span data-ttu-id="53875-234">**Ukázka 1: Extrakce dat z objektu a pole**</span><span class="sxs-lookup"><span data-stu-id="53875-234">**Sample 1: extract data from object and array**</span></span>

<span data-ttu-id="53875-235">V této ukázce očekáváte, že jeden kořenový objekt JSON mapuje toosingle záznam v tabulkovém výsledek.</span><span class="sxs-lookup"><span data-stu-id="53875-235">In this sample, you expect one root JSON object maps toosingle record in tabular result.</span></span> <span data-ttu-id="53875-236">Pokud máte soubor JSON s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="53875-236">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="53875-237">a chcete ji do tabulky Azure SQL v následujících hello formátu podle extrahování dat z objekty a pole toocopy:</span><span class="sxs-lookup"><span data-stu-id="53875-237">and you want toocopy it into an Azure SQL table in hello following format, by extracting data from both objects and array:</span></span>

| <span data-ttu-id="53875-238">id</span><span class="sxs-lookup"><span data-stu-id="53875-238">id</span></span> | <span data-ttu-id="53875-239">deviceType</span><span class="sxs-lookup"><span data-stu-id="53875-239">deviceType</span></span> | <span data-ttu-id="53875-240">targetResourceType</span><span class="sxs-lookup"><span data-stu-id="53875-240">targetResourceType</span></span> | <span data-ttu-id="53875-241">resourceManagmentProcessRunId</span><span class="sxs-lookup"><span data-stu-id="53875-241">resourceManagmentProcessRunId</span></span> | <span data-ttu-id="53875-242">occurrenceTime</span><span class="sxs-lookup"><span data-stu-id="53875-242">occurrenceTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="53875-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span><span class="sxs-lookup"><span data-stu-id="53875-243">ed0e4960-d9c5-11e6-85dc-d7996816aad3</span></span> | <span data-ttu-id="53875-244">PC</span><span class="sxs-lookup"><span data-stu-id="53875-244">PC</span></span> | <span data-ttu-id="53875-245">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="53875-245">Microsoft.Compute/virtualMachines</span></span> | <span data-ttu-id="53875-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span><span class="sxs-lookup"><span data-stu-id="53875-246">827f8aaa-ab72-437c-ba48-d8917a7336a3</span></span> | <span data-ttu-id="53875-247">1/13/2017 11:24:37 AM</span><span class="sxs-lookup"><span data-stu-id="53875-247">1/13/2017 11:24:37 AM</span></span> |

<span data-ttu-id="53875-248">vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="53875-248">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="53875-249">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="53875-249">More specifically:</span></span>

- <span data-ttu-id="53875-250">`structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data.</span><span class="sxs-lookup"><span data-stu-id="53875-250">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="53875-251">Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců.</span><span class="sxs-lookup"><span data-stu-id="53875-251">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="53875-252">Další informace najdete v tématu věnovaném [určení definice struktury pro obdélníkové datové sady](#specifying-structure-definition-for-rectangular-datasets).</span><span class="sxs-lookup"><span data-stu-id="53875-252">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="53875-253">`jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z.</span><span class="sxs-lookup"><span data-stu-id="53875-253">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="53875-254">toocopy data z pole, můžete použít **pole [x] .property** tooextract hodnotu hello zadána vlastnost z objektu x hello, nebo můžete použít  **pole [*] .property** toofind Hodnota Hello z libovolného objektu, který obsahuje tato vlastnost.</span><span class="sxs-lookup"><span data-stu-id="53875-254">toocopy data from array, you can use **array[x].property** tooextract value of hello given property from hello xth object, or you can use **array[*].property** toofind hello value from any object containing such property.</span></span>

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

<span data-ttu-id="53875-255">**Příklad 2: mezi použít více objektů se stejný vzor z pole hello**</span><span class="sxs-lookup"><span data-stu-id="53875-255">**Sample 2: cross apply multiple objects with hello same pattern from array**</span></span>

<span data-ttu-id="53875-256">V této ukázce předpokládáte tootransform jeden kořenový objekt JSON do více záznamů v tabulkovém výsledek.</span><span class="sxs-lookup"><span data-stu-id="53875-256">In this sample, you expect tootransform one root JSON object into multiple records in tabular result.</span></span> <span data-ttu-id="53875-257">Pokud máte soubor JSON s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="53875-257">If you have a JSON file with hello following content:</span></span>  

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
<span data-ttu-id="53875-258">a chcete ji do tabulky Azure SQL v následujících hello naformátovat pomocí sloučení hello data uvnitř pole hello toocopy a křížové spojení s informacemi kořenové běžné hello:</span><span class="sxs-lookup"><span data-stu-id="53875-258">and you want toocopy it into an Azure SQL table in hello following format, by flattening hello data inside hello array and cross join with hello common root info:</span></span>

| <span data-ttu-id="53875-259">ordernumber</span><span class="sxs-lookup"><span data-stu-id="53875-259">ordernumber</span></span> | <span data-ttu-id="53875-260">orderdate</span><span class="sxs-lookup"><span data-stu-id="53875-260">orderdate</span></span> | <span data-ttu-id="53875-261">order_pd</span><span class="sxs-lookup"><span data-stu-id="53875-261">order_pd</span></span> | <span data-ttu-id="53875-262">order_price</span><span class="sxs-lookup"><span data-stu-id="53875-262">order_price</span></span> | <span data-ttu-id="53875-263">city</span><span class="sxs-lookup"><span data-stu-id="53875-263">city</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="53875-264">01</span><span class="sxs-lookup"><span data-stu-id="53875-264">01</span></span> | <span data-ttu-id="53875-265">20170122</span><span class="sxs-lookup"><span data-stu-id="53875-265">20170122</span></span> | <span data-ttu-id="53875-266">P1</span><span class="sxs-lookup"><span data-stu-id="53875-266">P1</span></span> | <span data-ttu-id="53875-267">23</span><span class="sxs-lookup"><span data-stu-id="53875-267">23</span></span> | <span data-ttu-id="53875-268">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="53875-268">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="53875-269">01</span><span class="sxs-lookup"><span data-stu-id="53875-269">01</span></span> | <span data-ttu-id="53875-270">20170122</span><span class="sxs-lookup"><span data-stu-id="53875-270">20170122</span></span> | <span data-ttu-id="53875-271">P2</span><span class="sxs-lookup"><span data-stu-id="53875-271">P2</span></span> | <span data-ttu-id="53875-272">13</span><span class="sxs-lookup"><span data-stu-id="53875-272">13</span></span> | <span data-ttu-id="53875-273">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="53875-273">[{"sanmateo":"No 1"}]</span></span> |
| <span data-ttu-id="53875-274">01</span><span class="sxs-lookup"><span data-stu-id="53875-274">01</span></span> | <span data-ttu-id="53875-275">20170122</span><span class="sxs-lookup"><span data-stu-id="53875-275">20170122</span></span> | <span data-ttu-id="53875-276">P3</span><span class="sxs-lookup"><span data-stu-id="53875-276">P3</span></span> | <span data-ttu-id="53875-277">231</span><span class="sxs-lookup"><span data-stu-id="53875-277">231</span></span> | <span data-ttu-id="53875-278">[{"sanmateo":"No 1"}]</span><span class="sxs-lookup"><span data-stu-id="53875-278">[{"sanmateo":"No 1"}]</span></span> |

<span data-ttu-id="53875-279">vstupní datové sady Hello s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="53875-279">hello input dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="53875-280">A konkrétně:</span><span class="sxs-lookup"><span data-stu-id="53875-280">More specifically:</span></span>

- <span data-ttu-id="53875-281">`structure`oddíl definuje názvy sloupců hello přizpůsobit a datový typ odpovídající hello při převodu tootabular data.</span><span class="sxs-lookup"><span data-stu-id="53875-281">`structure` section defines hello customized column names and hello corresponding data type while converting tootabular data.</span></span> <span data-ttu-id="53875-282">Tato část se **volitelné** Pokud potřebujete toodo mapování sloupců.</span><span class="sxs-lookup"><span data-stu-id="53875-282">This section is **optional** unless you need toodo column mapping.</span></span> <span data-ttu-id="53875-283">Další informace najdete v tématu věnovaném [určení definice struktury pro obdélníkové datové sady](#specifying-structure-definition-for-rectangular-datasets).</span><span class="sxs-lookup"><span data-stu-id="53875-283">See [Specifying structure definition for rectangular datasets](#specifying-structure-definition-for-rectangular-datasets) section for more details.</span></span>
- <span data-ttu-id="53875-284">`jsonNodeReference`označuje tooiterate a extrahování dat z objektů hello se stejný vzor pod hello **pole** orderlines.</span><span class="sxs-lookup"><span data-stu-id="53875-284">`jsonNodeReference` indicates tooiterate and extract data from hello objects with hello same pattern under **array** orderlines.</span></span>
- <span data-ttu-id="53875-285">`jsonPathDefinition`Určuje cestu hello JSON pro každý sloupec, která určuje, kde tooextract hello data z.</span><span class="sxs-lookup"><span data-stu-id="53875-285">`jsonPathDefinition` specifies hello JSON path for each column indicating where tooextract hello data from.</span></span> <span data-ttu-id="53875-286">V tomto příkladu "ordernumber", "orderdate" a "Město, jsou ve kořenový objekt s počínaje"$.", zatímco"order_pd"a"order_price"jsou definovány s cestou odvozen z elementu pole hello bez"$". cesta JSON.</span><span class="sxs-lookup"><span data-stu-id="53875-286">In this example, "ordernumber", "orderdate" and "city" are under root object with JSON path starting with "$.", while "order_pd" and "order_price" are defined with path derived from hello array element without "$.".</span></span>

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

<span data-ttu-id="53875-287">**Všimněte si hello následující body:**</span><span class="sxs-lookup"><span data-stu-id="53875-287">**Note hello following points:**</span></span>

* <span data-ttu-id="53875-288">Pokud hello `structure` a `jsonPathDefinition` nejsou definovány v datové sadě služby Data Factory hello, hello aktivity kopírování zjistí hello schématu z první objekt hello a vyrovnání hello celý objekt.</span><span class="sxs-lookup"><span data-stu-id="53875-288">If hello `structure` and `jsonPathDefinition` are not defined in hello Data Factory dataset, hello Copy Activity detects hello schema from hello first object and flatten hello whole object.</span></span>
* <span data-ttu-id="53875-289">Pokud hello vstupu JSON má pole, převede hello aktivity kopírování ve výchozím nastavení hello celé pole hodnotu na řetězec.</span><span class="sxs-lookup"><span data-stu-id="53875-289">If hello JSON input has an array, by default hello Copy Activity converts hello entire array value into a string.</span></span> <span data-ttu-id="53875-290">Můžete zvolit tooextract dat pomocí `jsonNodeReference` nebo `jsonPathDefinition`, nebo přeskočit zadáním není v `jsonPathDefinition`.</span><span class="sxs-lookup"><span data-stu-id="53875-290">You can choose tooextract data from it using `jsonNodeReference` and/or `jsonPathDefinition`, or skip it by not specifying it in `jsonPathDefinition`.</span></span>
* <span data-ttu-id="53875-291">Pokud existují duplicitní názvy v hello stejné úrovni, hello aktivity kopírování vybere hello naposledy.</span><span class="sxs-lookup"><span data-stu-id="53875-291">If there are duplicate names at hello same level, hello Copy Activity picks hello last one.</span></span>
* <span data-ttu-id="53875-292">V názvech vlastností se rozlišují velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="53875-292">Property names are case-sensitive.</span></span> <span data-ttu-id="53875-293">Vlastnosti se stejným názvem, ale různým použitím velkých a malých písmen, se budou považovat za různé.</span><span class="sxs-lookup"><span data-stu-id="53875-293">Two properties with same name but different casings are treated as two separate properties.</span></span>

<span data-ttu-id="53875-294">**Případ 2: Zápis dat tooJSON souboru**</span><span class="sxs-lookup"><span data-stu-id="53875-294">**Case 2: Writing data tooJSON file**</span></span>

<span data-ttu-id="53875-295">Pokud máte v SQL Database tuto tabulku:</span><span class="sxs-lookup"><span data-stu-id="53875-295">If you have below table in SQL Database:</span></span>

| <span data-ttu-id="53875-296">id</span><span class="sxs-lookup"><span data-stu-id="53875-296">id</span></span> | <span data-ttu-id="53875-297">order_date</span><span class="sxs-lookup"><span data-stu-id="53875-297">order_date</span></span> | <span data-ttu-id="53875-298">order_price</span><span class="sxs-lookup"><span data-stu-id="53875-298">order_price</span></span> | <span data-ttu-id="53875-299">order_by</span><span class="sxs-lookup"><span data-stu-id="53875-299">order_by</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="53875-300">1</span><span class="sxs-lookup"><span data-stu-id="53875-300">1</span></span> | <span data-ttu-id="53875-301">20170119</span><span class="sxs-lookup"><span data-stu-id="53875-301">20170119</span></span> | <span data-ttu-id="53875-302">2000</span><span class="sxs-lookup"><span data-stu-id="53875-302">2000</span></span> | <span data-ttu-id="53875-303">David</span><span class="sxs-lookup"><span data-stu-id="53875-303">David</span></span> |
| <span data-ttu-id="53875-304">2</span><span class="sxs-lookup"><span data-stu-id="53875-304">2</span></span> | <span data-ttu-id="53875-305">20170120</span><span class="sxs-lookup"><span data-stu-id="53875-305">20170120</span></span> | <span data-ttu-id="53875-306">3500</span><span class="sxs-lookup"><span data-stu-id="53875-306">3500</span></span> | <span data-ttu-id="53875-307">Patrick</span><span class="sxs-lookup"><span data-stu-id="53875-307">Patrick</span></span> |
| <span data-ttu-id="53875-308">3</span><span class="sxs-lookup"><span data-stu-id="53875-308">3</span></span> | <span data-ttu-id="53875-309">20170121</span><span class="sxs-lookup"><span data-stu-id="53875-309">20170121</span></span> | <span data-ttu-id="53875-310">4000</span><span class="sxs-lookup"><span data-stu-id="53875-310">4000</span></span> | <span data-ttu-id="53875-311">Jason</span><span class="sxs-lookup"><span data-stu-id="53875-311">Jason</span></span> |

<span data-ttu-id="53875-312">a pro každý záznam očekáváte, že toowrite objekt tooa JSON v následující formát:</span><span class="sxs-lookup"><span data-stu-id="53875-312">and for each record, you expect toowrite tooa JSON object in below format:</span></span>
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

<span data-ttu-id="53875-313">Hello výstupní datovou sadu s **JsonFormat** typ je definován následujícím způsobem: (částečné definice se pouze relevantní částmi hello).</span><span class="sxs-lookup"><span data-stu-id="53875-313">hello output dataset with **JsonFormat** type is defined as follows: (partial definition with only hello relevant parts).</span></span> <span data-ttu-id="53875-314">Přesněji řečeno `structure` oddíl definuje názvy vlastností hello přizpůsobit v cílovém souboru `nestingSeparator` (výchozí hodnota je ".") bude použité tooidentify hello vnoření vrstvu ze hello název.</span><span class="sxs-lookup"><span data-stu-id="53875-314">More specifically, `structure` section defines hello customized property names in destination file, `nestingSeparator` (default is ".") will be used tooidentify hello nest layer from hello name.</span></span> <span data-ttu-id="53875-315">Tato část se **volitelné** Pokud název vlastnosti hello toochange porovnávání s název zdrojového sloupce, nebo se některé vlastnosti hello vnořit.</span><span class="sxs-lookup"><span data-stu-id="53875-315">This section is **optional** unless you want toochange hello property name comparing with source column name, or nest some of hello properties.</span></span>

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

### <a name="specifying-avroformat"></a><span data-ttu-id="53875-316">Zadávání formátu AvroFormat</span><span class="sxs-lookup"><span data-stu-id="53875-316">Specifying AvroFormat</span></span>
<span data-ttu-id="53875-317">Chcete-li tooparse hello Avro soubory nebo zapisovat hello data ve formátu Avro, nastavte hello `format` `type` vlastnost příliš**AvroFormat**.</span><span class="sxs-lookup"><span data-stu-id="53875-317">If you want tooparse hello Avro files or write hello data in Avro format, set hello `format` `type` property too**AvroFormat**.</span></span> <span data-ttu-id="53875-318">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="53875-318">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="53875-319">Příklad:</span><span class="sxs-lookup"><span data-stu-id="53875-319">Example:</span></span>

```json
"format":
{
    "type": "AvroFormat",
}
```

<span data-ttu-id="53875-320">Formát Avro toouse v tabulce Hive, naleznete v části příliš[kurz Apache Hive](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span><span class="sxs-lookup"><span data-stu-id="53875-320">toouse Avro format in a Hive table, you can refer too[Apache Hive’s tutorial](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).</span></span>

<span data-ttu-id="53875-321">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="53875-321">Note hello following points:</span></span>  

* <span data-ttu-id="53875-322">[Komplexní datové typy](http://avro.apache.org/docs/current/spec.html#schema_complex) se nepodporují (záznamy, výčty, pole, mapy, sjednocení a pevné typy).</span><span class="sxs-lookup"><span data-stu-id="53875-322">[Complex data types](http://avro.apache.org/docs/current/spec.html#schema_complex) are not supported (records, enums, arrays, maps, unions and fixed).</span></span>

### <a name="specifying-orcformat"></a><span data-ttu-id="53875-323">Zadávání formátu OrcFormat</span><span class="sxs-lookup"><span data-stu-id="53875-323">Specifying OrcFormat</span></span>
<span data-ttu-id="53875-324">Chcete-li tooparse hello ORC soubory nebo zapisovat hello data ve formátu ORC, nastavte hello `format` `type` vlastnost příliš**OrcFormat**.</span><span class="sxs-lookup"><span data-stu-id="53875-324">If you want tooparse hello ORC files or write hello data in ORC format, set hello `format` `type` property too**OrcFormat**.</span></span> <span data-ttu-id="53875-325">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="53875-325">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="53875-326">Příklad:</span><span class="sxs-lookup"><span data-stu-id="53875-326">Example:</span></span>

```json
"format":
{
    "type": "OrcFormat"
}
```

> [!IMPORTANT]
> <span data-ttu-id="53875-327">Pokud nejsou kopírování souborů ORC **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime).</span><span class="sxs-lookup"><span data-stu-id="53875-327">If you are not copying ORC files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="53875-328">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="53875-328">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="53875-329">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="53875-329">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="53875-330">Vyberte příslušné hello.</span><span class="sxs-lookup"><span data-stu-id="53875-330">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="53875-331">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="53875-331">Note hello following points:</span></span>

* <span data-ttu-id="53875-332">Komplexní datové typy se nepodporují (STRUCT, MAP, LIST, UNION).</span><span class="sxs-lookup"><span data-stu-id="53875-332">Complex data types are not supported (STRUCT, MAP, LIST, UNION)</span></span>
* <span data-ttu-id="53875-333">Soubory ORC mají tři [možnosti komprese](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB a SNAPPY.</span><span class="sxs-lookup"><span data-stu-id="53875-333">ORC file has three [compression-related options](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/): NONE, ZLIB, SNAPPY.</span></span> <span data-ttu-id="53875-334">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="53875-334">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="53875-335">Používá kompresi hello kodeků je v datech hello tooread metadata hello.</span><span class="sxs-lookup"><span data-stu-id="53875-335">It uses hello compression codec is in hello metadata tooread hello data.</span></span> <span data-ttu-id="53875-336">Ale při zápisu souboru ORC tooan, Data Factory zvolí ZLIB, což je výchozí hodnotou hello ORC.</span><span class="sxs-lookup"><span data-stu-id="53875-336">However, when writing tooan ORC file, Data Factory chooses ZLIB, which is hello default for ORC.</span></span> <span data-ttu-id="53875-337">V současné době není žádná možnost toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="53875-337">Currently, there is no option toooverride this behavior.</span></span>

### <a name="specifying-parquetformat"></a><span data-ttu-id="53875-338">Zadávání formátu ParquetFormat</span><span class="sxs-lookup"><span data-stu-id="53875-338">Specifying ParquetFormat</span></span>
<span data-ttu-id="53875-339">Chcete-li tooparse hello Parquet soubory nebo zapisovat hello data ve formátu Parquet, nastavte hello `format` `type` vlastnost příliš**ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="53875-339">If you want tooparse hello Parquet files or write hello data in Parquet format, set hello `format` `type` property too**ParquetFormat**.</span></span> <span data-ttu-id="53875-340">Není nutné toospecify všechny vlastnosti v části formátu hello v rámci typeProperties oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="53875-340">You do not need toospecify any properties in hello Format section within hello typeProperties section.</span></span> <span data-ttu-id="53875-341">Příklad:</span><span class="sxs-lookup"><span data-stu-id="53875-341">Example:</span></span>

```json
"format":
{
    "type": "ParquetFormat"
}
```
> [!IMPORTANT]
> <span data-ttu-id="53875-342">Pokud nejsou kopírování souborů Parquet **jako-je** mezi místní a cloudové úložiště dat, musíte na počítači brány tooinstall hello prostředí JRE 8 (prostředí Java Runtime).</span><span class="sxs-lookup"><span data-stu-id="53875-342">If you are not copying Parquet files **as-is** between on-premises and cloud data stores, you need tooinstall hello JRE 8 (Java Runtime Environment) on your gateway machine.</span></span> <span data-ttu-id="53875-343">64bitová verze brány vyžaduje 64bitové prostředí JRE a 32bitová verze brány vyžaduje 32bitové prostředí JRE.</span><span class="sxs-lookup"><span data-stu-id="53875-343">A 64-bit gateway requires 64-bit JRE and 32-bit gateway requires 32-bit JRE.</span></span> <span data-ttu-id="53875-344">Obě verze najdete [tady](http://go.microsoft.com/fwlink/?LinkId=808605).</span><span class="sxs-lookup"><span data-stu-id="53875-344">You can find both versions from [here](http://go.microsoft.com/fwlink/?LinkId=808605).</span></span> <span data-ttu-id="53875-345">Vyberte příslušné hello.</span><span class="sxs-lookup"><span data-stu-id="53875-345">Choose hello appropriate one.</span></span>
>
>

<span data-ttu-id="53875-346">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="53875-346">Note hello following points:</span></span>

* <span data-ttu-id="53875-347">Komplexní datové typy se nepodporují (MAP, LIST).</span><span class="sxs-lookup"><span data-stu-id="53875-347">Complex data types are not supported (MAP, LIST)</span></span>
* <span data-ttu-id="53875-348">Soubor parquet má následující možnosti týkající se komprese hello: NONE, SNAPPY, GZIP a LZO.</span><span class="sxs-lookup"><span data-stu-id="53875-348">Parquet file has hello following compression-related options: NONE, SNAPPY, GZIP, and LZO.</span></span> <span data-ttu-id="53875-349">Data Factory podporuje čtení dat ze souborů ORC v libovolném z těchto komprimovaných formátů.</span><span class="sxs-lookup"><span data-stu-id="53875-349">Data Factory supports reading data from ORC file in any of these compressed formats.</span></span> <span data-ttu-id="53875-350">Kompresní kodek hello používá v hello metadata tooread hello data.</span><span class="sxs-lookup"><span data-stu-id="53875-350">It uses hello compression codec in hello metadata tooread hello data.</span></span> <span data-ttu-id="53875-351">Ale při zápisu souboru Parquet tooa, Data Factory zvolí SNAPPY, což je výchozí hello Parquet formát.</span><span class="sxs-lookup"><span data-stu-id="53875-351">However, when writing tooa Parquet file, Data Factory chooses SNAPPY, which is hello default for Parquet format.</span></span> <span data-ttu-id="53875-352">V současné době není žádná možnost toooverride toto chování.</span><span class="sxs-lookup"><span data-stu-id="53875-352">Currently, there is no option toooverride this behavior.</span></span>
