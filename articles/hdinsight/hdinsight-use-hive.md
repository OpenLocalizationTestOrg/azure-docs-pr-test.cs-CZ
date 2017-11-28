---
title: Co je Apache Hive a HiveQL - Azure HDInsight | Microsoft Docs
description: "Apache Hive je systém datového skladu pro Hadoop. Můžete dát dotaz na data uložená v Hive pomocí HiveQL, který podobná Transact-SQL. V tomto dokumentu další informace o použití Hive a HiveQL s Azure HDInsight."
keywords: "hiveql, co je hive, hadoop hiveql, postup používání hive, přečtěte si, hive, co je hive"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 6b3ee17141f773bec07cf40e0b6d63363e9b5164
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a><span data-ttu-id="96aa0-106">Co je Apache Hive a HiveQL v Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="96aa0-106">What is Apache Hive and HiveQL on Azure HDInsight?</span></span>

<span data-ttu-id="96aa0-107">[Apache Hive](http://hive.apache.org/) je systém datového skladu pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="96aa0-107">[Apache Hive](http://hive.apache.org/) is a data warehouse system for Hadoop.</span></span> <span data-ttu-id="96aa0-108">Hive umožňuje shrnutí dat, dotazy a analýzy dat.</span><span class="sxs-lookup"><span data-stu-id="96aa0-108">Hive enables data summarization, querying, and analysis of data.</span></span> <span data-ttu-id="96aa0-109">Dotazy Hive jsou zapsány ve HiveQL, což je podobná SQL dotazovací jazyk.</span><span class="sxs-lookup"><span data-stu-id="96aa0-109">Hive queries are written in HiveQL, which is a query language similar to SQL.</span></span>

<span data-ttu-id="96aa0-110">Hive umožňuje strukturu projektu na do značné míry Nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="96aa0-110">Hive allows you to project structure on largely unstructured data.</span></span> <span data-ttu-id="96aa0-111">Po definování strukturu, můžete zadávat dotazy na data bez vědomí Java nebo MapReduce HiveQL.</span><span class="sxs-lookup"><span data-stu-id="96aa0-111">After you define the structure, you can use HiveQL to query the data without knowledge of Java or MapReduce.</span></span>

<span data-ttu-id="96aa0-112">HDInsight poskytuje několik typů clusteru, které jsou přizpůsobená pro konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="96aa0-112">HDInsight provides several cluster types, which are tuned for specific workloads.</span></span> <span data-ttu-id="96aa0-113">Pro dotazy Hive se nejčastěji používají následující typy clusteru:</span><span class="sxs-lookup"><span data-stu-id="96aa0-113">The following cluster types are most often used for Hive queries:</span></span>

* <span data-ttu-id="96aa0-114">__Interaktivní Hive__: cluster A Hadoop, který poskytuje [nízká latence analytického zpracování (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) funkce zlepšit dobu odezvy na interaktivních dotazů.</span><span class="sxs-lookup"><span data-stu-id="96aa0-114">__Interactive Hive__: A Hadoop cluster that provides [Low Latency Analytical Processing (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionality to improve response times for interactive queries.</span></span> <span data-ttu-id="96aa0-115">Další informace najdete v tématu [začínat interaktivní Hive v HDInsight](hdinsight-hadoop-use-interactive-hive.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-115">For more information, see the [Start with Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

* <span data-ttu-id="96aa0-116">__Hadoop__: cluster A Hadoop, který je přizpůsobená pro dávkové zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="96aa0-116">__Hadoop__: A Hadoop cluster that is tuned for batch processing workloads.</span></span> <span data-ttu-id="96aa0-117">Další informace najdete v tématu [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-117">For more information, see the [Start with Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) document.</span></span>

* <span data-ttu-id="96aa0-118">__Spark__: Apache Spark obsahuje integrovanou funkci pro práci s Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-118">__Spark__: Apache Spark has built-in functionality for working with Hive.</span></span> <span data-ttu-id="96aa0-119">Další informace najdete v tématu [začínat Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-119">For more information, see the [Start with Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) document.</span></span>

* <span data-ttu-id="96aa0-120">__HBase__: HiveQL lze použít k dotazování na data uložená v HBase.</span><span class="sxs-lookup"><span data-stu-id="96aa0-120">__HBase__: HiveQL can be used to query data stored in HBase.</span></span> <span data-ttu-id="96aa0-121">Další informace najdete v tématu [začínat HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-121">For more information, see the [Start with HBase on HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) document.</span></span>

## <a name="how-to-use-hive"></a><span data-ttu-id="96aa0-122">Postup použití Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-122">How to use Hive</span></span>

<span data-ttu-id="96aa0-123">Pomocí následující tabulky způsob používání Hive s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="96aa0-123">Use the following table to discover how to use Hive with HDInsight:</span></span>

| <span data-ttu-id="96aa0-124">**Tuto metodu použijte** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="96aa0-124">**Use this method** if you want...</span></span> | <span data-ttu-id="96aa0-125">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="96aa0-125">...an **interactive** shell</span></span> | <span data-ttu-id="96aa0-126">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="96aa0-126">...**batch** processing</span></span> | <span data-ttu-id="96aa0-127">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="96aa0-127">...with this **cluster operating system**</span></span> | <span data-ttu-id="96aa0-128">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="96aa0-128">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="96aa0-129">Zobrazení Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-129">Hive View</span></span>](hdinsight-hadoop-use-hive-ambari-view.md) |<span data-ttu-id="96aa0-130">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-130">✔</span></span> |<span data-ttu-id="96aa0-131">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-131">✔</span></span> |<span data-ttu-id="96aa0-132">Linux</span><span class="sxs-lookup"><span data-stu-id="96aa0-132">Linux</span></span> |<span data-ttu-id="96aa0-133">Žádné (prohlížeč na základě)</span><span class="sxs-lookup"><span data-stu-id="96aa0-133">Any (browser based)</span></span> |
| [<span data-ttu-id="96aa0-134">Beeline klienta</span><span class="sxs-lookup"><span data-stu-id="96aa0-134">Beeline client</span></span>](hdinsight-hadoop-use-hive-beeline.md) |<span data-ttu-id="96aa0-135">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-135">✔</span></span> |<span data-ttu-id="96aa0-136">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-136">✔</span></span> |<span data-ttu-id="96aa0-137">Linux</span><span class="sxs-lookup"><span data-stu-id="96aa0-137">Linux</span></span> |<span data-ttu-id="96aa0-138">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="96aa0-138">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="96aa0-139">REST API</span><span class="sxs-lookup"><span data-stu-id="96aa0-139">REST API</span></span>](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |<span data-ttu-id="96aa0-140">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-140">✔</span></span> |<span data-ttu-id="96aa0-141">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="96aa0-141">Linux or Windows*</span></span> |<span data-ttu-id="96aa0-142">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="96aa0-142">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="96aa0-143">Nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96aa0-143">HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |<span data-ttu-id="96aa0-144">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-144">✔</span></span> |<span data-ttu-id="96aa0-145">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="96aa0-145">Linux or Windows*</span></span> |<span data-ttu-id="96aa0-146">Windows</span><span class="sxs-lookup"><span data-stu-id="96aa0-146">Windows</span></span> |
| [<span data-ttu-id="96aa0-147">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="96aa0-147">Windows PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |<span data-ttu-id="96aa0-148">✔</span><span class="sxs-lookup"><span data-stu-id="96aa0-148">✔</span></span> |<span data-ttu-id="96aa0-149">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="96aa0-149">Linux or Windows*</span></span> |<span data-ttu-id="96aa0-150">Windows</span><span class="sxs-lookup"><span data-stu-id="96aa0-150">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="96aa0-151">\*Linux je jenom operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="96aa0-151">\* Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="96aa0-152">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="96aa0-152">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="96aa0-153">Pokud používáte cluster HDInsight se systémem Windows, můžete použít [dotazu konzoly](hdinsight-hadoop-use-hive-query-console.md) z prohlížeče nebo [vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md) ke spouštění dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-153">If you are using a Windows-based HDInsight cluster, you can use the [Query console](hdinsight-hadoop-use-hive-query-console.md) from your browser or [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md) to run Hive queries.</span></span>

## <a name="hiveql-language-reference"></a><span data-ttu-id="96aa0-154">Referenční dokumentace jazyka HiveQL</span><span class="sxs-lookup"><span data-stu-id="96aa0-154">HiveQL language reference</span></span>

<span data-ttu-id="96aa0-155">Referenční dokumentace jazyka HiveQL je k dispozici v [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span><span class="sxs-lookup"><span data-stu-id="96aa0-155">HiveQL language reference is available in the [language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span></span>

## <a name="hive-and-data-structure"></a><span data-ttu-id="96aa0-156">Hive a datové struktury</span><span class="sxs-lookup"><span data-stu-id="96aa0-156">Hive and data structure</span></span>

<span data-ttu-id="96aa0-157">Jak pracovat s strukturovaných a částečně strukturovaných dat plně chápe, Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-157">Hive understands how to work with structured and semi-structured data.</span></span> <span data-ttu-id="96aa0-158">Například textové soubory kde pole jsou oddělená konkrétní znaků.</span><span class="sxs-lookup"><span data-stu-id="96aa0-158">For example, text files where the fields are delimited by specific characters.</span></span> <span data-ttu-id="96aa0-159">Následující příkaz HiveQL vytvoří tabulku nad daty oddělených mezerami:</span><span class="sxs-lookup"><span data-stu-id="96aa0-159">The following HiveQL statement creates a table over space-delimited data:</span></span>

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

<span data-ttu-id="96aa0-160">Hive podporuje i vlastní **serializátor/deserializers (SerDe)** složitý nebo nepravidelně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="96aa0-160">Hive also supports custom **serializer/deserializers (SerDe)** for complex or irregularly structured data.</span></span> <span data-ttu-id="96aa0-161">Další informace najdete v tématu [použití vlastní SerDe JSON s HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-161">For more information, see the [How to use a custom JSON SerDe with HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.</span></span>

<span data-ttu-id="96aa0-162">Další informace o souboru formátů podporovaných Hive naleznete v tématu [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span><span class="sxs-lookup"><span data-stu-id="96aa0-162">For more information on file formats supported by Hive, see the [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span></span>

## <a name="hive-internal-tables-vs-external-tables"></a><span data-ttu-id="96aa0-163">Interní tabulky vs externích tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-163">Hive internal tables vs external tables</span></span>

<span data-ttu-id="96aa0-164">Existují dva typy tabulek, které můžete vytvořit pomocí Hive:</span><span class="sxs-lookup"><span data-stu-id="96aa0-164">There are two types of tables that you can create with Hive:</span></span>

* <span data-ttu-id="96aa0-165">__Interní__: Data jsou uložena v datovém skladu Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-165">__Internal__: Data is stored in the Hive data warehouse.</span></span> <span data-ttu-id="96aa0-166">Datový sklad je umístěn v `/hive/warehouse/` na výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="96aa0-166">The data warehouse is located at `/hive/warehouse/` on the default storage for the cluster.</span></span>

    <span data-ttu-id="96aa0-167">Použití interní tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="96aa0-167">Use internal tables when:</span></span>

    * <span data-ttu-id="96aa0-168">Data jsou dočasné.</span><span class="sxs-lookup"><span data-stu-id="96aa0-168">Data is temporary.</span></span>
    * <span data-ttu-id="96aa0-169">Chcete Hive ke správě životního cyklu tabulky a data.</span><span class="sxs-lookup"><span data-stu-id="96aa0-169">You want Hive to manage the lifecycle of the table and data.</span></span>

* <span data-ttu-id="96aa0-170">__Externí__: Data jsou uložena mimo datového skladu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-170">__External__: Data is stored outside the data warehouse.</span></span> <span data-ttu-id="96aa0-171">Data můžete uložit na jakékoli úložiště dostupné v clusteru.</span><span class="sxs-lookup"><span data-stu-id="96aa0-171">The data can be stored on any storage accessible by the cluster.</span></span>

    <span data-ttu-id="96aa0-172">Použijte externí tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="96aa0-172">Use external tables when:</span></span>

    * <span data-ttu-id="96aa0-173">Data se také používají mimo Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-173">The data is also used outside of Hive.</span></span> <span data-ttu-id="96aa0-174">Například soubory, data se aktualizují jiným procesem (který není uzamčení souborů.)</span><span class="sxs-lookup"><span data-stu-id="96aa0-174">For example, the data files are updated by another process (that does not lock the files.)</span></span>
    * <span data-ttu-id="96aa0-175">Data musí zůstat v základní umístění i po vyřazení v tabulce.</span><span class="sxs-lookup"><span data-stu-id="96aa0-175">Data needs to remain in the underlying location, even after dropping the table.</span></span>
    * <span data-ttu-id="96aa0-176">Je nutné do vlastního umístění, jako je například účet jiné než výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="96aa0-176">You need a custom location, such as a non-default storage account.</span></span>
    * <span data-ttu-id="96aa0-177">Program než hive spravuje formát dat, umístění atd.</span><span class="sxs-lookup"><span data-stu-id="96aa0-177">A program other than hive manages the data format, location, etc.</span></span>

<span data-ttu-id="96aa0-178">Další informace najdete v tématu [Hive interní a externí tabulky ÚVOD] [ cindygross-hive-tables] příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-178">For more information, see the [Hive Internal and External Tables Intro][cindygross-hive-tables] blog post.</span></span>

## <a name="user-defined-functions-udf"></a><span data-ttu-id="96aa0-179">Uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="96aa0-179">User-defined functions (UDF)</span></span>

<span data-ttu-id="96aa0-180">Hive lze také prostřednictvím rozšířit **uživatelsky definované funkce (UDF)**.</span><span class="sxs-lookup"><span data-stu-id="96aa0-180">Hive can also be extended through **user-defined functions (UDF)**.</span></span> <span data-ttu-id="96aa0-181">Uživatelem definovanou FUNKCI umožňuje implementovat logiku, která není modelován snadno nebo funkce v HiveQL.</span><span class="sxs-lookup"><span data-stu-id="96aa0-181">A UDF allows you to implement functionality or logic that isn't easily modeled in HiveQL.</span></span> <span data-ttu-id="96aa0-182">Příklad použití UDF s Hive najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="96aa0-182">For an example of using UDFs with Hive, see the following documents:</span></span>

* [<span data-ttu-id="96aa0-183">Uživatelem definované funkce Java pomocí Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-183">Use a Java user-defined function with Hive</span></span>](hdinsight-hadoop-hive-java-udf.md)

* [<span data-ttu-id="96aa0-184">Uživatelem definované funkce Python pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="96aa0-184">Use a Python user-defined function with Hive and Pig</span></span>](hdinsight-python.md)

* [<span data-ttu-id="96aa0-185">C# uživatelem definované funkce pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="96aa0-185">Use a C# user-defined function with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="96aa0-186">Postup přidání vlastní uživatelem definované funkce Hive HDInsight</span><span class="sxs-lookup"><span data-stu-id="96aa0-186">How to add a custom Hive user-defined function to HDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [<span data-ttu-id="96aa0-187">Příklad Hive uživatelsky definované funkce pro převod formátů data a času na časové razítko Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-187">An example Hive user-defined function to convert date/time formats to Hive timestamp</span></span>](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <span data-ttu-id="96aa0-188"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="96aa0-188"><a id="data"></a>Example data</span></span>

<span data-ttu-id="96aa0-189">Hive v HDInsight obsahuje předem načtený vnitřní tabulku s názvem `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="96aa0-189">Hive on HDInsight comes pre-loaded with an internal table named `hivesampletable`.</span></span> <span data-ttu-id="96aa0-190">HDInsight také poskytuje příklad datových sad, které lze použít s Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-190">HDInsight also provides example data sets that can be used with Hive.</span></span> <span data-ttu-id="96aa0-191">Tyto sady dat se ukládají do `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="96aa0-191">These data sets are stored in the `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="96aa0-192">Tyto adresáře neexistují ve výchozím nastavení úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="96aa0-192">These directories exist in the default storage for your cluster.</span></span>

## <span data-ttu-id="96aa0-193"><a id="job"></a>Příklad dotazu Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-193"><a id="job"></a>Example Hive query</span></span>

<span data-ttu-id="96aa0-194">Následující příkazy HiveQL projektu sloupce na `/example/data/sample.log` souboru:</span><span class="sxs-lookup"><span data-stu-id="96aa0-194">The following HiveQL statements project columns onto the `/example/data/sample.log` file:</span></span>

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

<span data-ttu-id="96aa0-195">Příkazy HiveQL v předchozím příkladu, proveďte následující akce:</span><span class="sxs-lookup"><span data-stu-id="96aa0-195">In the previous example, the HiveQL statements perform the following actions:</span></span>

* <span data-ttu-id="96aa0-196">`set hive.execution.engine=tez;`: Nastaví použít Tez modulu pro spouštění.</span><span class="sxs-lookup"><span data-stu-id="96aa0-196">`set hive.execution.engine=tez;`: Sets the execution engine to use Tez.</span></span> <span data-ttu-id="96aa0-197">Pomocí Tez místo MapReduce můžete poskytnout zvýšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="96aa0-197">Using Tez instead of MapReduce can provide an increase in query performance.</span></span> <span data-ttu-id="96aa0-198">Další informace o Tez naleznete v tématu [použití rozhraní Apache Tez pro zlepšení výkonu](#usetez) části.</span><span class="sxs-lookup"><span data-stu-id="96aa0-198">For more information on Tez, see the [Use Apache Tez for improved performance](#usetez) section.</span></span>

    > [!NOTE]
    > <span data-ttu-id="96aa0-199">Tento příkaz je jenom když používáte cluster HDInsight se systémem Windows je vyžadováno.</span><span class="sxs-lookup"><span data-stu-id="96aa0-199">This statement is only required when using a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="96aa0-200">Tez je výchozí modul provádění HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="96aa0-200">Tez is the default execution engine for Linux-based HDInsight.</span></span>

* <span data-ttu-id="96aa0-201">`DROP TABLE`: Pokud v tabulce již existuje, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="96aa0-201">`DROP TABLE`: If the table already exists, delete it.</span></span>

* <span data-ttu-id="96aa0-202">`CREATE EXTERNAL TABLE`: Vytvoří novou **externí** tabulky v Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-202">`CREATE EXTERNAL TABLE`: Creates a new **external** table in Hive.</span></span> <span data-ttu-id="96aa0-203">Externí tabulky pouze uložit definici tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-203">External tables only store the table definition in Hive.</span></span> <span data-ttu-id="96aa0-204">Data je ponechán v původním umístění a v původním formátu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-204">The data is left in the original location and in the original format.</span></span>

* <span data-ttu-id="96aa0-205">`ROW FORMAT`: Určuje způsob formátování data Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-205">`ROW FORMAT`: Tells Hive how the data is formatted.</span></span> <span data-ttu-id="96aa0-206">V takovém případě polí v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="96aa0-206">In this case, the fields in each log are separated by a space.</span></span>

* <span data-ttu-id="96aa0-207">`STORED AS TEXTFILE LOCATION`: Informuje Hive se uloží data ( `example/data` adresáře) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="96aa0-207">`STORED AS TEXTFILE LOCATION`: Tells Hive where the data is stored (the `example/data` directory) and that it is stored as text.</span></span> <span data-ttu-id="96aa0-208">Data mohou být v jednom souboru nebo rozloženy více souborů v adresáři.</span><span class="sxs-lookup"><span data-stu-id="96aa0-208">The data can be in one file or spread across multiple files within the directory.</span></span>

* <span data-ttu-id="96aa0-209">`SELECT`: Počet všech řádků vybere kde sloupec **t4** obsahuje hodnotu **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="96aa0-209">`SELECT`: Selects a count of all rows where the column **t4** contains the value **[ERROR]**.</span></span> <span data-ttu-id="96aa0-210">Tento příkaz vrátí hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-210">This statement returns a value of **3** because there are three rows that contain this value.</span></span>

* <span data-ttu-id="96aa0-211">`INPUT__FILE__NAME LIKE '%.log'`-Hive se pokusí použít schéma pro všechny soubory v adresáři.</span><span class="sxs-lookup"><span data-stu-id="96aa0-211">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts to apply the schema to all files in the directory.</span></span> <span data-ttu-id="96aa0-212">V takovém případě adresáře obsahuje soubory, které neodpovídají žádné schéma.</span><span class="sxs-lookup"><span data-stu-id="96aa0-212">In this case, the directory contains files that do not match the schema.</span></span> <span data-ttu-id="96aa0-213">Aby paměti data ve výsledcích tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="96aa0-213">To prevent garbage data in the results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

> [!NOTE]
> <span data-ttu-id="96aa0-214">Externí tabulky by měl být použit při očekáváte, že v základních datech aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="96aa0-214">External tables should be used when you expect the underlying data to be updated by an external source.</span></span> <span data-ttu-id="96aa0-215">Například procesu nahrávání automatizované dat, nebo operaci MapReduce.</span><span class="sxs-lookup"><span data-stu-id="96aa0-215">For example, an automated data upload process, or MapReduce operation.</span></span>
>
> <span data-ttu-id="96aa0-216">Vyřazení externí tabulku nemá **není** odstranit data, odstraní pouze definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="96aa0-216">Dropping an external table does **not** delete the data, it only deletes the table definition.</span></span>

<span data-ttu-id="96aa0-217">Chcete-li vytvořit **interní** místo externí tabulky, použijte následující HiveQL:</span><span class="sxs-lookup"><span data-stu-id="96aa0-217">To create an **internal** table instead of external, use the following HiveQL:</span></span>

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

<span data-ttu-id="96aa0-218">Tyto příkazy provádět následující akce:</span><span class="sxs-lookup"><span data-stu-id="96aa0-218">These statements perform the following actions:</span></span>

* <span data-ttu-id="96aa0-219">`CREATE TABLE IF NOT EXISTS`: Pokud tabulka neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="96aa0-219">`CREATE TABLE IF NOT EXISTS`: If the table does not exist, create it.</span></span> <span data-ttu-id="96aa0-220">Protože **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="96aa0-220">Because the **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="96aa0-221">V tabulce je uložená v datovém skladu Hive a úplně spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-221">The table is stored in the Hive data warehouse and is managed completely by Hive.</span></span>

* <span data-ttu-id="96aa0-222">`STORED AS ORC`: Ukládá data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="96aa0-222">`STORED AS ORC`: Stores the data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="96aa0-223">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-223">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

* <span data-ttu-id="96aa0-224">`INSERT OVERWRITE ... SELECT`: Vybere řádky z **log4jLogs** tabulku, která obsahuje **[Chyba]**a potom vkládá data do **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="96aa0-224">`INSERT OVERWRITE ... SELECT`: Selects rows from the **log4jLogs** table that contains **[ERROR]**, and then inserts the data into the **errorLogs** table.</span></span>

> [!NOTE]
> <span data-ttu-id="96aa0-225">Na rozdíl od externích tabulek se odstranit interní tabulku taky odstraní základní data.</span><span class="sxs-lookup"><span data-stu-id="96aa0-225">Unlike external tables, dropping an internal table also deletes the underlying data.</span></span>

## <a name="improve-hive-query-performance"></a><span data-ttu-id="96aa0-226">Zlepšení výkonu dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="96aa0-226">Improve Hive query performance</span></span>

### <span data-ttu-id="96aa0-227"><a id="usetez"></a>Apache Tez</span><span class="sxs-lookup"><span data-stu-id="96aa0-227"><a id="usetez"></a>Apache Tez</span></span>

<span data-ttu-id="96aa0-228">[Apache Tez](http://tez.apache.org) je rozhraní, které umožňuje data náročné aplikace, například Hive, spusťte mnohem efektivněji ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="96aa0-228">[Apache Tez](http://tez.apache.org) is a framework that allows data intensive applications, such as Hive, to run much more efficiently at scale.</span></span> <span data-ttu-id="96aa0-229">Ve výchozím nastavení pro clustery HDInsight se systémem Linux je povolené tez.</span><span class="sxs-lookup"><span data-stu-id="96aa0-229">Tez is enabled by default for Linux-based HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="96aa0-230">Tez není momentálně vypnuto ve výchozím nastavení pro clustery HDInsight se systémem Windows a musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="96aa0-230">Tez is currently off by default for Windows-based HDInsight clusters and it must be enabled.</span></span> <span data-ttu-id="96aa0-231">Pokud chcete využít výhod Tez, nutné nastavit následující hodnotu pro dotaz Hive:</span><span class="sxs-lookup"><span data-stu-id="96aa0-231">To take advantage of Tez, the following value must be set for a Hive query:</span></span>
>
> `set hive.execution.engine=tez;`
>
> <span data-ttu-id="96aa0-232">Tez je výchozí modul pro clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="96aa0-232">Tez is the default engine for Linux-based HDInsight clusters.</span></span>

<span data-ttu-id="96aa0-233">[Hive na dokumentech návrhu Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) obsahuje podrobnosti o volby implementace a vyladění konfigurace.</span><span class="sxs-lookup"><span data-stu-id="96aa0-233">The [Hive on Tez design documents](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) contains details about the implementation choices and tuning configurations.</span></span>

<span data-ttu-id="96aa0-234">Ladění úloh proběhla za použití Tez, HDInsight poskytuje následující webové uživatelská rozhraní, které vám umožní zobrazit podrobnosti o úlohách Tez:</span><span class="sxs-lookup"><span data-stu-id="96aa0-234">To aid in debugging jobs ran using Tez, HDInsight provides the following web UIs that allow you to view details of Tez jobs:</span></span>

* [<span data-ttu-id="96aa0-235">Použití zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="96aa0-235">Use the Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

* [<span data-ttu-id="96aa0-236">Pomocí uživatelského rozhraní Tez na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="96aa0-236">Use the Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a><span data-ttu-id="96aa0-237">Nízká latence analytického zpracování (LLAP)</span><span class="sxs-lookup"><span data-stu-id="96aa0-237">Low Latency Analytical Processing (LLAP)</span></span>

<span data-ttu-id="96aa0-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (někdy označované jako Live dlouhé a proces) je nová funkce v podregistru 2.0, který umožňuje ukládání do mezipaměti v paměti dotazů.</span><span class="sxs-lookup"><span data-stu-id="96aa0-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (sometimes known as Live Long and Process) is a new feature in Hive 2.0 that allows in-memory caching of queries.</span></span> <span data-ttu-id="96aa0-239">LLAP umožňuje mnohem rychlejší než dotazů Hive [26 x rychlejší než Hive 1.x v některých případech](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span><span class="sxs-lookup"><span data-stu-id="96aa0-239">LLAP makes Hive queries much faster, up to [26x faster than Hive 1.x in some cases](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span></span>

<span data-ttu-id="96aa0-240">HDInsight poskytuje LLAP v clusteru typu interaktivní Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-240">HDInsight provides LLAP in the Interactive Hive cluster type.</span></span> <span data-ttu-id="96aa0-241">Další informace najdete v tématu [začínat interaktivní Hive](hdinsight-hadoop-use-interactive-hive.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="96aa0-241">For more information, see the [Start with Interactive Hive](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

## <a name="hive-jobs-and-sql-server-integration-services"></a><span data-ttu-id="96aa0-242">Úlohy Hive a SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="96aa0-242">Hive jobs and SQL Server Integration Services</span></span>

<span data-ttu-id="96aa0-243">Integrace služby SSIS (SQL Server) můžete použít ke spuštění úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="96aa0-243">You can use SQL Server Integration Services (SSIS) to run a Hive job.</span></span> <span data-ttu-id="96aa0-244">Azure Feature Pack pro službu SSIS poskytuje následující součásti, které pracují se úlohy Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="96aa0-244">The Azure Feature Pack for SSIS provides the following components that work with Hive jobs on HDInsight.</span></span>

* <span data-ttu-id="96aa0-245">[Úloha Azure HDInsight Hive][hivetask]</span><span class="sxs-lookup"><span data-stu-id="96aa0-245">[Azure HDInsight Hive Task][hivetask]</span></span>

* <span data-ttu-id="96aa0-246">[Správce připojení k Azure předplatného][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="96aa0-246">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="96aa0-247">Další informace o Azure Feature Pack pro služby SSIS [sem][ssispack].</span><span class="sxs-lookup"><span data-stu-id="96aa0-247">Learn more about the Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="96aa0-248"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="96aa0-248"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="96aa0-249">Teď, když jste se naučili novinky Hive a způsobu jeho použití s Hadoop v HDInsight, pomocí následujících odkazů a prozkoumejte další způsoby, jak pracovat s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="96aa0-249">Now that you've learned what Hive is and how to use it with Hadoop in HDInsight, use the following links to explore other ways to work with Azure HDInsight.</span></span>

* <span data-ttu-id="96aa0-250">[Nahrání dat do služby HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="96aa0-250">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="96aa0-251">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="96aa0-251">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="96aa0-252">[Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="96aa0-252">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
