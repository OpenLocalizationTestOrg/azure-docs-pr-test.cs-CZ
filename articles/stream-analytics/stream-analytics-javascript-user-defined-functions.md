---
title: "uživatelem definované funkce Stream Analytics JavaScript aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 28eeb8f6437c23989e8887687b950361fed4414c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-javascript-user-defined-functions"></a><span data-ttu-id="318fc-104">Uživatelem definované funkce Azure Stream Analytics JavaScript</span><span class="sxs-lookup"><span data-stu-id="318fc-104">Azure Stream Analytics JavaScript user-defined functions</span></span>
<span data-ttu-id="318fc-105">Azure Stream Analytics podporuje uživatelsky definované funkce, které jsou napsané v jazyce JavaScript.</span><span class="sxs-lookup"><span data-stu-id="318fc-105">Azure Stream Analytics supports user-defined functions written in JavaScript.</span></span> <span data-ttu-id="318fc-106">S bohatou sadu hello **řetězec**, **RegExp**, **matematické**, **pole**, a **datum** metody, Transformace komplexní dat pomocí úlohy Stream Analytics poskytuje JavaScript, stane jednodušší toocreate.</span><span class="sxs-lookup"><span data-stu-id="318fc-106">With hello rich set of **String**, **RegExp**, **Math**, **Array**, and **Date** methods that JavaScript provides, complex data transformations with Stream Analytics jobs become easier toocreate.</span></span>

## <a name="javascript-user-defined-functions"></a><span data-ttu-id="318fc-107">Uživatelem definované funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="318fc-107">JavaScript user-defined functions</span></span>
<span data-ttu-id="318fc-108">Uživatelem definované funkce jazyka JavaScript podporovat bezstavové, pouze výpočetní skalární funkce, které nevyžadují externí připojení.</span><span class="sxs-lookup"><span data-stu-id="318fc-108">JavaScript user-defined functions support stateless, compute-only scalar functions that do not require external connectivity.</span></span> <span data-ttu-id="318fc-109">Hello návratové hodnoty funkce lze pouze skalární hodnotu (jeden).</span><span class="sxs-lookup"><span data-stu-id="318fc-109">hello return value of a function can only be a scalar (single) value.</span></span> <span data-ttu-id="318fc-110">Po přidání úlohu tooa uživatelsky definované funkce JavaScript, je funkce hello použít kdekoli v hello dotazu, jako je integrovaná skalární funkce.</span><span class="sxs-lookup"><span data-stu-id="318fc-110">After you add a JavaScript user-defined function tooa job, you can use hello function anywhere in hello query, like a built-in scalar function.</span></span>

<span data-ttu-id="318fc-111">Tady je několik scénářů, kde mohou být užitečné JavaScript uživatelsky definované funkce:</span><span class="sxs-lookup"><span data-stu-id="318fc-111">Here are some scenarios where you might find JavaScript user-defined functions useful:</span></span>
* <span data-ttu-id="318fc-112">Analýza a manipulace s řetězci, které mají regulární výraz funkce, například **Regexp_Replace()** a **Regexp_Extract()**</span><span class="sxs-lookup"><span data-stu-id="318fc-112">Parsing and manipulating strings that have regular expression functions, for example, **Regexp_Replace()** and **Regexp_Extract()**</span></span>
* <span data-ttu-id="318fc-113">Kódování dat, například binární šestnáctkově převod a dekódování</span><span class="sxs-lookup"><span data-stu-id="318fc-113">Decoding and encoding data, for example, binary-to-hex conversion</span></span>
* <span data-ttu-id="318fc-114">Provádění mathematic výpočtů v jazyce JavaScript **matematické** funkce</span><span class="sxs-lookup"><span data-stu-id="318fc-114">Performing mathematic computations with JavaScript **Math** functions</span></span>
* <span data-ttu-id="318fc-115">Provádění operací pole jako řazení, spojení, najít a výplně</span><span class="sxs-lookup"><span data-stu-id="318fc-115">Performing array operations like sort, join, find, and fill</span></span>

