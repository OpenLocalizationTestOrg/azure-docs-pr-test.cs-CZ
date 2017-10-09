---
title: "prostředky Azure Storage aaaList s hello Klientská knihovna pro úložiště pro jazyk C++ | Microsoft Docs"
description: "Zjistěte, jak toouse hello výpis rozhraní API v Microsoft Azure Storage Client Library C++ tooenumerate kontejnerů, objektů BLOB, fronty, tabulky a entity."
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
ms.openlocfilehash: a76a5ce3cd690f32914f8f0c1f64273f13c5063e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="list-azure-storage-resources-in-c"></a>Seznam prostředků úložiště Azure v jazyce C++
Výpis operace jsou klíče toomany vývojové scénáře s Azure Storage. Tento článek popisuje, jak efektivně toomost výčet objektů ve službě Azure Storage pomocí hello výpis součástí hello Klientská knihovna pro úložiště Microsoft Azure pro jazyk C++ rozhraní API.

> [!NOTE]
> Tato příručka cílem hello Klientská knihovna pro úložiště Azure pro C++ verze 2.x, která je k dispozici prostřednictvím [NuGet](http://www.nuget.org/packages/wastorage) nebo [Githubu](https://github.com/Azure/azure-storage-cpp).
> 
> 

Hello Klientská knihovna pro úložiště poskytuje různé metody toolist nebo objekty dotazu ve službě Azure Storage. Tento článek řeší hello následující scénáře:

* Seznam kontejnery v účtu
* Seznam objektů BLOB v kontejneru nebo objekt blob virtuální adresář
* Seznam front v účtu
* Seznam tabulek v účtu
* Dotaz entity v tabulce

Každá z těchto metod se zobrazí při použití různých přetížení pro různé scénáře.

## <a name="asynchronous-versus-synchronous"></a>Asynchronní a synchronní
Protože hello Klientská knihovna pro úložiště pro jazyk C++ je postavená na hello [knihovny C++ REST](https://github.com/Microsoft/cpprestsdk), podporujeme ze své podstaty asynchronních operací s použitím [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Například:

```cpp
pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;
```

Synchronní operace zabalení hello odpovídající asynchronních operací:

```cpp
list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
{
    return list_blobs_segmented_async(token).get();
}
```

Pokud pracujete s více vláken aplikace nebo služby, doporučujeme použít asynchronní hello rozhraní API přímo místo vytvoření vlákna toocall hello synchronizace rozhraní API, což významně ovlivňuje výkon.

## <a name="segmented-listing"></a>Segmentované výpis
škálování Hello cloudového úložiště vyžaduje segmentovaným výpis. Například můžete mít přes milion objekty BLOB v kontejneru objektů blob v Azure nebo přes miliardu entity v Azure Table. Tyto nejsou teoretické čísla, ale případy využití skutečné oddělení.

Proto je nepraktické toolist všechny objekty v jedné odpovědi. Místo toho můžete vytvořit seznam objektů s použitím stránkování. Každý výpis rozhraní API hello má *segmentované* přetížení.

Hello odpověď operace segmentovaným výpis obsahuje:

* <i>_segment</i>, který obsahuje hello sadu výsledků vrácených pro jednoho volání toohello, výpis rozhraní API.
* *continuation_token*, který je předat další volání toohello v pořadí tooget hello další stránky výsledků. Pokud nejsou žádné další výsledky tooreturn, token pokračování hello má hodnotu null.

Například typický volání toolist všech objektů BLOB v kontejneru vypadat jako hello následující fragment kódu. Hello kód je k dispozici v našem [ukázky](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

```cpp
// List blobs in hello blob container
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

Všimněte si, že hello počet výsledků vrácených na stránce je řízena hello parametr *max_results* v hello přetížení každé rozhraní API, například:

```cpp
list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
    blob_listing_details::values includes, int max_results, const continuation_token& token,
    const blob_request_options& options, operation_context context)
```

Pokud nezadáte hello *max_results* parametr, hello výchozí maximální hodnota, která si výsledky too5000 je vrácený v jediné stránce.

Všimněte si, že dotaz Azure Table storage může vrátit žádné záznamy nebo méně záznamy než hodnota hello hello *max_results* parametr, který jste zadali, i když hello pokračovací token není prázdný. Jedním z důvodů může být, že tento dotaz hello nelze dokončit v pět sekund. Také hello pokračovací token není prázdný, hello dotazu by měly pokračovat a váš kód by neměl předpokládají hello velikost segmentu výsledky.

Hello doporučujeme kódování vzor pro většinu scénářů je segmentované výpis, který poskytuje explicitní průběh výpis nebo dotazování, a jak služba hello odpovídá tooeach požadavku. Platí to hlavně o služby nebo aplikace C++ mohou pomoci nižší úrovně kontrolu nad hello výpis průběh řízení paměti a výkon.

## <a name="greedy-listing"></a>Výpis typu greedy
Dřívějších verzích hello Klientská knihovna pro úložiště pro jazyk C++ (verze 0.5.0 Preview a starší) zahrnuté nesegmentovaný výpis rozhraní API pro tabulky a fronty, stejně jako hello následující ukázka:

```cpp
std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
std::vector<table_entity> execute_query(const table_query& query) const;
std::vector<cloud_queue> list_queues() const;
```

Tyto metody byly implementovány jako obálky segmentovaným rozhraní API. Pro každou odpověď segmentovaným výpis hello kód připojí vektoru tooa hello výsledky a vrátí všechny výsledky po skenování úplné kontejnery hello.

Tento přístup může pracovat v případě, že účet úložiště hello nebo tabulka obsahuje malý počet objektů. Ale zvýšení hello počet objektů, požadované hello paměti může zvýšit bez omezení, protože všechny výsledky zůstává v paměti. Jedné operace výpisu může trvat velmi dlouhou dobu, během které hello měl volající žádné informace o jejím průběhu.

Tyto typu greedy výpis rozhraní API v hello SDK neexistuje v jazyce C#, Java, nebo hello prostředí JavaScript Node.js. hello potenciálních problémů tooavoid použití těchto typu greedy rozhraní API, jsme odebrali je ve verzi 0.6.0 Preview.

Pokud váš kód volá tyto typu greedy rozhraní API:

```cpp
std::vector<azure::storage::table_entity> entities = table.execute_query(query);
for (auto it = entities.cbegin(); it != entities.cend(); ++it)
{
    process_entity(*it);
}
```

Potom upravte kódu toouse hello segmentované výpis rozhraní API:

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

Zadáním hello *max_results* parametr hello segmentu, můžou vyrovnávat mezi hello počet požadavků a požadavky na paměť toomeet výkonu využití pro vaši aplikaci.

Kromě toho pokud jste pomocí segmentovaného výpis rozhraní API, ale hello data ukládat v kolekci místní ve stylu "chamtivého", také důrazně doporučujeme, aby zrefaktorujete váš kód toohandle ukládání dat v kolekci místní pečlivě ve velkém měřítku.

## <a name="lazy-listing"></a>Opožděné výpis
I když typu greedy výpis vyvolá potenciální problémy, je vhodné, pokud v kontejneru hello nejsou k dispozici příliš mnoho objektů.

Pokud používáte také jazyka C# nebo Java SDK Oracle, měli byste se seznámit s hello vyčíslitelná programovací model, který nabízí opožděné stylu výpis, kde hello na určité posunu pouze načítání v případě potřeby. V jazyce C++ šablonu na základě iterator hello také poskytuje podobné přístup.

V typické opožděné seznamu rozhraní API, pomocí **list_blobs** jako příklad vypadá podobně jako tento:

```cpp
list_blob_item_iterator list_blobs() const;
```

Fragment kódu typické, která používá vzor opožděné výpis hello může vypadat například takto:

```cpp
// List blobs in hello blob container
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

Všimněte si, že opožděné výpis je k dispozici pouze v synchronním režimu.

Porovnání s typu greedy výpis, opožděné výpis načte data jenom v případě potřeby. V části hello zahrnuje ho načte data ze služby Azure Storage jenom v případě, že další iterator hello posouvá do další segment. Proto je využití paměti je řízena s ohraničenou velikost a operace hello je rychlé.

Opožděné výpis rozhraní API jsou součástí hello Klientská knihovna pro úložiště pro jazyk C++ ve verzi 2.2.0.

## <a name="conclusion"></a>Závěr
V tomto článku jsme probrali různých přetížení pro výpis rozhraní API pro různé objekty v hello Klientská knihovna pro úložiště pro jazyk C++. toosummarize:

* Rozhraní API pro asynchronní důrazně doporučujeme v části scénářích s více vláken.
* Segmentovaným výpis se doporučuje pro většinu scénářů.
* Opožděné výpis je k dispozici v knihovně hello jako vhodnou obálku v synchronní scénáře.
* Výpis typu greedy se nedoporučuje a byla odebrána z knihovny hello.

## <a name="next-steps"></a>Další kroky
Další informace o Azure Storage a klientské knihovny pro jazyk C++ najdete v části hello následující prostředky.

* [Jak toouse úložiště objektů Blob z jazyka C++](../blobs/storage-c-plus-plus-how-to-use-blobs.md)
* [Jak toouse úložiště Table z jazyka C++](../../cosmos-db/table-storage-how-to-use-c-plus.md)
* [Jak toouse Queue Storage z jazyka C++](../storage-c-plus-plus-how-to-use-queues.md)
* [Azure Klientská knihovna pro úložiště pro dokumentaci k rozhraní API jazyka C++.](http://azure.github.io/azure-storage-cpp/)
* [Blog týmu Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
* [Dokumentace k Azure Storage](https://azure.microsoft.com/documentation/services/storage/)

