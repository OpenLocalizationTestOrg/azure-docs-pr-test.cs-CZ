---
title: "Rozšíření skriptů U-SQL s Pythonem v Azure Data Lake Analytics | Microsoft Docs"
description: "Postup spuštění kódu jazyka Python v skriptů U-SQL"
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
ms.openlocfilehash: d18ef1f747aee2fa01cef9891432d0461031ee4c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-get-started-with-extending-u-sql-with-python"></a><span data-ttu-id="f3daf-103">Kurz: Začínáme s jazykem U-SQL s Pythonem rozšíření</span><span class="sxs-lookup"><span data-stu-id="f3daf-103">Tutorial: Get started with extending U-SQL with Python</span></span>

<span data-ttu-id="f3daf-104">Rozšíření Python pro U-SQL umožňují vývojářům provádět masivně paralelní provádění kódu jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="f3daf-104">Python Extensions for U-SQL enable developers to perform massively parallel execution of Python code.</span></span> <span data-ttu-id="f3daf-105">Následující příklad ukazuje základní kroky:</span><span class="sxs-lookup"><span data-stu-id="f3daf-105">The following example illustrates the basic steps:</span></span>

* <span data-ttu-id="f3daf-106">Použití `REFERENCE ASSEMBLY` příkaz povolení rozšíření Python pro skript U-SQL</span><span class="sxs-lookup"><span data-stu-id="f3daf-106">Use the `REFERENCE ASSEMBLY` statement to enable Python extensions for the U-SQL Script</span></span>
* <span data-ttu-id="f3daf-107">Pomocí `REDUCE` operace vstupní data pro klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="f3daf-107">Using the `REDUCE` operation to partition the input data on a key</span></span>
* <span data-ttu-id="f3daf-108">Rozšíření Python pro U-SQL zahrnují předdefinované reduktorem (`Extension.Python.Reducer`) spouští se v jednotlivých vrchol přiřazené reduktorem kód Python</span><span class="sxs-lookup"><span data-stu-id="f3daf-108">The Python extensions for U-SQL include a built-in reducer (`Extension.Python.Reducer`) that runs Python code on each vertex assigned to the reducer</span></span>
* <span data-ttu-id="f3daf-109">Skript U-SQL obsahuje vestavěný kód Python, který má funkci s názvem `usqlml_main` který přijímá pandas DataFrame jako vstup a vrátí pandas DataFrame jako výstup.</span><span class="sxs-lookup"><span data-stu-id="f3daf-109">The U-SQL script contains the embedded Python code that has a function called `usqlml_main` that accepts a pandas DataFrame as input and returns a pandas DataFrame as output.</span></span>

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
        TO "/tweetmentions.csv"
        USING Outputters.Csv();

## <a name="how-python-integrates-with-u-sql"></a><span data-ttu-id="f3daf-110">Jak Python integruje s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="f3daf-110">How Python Integrates with U-SQL</span></span>

### <a name="datatypes"></a><span data-ttu-id="f3daf-111">Datové typy</span><span class="sxs-lookup"><span data-stu-id="f3daf-111">Datatypes</span></span>

* <span data-ttu-id="f3daf-112">Řetězec a číselné sloupce z U-SQL se převedou jako-mezi Pandas a jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="f3daf-112">String and numeric columns from U-SQL are converted as-is between Pandas and U-SQL</span></span>
* <span data-ttu-id="f3daf-113">Hodnoty Null U-SQL se převedou do a z Pandas `NA` hodnoty</span><span class="sxs-lookup"><span data-stu-id="f3daf-113">U-SQL Nulls are converted to and from Pandas `NA` values</span></span>

### <a name="schemas"></a><span data-ttu-id="f3daf-114">Schémata</span><span class="sxs-lookup"><span data-stu-id="f3daf-114">Schemas</span></span>

* <span data-ttu-id="f3daf-115">Index vektorů v Pandas nejsou podporovány v U-SQL.</span><span class="sxs-lookup"><span data-stu-id="f3daf-115">Index vectors in Pandas are not supported in U-SQL.</span></span> <span data-ttu-id="f3daf-116">Všechny snímky vstupní data ve funkci Python mít vždy 64-bit číselné index od 0 do počet řádků minus 1.</span><span class="sxs-lookup"><span data-stu-id="f3daf-116">All input data frames in the Python function always have a 64-bit numerical index from 0 through the number of rows minus 1.</span></span> 
* <span data-ttu-id="f3daf-117">Datové sady U-SQL nemůže mít duplicitní názvy sloupců</span><span class="sxs-lookup"><span data-stu-id="f3daf-117">U-SQL datasets cannot have duplicate column names</span></span>
* <span data-ttu-id="f3daf-118">Názvy sloupců datových sad U-SQL, které nejsou typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="f3daf-118">U-SQL datasets column names that are not strings.</span></span> 

