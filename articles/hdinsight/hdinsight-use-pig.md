---
title: aaaUse Hadoop Pig v HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse vepřových s Hadoop v HDInsight."
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
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a><span data-ttu-id="c8f3f-103">Použijte Pig s Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c8f3f-103">Use Pig with Hadoop on HDInsight</span></span>

<span data-ttu-id="c8f3f-104">Zjistěte, jak toouse [Apache Pig](http://pig.apache.org/) s HDInsight...</span><span class="sxs-lookup"><span data-stu-id="c8f3f-104">Learn how toouse [Apache Pig](http://pig.apache.org/) with HDInsight...</span></span>

<span data-ttu-id="c8f3f-105">Pig je platforma pro vytváření programů pro Hadoop pomocí procedurální jazyka známé jako *Pig Latin*.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-105">Pig is a platform for creating programs for Hadoop by using a procedural language known as *Pig Latin*.</span></span> <span data-ttu-id="c8f3f-106">Pig je alternativní tooJava pro vytvoření *MapReduce* řešení a je součástí Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-106">Pig is an alternative tooJava for creating *MapReduce* solutions, and it is included with Azure HDInsight.</span></span> <span data-ttu-id="c8f3f-107">Použijte následující tabulku toodiscover hello různé způsoby, že lze použít Pig s HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="c8f3f-107">Use hello following table toodiscover hello various ways that Pig can be used with HDInsight:</span></span>

| <span data-ttu-id="c8f3f-108">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="c8f3f-108">**Use this** if you want...</span></span> | <span data-ttu-id="c8f3f-109">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="c8f3f-109">...an **interactive** shell</span></span> | <span data-ttu-id="c8f3f-110">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="c8f3f-110">...**batch** processing</span></span> | <span data-ttu-id="c8f3f-111">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="c8f3f-111">...with this **cluster operating system**</span></span> | <span data-ttu-id="c8f3f-112">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="c8f3f-112">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="c8f3f-113">SSH</span><span class="sxs-lookup"><span data-stu-id="c8f3f-113">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="c8f3f-114">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-114">✔</span></span> |<span data-ttu-id="c8f3f-115">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-115">✔</span></span> |<span data-ttu-id="c8f3f-116">Linux</span><span class="sxs-lookup"><span data-stu-id="c8f3f-116">Linux</span></span> |<span data-ttu-id="c8f3f-117">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c8f3f-118">REST API</span><span class="sxs-lookup"><span data-stu-id="c8f3f-118">REST API</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="c8f3f-119">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-119">✔</span></span> |<span data-ttu-id="c8f3f-120">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-120">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-121">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-121">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c8f3f-122">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="c8f3f-122">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="c8f3f-123">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-123">✔</span></span> |<span data-ttu-id="c8f3f-124">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-124">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-125">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="c8f3f-125">Windows (for now)</span></span> |
| [<span data-ttu-id="c8f3f-126">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8f3f-126">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="c8f3f-127">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-127">✔</span></span> |<span data-ttu-id="c8f3f-128">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-128">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-129">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-129">Windows</span></span> |
| <span data-ttu-id="c8f3f-130">[Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="c8f3f-130">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="c8f3f-131">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-131">✔</span></span> |<span data-ttu-id="c8f3f-132">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-132">✔</span></span> |<span data-ttu-id="c8f3f-133">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-133">Windows</span></span> |<span data-ttu-id="c8f3f-134">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-134">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c8f3f-135">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-135">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c8f3f-136">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c8f3f-136">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="c8f3f-137"><a id="why"></a>Proč používat Pig</span><span class="sxs-lookup"><span data-stu-id="c8f3f-137"><a id="why"></a>Why use Pig</span></span>

<span data-ttu-id="c8f3f-138">Jeden z hello výzev zpracování dat pomocí MapReduce v Hadoop je implementace logika zpracování pomocí pouze mapu a snižte funkce.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-138">One of hello challenges of processing data by using MapReduce in Hadoop is implementing your processing logic by using only a map and a reduce function.</span></span> <span data-ttu-id="c8f3f-139">Pro komplexní zpracování, je často mají toobreak zpracování do více operací MapReduce, které jsou zřetězené společně tooachieve hello požadovaného výsledku.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-139">For complex processing, you often have toobreak processing into multiple MapReduce operations that are chained together tooachieve hello desired result.</span></span>

<span data-ttu-id="c8f3f-140">Pig vám umožní toodefine zpracování jako řadu transformace, které hello dat prochází tooproduce hello potřeby výstup.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-140">Pig allows you toodefine processing as a series of transformations that hello data flows through tooproduce hello desired output.</span></span>

<span data-ttu-id="c8f3f-141">jazyka Pig Latin Hello umožňuje vám toodescribe hello tok dat z nezpracovaná vstupu prostřednictvím jeden nebo více transformace, výstup hello potřeby tooproduce.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-141">hello Pig Latin language allows you toodescribe hello data flow from raw input, through one or more transformations, tooproduce hello desired output.</span></span> <span data-ttu-id="c8f3f-142">Pig Latin programy postupujte podle tohoto vzoru Obecné:</span><span class="sxs-lookup"><span data-stu-id="c8f3f-142">Pig Latin programs follow this general pattern:</span></span>

* <span data-ttu-id="c8f3f-143">**Zatížení**: čtení dat toobe s nimi manipulovat, hello systému souborů</span><span class="sxs-lookup"><span data-stu-id="c8f3f-143">**Load**: Read data toobe manipulated from hello file system</span></span>

* <span data-ttu-id="c8f3f-144">**Transformace**: pracovat s daty hello</span><span class="sxs-lookup"><span data-stu-id="c8f3f-144">**Transform**: Manipulate hello data</span></span>

* <span data-ttu-id="c8f3f-145">**Výpis stavu nebo ukládat**: výstupní data toohello obrazovky nebo uloží jej pro zpracování</span><span class="sxs-lookup"><span data-stu-id="c8f3f-145">**Dump or store**: Output data toohello screen or store it for processing</span></span>

### <a name="user-defined-functions"></a><span data-ttu-id="c8f3f-146">Uživatelem definované funkce</span><span class="sxs-lookup"><span data-stu-id="c8f3f-146">User-defined functions</span></span>

<span data-ttu-id="c8f3f-147">Pig Latin také podporuje uživatelsky definované funkce (UDF), což vám umožní tooinvoke externí součásti, které implementují logiku, která je obtížné toomodel v Pig Latin.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-147">Pig Latin also supports user-defined functions (UDF), which allows you tooinvoke external components that implement logic that is difficult toomodel in Pig Latin.</span></span>

<span data-ttu-id="c8f3f-148">Další informace o Pig Latin najdete v tématu [Pig Latin odkazu ruční 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) a [Pig Latin odkaz ruční 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span><span class="sxs-lookup"><span data-stu-id="c8f3f-148">For more information about Pig Latin, see [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) and [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).</span></span>

<span data-ttu-id="c8f3f-149">Příklad použití UDF s Pig naleznete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="c8f3f-149">For an example of using UDFs with Pig, see hello following documents:</span></span>

* <span data-ttu-id="c8f3f-150">[Pig v HDInsight pomocí DataFu](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu je kolekce užitečné UDF udržované Apache</span><span class="sxs-lookup"><span data-stu-id="c8f3f-150">[Use DataFu with Pig in HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) - DataFu is a collection of useful UDFs maintained by Apache</span></span>
* [<span data-ttu-id="c8f3f-151">Používat jazyk Python s Pig a Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c8f3f-151">Use Python with Pig and Hive in HDInsight</span></span>](hdinsight-python.md)
* [<span data-ttu-id="c8f3f-152">Použití jazyka C# s Hive a Pig v HDInsight</span><span class="sxs-lookup"><span data-stu-id="c8f3f-152">Use C# with Hive and Pig in HDInsight</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <span data-ttu-id="c8f3f-153"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="c8f3f-153"><a id="data"></a>Example data</span></span>

<span data-ttu-id="c8f3f-154">HDInsight poskytuje různé příklad datových sad, které jsou uloženy v hello `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-154">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="c8f3f-155">Tyto adresáře jsou hello výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-155">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="c8f3f-156">Příklad Pig Hello v tomto dokumentu používá hello *log4j* souboru z `/example/data/sample.log`.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-156">hello Pig example in this document uses hello *log4j* file from `/example/data/sample.log`.</span></span>

<span data-ttu-id="c8f3f-157">V souboru hello každý protokol se skládá z řádku pole, která obsahuje `[LOG LEVEL]` pole typu a hello závažnost hello tooshow například:</span><span class="sxs-lookup"><span data-stu-id="c8f3f-157">Each log inside hello file consists of a line of fields that contains a `[LOG LEVEL]` field tooshow hello type and hello severity, for example:</span></span>

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

<span data-ttu-id="c8f3f-158">V předchozím příkladu hello je hello úroveň protokolu chyba.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-158">In hello previous example, hello log level is ERROR.</span></span>

> [!NOTE]
> <span data-ttu-id="c8f3f-159">Můžete také vygenerovat soubor log4j pomocí hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) protokolování nástroj a pak nahrajte tento soubor tooyour objekt blob.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-159">You can also generate a log4j file by using hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) logging tool and then upload that file tooyour blob.</span></span> <span data-ttu-id="c8f3f-160">V tématu [nahrát Data tooHDInsight](hdinsight-upload-data.md) pokyny.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-160">See [Upload Data tooHDInsight](hdinsight-upload-data.md) for instructions.</span></span> <span data-ttu-id="c8f3f-161">Další informace o použití objektů BLOB v úložišti Azure s HDInsight naleznete v tématu [použití Azure Blob Storage s HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="c8f3f-161">For more information about how blobs in Azure storage are used with HDInsight, see [Use Azure Blob Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>

## <span data-ttu-id="c8f3f-162"><a id="job"></a>Příklad úloh</span><span class="sxs-lookup"><span data-stu-id="c8f3f-162"><a id="job"></a>Example job</span></span>

<span data-ttu-id="c8f3f-163">Hello následující úlohy Pig Latin načte hello `sample.log` soubor z hello výchozí úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-163">hello following Pig Latin job loads hello `sample.log` file from hello default storage for your HDInsight cluster.</span></span> <span data-ttu-id="c8f3f-164">Potom provede řady transformací, jejichž výsledkem počet jak mnohokrát každý vstupní data úrovně hello k chybě v protokolu.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-164">Then it performs a series of transformations that result in a count of how many times each log level occurred in hello input data.</span></span> <span data-ttu-id="c8f3f-165">výsledky Hello se zapisují tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-165">hello results are written tooSTDOUT.</span></span>

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

<span data-ttu-id="c8f3f-166">Hello následující obrázek zobrazuje souhrn co každý transformace nemá toohello data.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-166">hello following image shows a summary of what each transformation does toohello data.</span></span>

![Grafické znázornění transformací hello][image-hdi-pig-data-transformation]

## <span data-ttu-id="c8f3f-168"><a id="run"></a>Spustit úlohu Pig Latin hello</span><span class="sxs-lookup"><span data-stu-id="c8f3f-168"><a id="run"></a>Run hello Pig Latin job</span></span>

<span data-ttu-id="c8f3f-169">HDInsight můžete spustit úlohy Pig Latin pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-169">HDInsight can run Pig Latin jobs by using a variety of methods.</span></span> <span data-ttu-id="c8f3f-170">Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-170">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="c8f3f-171">**Použít** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="c8f3f-171">**Use this** if you want...</span></span> | <span data-ttu-id="c8f3f-172">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="c8f3f-172">...an **interactive** shell</span></span> | <span data-ttu-id="c8f3f-173">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="c8f3f-173">...**batch** processing</span></span> | <span data-ttu-id="c8f3f-174">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="c8f3f-174">...with this **cluster operating system**</span></span> | <span data-ttu-id="c8f3f-175">.. .from to **klienta**</span><span class="sxs-lookup"><span data-stu-id="c8f3f-175">...from this **client**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="c8f3f-176">SSH</span><span class="sxs-lookup"><span data-stu-id="c8f3f-176">SSH</span></span>](hdinsight-hadoop-use-pig-ssh.md) |<span data-ttu-id="c8f3f-177">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-177">✔</span></span> |<span data-ttu-id="c8f3f-178">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-178">✔</span></span> |<span data-ttu-id="c8f3f-179">Linux</span><span class="sxs-lookup"><span data-stu-id="c8f3f-179">Linux</span></span> |<span data-ttu-id="c8f3f-180">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c8f3f-181">Curl</span><span class="sxs-lookup"><span data-stu-id="c8f3f-181">Curl</span></span>](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |<span data-ttu-id="c8f3f-182">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-182">✔</span></span> |<span data-ttu-id="c8f3f-183">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-183">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-184">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-184">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="c8f3f-185">Sada .NET SDK pro Hadoop</span><span class="sxs-lookup"><span data-stu-id="c8f3f-185">.NET SDK for Hadoop</span></span>](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |<span data-ttu-id="c8f3f-186">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-186">✔</span></span> |<span data-ttu-id="c8f3f-187">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-187">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-188">Windows (prozatím)</span><span class="sxs-lookup"><span data-stu-id="c8f3f-188">Windows (for now)</span></span> |
| [<span data-ttu-id="c8f3f-189">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="c8f3f-189">Windows PowerShell</span></span>](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |<span data-ttu-id="c8f3f-190">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-190">✔</span></span> |<span data-ttu-id="c8f3f-191">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-191">Linux or Windows</span></span> |<span data-ttu-id="c8f3f-192">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-192">Windows</span></span> |
| <span data-ttu-id="c8f3f-193">[Vzdálená plocha](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="c8f3f-193">[Remote Desktop](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="c8f3f-194">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-194">✔</span></span> |<span data-ttu-id="c8f3f-195">✔</span><span class="sxs-lookup"><span data-stu-id="c8f3f-195">✔</span></span> |<span data-ttu-id="c8f3f-196">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-196">Windows</span></span> |<span data-ttu-id="c8f3f-197">Windows</span><span class="sxs-lookup"><span data-stu-id="c8f3f-197">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="c8f3f-198">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-198">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c8f3f-199">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c8f3f-199">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="pig-and-sql-server-integration-services"></a><span data-ttu-id="c8f3f-200">Pig a SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="c8f3f-200">Pig and SQL Server Integration Services</span></span>

<span data-ttu-id="c8f3f-201">Pomocí integrace služby SSIS (SQL Server) toorun úlohu Pig.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-201">You can use SQL Server Integration Services (SSIS) toorun a Pig job.</span></span> <span data-ttu-id="c8f3f-202">Hello Azure Feature Pack pro službu SSIS poskytuje hello následující součásti, které pracují se úlohy Pig v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-202">hello Azure Feature Pack for SSIS provides hello following components that work with Pig jobs on HDInsight.</span></span>

* <span data-ttu-id="c8f3f-203">[Úloha Azure HDInsight Pig][pigtask]</span><span class="sxs-lookup"><span data-stu-id="c8f3f-203">[Azure HDInsight Pig Task][pigtask]</span></span>

* <span data-ttu-id="c8f3f-204">[Správce připojení k Azure předplatného][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="c8f3f-204">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="c8f3f-205">Další informace o hello Azure Feature Pack pro služby SSIS [sem][ssispack].</span><span class="sxs-lookup"><span data-stu-id="c8f3f-205">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="c8f3f-206"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8f3f-206"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="c8f3f-207">Teď, když jste se naučili, jak toouse Pig s HDInsight, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c8f3f-207">Now that you have learned how toouse Pig with HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="c8f3f-208">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="c8f3f-208">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="c8f3f-209">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="c8f3f-209">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* [<span data-ttu-id="c8f3f-210">Použití nástroje Sqoop s HDInsight</span><span class="sxs-lookup"><span data-stu-id="c8f3f-210">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop.md)
* [<span data-ttu-id="c8f3f-211">Použijte Oozie s HDInsight</span><span class="sxs-lookup"><span data-stu-id="c8f3f-211">Use Oozie with HDInsight</span></span>](hdinsight-use-oozie.md)
* <span data-ttu-id="c8f3f-212">[Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="c8f3f-212">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

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
