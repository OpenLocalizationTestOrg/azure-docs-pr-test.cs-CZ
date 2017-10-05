---
title: "Uživatelem definované funkce Azure Stream Analytics JavaScript | Microsoft Docs"
description: "Provedení mechanismy rozšířený dotaz s uživatelem definované funkce jazyka JavaScript"
keywords: "JavaScript, funkce udf definované uživatelem"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: e4a9e6c7078031240c22a51378c0459426b7f626
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a><span data-ttu-id="ed8f5-104">Uživatelem definované funkce Azure Stream Analytics JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed8f5-104">Azure Stream Analytics JavaScript user-defined functions</span></span>
<span data-ttu-id="ed8f5-105">Azure Stream Analytics podporuje uživatelsky definované funkce, které jsou napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-105">Azure Stream Analytics supports user-defined functions written in JavaScript.</span></span> <span data-ttu-id="ed8f5-106">S bohaté sada **řetězec**, **RegExp**, **matematické**, **pole**, a **datum** metody této JavaScript poskytuje, transformace komplexní dat pomocí služby Stream Analytics bude snazší k vytvoření úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-106">With the rich set of **String**, **RegExp**, **Math**, **Array**, and **Date** methods that JavaScript provides, complex data transformations with Stream Analytics jobs become easier to create.</span></span>

## <a name="javascript-user-defined-functions"></a><span data-ttu-id="ed8f5-107">Uživatelem definované funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed8f5-107">JavaScript user-defined functions</span></span>
<span data-ttu-id="ed8f5-108">Uživatelem definované funkce jazyka JavaScript podporovat bezstavové, pouze výpočetní skalární funkce, které nevyžadují externí připojení.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-108">JavaScript user-defined functions support stateless, compute-only scalar functions that do not require external connectivity.</span></span> <span data-ttu-id="ed8f5-109">Návratová hodnota funkce lze pouze skalární hodnotu (jeden).</span><span class="sxs-lookup"><span data-stu-id="ed8f5-109">The return value of a function can only be a scalar (single) value.</span></span> <span data-ttu-id="ed8f5-110">Po přidání uživatelem definované funkce jazyka JavaScript do úlohy, můžete použít funkci kdekoli v dotazu, jako je integrovaná skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-110">After you add a JavaScript user-defined function to a job, you can use the function anywhere in the query, like a built-in scalar function.</span></span>

<span data-ttu-id="ed8f5-111">Tady je několik scénářů, kde mohou být užitečné JavaScript uživatelsky definované funkce:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-111">Here are some scenarios where you might find JavaScript user-defined functions useful:</span></span>
* <span data-ttu-id="ed8f5-112">Analýza a manipulace s řetězci, které mají regulární výraz funkce, například **Regexp_Replace()** a **Regexp_Extract()**</span><span class="sxs-lookup"><span data-stu-id="ed8f5-112">Parsing and manipulating strings that have regular expression functions, for example, **Regexp_Replace()** and **Regexp_Extract()**</span></span>
* <span data-ttu-id="ed8f5-113">Kódování dat, například binární šestnáctkově převod a dekódování</span><span class="sxs-lookup"><span data-stu-id="ed8f5-113">Decoding and encoding data, for example, binary-to-hex conversion</span></span>
* <span data-ttu-id="ed8f5-114">Provádění mathematic výpočtů v jazyce JavaScript **matematické** funkce</span><span class="sxs-lookup"><span data-stu-id="ed8f5-114">Performing mathematic computations with JavaScript **Math** functions</span></span>
* <span data-ttu-id="ed8f5-115">Provádění operací pole jako řazení, spojení, najít a výplně</span><span class="sxs-lookup"><span data-stu-id="ed8f5-115">Performing array operations like sort, join, find, and fill</span></span>

<span data-ttu-id="ed8f5-116">Zde jsou některé věci, které nemůžete dělat s uživatelem definované funkce jazyka JavaScript v Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-116">Here are some things that you cannot do with a JavaScript user-defined function in Stream Analytics:</span></span>
* <span data-ttu-id="ed8f5-117">Volání na externí koncové body REST, například provádění obrátíte vyhledávání IP nebo vyžádání referenční data z externího zdroje</span><span class="sxs-lookup"><span data-stu-id="ed8f5-117">Call out external REST endpoints, for example, performing reverse IP lookup or pulling reference data from an external source</span></span>
* <span data-ttu-id="ed8f5-118">Deserializace na vstupy/výstupy nebo provést vlastní události formát serializace</span><span class="sxs-lookup"><span data-stu-id="ed8f5-118">Perform custom event format serialization or deserialization on inputs/outputs</span></span>
* <span data-ttu-id="ed8f5-119">Vytvořte vlastní agregace</span><span class="sxs-lookup"><span data-stu-id="ed8f5-119">Create custom aggregates</span></span>