<span data-ttu-id="318fc-116">Zde jsou některé věci, které nemůžete dělat s uživatelem definované funkce jazyka JavaScript v Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="318fc-116">Here are some things that you cannot do with a JavaScript user-defined function in Stream Analytics:</span></span>
* <span data-ttu-id="318fc-117">Volání na externí koncové body REST, například provádění obrátíte vyhledávání IP nebo vyžádání referenční data z externího zdroje</span><span class="sxs-lookup"><span data-stu-id="318fc-117">Call out external REST endpoints, for example, performing reverse IP lookup or pulling reference data from an external source</span></span>
* <span data-ttu-id="318fc-118">Deserializace na vstupy/výstupy nebo provést vlastní události formát serializace</span><span class="sxs-lookup"><span data-stu-id="318fc-118">Perform custom event format serialization or deserialization on inputs/outputs</span></span>
* <span data-ttu-id="318fc-119">Vytvořte vlastní agregace</span><span class="sxs-lookup"><span data-stu-id="318fc-119">Create custom aggregates</span></span>

<span data-ttu-id="318fc-120">I když funguje jako **Date.GetDate()** nebo **Math.random()** nejsou blokována v definici funkce hello byste neměli používat je.</span><span class="sxs-lookup"><span data-stu-id="318fc-120">Although functions like **Date.GetDate()** or **Math.random()** are not blocked in hello functions definition, you should avoid using them.</span></span> <span data-ttu-id="318fc-121">Tyto funkce **nepodporují** návratový hello stejné vést pokaždé, když je volají a hello služby Azure Stream Analytics nezachovat deník volání funkce a vrátí výsledky.</span><span class="sxs-lookup"><span data-stu-id="318fc-121">These functions **do not** return hello same result every time you call them, and hello Azure Stream Analytics service does not keep a journal of function invocations and returned results.</span></span> <span data-ttu-id="318fc-122">Pokud funkce vrátí výsledek různých na hello stejné události, opakovatelnost není zaručena při restartování úlohy vámi nebo služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="318fc-122">If a function returns different result on hello same events, repeatability is not guaranteed when a job is restarted by you or by hello Stream Analytics service.</span></span>

## <a name="add-a-javascript-user-defined-function-in-hello-azure-portal"></a><span data-ttu-id="318fc-123">Přidání uživatelem definované funkce jazyka JavaScript v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="318fc-123">Add a JavaScript user-defined function in hello Azure portal</span></span>
<span data-ttu-id="318fc-124">toocreate jednoduché funkce jazyka JavaScript uživatelem definované v části existující úlohy Stream Analytics, proveďte tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="318fc-124">toocreate a simple JavaScript user-defined function under an existing Stream Analytics job, do these steps:</span></span>