### <a name="python-versions"></a><span data-ttu-id="f3daf-119">Verze jazyka Python</span><span class="sxs-lookup"><span data-stu-id="f3daf-119">Python Versions</span></span>
<span data-ttu-id="f3daf-120">Je podporován pouze Python 3.5.1 (zkompilovaném pro Windows).</span><span class="sxs-lookup"><span data-stu-id="f3daf-120">Only Python 3.5.1 (compiled for Windows) is supported.</span></span> 

### <a name="standard-python-modules"></a><span data-ttu-id="f3daf-121">Standardní moduly jazyka Python</span><span class="sxs-lookup"><span data-stu-id="f3daf-121">Standard Python modules</span></span>
<span data-ttu-id="f3daf-122">Všechny standardní moduly Python jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="f3daf-122">All the standard Python modules are included.</span></span>

### <a name="additional-python-modules"></a><span data-ttu-id="f3daf-123">Další moduly jazyka Python</span><span class="sxs-lookup"><span data-stu-id="f3daf-123">Additional Python modules</span></span>
<span data-ttu-id="f3daf-124">Kromě standardní knihovny jazyka Python mají několik běžně používané python knihovny:</span><span class="sxs-lookup"><span data-stu-id="f3daf-124">Besides the standard Python libraries, several commonly used python libraries are included:</span></span>

    pandas
    numpy
    numexpr

### <a name="exception-messages"></a><span data-ttu-id="f3daf-125">Zprávy o výjimkách</span><span class="sxs-lookup"><span data-stu-id="f3daf-125">Exception Messages</span></span>
<span data-ttu-id="f3daf-126">V současné době výjimku v kódu jazyka Python objeví jako obecný vrchol selhání.</span><span class="sxs-lookup"><span data-stu-id="f3daf-126">Currently, an exception in Python code shows up as generic vertex failure.</span></span> <span data-ttu-id="f3daf-127">Chybové zprávy, které úlohy U-SQL v budoucnu, se zobrazí zpráva o výjimce Python.</span><span class="sxs-lookup"><span data-stu-id="f3daf-127">In the future, the U-SQL Job error messages will display the Python exception message.</span></span>

### <a name="input-and-output-size-limitations"></a><span data-ttu-id="f3daf-128">Vstup a výstup omezení velikosti</span><span class="sxs-lookup"><span data-stu-id="f3daf-128">Input and Output size limitations</span></span>
<span data-ttu-id="f3daf-129">Všechny vrcholy má omezené množství paměti přidělené k němu.</span><span class="sxs-lookup"><span data-stu-id="f3daf-129">Every vertex has a limited amount of memory assigned to it.</span></span> <span data-ttu-id="f3daf-130">V současné době je tento limit pro AU 6 GB.</span><span class="sxs-lookup"><span data-stu-id="f3daf-130">Currently, that limit is 6 GB for an AU.</span></span> <span data-ttu-id="f3daf-131">Vzhledem k tomu, že vstupní a výstupní DataFrames musí existovat v paměti v kódu jazyka Python, celková velikost vstupní a výstupní nesmí překročit 6 GB.</span><span class="sxs-lookup"><span data-stu-id="f3daf-131">Because the input and output DataFrames must exist in memory in the Python code, the total size for the input and output cannot exceed 6 GB.</span></span>

## <a name="see-also"></a><span data-ttu-id="f3daf-132">Viz také</span><span class="sxs-lookup"><span data-stu-id="f3daf-132">See also</span></span>
* [<span data-ttu-id="f3daf-133">Přehled služby Microsoft Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="f3daf-133">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="f3daf-134">Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3daf-134">Develop U-SQL scripts using Data Lake Tools for Visual Studio</span></span>](data-lake-analytics-data-lake-tools-get-started.md)
* [<span data-ttu-id="f3daf-135">Pro úlohy Azure Data Lake Analytics pomocí U-SQL okno funkce</span><span class="sxs-lookup"><span data-stu-id="f3daf-135">Using U-SQL window functions for Azure Data Lake Analytics jobs</span></span>](data-lake-analytics-use-window-functions.md)