<span data-ttu-id="ed8f5-120">I když funguje jako **Date.GetDate()** nebo **Math.random()** nejsou blokována v definici funkce byste neměli používat je.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-120">Although functions like **Date.GetDate()** or **Math.random()** are not blocked in the functions definition, you should avoid using them.</span></span> <span data-ttu-id="ed8f5-121">Tyto funkce **nepodporují** pokaždé, když je volají a službu Azure Stream Analytics nezachovat deník volání funkce vrátí stejné výsledky a vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-121">These functions **do not** return the same result every time you call them, and the Azure Stream Analytics service does not keep a journal of function invocations and returned results.</span></span> <span data-ttu-id="ed8f5-122">Pokud funkce vrátí výsledek různých na stejné události, opakovatelnost není zaručena při restartování úlohy vy nebo pomocí služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-122">If a function returns different result on the same events, repeatability is not guaranteed when a job is restarted by you or by the Stream Analytics service.</span></span>

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a><span data-ttu-id="ed8f5-123">Přidat JavaScript uživatelsky definované funkce na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="ed8f5-123">Add a JavaScript user-defined function in the Azure portal</span></span>
<span data-ttu-id="ed8f5-124">Pokud chcete vytvořit jednoduché funkce jazyka JavaScript uživatelem definované v části existující úlohy Stream Analytics, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-124">To create a simple JavaScript user-defined function under an existing Stream Analytics job, do these steps:</span></span>