1.  <span data-ttu-id="318fc-125">V hello portálu Azure najděte úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="318fc-125">In hello Azure portal, find your Stream Analytics job.</span></span>
2.  <span data-ttu-id="318fc-126">V části **úlohy TOPOLOGIE**, vyberte funkce.</span><span class="sxs-lookup"><span data-stu-id="318fc-126">Under **JOB TOPOLOGY**, select your function.</span></span> <span data-ttu-id="318fc-127">Zobrazí se prázdný seznam funkcí.</span><span class="sxs-lookup"><span data-stu-id="318fc-127">An empty list of functions appears.</span></span>
3.  <span data-ttu-id="318fc-128">toocreate nové uživatelsky definované funkce, vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="318fc-128">toocreate a new user-defined function, select **Add**.</span></span>
4.  <span data-ttu-id="318fc-129">Na hello **novou funkci** okně pro **typ funkce**, vyberte **JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="318fc-129">On hello **New Function** blade, for **Function Type**, select **JavaScript**.</span></span> <span data-ttu-id="318fc-130">Funkce výchozí šablona služby se zobrazí v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="318fc-130">A default function template appears in hello editor.</span></span>
5.  <span data-ttu-id="318fc-131">Pro hello **UDF alias**, zadejte **hex2Int**a změňte implementace funkce hello následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="318fc-131">For hello **UDF alias**, enter **hex2Int**, and change hello function implementation as follows:</span></span>

    ```
    // Convert Hex value toointeger.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  <span data-ttu-id="318fc-132">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="318fc-132">Select **Save**.</span></span> <span data-ttu-id="318fc-133">Funkce se zobrazí v seznamu hello funkcí.</span><span class="sxs-lookup"><span data-stu-id="318fc-133">Your function appears in hello list of functions.</span></span>
7.  <span data-ttu-id="318fc-134">Vyberte hello nové **hex2Int** funkce a zkontrolujte definici funkce hello.</span><span class="sxs-lookup"><span data-stu-id="318fc-134">Select hello new **hex2Int** function, and check hello function definition.</span></span> <span data-ttu-id="318fc-135">Mají všechny funkce **UDF** předponu přidané toohello funkce alias.</span><span class="sxs-lookup"><span data-stu-id="318fc-135">All functions have a **UDF** prefix added toohello function alias.</span></span> <span data-ttu-id="318fc-136">Je třeba příliš*obsahovat předponu hello* při volání funkce hello v dotazu Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="318fc-136">You need too*include hello prefix* when you call hello function in your Stream Analytics query.</span></span> <span data-ttu-id="318fc-137">V takovém případě volání **UDF.hex2Int**.</span><span class="sxs-lookup"><span data-stu-id="318fc-137">In this case, you call **UDF.hex2Int**.</span></span>

## <a name="call-a-javascript-user-defined-function-in-a-query"></a><span data-ttu-id="318fc-138">Volání uživatelem definované funkce JavaScript v dotazu</span><span class="sxs-lookup"><span data-stu-id="318fc-138">Call a JavaScript user-defined function in a query</span></span>

1. <span data-ttu-id="318fc-139">V hello dotazu editor, v části **úlohy TOPOLOGIE**, vyberte **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="318fc-139">In hello query editor, under **JOB TOPOLOGY**, select **Query**.</span></span>
2.  <span data-ttu-id="318fc-140">Upravit dotaz a pak zavolají hello uživatelsky definované funkce, například takto:</span><span class="sxs-lookup"><span data-stu-id="318fc-140">Edit your query, and then call hello user-defined function, like this:</span></span>

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  <span data-ttu-id="318fc-141">tooupload hello ukázkový datový soubor, klikněte pravým tlačítkem na hello úlohy vstup.</span><span class="sxs-lookup"><span data-stu-id="318fc-141">tooupload hello sample data file, right-click hello job input.</span></span>
4.  <span data-ttu-id="318fc-142">tootest dotazu, vyberte **Test**.</span><span class="sxs-lookup"><span data-stu-id="318fc-142">tootest your query, select **Test**.</span></span>


## <a name="supported-javascript-objects"></a><span data-ttu-id="318fc-143">Podporované JavaScript – objekty</span><span class="sxs-lookup"><span data-stu-id="318fc-143">Supported JavaScript objects</span></span>
<span data-ttu-id="318fc-144">Uživatelem definované funkce Azure Stream Analytics JavaScript podporují standard, předdefinovaných objektů jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="318fc-144">Azure Stream Analytics JavaScript user-defined functions support standard, built-in JavaScript objects.</span></span> <span data-ttu-id="318fc-145">Seznam těchto objektů najdete v tématu [globální objekty](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span><span class="sxs-lookup"><span data-stu-id="318fc-145">For a list of these objects, see [Global Objects](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).</span></span>

### <a name="stream-analytics-and-javascript-type-conversion"></a><span data-ttu-id="318fc-146">Stream Analytics a JavaScript typ – převod</span><span class="sxs-lookup"><span data-stu-id="318fc-146">Stream Analytics and JavaScript type conversion</span></span>

<span data-ttu-id="318fc-147">Existují rozdíly v hello typy, které podporují hello Stream Analytics dotazovací jazyk a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="318fc-147">There are differences in hello types that hello Stream Analytics query language and JavaScript support.</span></span> <span data-ttu-id="318fc-148">Tato tabulka uvádí hello převod mapování mezi hello dva:</span><span class="sxs-lookup"><span data-stu-id="318fc-148">This table lists hello conversion mappings between hello two:</span></span>

<span data-ttu-id="318fc-149">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="318fc-149">Stream Analytics</span></span> | <span data-ttu-id="318fc-150">JavaScript</span><span class="sxs-lookup"><span data-stu-id="318fc-150">JavaScript</span></span>
--- | ---
<span data-ttu-id="318fc-151">bigint</span><span class="sxs-lookup"><span data-stu-id="318fc-151">bigint</span></span> | <span data-ttu-id="318fc-152">Číslo (JavaScript může představovat jenom celá čísla nahoru tooprecisely 2 ^ 53)</span><span class="sxs-lookup"><span data-stu-id="318fc-152">Number (JavaScript can only represent integers up tooprecisely 2^53)</span></span>
<span data-ttu-id="318fc-153">Data a času</span><span class="sxs-lookup"><span data-stu-id="318fc-153">DateTime</span></span> | <span data-ttu-id="318fc-154">Datum (JavaScript pouze podporuje v milisekundách)</span><span class="sxs-lookup"><span data-stu-id="318fc-154">Date (JavaScript only supports milliseconds)</span></span>
<span data-ttu-id="318fc-155">Double</span><span class="sxs-lookup"><span data-stu-id="318fc-155">double</span></span> | <span data-ttu-id="318fc-156">Číslo</span><span class="sxs-lookup"><span data-stu-id="318fc-156">Number</span></span>
<span data-ttu-id="318fc-157">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="318fc-157">nvarchar(MAX)</span></span> | <span data-ttu-id="318fc-158">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318fc-158">String</span></span>
<span data-ttu-id="318fc-159">záznam</span><span class="sxs-lookup"><span data-stu-id="318fc-159">Record</span></span> | <span data-ttu-id="318fc-160">Objekt</span><span class="sxs-lookup"><span data-stu-id="318fc-160">Object</span></span>
<span data-ttu-id="318fc-161">Pole</span><span class="sxs-lookup"><span data-stu-id="318fc-161">Array</span></span> | <span data-ttu-id="318fc-162">Pole</span><span class="sxs-lookup"><span data-stu-id="318fc-162">Array</span></span>
<span data-ttu-id="318fc-163">HODNOTU NULL</span><span class="sxs-lookup"><span data-stu-id="318fc-163">NULL</span></span> | <span data-ttu-id="318fc-164">Hodnotu Null</span><span class="sxs-lookup"><span data-stu-id="318fc-164">Null</span></span>


<span data-ttu-id="318fc-165">Zde jsou převody JavaScript Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="318fc-165">Here are JavaScript-to-Stream Analytics conversions:</span></span>


<span data-ttu-id="318fc-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="318fc-166">JavaScript</span></span> | <span data-ttu-id="318fc-167">Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="318fc-167">Stream Analytics</span></span>
--- | ---
<span data-ttu-id="318fc-168">Číslo</span><span class="sxs-lookup"><span data-stu-id="318fc-168">Number</span></span> | <span data-ttu-id="318fc-169">Bigint (Pokud hello číslo se zaokrouhlí a mezi dlouho. MinValue a dlouho. MaxValue; jinak je dvojité)</span><span class="sxs-lookup"><span data-stu-id="318fc-169">Bigint (if hello number is round and between long.MinValue and long.MaxValue; otherwise, it's double)</span></span>
<span data-ttu-id="318fc-170">Datum</span><span class="sxs-lookup"><span data-stu-id="318fc-170">Date</span></span> | <span data-ttu-id="318fc-171">Data a času</span><span class="sxs-lookup"><span data-stu-id="318fc-171">DateTime</span></span>
<span data-ttu-id="318fc-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="318fc-172">String</span></span> | <span data-ttu-id="318fc-173">nvarchar(max)</span><span class="sxs-lookup"><span data-stu-id="318fc-173">nvarchar(MAX)</span></span>
<span data-ttu-id="318fc-174">Objekt</span><span class="sxs-lookup"><span data-stu-id="318fc-174">Object</span></span> | <span data-ttu-id="318fc-175">záznam</span><span class="sxs-lookup"><span data-stu-id="318fc-175">Record</span></span>
<span data-ttu-id="318fc-176">Pole</span><span class="sxs-lookup"><span data-stu-id="318fc-176">Array</span></span> | <span data-ttu-id="318fc-177">Pole</span><span class="sxs-lookup"><span data-stu-id="318fc-177">Array</span></span>
<span data-ttu-id="318fc-178">Hodnotu Null, Nedefinovaná</span><span class="sxs-lookup"><span data-stu-id="318fc-178">Null, Undefined</span></span> | <span data-ttu-id="318fc-179">HODNOTU NULL</span><span class="sxs-lookup"><span data-stu-id="318fc-179">NULL</span></span>
<span data-ttu-id="318fc-180">Jiný typ (například funkce nebo chyba)</span><span class="sxs-lookup"><span data-stu-id="318fc-180">Any other type (for example, a function or error)</span></span> | <span data-ttu-id="318fc-181">Není podporované (výsledky v chybě za běhu)</span><span class="sxs-lookup"><span data-stu-id="318fc-181">Not supported (results in runtime error)</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="318fc-182">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="318fc-182">Troubleshooting</span></span>
<span data-ttu-id="318fc-183">Chyby jazyka JavaScript runtime jsou považovány za kritické a jsou prezentované prostřednictvím protokolu aktivit hello.</span><span class="sxs-lookup"><span data-stu-id="318fc-183">JavaScript runtime errors are considered fatal, and are surfaced through hello Activity log.</span></span> <span data-ttu-id="318fc-184">tooretrieve hello přihlášení, hello portálu Azure, přejděte tooyour úlohy a vyberte **protokol aktivit**.</span><span class="sxs-lookup"><span data-stu-id="318fc-184">tooretrieve hello log, in hello Azure portal, go tooyour job and select **Activity log**.</span></span>


## <a name="other-javascript-user-defined-function-patterns"></a><span data-ttu-id="318fc-185">Dalšími vzory uživatelsky definované funkce jazyka JavaScript</span><span class="sxs-lookup"><span data-stu-id="318fc-185">Other JavaScript user-defined function patterns</span></span>

### <a name="write-nested-json-toooutput"></a><span data-ttu-id="318fc-186">Zápis vnořené toooutput JSON</span><span class="sxs-lookup"><span data-stu-id="318fc-186">Write nested JSON toooutput</span></span>
<span data-ttu-id="318fc-187">Pokud máte následné zpracování krok, který používá úloha Stream Analytics výstup jako vstup a vyžaduje formátu JSON, můžete napsat toooutput řetězec formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="318fc-187">If you have a follow-up processing step that uses a Stream Analytics job output as input, and it requires a JSON format, you can write a JSON string toooutput.</span></span> <span data-ttu-id="318fc-188">Další příklad Hello volá hello **JSON.stringify()** funkce toopack všechny dvojice název hodnota hello vstup a zapsat je jako jeden řetězec hodnotu ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="318fc-188">hello next example calls hello **JSON.stringify()** function toopack all name/value pairs of hello input, and then write them as a single string value in output.</span></span>

<span data-ttu-id="318fc-189">**Definice JavaScript uživatelsky definované funkce:**</span><span class="sxs-lookup"><span data-stu-id="318fc-189">**JavaScript user-defined function definition:**</span></span>

```
function main(x) {
return JSON.stringify(x);
}
```

<span data-ttu-id="318fc-190">**Ukázkový dotaz:**</span><span class="sxs-lookup"><span data-stu-id="318fc-190">**Sample query:**</span></span>
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

## <a name="get-help"></a><span data-ttu-id="318fc-191">Podpora</span><span class="sxs-lookup"><span data-stu-id="318fc-191">Get help</span></span>
<span data-ttu-id="318fc-192">Potřebujete další pomoc, zkuste naši [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="318fc-192">For additional help, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="318fc-193">Další kroky</span><span class="sxs-lookup"><span data-stu-id="318fc-193">Next steps</span></span>
* [<span data-ttu-id="318fc-194">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="318fc-194">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="318fc-195">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="318fc-195">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="318fc-196">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="318fc-196">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="318fc-197">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="318fc-197">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="318fc-198">Správa Azure Stream Analytics odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="318fc-198">Azure Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
