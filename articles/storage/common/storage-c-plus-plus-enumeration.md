---
title: "Seznam prostředků úložiště Azure s knihovnou klienta úložiště pro jazyk C++ | Microsoft Docs"
description: "Naučte se používat rozhraní API seznamu v Microsoft Azure Klientská knihovna pro úložiště pro jazyk C++ výčet kontejnerů, objektů BLOB, fronty, tabulky a entity."
documentationcenter: .net
services: storage
author: dineshmurthy
manager: jahogg
editor: tysonn
ms.assetid: 33563639-2945-4567-9254-bc4a7e80698f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: dineshm
ms.openlocfilehash: 9844412739f4f6f95416f81347f0f2eeeca62bea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="list-azure-storage-resources-in-c"></a><span data-ttu-id="770d8-103">Seznam prostředků úložiště Azure v jazyce C++</span><span class="sxs-lookup"><span data-stu-id="770d8-103">List Azure Storage resources in C++</span></span>
<span data-ttu-id="770d8-104">Seznam operací jsou klíčem k mnoha vývojové scénáře s Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="770d8-104">Listing operations are key to many development scenarios with Azure Storage.</span></span> <span data-ttu-id="770d8-105">Tento článek popisuje postup nejvíce efektivně zobrazení výčtu objektů ve službě Azure Storage pomocí rozhraní API, které jsou součástí Klientská knihovna pro úložiště Microsoft Azure pro jazyk C++ výpis.</span><span class="sxs-lookup"><span data-stu-id="770d8-105">This article describes how to most efficiently enumerate objects in Azure Storage using the listing APIs provided in the Microsoft Azure Storage Client Library for C++.</span></span>