1.  <span data-ttu-id="ed8f5-125">Na portálu Azure najdete úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-125">In the Azure portal, find your Stream Analytics job.</span></span>
2.  <span data-ttu-id="ed8f5-126">V části **úlohy TOPOLOGIE**, vyberte funkce.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-126">Under **JOB TOPOLOGY**, select your function.</span></span> <span data-ttu-id="ed8f5-127">Zobrazí se prázdný seznam funkcí.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-127">An empty list of functions appears.</span></span>
3.  <span data-ttu-id="ed8f5-128">Chcete-li vytvořit nové uživatelsky definované funkce, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-128">To create a new user-defined function, select **Add**.</span></span>
4.  <span data-ttu-id="ed8f5-129">Na **novou funkci** okně pro **typ funkce**, vyberte **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-129">On the **New Function** blade, for **Function Type**, select **JavaScript**.</span></span> <span data-ttu-id="ed8f5-130">Funkce výchozí šablona služby se zobrazí v editoru.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-130">A default function template appears in the editor.</span></span>
5.  <span data-ttu-id="ed8f5-131">Pro **UDF alias**, zadejte **hex2Int**a změňte implementaci funkce následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-131">For the **UDF alias**, enter **hex2Int**, and change the function implementation as follows:</span></span>

    ```
    // Convert Hex value to integer.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  <span data-ttu-id="ed8f5-132">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-132">Select **Save**.</span></span> <span data-ttu-id="ed8f5-133">Funkce se zobrazí v seznamu funkcí.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-133">Your function appears in the list of functions.</span></span>
7.  <span data-ttu-id="ed8f5-134">Vyberte novou **hex2Int** funkce a zkontrolujte definici funkce.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-134">Select the new **hex2Int** function, and check the function definition.</span></span> <span data-ttu-id="ed8f5-135">Mají všechny funkce **UDF** předponu přidat do funkce alias.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-135">All functions have a **UDF** prefix added to the function alias.</span></span> <span data-ttu-id="ed8f5-136">Budete muset *obsahovat předponu* při volání funkce v dotazu Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-136">You need to *include the prefix* when you call the function in your Stream Analytics query.</span></span> <span data-ttu-id="ed8f5-137">V takovém případě volání **UDF.hex2Int**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-137">In this case, you call **UDF.hex2Int**.</span></span>

## <a name="call-a-javascript-user-defined-function-in-a-query"></a><span data-ttu-id="ed8f5-138">Volání uživatelem definované funkce JavaScript v dotazu</span><span class="sxs-lookup"><span data-stu-id="ed8f5-138">Call a JavaScript user-defined function in a query</span></span>

1. <span data-ttu-id="ed8f5-139">V editoru dotazů v rámci **úlohy TOPOLOGIE**, vyberte **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-139">In the query editor, under **JOB TOPOLOGY**, select **Query**.</span></span>
2.  <span data-ttu-id="ed8f5-140">Upravit dotaz a pak zavolají uživatelsky definované funkce, například takto:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-140">Edit your query, and then call the user-defined function, like this:</span></span>

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  <span data-ttu-id="ed8f5-141">Pokud chcete nahrát ukázkový datový soubor, klikněte pravým tlačítkem na vstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-141">To upload the sample data file, right-click the job input.</span></span>
4.  <span data-ttu-id="ed8f5-142">Chcete-li otestovat dotazu, vyberte **testování**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-142">To test your query, select **Test**.</span></span>


## <a name="supported-javascript-objects"></a><span data-ttu-id="ed8f5-143">Podporované JavaScript – objekty</span><span class="sxs-lookup"><span data-stu-id="ed8f5-143">Supported JavaScript objects</span></span>
<span data-ttu-id="ed8f5-144">Uživatelem definované funkce Azure Stream Analytics JavaScript podporují standard, předdefinovaných objektů jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-144">Azure Stream Analytics JavaScript user-defined functions support standard, built-in JavaScript objects.</span></span> <span data-ttu-id="ed8f5-145">Seznam těchto objektů najdete v tématu [globální objekty](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span><span class="sxs-lookup"><span data-stu-id="ed8f5-145">For a list of these objects, see [Global Objects](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span></span>

### <a name="stream-analytics-and-javascript-type-conversion"></a><span data-ttu-id="ed8f5-146">Stream Analytics a JavaScript typ – převod</span><span class="sxs-lookup"><span data-stu-id="ed8f5-146">Stream Analytics and JavaScript type conversion</span></span>

<span data-ttu-id="ed8f5-147">Existují rozdíly v typech, Stream Analytics query language a podporu jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-147">There are differences in the types that the Stream Analytics query language and JavaScript support.</span></span> <span data-ttu-id="ed8f5-148">Tato tabulka uvádí převod mapování mezi těmito dvěma:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-148">This table lists the conversion mappings between the two:</span></span>

<span data-ttu-id="ed8f5-149">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ed8f5-149">Stream Analytics</span></span> | <span data-ttu-id="ed8f5-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed8f5-150">JavaScript</span></span>
--- | ---
<span data-ttu-id="ed8f5-151">bigint</span><span class="sxs-lookup"><span data-stu-id="ed8f5-151">bigint</span></span> | <span data-ttu-id="ed8f5-152">Číslo (JavaScript může představovat jenom celá čísla přesně 2 ^ 53)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-152">Number (JavaScript can only represent integers up to precisely 2^53)</span></span>
<span data-ttu-id="ed8f5-153">Data a času</span><span class="sxs-lookup"><span data-stu-id="ed8f5-153">DateTime</span></span> | <span data-ttu-id="ed8f5-154">Datum (JavaScript pouze podporuje v milisekundách)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-154">Date (JavaScript only supports milliseconds)</span></span>
<span data-ttu-id="ed8f5-155">Double</span><span class="sxs-lookup"><span data-stu-id="ed8f5-155">double</span></span> | <span data-ttu-id="ed8f5-156">Číslo</span><span class="sxs-lookup"><span data-stu-id="ed8f5-156">Number</span></span>
<span data-ttu-id="ed8f5-157">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-157">nvarchar(MAX)</span></span> | <span data-ttu-id="ed8f5-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8f5-158">String</span></span>
<span data-ttu-id="ed8f5-159">záznam</span><span class="sxs-lookup"><span data-stu-id="ed8f5-159">Record</span></span> | <span data-ttu-id="ed8f5-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="ed8f5-160">Object</span></span>
<span data-ttu-id="ed8f5-161">Pole</span><span class="sxs-lookup"><span data-stu-id="ed8f5-161">Array</span></span> | <span data-ttu-id="ed8f5-162">Pole</span><span class="sxs-lookup"><span data-stu-id="ed8f5-162">Array</span></span>
<span data-ttu-id="ed8f5-163">HODNOTU NULL</span><span class="sxs-lookup"><span data-stu-id="ed8f5-163">NULL</span></span> | <span data-ttu-id="ed8f5-164">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="ed8f5-164">Null</span></span>


<span data-ttu-id="ed8f5-165">Zde jsou převody JavaScript Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="ed8f5-165">Here are JavaScript-to-Stream Analytics conversions:</span></span>


<span data-ttu-id="ed8f5-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed8f5-166">JavaScript</span></span> | <span data-ttu-id="ed8f5-167">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ed8f5-167">Stream Analytics</span></span>
--- | ---
<span data-ttu-id="ed8f5-168">Číslo</span><span class="sxs-lookup"><span data-stu-id="ed8f5-168">Number</span></span> | <span data-ttu-id="ed8f5-169">Bigint (Pokud je číslo se zaokrouhlí a mezi dlouho. MinValue a dlouho. MaxValue; jinak je dvojité)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-169">Bigint (if the number is round and between long.MinValue and long.MaxValue; otherwise, it's double)</span></span>
<span data-ttu-id="ed8f5-170">Datum</span><span class="sxs-lookup"><span data-stu-id="ed8f5-170">Date</span></span> | <span data-ttu-id="ed8f5-171">Data a času</span><span class="sxs-lookup"><span data-stu-id="ed8f5-171">DateTime</span></span>
<span data-ttu-id="ed8f5-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8f5-172">String</span></span> | <span data-ttu-id="ed8f5-173">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-173">nvarchar(MAX)</span></span>
<span data-ttu-id="ed8f5-174">Objekt</span><span class="sxs-lookup"><span data-stu-id="ed8f5-174">Object</span></span> | <span data-ttu-id="ed8f5-175">záznam</span><span class="sxs-lookup"><span data-stu-id="ed8f5-175">Record</span></span>
<span data-ttu-id="ed8f5-176">Pole</span><span class="sxs-lookup"><span data-stu-id="ed8f5-176">Array</span></span> | <span data-ttu-id="ed8f5-177">Pole</span><span class="sxs-lookup"><span data-stu-id="ed8f5-177">Array</span></span>
<span data-ttu-id="ed8f5-178">Hodnotu Null, Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="ed8f5-178">Null, Undefined</span></span> | <span data-ttu-id="ed8f5-179">HODNOTU NULL</span><span class="sxs-lookup"><span data-stu-id="ed8f5-179">NULL</span></span>
<span data-ttu-id="ed8f5-180">Jiný typ (například funkce nebo chyba)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-180">Any other type (for example, a function or error)</span></span> | <span data-ttu-id="ed8f5-181">Není podporované (výsledky v chybě za běhu)</span><span class="sxs-lookup"><span data-stu-id="ed8f5-181">Not supported (results in runtime error)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ed8f5-182">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="ed8f5-182">Troubleshooting</span></span>
<span data-ttu-id="ed8f5-183">Chyby jazyka JavaScript runtime jsou považovány za kritické a jsou prezentované prostřednictvím protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-183">JavaScript runtime errors are considered fatal, and are surfaced through the Activity log.</span></span> <span data-ttu-id="ed8f5-184">Načtení protokolu, na portálu Azure přejděte na úlohu a vyberte **protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-184">To retrieve the log, in the Azure portal, go to your job and select **Activity log**.</span></span>


## <a name="other-javascript-user-defined-function-patterns"></a><span data-ttu-id="ed8f5-185">Dalšími vzory uživatelsky definované funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="ed8f5-185">Other JavaScript user-defined function patterns</span></span>

### <a name="write-nested-json-to-output"></a><span data-ttu-id="ed8f5-186">Zápis vnořené JSON na výstup</span><span class="sxs-lookup"><span data-stu-id="ed8f5-186">Write nested JSON to output</span></span>
<span data-ttu-id="ed8f5-187">Pokud máte následné zpracování krok, který používá úloha Stream Analytics výstup jako vstup a vyžaduje formátu JSON, můžete napsat do výstupního řetězce formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-187">If you have a follow-up processing step that uses a Stream Analytics job output as input, and it requires a JSON format, you can write a JSON string to output.</span></span> <span data-ttu-id="ed8f5-188">Další příklad volání **JSON.stringify()** funkci, aby se pack všechny dvojice název/hodnota vstupu a zapsat je jako jeden řetězec hodnotu ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="ed8f5-188">The next example calls the **JSON.stringify()** function to pack all name/value pairs of the input, and then write them as a single string value in output.</span></span>

<span data-ttu-id="ed8f5-189">**Definice JavaScript uživatelsky definované funkce:**</span><span class="sxs-lookup"><span data-stu-id="ed8f5-189">**JavaScript user-defined function definition:**</span></span>

```
function main(x) {
return JSON.stringify(x);
}
```

<span data-ttu-id="ed8f5-190">**Ukázkový dotaz:**</span><span class="sxs-lookup"><span data-stu-id="ed8f5-190">**Sample query:**</span></span>
```
SELECT
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a><span data-ttu-id="ed8f5-191">Podpora</span><span class="sxs-lookup"><span data-stu-id="ed8f5-191">Get help</span></span>
<span data-ttu-id="ed8f5-192">Potřebujete další pomoc, zkuste naši [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ed8f5-192">For additional help, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ed8f5-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed8f5-193">Next steps</span></span>
* [<span data-ttu-id="ed8f5-194">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ed8f5-194">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ed8f5-195">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ed8f5-195">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ed8f5-196">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ed8f5-196">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ed8f5-197">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ed8f5-197">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ed8f5-198">Správa Azure Stream Analytics odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="ed8f5-198">Azure Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
