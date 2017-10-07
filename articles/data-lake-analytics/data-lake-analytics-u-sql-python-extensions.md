---
title: "aaaExtend U-SQL skriptů s Pythonem v Azure Data Lake Analytics | Microsoft Docs"
description: "Zjistěte, jak kód toorun Python na skriptů U-SQL"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: c1c74e5e-3e4a-41ab-9e3f-e9085da1d315
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/20/2017
ms.author: saveenr
ms.openlocfilehash: f051f56f67522d4f2b8e6e54fd21a5c95ce3ba92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="9c3d3-103">Kurz: Začínáme s jazykem U-SQL s Pythonem rozšíření</span><span class="sxs-lookup"><span data-stu-id="9c3d3-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="9c3d3-104">Rozšíření Python pro U-SQL umožňují vývojářům tooperform massively paralelní provádění kódu jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-104">Python Extensions for U-SQL enable developers tooperform massively parallel execution of Python code.</span></span> <span data-ttu-id="9c3d3-105">Hello následující příklad znázorňuje hello základní kroky:</span><span class="sxs-lookup"><span data-stu-id="9c3d3-105">hello following example illustrates hello basic steps:</span></span>

* <span data-ttu-id="9c3d3-106">Použití hello `REFERENCE ASSEMBLY` příkaz tooenable Python rozšíření pro hello skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="9c3d3-106">Use hello `REFERENCE ASSEMBLY` statement tooenable Python extensions for hello U-SQL Script</span></span>
* <span data-ttu-id="9c3d3-107">Pomocí hello `REDUCE` operace toopartition hello vstupní data pro klíč</span><span class="sxs-lookup"><span data-stu-id="9c3d3-107">Using hello `REDUCE` operation toopartition hello input data on a key</span></span>
* <span data-ttu-id="9c3d3-108">Hello Python rozšíření pro U-SQL zahrnují předdefinované reduktorem (`Extension.Python.Reducer`) spouští se kód Python v každé reduktorem toohello vrchol přiřazen</span><span class="sxs-lookup"><span data-stu-id="9c3d3-108">hello Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned toohello reducer</span></span>
* <span data-ttu-id="9c3d3-109">Hello skript U-SQL obsahuje kód hello vložených Python, který má funkci s názvem `usqlml_main` který přijímá pandas DataFrame jako vstup a vrátí pandas DataFrame jako výstup.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-109">hello U-SQL script contains hello embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

--

    REFERENCE ASSEMBLY [ExtPython];

    DECLARE @myScript = @"
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ";

    @t  = 
        SELECT * FROM 
           (VALUES
               ("D1","T1","A1","@foo Hello World @bar"),
               ("D2","T2","A2","@baz Hello World @beer")
           ) AS 
               D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer(pyScript:@myScript);

    OUTPUT @m
        too"/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="9c3d3-110">Jak Python integruje s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="9c3d3-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="9c3d3-111">Datové typy</span><span class="sxs-lookup"><span data-stu-id="9c3d3-111">Datatypes</span></span>

* <span data-ttu-id="9c3d3-112">Řetězec a číselné sloupce z U-SQL se převedou jako-mezi Pandas a jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="9c3d3-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="9c3d3-113">Hodnoty Null U-SQL jsou převedené tooand z Pandas `NA` hodnoty</span><span class="sxs-lookup"><span data-stu-id="9c3d3-113">U-SQL Nulls are converted tooand from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="9c3d3-114">Schémata</span><span class="sxs-lookup"><span data-stu-id="9c3d3-114">Schemas</span></span>

* <span data-ttu-id="9c3d3-115">Index vektorů v Pandas nejsou podporovány v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="9c3d3-116">Všechny snímky vstupní data v hello Python funkce mít vždy 64-bit číselné index od 0 do hello počet řádků minus 1.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-116">All input data frames in hello Python function always have a 64-bit numerical index from 0 through hello number of rows minus 1.</span></span> 
* <span data-ttu-id="9c3d3-117">Datové sady U-SQL nemůže mít duplicitní názvy sloupců</span><span class="sxs-lookup"><span data-stu-id="9c3d3-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="9c3d3-118">Názvy sloupců datových sad U-SQL, které nejsou typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="9c3d3-119">Verze jazyka Python</span><span class="sxs-lookup"><span data-stu-id="9c3d3-119">Python Versions</span></span>
<span data-ttu-id="9c3d3-120">Je podporován pouze Python 3.5.1 (zkompilovaném pro Windows).</span><span class="sxs-lookup"><span data-stu-id="9c3d3-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="9c3d3-121">Standardní moduly jazyka Python</span><span class="sxs-lookup"><span data-stu-id="9c3d3-121">Standard Python modules</span></span>
<span data-ttu-id="9c3d3-122">Všechny moduly standardní Python hello jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-122">All hello standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="9c3d3-123">Další moduly jazyka Python</span><span class="sxs-lookup"><span data-stu-id="9c3d3-123">Additional Python modules</span></span>
<span data-ttu-id="9c3d3-124">Kromě hello standardní knihovny jazyka Python, mají několik běžně používané python knihovny:</span><span class="sxs-lookup"><span data-stu-id="9c3d3-124">Besides hello standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="9c3d3-125">Zprávy o výjimkách</span><span class="sxs-lookup"><span data-stu-id="9c3d3-125">Exception Messages</span></span>
<span data-ttu-id="9c3d3-126">V současné době výjimku v kódu jazyka Python objeví jako obecný vrchol selhání.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="9c3d3-127">V budoucích hello hello úlohy U-SQL chybové zprávy se zobrazí zpráva o výjimce hello Python.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-127">In hello future, hello U-SQL Job error messages will display hello Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="9c3d3-128">Vstup a výstup omezení velikosti</span><span class="sxs-lookup"><span data-stu-id="9c3d3-128">Input and Output size limitations</span></span>
<span data-ttu-id="9c3d3-129">Všechny vrcholy má omezené množství paměti přidělené tooit.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-129">Every vertex has a limited amount of memory assigned tooit.</span></span> <span data-ttu-id="9c3d3-130">V současné době je tento limit pro AU 6 GB.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="9c3d3-131">Protože hello vstupní a výstupní DataFrames musí existovat v paměti hello kód Python, celková velikost hello hello vstupní a výstupní nesmí překročit 6 GB.</span><span class="sxs-lookup"><span data-stu-id="9c3d3-131">Because hello input and output DataFrames must exist in memory in hello Python code, hello total size for hello input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="9c3d3-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="9c3d3-132">See also</span></span>
* [<span data-ttu-id="9c3d3-133">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="9c3d3-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="9c3d3-134">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9c3d3-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="9c3d3-135">Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce</span><span class="sxs-lookup"><span data-stu-id="9c3d3-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

