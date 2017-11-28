---
title: "Použijte Hadoop Pig v HDInsight | Microsoft Docs"
description: "Další informace o použití Pig s Hadoop v HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 474f901ffdaf1ed372ace19076ef723b8b10cb9a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a><span data-ttu-id="f1573-103">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f1573-103">Use Pig with Hadoop on HDInsight</span></span>

<span data-ttu-id="f1573-104">Další informace o použití [Apache Pig](http://pig.apache.org/) s HDInsight...</span><span class="sxs-lookup"><span data-stu-id="f1573-104">Learn how to use [Apache Pig](http://pig.apache.org/) with HDInsight...</span></span>

<span data-ttu-id="f1573-105">Pig je platforma pro vytváření programů pro Hadoop pomocí procedurální jazyka známé jako *Pig Latin*.</span><span class="sxs-lookup"><span data-stu-id="f1573-105">Pig is a platform for creating programs for Hadoop by using a procedural language known as *Pig Latin*.</span></span> <span data-ttu-id="f1573-106">Pig je alternativa k Java k vytváření *MapReduce* řešení a je součástí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f1573-106">Pig is an alternative to Java for creating *MapReduce* solutions, and it is included with Azure HDInsight.</span></span> <span data-ttu-id="f1573-107">Následující tabulku použijte ke zjištění různých způsobech, kterými vepřových lze použít s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="f1573-107">Use the following table to discover the various ways that Pig can be used with HDInsight:</span></span>

| <span data-ttu-id="f1573-108">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="f1573-108">**Use this** if you want...</span></span> | <span data-ttu-id="f1573-109">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="f1573-109">...an **interactive** shell</span></span> | <span data-ttu-id="f1573-110">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="f1573-110">...**batch** processing</span></span> | <span data-ttu-id="f1573-111">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="f1573-111">...with this **cluster operating system**</span></span> | <span data-ttu-id="f1573-112">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="f1573-112">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="f1573-113">SSH</span><span class="sxs-lookup"><span data-stu-id="f1573-113">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="f1573-114">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-114">✔</span></span> |<span data-ttu-id="f1573-115">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-115">✔</span></span> |<span data-ttu-id="f1573-116">Linux</span><span class="sxs-lookup"><span data-stu-id="f1573-116">Linux</span></span> |<span data-ttu-id="f1573-117">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f1573-118">REST API</span><span class="sxs-lookup"><span data-stu-id="f1573-118">REST API</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="f1573-119">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-119">✔</span></span> |<span data-ttu-id="f1573-120">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-120">Linux or Windows</span></span> |<span data-ttu-id="f1573-121">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-121">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f1573-122">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="f1573-122">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="f1573-123">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-123">✔</span></span> |<span data-ttu-id="f1573-124">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-124">Linux or Windows</span></span> |<span data-ttu-id="f1573-125">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="f1573-125">Windows (for now)</span></span> |
| [<span data-ttu-id="f1573-126">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1573-126">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="f1573-127">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-127">✔</span></span> |<span data-ttu-id="f1573-128">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-128">Linux or Windows</span></span> |<span data-ttu-id="f1573-129">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-129">Windows</span></span> |
| <span data-ttu-id="f1573-130">[Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="f1573-130">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="f1573-131">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-131">✔</span></span> |<span data-ttu-id="f1573-132">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-132">✔</span></span> |<span data-ttu-id="f1573-133">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-133">Windows</span></span> |<span data-ttu-id="f1573-134">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-134">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f1573-135">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="f1573-135">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f1573-136">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f1573-136">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="f1573-137"><a id="why"></a>Proč používat Pig</span><span class="sxs-lookup"><span data-stu-id="f1573-137"><a id="why"></a>Why use Pig</span></span>

<span data-ttu-id="f1573-138">Jedním z problémů zpracování dat pomocí MapReduce v Hadoop je implementace logika zpracování pomocí pouze mapu a snižte funkce.</span><span class="sxs-lookup"><span data-stu-id="f1573-138">One of the challenges of processing data by using MapReduce in Hadoop is implementing your processing logic by using only a map and a reduce function.</span></span> <span data-ttu-id="f1573-139">Komplexní zpracování, je často nutné na přerušení zpracování do více operací MapReduce, které jsou zřetězené dohromady a dosáhnout požadovaného výsledku.</span><span class="sxs-lookup"><span data-stu-id="f1573-139">For complex processing, you often have to break processing into multiple MapReduce operations that are chained together to achieve the desired result.</span></span>

<span data-ttu-id="f1573-140">Pig umožňuje definovat zpracování jako řadu transformace, které se data prochází k vytvoření požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="f1573-140">Pig allows you to define processing as a series of transformations that the data flows through to produce the desired output.</span></span>

<span data-ttu-id="f1573-141">Jazyka Pig Latin můžete k popisu toku dat z nezpracovaná vstupu prostřednictvím jeden nebo více transformace, k vytvoření požadované výstup.</span><span class="sxs-lookup"><span data-stu-id="f1573-141">The Pig Latin language allows you to describe the data flow from raw input, through one or more transformations, to produce the desired output.</span></span> <span data-ttu-id="f1573-142">Pig Latin programy postupujte podle tohoto vzoru Obecné:</span><span class="sxs-lookup"><span data-stu-id="f1573-142">Pig Latin programs follow this general pattern:</span></span>

* <span data-ttu-id="f1573-143">**Zatížení**: čtení dat manipulaci s těmito ze systému souborů</span><span class="sxs-lookup"><span data-stu-id="f1573-143">**Load**: Read data to be manipulated from the file system</span></span>

* <span data-ttu-id="f1573-144">**Transformace**: manipulovat s daty</span><span class="sxs-lookup"><span data-stu-id="f1573-144">**Transform**: Manipulate the data</span></span>

* <span data-ttu-id="f1573-145">**Výpis stavu nebo ukládat**: výstupní data na obrazovku nebo ho uložit pro zpracování</span><span class="sxs-lookup"><span data-stu-id="f1573-145">**Dump or store**: Output data to the screen or store it for processing</span></span>

### <a name="user-defined-functions"></a><span data-ttu-id="f1573-146">Uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="f1573-146">User-defined functions</span></span>

<span data-ttu-id="f1573-147">Pig Latin také podporuje uživatelsky definované funkce (UDF), která umožňuje vyvolání externí součásti, které implementují logiku, která je obtížné model v Pig Latin.</span><span class="sxs-lookup"><span data-stu-id="f1573-147">Pig Latin also supports user-defined functions (UDF), which allows you to invoke external components that implement logic that is difficult to model in Pig Latin.</span></span>

<span data-ttu-id="f1573-148">Další informace o Pig Latin najdete v tématu [Pig Latin odkazu ruční 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) a [Pig Latin odkaz ruční 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span><span class="sxs-lookup"><span data-stu-id="f1573-148">For more information about Pig Latin, see [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) and [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span></span>

<span data-ttu-id="f1573-149">Příklad použití UDF s Pig najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="f1573-149">For an example of using UDFs with Pig, see the following documents:</span></span>

* <span data-ttu-id="f1573-150">[Pig v HDInsight pomocí DataFu](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu je kolekce užitečné UDF udržované Apache</span><span class="sxs-lookup"><span data-stu-id="f1573-150">[Use DataFu with Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is a collection of useful UDFs maintained by Apache</span></span>
* [<span data-ttu-id="f1573-151">Používat jazyk Python s Pig a Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f1573-151">Use Python with Pig and Hive in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="f1573-152">Použití jazyka C# s Hive a Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="f1573-152">Use C# with Hive and Pig in HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <span data-ttu-id="f1573-153"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="f1573-153"><a id="data"></a>Example data</span></span>

<span data-ttu-id="f1573-154">HDInsight poskytuje různé příklad datových sad, které jsou uloženy v `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="f1573-154">HDInsight provides various example data sets, which are stored in the `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="f1573-155">Tyto adresáře jsou ve výchozím nastavení úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="f1573-155">These directories are in the default storage for your cluster.</span></span> <span data-ttu-id="f1573-156">Pig příklad v tomto dokumentu používá *log4j* souboru z `/example/data/sample.log`.</span><span class="sxs-lookup"><span data-stu-id="f1573-156">The Pig example in this document uses the *log4j* file from `/example/data/sample.log`.</span></span>

<span data-ttu-id="f1573-157">Každý protokol uvnitř souboru se skládá z řádku pole, která obsahuje `[LOG LEVEL]` pole, které chcete zobrazit typ a závažnost, například:</span><span class="sxs-lookup"><span data-stu-id="f1573-157">Each log inside the file consists of a line of fields that contains a `[LOG LEVEL]` field to show the type and the severity, for example:</span></span>

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

<span data-ttu-id="f1573-158">V předchozím příkladu je úroveň protokolu chyba.</span><span class="sxs-lookup"><span data-stu-id="f1573-158">In the previous example, the log level is ERROR.</span></span>

> [!NOTE]
> <span data-ttu-id="f1573-159">Můžete také vygenerovat soubor log4j pomocí [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) protokolování nástroj a pak tento soubor nahrajte do objektu blob služby.</span><span class="sxs-lookup"><span data-stu-id="f1573-159">You can also generate a log4j file by using the [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logging tool and then upload that file to your blob.</span></span> <span data-ttu-id="f1573-160">V tématu [nahrát Data do HDInsight](hdinsight-upload-data.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="f1573-160">See [Upload Data to HDInsight](hdinsight-upload-data.md) for instructions.</span></span> <span data-ttu-id="f1573-161">Další informace o použití objektů BLOB v úložišti Azure s HDInsight naleznete v tématu [použití Azure Blob Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f1573-161">For more information about how blobs in Azure storage are used with HDInsight, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>

## <span data-ttu-id="f1573-162"><a id="job"></a>Příklad úloh</span><span class="sxs-lookup"><span data-stu-id="f1573-162"><a id="job"></a>Example job</span></span>

<span data-ttu-id="f1573-163">Načte následující úlohy Pig Latin `sample.log` soubor z výchozí úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f1573-163">The following Pig Latin job loads the `sample.log` file from the default storage for your HDInsight cluster.</span></span> <span data-ttu-id="f1573-164">Pak provede řady transformací, jejichž výsledkem počet kolikrát vstupních dat došlo k chybě každou úroveň protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1573-164">Then it performs a series of transformations that result in a count of how many times each log level occurred in the input data.</span></span> <span data-ttu-id="f1573-165">Výsledky se zapisují do STDOUT.</span><span class="sxs-lookup"><span data-stu-id="f1573-165">The results are written to STDOUT.</span></span>

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

<span data-ttu-id="f1573-166">Následující obrázek zobrazuje souhrn každý transformace nemá k datům.</span><span class="sxs-lookup"><span data-stu-id="f1573-166">The following image shows a summary of what each transformation does to the data.</span></span>

![Grafické znázornění transformací][image-hdi-pig-data-transformation]

## <span data-ttu-id="f1573-168"><a id="run"></a>Spustit úlohu Pig Latin</span><span class="sxs-lookup"><span data-stu-id="f1573-168"><a id="run"></a>Run the Pig Latin job</span></span>

<span data-ttu-id="f1573-169">HDInsight můžete spustit úlohy Pig Latin pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="f1573-169">HDInsight can run Pig Latin jobs by using a variety of methods.</span></span> <span data-ttu-id="f1573-170">Následující tabulku použijte k rozhodování, jakou metodu je pro vás nejvhodnější a potom klepněte na odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="f1573-170">Use the following table to decide which method is right for you, then follow the link for a walkthrough.</span></span>

| <span data-ttu-id="f1573-171">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="f1573-171">**Use this** if you want...</span></span> | <span data-ttu-id="f1573-172">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="f1573-172">...an **interactive** shell</span></span> | <span data-ttu-id="f1573-173">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="f1573-173">...**batch** processing</span></span> | <span data-ttu-id="f1573-174">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="f1573-174">...with this **cluster operating system**</span></span> | <span data-ttu-id="f1573-175">.. .from to **klienta**</span><span class="sxs-lookup"><span data-stu-id="f1573-175">...from this **client**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="f1573-176">SSH</span><span class="sxs-lookup"><span data-stu-id="f1573-176">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="f1573-177">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-177">✔</span></span> |<span data-ttu-id="f1573-178">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-178">✔</span></span> |<span data-ttu-id="f1573-179">Linux</span><span class="sxs-lookup"><span data-stu-id="f1573-179">Linux</span></span> |<span data-ttu-id="f1573-180">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f1573-181">Curl</span><span class="sxs-lookup"><span data-stu-id="f1573-181">Curl</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="f1573-182">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-182">✔</span></span> |<span data-ttu-id="f1573-183">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-183">Linux or Windows</span></span> |<span data-ttu-id="f1573-184">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-184">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="f1573-185">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="f1573-185">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="f1573-186">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-186">✔</span></span> |<span data-ttu-id="f1573-187">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-187">Linux or Windows</span></span> |<span data-ttu-id="f1573-188">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="f1573-188">Windows (for now)</span></span> |
| [<span data-ttu-id="f1573-189">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1573-189">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="f1573-190">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-190">✔</span></span> |<span data-ttu-id="f1573-191">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-191">Linux or Windows</span></span> |<span data-ttu-id="f1573-192">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-192">Windows</span></span> |
| <span data-ttu-id="f1573-193">[Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="f1573-193">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="f1573-194">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-194">✔</span></span> |<span data-ttu-id="f1573-195">✔</span><span class="sxs-lookup"><span data-stu-id="f1573-195">✔</span></span> |<span data-ttu-id="f1573-196">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-196">Windows</span></span> |<span data-ttu-id="f1573-197">Windows</span><span class="sxs-lookup"><span data-stu-id="f1573-197">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="f1573-198">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="f1573-198">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f1573-199">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f1573-199">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="pig-and-sql-server-integration-services"></a><span data-ttu-id="f1573-200">Pig a SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="f1573-200">Pig and SQL Server Integration Services</span></span>

<span data-ttu-id="f1573-201">Integrace služby SSIS (SQL Server) můžete použít ke spuštění úlohy Pig.</span><span class="sxs-lookup"><span data-stu-id="f1573-201">You can use SQL Server Integration Services (SSIS) to run a Pig job.</span></span> <span data-ttu-id="f1573-202">Azure Feature Pack pro službu SSIS poskytuje následující součásti, které práce s úlohami Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f1573-202">The Azure Feature Pack for SSIS provides the following components that work with Pig jobs on HDInsight.</span></span>

* <span data-ttu-id="f1573-203">[Úloha Azure HDInsight Pig][pigtask]</span><span class="sxs-lookup"><span data-stu-id="f1573-203">[Azure HDInsight Pig Task][pigtask]</span></span>

* <span data-ttu-id="f1573-204">[Správce připojení k Azure předplatného][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="f1573-204">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="f1573-205">Další informace o Azure Feature Pack pro služby SSIS [sem][ssispack].</span><span class="sxs-lookup"><span data-stu-id="f1573-205">Learn more about the Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="f1573-206"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1573-206"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="f1573-207">Teď, když jste se naučili používání Pig s HDInsight, pomocí následujících odkazů a prozkoumejte další způsoby, jak pracovat s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f1573-207">Now that you have learned how to use Pig with HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* <span data-ttu-id="f1573-208">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="f1573-208">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="f1573-209">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f1573-209">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* [<span data-ttu-id="f1573-210">Použití nástroje Sqoop s HDInsight</span><span class="sxs-lookup"><span data-stu-id="f1573-210">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop.md)
* [<span data-ttu-id="f1573-211">Použijte Oozie s HDInsight</span><span class="sxs-lookup"><span data-stu-id="f1573-211">Use Oozie with HDInsight</span></span>](hdinsight-use-oozie.md)
* <span data-ttu-id="f1573-212">[Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="f1573-212">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