> [!NOTE]
> <span data-ttu-id="770d8-106">Tato příručka cílem Klientská knihovna pro úložiště Azure pro C++ verze 2.x, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp).</span><span class="sxs-lookup"><span data-stu-id="770d8-106">This guide targets the Azure Storage Client Library for C++ version 2.x, which is available via [NuGet](http://www.nuget.org/packages/wastorage) or [GitHub](https://github.com/Azure/azure-storage-cpp).</span></span>
> 
> 

<span data-ttu-id="770d8-107">Klientská knihovna pro úložiště poskytuje celou řadu metod na seznam nebo dotaz, objekty ve službě Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="770d8-107">The Storage Client Library provides a variety of methods to list or query objects in Azure Storage.</span></span> <span data-ttu-id="770d8-108">Tento článek řeší následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="770d8-108">This article addresses the following scenarios:</span></span>

* <span data-ttu-id="770d8-109">Seznam kontejnery v účtu</span><span class="sxs-lookup"><span data-stu-id="770d8-109">List containers in an account</span></span>
* <span data-ttu-id="770d8-110">Seznam objektů BLOB v kontejneru nebo objekt blob virtuální adresář</span><span class="sxs-lookup"><span data-stu-id="770d8-110">List blobs in a container or virtual blob directory</span></span>
* <span data-ttu-id="770d8-111">Seznam front v účtu</span><span class="sxs-lookup"><span data-stu-id="770d8-111">List queues in an account</span></span>
* <span data-ttu-id="770d8-112">Seznam tabulek v účtu</span><span class="sxs-lookup"><span data-stu-id="770d8-112">List tables in an account</span></span>
* <span data-ttu-id="770d8-113">Dotaz entity v tabulce</span><span class="sxs-lookup"><span data-stu-id="770d8-113">Query entities in a table</span></span>

<span data-ttu-id="770d8-114">Každá z těchto metod se zobrazí při použití různých přetížení pro různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="770d8-114">Each of these methods is shown using different overloads for different scenarios.</span></span>

## <a name="asynchronous-versus-synchronous"></a><span data-ttu-id="770d8-115">Asynchronní a synchronní</span><span class="sxs-lookup"><span data-stu-id="770d8-115">Asynchronous versus synchronous</span></span>
<span data-ttu-id="770d8-116">Vzhledem k tomu, že klientská knihovna pro úložiště pro jazyk C++ je postavený na [knihovny C++ REST](https://github.com/Microsoft/cpprestsdk), podporujeme ze své podstaty asynchronních operací s použitím [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span><span class="sxs-lookup"><span data-stu-id="770d8-116">Because the Storage Client Library for C++ is built on top of the [C++ REST library](https://github.com/Microsoft/cpprestsdk), we inherently support asynchronous operations by using [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html).</span></span> <span data-ttu-id="770d8-117">Například:</span><span class="sxs-lookup"><span data-stu-id="770d8-117">For example:</span></span>

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

<span data-ttu-id="770d8-118">Synchronní operace zabalení odpovídající asynchronních operací:</span><span class="sxs-lookup"><span data-stu-id="770d8-118">Synchronous operations wrap the corresponding asynchronous operations:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

<span data-ttu-id="770d8-119">Pokud pracujete s více vláken aplikacím nebo službám, doporučujeme použít asynchronní rozhraní API přímo místo vytvoření vlákna k volání rozhraní API, což významně ovlivňuje výkon synchronizace.</span><span class="sxs-lookup"><span data-stu-id="770d8-119">If you are working with multiple threading applications or services, we recommend that you use the async APIs directly instead of creating a thread to call the sync APIs, which significantly impacts your performance.</span></span>

## <a name="segmented-listing"></a><span data-ttu-id="770d8-120">Segmentované výpis</span><span class="sxs-lookup"><span data-stu-id="770d8-120">Segmented listing</span></span>
<span data-ttu-id="770d8-121">Škálování cloudové úložiště vyžaduje segmentovaným výpis.</span><span class="sxs-lookup"><span data-stu-id="770d8-121">The scale of cloud storage requires segmented listing.</span></span> <span data-ttu-id="770d8-122">Například můžete mít přes milion objekty BLOB v kontejneru objektů blob v Azure nebo přes miliardu entity v Azure Table.</span><span class="sxs-lookup"><span data-stu-id="770d8-122">For example, you can have over a million blobs in an Azure blob container or over a billion entities in an Azure Table.</span></span> <span data-ttu-id="770d8-123">Tyto nejsou teoretické čísla, ale případy využití skutečné oddělení.</span><span class="sxs-lookup"><span data-stu-id="770d8-123">These are not theoretical numbers, but real customer usage cases.</span></span>

<span data-ttu-id="770d8-124">Je proto nepraktické seznam všech objektů v odpověď o jedné.</span><span class="sxs-lookup"><span data-stu-id="770d8-124">It is therefore impractical to list all objects in a single response.</span></span> <span data-ttu-id="770d8-125">Místo toho můžete vytvořit seznam objektů s použitím stránkování.</span><span class="sxs-lookup"><span data-stu-id="770d8-125">Instead, you can list objects using paging.</span></span> <span data-ttu-id="770d8-126">Každý výpis rozhraní API má *segmentované* přetížení.</span><span class="sxs-lookup"><span data-stu-id="770d8-126">Each of the listing APIs has a *segmented* overload.</span></span>

<span data-ttu-id="770d8-127">Odpověď operace segmentovaným výpis obsahuje:</span><span class="sxs-lookup"><span data-stu-id="770d8-127">The response for a segmented listing operation includes:</span></span>

* <span data-ttu-id="770d8-128"><i>_segment</i>, který obsahuje sadu výsledků vrácených pro jednoho volání rozhraní API seznamu.</span><span class="sxs-lookup"><span data-stu-id="770d8-128"><i>_segment</i>, which contains the set of results returned for a single call to the listing API.</span></span>
* <span data-ttu-id="770d8-129">*continuation_token*, který je předat do dalšího hovoru, chcete-li získat další stránky výsledků.</span><span class="sxs-lookup"><span data-stu-id="770d8-129">*continuation_token*, which is passed to the next call in order to get the next page of results.</span></span> <span data-ttu-id="770d8-130">Pokud nejsou žádné další výsledky vrátí, token pro pokračování není null.</span><span class="sxs-lookup"><span data-stu-id="770d8-130">When there are no more results to return, the continuation token is null.</span></span>

<span data-ttu-id="770d8-131">Například typický volání k vypsání všech objektů BLOB v kontejneru vypadat jako následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="770d8-131">For example, a typical call to list all blobs in a container may look like the following code snippet.</span></span> <span data-ttu-id="770d8-132">Kód je k dispozici v našem [ukázky](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span><span class="sxs-lookup"><span data-stu-id="770d8-132">The code is available in our [samples](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):</span></span>

```cpp
// List blobs in the blob container
azure::storage::continuation_token token;
do
{
    azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_diretory(it->as_directory());
    }
}

    token = segment.continuation_token();
}
while (!token.empty());
```

<span data-ttu-id="770d8-133">Všimněte si, že počet výsledků vrácených na stránce je řízena parametr *max_results* v přetížení každé rozhraní API, například:</span><span class="sxs-lookup"><span data-stu-id="770d8-133">Note that the number of results returned in a page can be controlled by the parameter *max_results* in the overload of each API, for example:</span></span>

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

<span data-ttu-id="770d8-134">Pokud nezadáte *max_results* parametr, bude výchozí maximální hodnota, která až 5000 výsledky se vrátí v jediné stránce.</span><span class="sxs-lookup"><span data-stu-id="770d8-134">If you do not specify the *max_results* parameter, the default maximum value of up to 5000 results is returned in a single page.</span></span>

<span data-ttu-id="770d8-135">Všimněte si, že dotaz Azure Table storage může vrátit žádné záznamy nebo méně záznamy než hodnota *max_results* parametr, který jste zadali, i když token pro pokračování není prázdný.</span><span class="sxs-lookup"><span data-stu-id="770d8-135">Also note that a query against Azure Table storage may return no records, or fewer records than the value of the *max_results* parameter that you specified, even if the continuation token is not empty.</span></span> <span data-ttu-id="770d8-136">Jedním z důvodů může být, že dotaz nelze dokončit v pět sekund.</span><span class="sxs-lookup"><span data-stu-id="770d8-136">One reason might be that the query could not complete in five seconds.</span></span> <span data-ttu-id="770d8-137">Také token pro pokračování není prázdná, dotaz by měly pokračovat ve a váš kód by neměl předpokládá velikost segmentu výsledky.</span><span class="sxs-lookup"><span data-stu-id="770d8-137">As long as the continuation token is not empty, the query should continue, and your code should not assume the size of segment results.</span></span>

<span data-ttu-id="770d8-138">Doporučené kódování vzor pro většinu scénářů je segmentované výpis, který poskytuje explicitní průběh výpis nebo dotazování a jak službu reagují na každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="770d8-138">The recommended coding pattern for most scenarios is segmented listing, which provides explicit progress of listing or querying, and how the service responds to each request.</span></span> <span data-ttu-id="770d8-139">Platí to hlavně o služby nebo aplikace C++ mohou pomoci nižší úrovně ovládací prvek průběh výpis řízení paměti a výkon.</span><span class="sxs-lookup"><span data-stu-id="770d8-139">Particularly for C++ applications or services, lower-level control of the listing progress may help control memory and performance.</span></span>

## <a name="greedy-listing"></a><span data-ttu-id="770d8-140">Výpis typu greedy</span><span class="sxs-lookup"><span data-stu-id="770d8-140">Greedy listing</span></span>
<span data-ttu-id="770d8-141">Starší verze Klientská knihovna pro úložiště pro jazyk C++ (verze 0.5.0 Preview a starší) zahrnuté nesegmentovaný výpis rozhraní API pro tabulky a fronty, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="770d8-141">Earlier versions of the Storage Client Library for C++ (versions 0.5.0 Preview and earlier) included non-segmented listing APIs for tables and queues, as in the following example:</span></span>

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

<span data-ttu-id="770d8-142">Tyto metody byly implementovány jako obálky segmentovaným rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="770d8-142">These methods were implemented as wrappers of segmented APIs.</span></span> <span data-ttu-id="770d8-143">Pro každou odpověď segmentovaným výpis kód připojí výsledky na vektor a vrátí všechny výsledky po skenování úplné kontejnery.</span><span class="sxs-lookup"><span data-stu-id="770d8-143">For each response of segmented listing, the code appended the results to a vector and returned all results after the full containers were scanned.</span></span>

<span data-ttu-id="770d8-144">Tento přístup může fungovat, pokud účet úložiště nebo tabulka obsahuje malý počet objektů.</span><span class="sxs-lookup"><span data-stu-id="770d8-144">This approach might work when the storage account or table contains a small number of objects.</span></span> <span data-ttu-id="770d8-145">Však větší počet objektů, požadované paměti může zvýšit bez omezení, protože všechny výsledky zůstává v paměti.</span><span class="sxs-lookup"><span data-stu-id="770d8-145">However, with an increase in the number of objects, the memory required could increase without limit, because all results remained in memory.</span></span> <span data-ttu-id="770d8-146">Jedné operace výpisu může trvat velmi dlouhou dobu, během které měl volající žádné informace o jejím průběhu.</span><span class="sxs-lookup"><span data-stu-id="770d8-146">One listing operation can take a very long time, during which the caller had no information about its progress.</span></span>

<span data-ttu-id="770d8-147">Tyto typu greedy výpis rozhraní API v sadě SDK neexistují v C#, Java nebo prostředí JavaScript Node.js.</span><span class="sxs-lookup"><span data-stu-id="770d8-147">These greedy listing APIs in the SDK do not exist in C#, Java, or the JavaScript Node.js environment.</span></span> <span data-ttu-id="770d8-148">Aby se zabránilo potenciálních problémů pomocí těchto typu greedy rozhraní API, jsme je v verzi odebrána 0.6.0 Preview.</span><span class="sxs-lookup"><span data-stu-id="770d8-148">To avoid the potential issues of using these greedy APIs, we removed them in version 0.6.0 Preview.</span></span>

<span data-ttu-id="770d8-149">Pokud váš kód volá tyto typu greedy rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="770d8-149">If your code is calling these greedy APIs:</span></span>

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

<span data-ttu-id="770d8-150">Potom upravte kód pro použití na segmentovaným výpis rozhraní API:</span><span class="sxs-lookup"><span data-stu-id="770d8-150">Then you should modify your code to use the segmented listing APIs:</span></span>

```cpp
azure::storage::continuation_token token;
do
{
    azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
    for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        process_entity(*it);
    }

    token = segment.continuation_token();
} while (!token.empty());
```

<span data-ttu-id="770d8-151">Zadáním *max_results* parametr segmentu, můžou vyrovnávat mezi počet požadavků a využití paměti pro splnění důležité informace o výkonu pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="770d8-151">By specifying the *max_results* parameter of the segment, you can balance between the numbers of requests and memory usage to meet performance considerations for your application.</span></span>

<span data-ttu-id="770d8-152">Kromě toho pokud používáte segmentovaným výpis rozhraní API, ale ukládání dat do místního shromažďování ve stylu "chamtivého", také důrazně doporučujeme, aby zrefaktorujete kód pro zpracování, ukládání dat v kolekci místní pečlivě ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="770d8-152">Additionally, if you're using segmented listing APIs, but store the data in a local collection in a "greedy" style, we also strongly recommend that you refactor your code to handle storing data in a local collection carefully at scale.</span></span>

## <a name="lazy-listing"></a><span data-ttu-id="770d8-153">Opožděné výpis</span><span class="sxs-lookup"><span data-stu-id="770d8-153">Lazy listing</span></span>
<span data-ttu-id="770d8-154">I když typu greedy výpis vyvolá potenciální problémy, je vhodné, pokud v kontejneru nejsou k dispozici příliš mnoho objektů.</span><span class="sxs-lookup"><span data-stu-id="770d8-154">Although greedy listing raised potential issues, it is convenient if there are not too many objects in the container.</span></span>

<span data-ttu-id="770d8-155">Pokud používáte také jazyka C# nebo Java SDK Oracle, měli byste se seznámit s vyčíslitelná programovací model, který nabízí opožděné stylu výpis, kde na určité posunu pouze načítání v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="770d8-155">If you're also using C# or Oracle Java SDKs, you should be familiar with the Enumerable programming model, which offers a lazy-style listing, where the data at a certain offset is only fetched if it is required.</span></span> <span data-ttu-id="770d8-156">V jazyce C++ šablonu na základě iterator také poskytuje podobné přístup.</span><span class="sxs-lookup"><span data-stu-id="770d8-156">In C++, the iterator-based template also provides a similar approach.</span></span>

<span data-ttu-id="770d8-157">V typické opožděné seznamu rozhraní API, pomocí **list_blobs** jako příklad vypadá podobně jako tento:</span><span class="sxs-lookup"><span data-stu-id="770d8-157">A typical lazy listing API, using **list_blobs** as an example, looks like this:</span></span>

```cpp
list_blob_item_iterator list_blobs() const;
```

<span data-ttu-id="770d8-158">Fragment kódu typické, která používá vzor opožděné výpis může vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="770d8-158">A typical code snippet that uses the lazy listing pattern might look like this:</span></span>

```cpp
// List blobs in the blob container
azure::storage::list_blob_item_iterator end_of_results;
for (auto it = container.list_blobs(); it != end_of_results; ++it)
{
    if (it->is_blob())
    {
        process_blob(it->as_blob());
    }
    else
    {
        process_directory(it->as_directory());
    }
}
```

<span data-ttu-id="770d8-159">Všimněte si, že opožděné výpis je k dispozici pouze v synchronním režimu.</span><span class="sxs-lookup"><span data-stu-id="770d8-159">Note that lazy listing is only available in synchronous mode.</span></span>

<span data-ttu-id="770d8-160">Porovnání s typu greedy výpis, opožděné výpis načte data jenom v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="770d8-160">Compared with greedy listing, lazy listing fetches data only when necessary.</span></span> <span data-ttu-id="770d8-161">V pozadí ho načte data ze služby Azure Storage jenom v případě, že další iterator posouvá do další segment.</span><span class="sxs-lookup"><span data-stu-id="770d8-161">Under the covers, it fetches data from Azure Storage only when the next iterator moves into next segment.</span></span> <span data-ttu-id="770d8-162">Proto s ohraničenou velikost je řízena využití paměti a tuto operaci je rychlé.</span><span class="sxs-lookup"><span data-stu-id="770d8-162">Therefore, memory usage is controlled with a bounded size, and the operation is fast.</span></span>

<span data-ttu-id="770d8-163">Opožděné výpis rozhraní API jsou součástí Klientská knihovna pro úložiště pro jazyk C++ ve verzi 2.2.0.</span><span class="sxs-lookup"><span data-stu-id="770d8-163">Lazy listing APIs are included in the Storage Client Library for C++ in version 2.2.0.</span></span>

## <a name="conclusion"></a><span data-ttu-id="770d8-164">Závěr</span><span class="sxs-lookup"><span data-stu-id="770d8-164">Conclusion</span></span>
<span data-ttu-id="770d8-165">V tomto článku jsme probrali různých přetížení pro výpis rozhraní API pro různé objekty v Klientská knihovna pro úložiště pro jazyk C++.</span><span class="sxs-lookup"><span data-stu-id="770d8-165">In this article, we discussed different overloads for listing APIs for various objects in the Storage Client Library for C++ .</span></span> <span data-ttu-id="770d8-166">Shrnutí:</span><span class="sxs-lookup"><span data-stu-id="770d8-166">To summarize:</span></span>

* <span data-ttu-id="770d8-167">Rozhraní API pro asynchronní důrazně doporučujeme v části scénářích s více vláken.</span><span class="sxs-lookup"><span data-stu-id="770d8-167">Async APIs are strongly recommended under multiple threading scenarios.</span></span>
* <span data-ttu-id="770d8-168">Segmentovaným výpis se doporučuje pro většinu scénářů.</span><span class="sxs-lookup"><span data-stu-id="770d8-168">Segmented listing is recommended for most scenarios.</span></span>
* <span data-ttu-id="770d8-169">Opožděné výpis je k dispozici v knihovně jako vhodnou obálku v synchronní scénáře.</span><span class="sxs-lookup"><span data-stu-id="770d8-169">Lazy listing is provided in the library as a convenient wrapper in synchronous scenarios.</span></span>
* <span data-ttu-id="770d8-170">Výpis typu greedy se nedoporučuje a byla odebrána z knihovny.</span><span class="sxs-lookup"><span data-stu-id="770d8-170">Greedy listing is not recommended and has been removed from the library.</span></span>

## <a name="next-steps"></a><span data-ttu-id="770d8-171">Další kroky</span><span class="sxs-lookup"><span data-stu-id="770d8-171">Next steps</span></span>
<span data-ttu-id="770d8-172">Další informace o Azure Storage a klientské knihovny jazyka C++ najdete v následujících materiálech.</span><span class="sxs-lookup"><span data-stu-id="770d8-172">For more information about Azure Storage and Client Library for C++, see the following resources.</span></span>

* [<span data-ttu-id="770d8-173">Používání úložiště Blob z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="770d8-173">How to use Blob Storage from C++</span></span>](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [<span data-ttu-id="770d8-174">Postup používání úložiště Table z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="770d8-174">How to use Table Storage from C++</span></span>](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [<span data-ttu-id="770d8-175">Postup používání úložiště Queue z jazyka C++</span><span class="sxs-lookup"><span data-stu-id="770d8-175">How to use Queue Storage from C++</span></span>](../storage-c-plus-plus-how-to-use-queues.md)
* [<span data-ttu-id="770d8-176">Azure Klientská knihovna pro úložiště pro dokumentaci k rozhraní API jazyka C++.</span><span class="sxs-lookup"><span data-stu-id="770d8-176">Azure Storage Client Library for C++ API documentation.</span></span>](http://azure.github.io/azure-storage-cpp/)
* [<span data-ttu-id="770d8-177">Blog týmu Azure Storage</span><span class="sxs-lookup"><span data-stu-id="770d8-177">Azure Storage Team Blog</span></span>](http://blogs.msdn.com/b/windowsazurestorage/)
* [<span data-ttu-id="770d8-178">Dokumentace k Azure Storage</span><span class="sxs-lookup"><span data-stu-id="770d8-178">Azure Storage Documentation</span></span>](https://azure.microsoft.com/documentation/services/storage/)

