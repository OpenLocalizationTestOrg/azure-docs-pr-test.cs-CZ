---
title: aaaWhat je Apache Hive a HiveQL - Azure HDInsight | Microsoft Docs
description: "Apache Hive je systém datového skladu pro Hadoop. Můžete dát dotaz na data uložená v Hive pomocí HiveQL, které podobné tooTransact-SQL. V tomto dokumentu, zjistěte, jak toouse Hive a HiveQL s Azure HDInsight."
keywords: "hiveql, co je hive, hadoop hiveql, hive, co je hive zjistěte, jak toouse hive,"
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
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a><span data-ttu-id="1baa7-106">Co je Apache Hive a HiveQL v Azure HDInsight?</span><span class="sxs-lookup"><span data-stu-id="1baa7-106">What is Apache Hive and HiveQL on Azure HDInsight?</span></span>

<span data-ttu-id="1baa7-107">[Apache Hive](http://hive.apache.org/) je systém datového skladu pro Hadoop.</span><span class="sxs-lookup"><span data-stu-id="1baa7-107">[Apache Hive](http://hive.apache.org/) is a data warehouse system for Hadoop.</span></span> <span data-ttu-id="1baa7-108">Hive umožňuje shrnutí dat, dotazy a analýzy dat.</span><span class="sxs-lookup"><span data-stu-id="1baa7-108">Hive enables data summarization, querying, and analysis of data.</span></span> <span data-ttu-id="1baa7-109">Dotazy Hive jsou zapsány ve HiveQL, který je podobný tooSQL dotaz jazyka.</span><span class="sxs-lookup"><span data-stu-id="1baa7-109">Hive queries are written in HiveQL, which is a query language similar tooSQL.</span></span>

<span data-ttu-id="1baa7-110">Hive můžete tooproject struktura na do značné míry Nestrukturovaná data.</span><span class="sxs-lookup"><span data-stu-id="1baa7-110">Hive allows you tooproject structure on largely unstructured data.</span></span> <span data-ttu-id="1baa7-111">Po definování hello struktura, můžete použít HiveQL tooquery hello dat bez znalosti Java nebo MapReduce.</span><span class="sxs-lookup"><span data-stu-id="1baa7-111">After you define hello structure, you can use HiveQL tooquery hello data without knowledge of Java or MapReduce.</span></span>

<span data-ttu-id="1baa7-112">HDInsight poskytuje několik typů clusteru, které jsou přizpůsobená pro konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="1baa7-112">HDInsight provides several cluster types, which are tuned for specific workloads.</span></span> <span data-ttu-id="1baa7-113">Hello následující typy clusteru se nejčastěji používají pro dotazy Hive:</span><span class="sxs-lookup"><span data-stu-id="1baa7-113">hello following cluster types are most often used for Hive queries:</span></span>

* <span data-ttu-id="1baa7-114">__Interaktivní Hive__: cluster A Hadoop, který poskytuje [nízká latence analytického zpracování (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) doby odezvy tooimprove funkce pro interaktivní dotazy.</span><span class="sxs-lookup"><span data-stu-id="1baa7-114">__Interactive Hive__: A Hadoop cluster that provides [Low Latency Analytical Processing (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) functionality tooimprove response times for interactive queries.</span></span> <span data-ttu-id="1baa7-115">Další informace najdete v tématu hello [začínat interaktivní Hive v HDInsight](hdinsight-hadoop-use-interactive-hive.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-115">For more information, see hello [Start with Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

* <span data-ttu-id="1baa7-116">__Hadoop__: cluster A Hadoop, který je přizpůsobená pro dávkové zpracování úlohy.</span><span class="sxs-lookup"><span data-stu-id="1baa7-116">__Hadoop__: A Hadoop cluster that is tuned for batch processing workloads.</span></span> <span data-ttu-id="1baa7-117">Další informace najdete v tématu hello [začněte s Hadoop v HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-117">For more information, see hello [Start with Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) document.</span></span>

* <span data-ttu-id="1baa7-118">__Spark__: Apache Spark obsahuje integrovanou funkci pro práci s Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-118">__Spark__: Apache Spark has built-in functionality for working with Hive.</span></span> <span data-ttu-id="1baa7-119">Další informace najdete v tématu hello [začínat Spark v HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-119">For more information, see hello [Start with Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) document.</span></span>

* <span data-ttu-id="1baa7-120">__HBase__: HiveQL lze použít tooquery data uložená v HBase.</span><span class="sxs-lookup"><span data-stu-id="1baa7-120">__HBase__: HiveQL can be used tooquery data stored in HBase.</span></span> <span data-ttu-id="1baa7-121">Další informace najdete v tématu hello [začínat HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-121">For more information, see hello [Start with HBase on HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) document.</span></span>

## <a name="how-toouse-hive"></a><span data-ttu-id="1baa7-122">Jak toouse Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-122">How toouse Hive</span></span>

<span data-ttu-id="1baa7-123">Použijte následující tabulku toodiscover jak toouse Hive s HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="1baa7-123">Use hello following table toodiscover how toouse Hive with HDInsight:</span></span>

| <span data-ttu-id="1baa7-124">**Tuto metodu použijte** Pokud chcete...</span><span class="sxs-lookup"><span data-stu-id="1baa7-124">**Use this method** if you want...</span></span> | <span data-ttu-id="1baa7-125">.. .an **interaktivní** prostředí</span><span class="sxs-lookup"><span data-stu-id="1baa7-125">...an **interactive** shell</span></span> | <span data-ttu-id="1baa7-126">... **batch** zpracování</span><span class="sxs-lookup"><span data-stu-id="1baa7-126">...**batch** processing</span></span> | <span data-ttu-id="1baa7-127">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="1baa7-127">...with this **cluster operating system**</span></span> | <span data-ttu-id="1baa7-128">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="1baa7-128">...from this **client operating system**</span></span> |
|:--- |:---:|:---:|:--- |:--- |
| [<span data-ttu-id="1baa7-129">Zobrazení Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-129">Hive View</span></span>](hdinsight-hadoop-use-hive-ambari-view.md) |<span data-ttu-id="1baa7-130">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-130">✔</span></span> |<span data-ttu-id="1baa7-131">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-131">✔</span></span> |<span data-ttu-id="1baa7-132">Linux</span><span class="sxs-lookup"><span data-stu-id="1baa7-132">Linux</span></span> |<span data-ttu-id="1baa7-133">Žádné (prohlížeč na základě)</span><span class="sxs-lookup"><span data-stu-id="1baa7-133">Any (browser based)</span></span> |
| [<span data-ttu-id="1baa7-134">Beeline klienta</span><span class="sxs-lookup"><span data-stu-id="1baa7-134">Beeline client</span></span>](hdinsight-hadoop-use-hive-beeline.md) |<span data-ttu-id="1baa7-135">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-135">✔</span></span> |<span data-ttu-id="1baa7-136">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-136">✔</span></span> |<span data-ttu-id="1baa7-137">Linux</span><span class="sxs-lookup"><span data-stu-id="1baa7-137">Linux</span></span> |<span data-ttu-id="1baa7-138">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="1baa7-138">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="1baa7-139">REST API</span><span class="sxs-lookup"><span data-stu-id="1baa7-139">REST API</span></span>](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |<span data-ttu-id="1baa7-140">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-140">✔</span></span> |<span data-ttu-id="1baa7-141">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="1baa7-141">Linux or Windows*</span></span> |<span data-ttu-id="1baa7-142">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="1baa7-142">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="1baa7-143">Nástroje HDInsight pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1baa7-143">HDInsight tools for Visual Studio</span></span>](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |<span data-ttu-id="1baa7-144">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-144">✔</span></span> |<span data-ttu-id="1baa7-145">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="1baa7-145">Linux or Windows*</span></span> |<span data-ttu-id="1baa7-146">Windows</span><span class="sxs-lookup"><span data-stu-id="1baa7-146">Windows</span></span> |
| [<span data-ttu-id="1baa7-147">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="1baa7-147">Windows PowerShell</span></span>](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |<span data-ttu-id="1baa7-148">✔</span><span class="sxs-lookup"><span data-stu-id="1baa7-148">✔</span></span> |<span data-ttu-id="1baa7-149">Linux nebo Windows *</span><span class="sxs-lookup"><span data-stu-id="1baa7-149">Linux or Windows*</span></span> |<span data-ttu-id="1baa7-150">Windows</span><span class="sxs-lookup"><span data-stu-id="1baa7-150">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="1baa7-151">\*Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="1baa7-151">\* Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1baa7-152">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1baa7-152">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="1baa7-153">Pokud používáte cluster HDInsight se systémem Windows, můžete použít hello [dotazu konzoly](hdinsight-hadoop-use-hive-query-console.md) z prohlížeče nebo [vzdálené plochy](hdinsight-hadoop-use-hive-remote-desktop.md) toorun dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-153">If you are using a Windows-based HDInsight cluster, you can use hello [Query console](hdinsight-hadoop-use-hive-query-console.md) from your browser or [Remote Desktop](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive queries.</span></span>

## <a name="hiveql-language-reference"></a><span data-ttu-id="1baa7-154">Referenční dokumentace jazyka HiveQL</span><span class="sxs-lookup"><span data-stu-id="1baa7-154">HiveQL language reference</span></span>

<span data-ttu-id="1baa7-155">Referenční dokumentace jazyka HiveQL je k dispozici v hello [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span><span class="sxs-lookup"><span data-stu-id="1baa7-155">HiveQL language reference is available in hello [language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).</span></span>

## <a name="hive-and-data-structure"></a><span data-ttu-id="1baa7-156">Hive a datové struktury</span><span class="sxs-lookup"><span data-stu-id="1baa7-156">Hive and data structure</span></span>

<span data-ttu-id="1baa7-157">Tom, jak toowork s strukturovaná a částečně strukturovaných dat plně chápe Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-157">Hive understands how toowork with structured and semi-structured data.</span></span> <span data-ttu-id="1baa7-158">Například textové soubory kde hello pole jsou oddělená konkrétní znaků.</span><span class="sxs-lookup"><span data-stu-id="1baa7-158">For example, text files where hello fields are delimited by specific characters.</span></span> <span data-ttu-id="1baa7-159">Následující příkaz HiveQL Hello vytvoří tabulku nad daty oddělených mezerami:</span><span class="sxs-lookup"><span data-stu-id="1baa7-159">hello following HiveQL statement creates a table over space-delimited data:</span></span>

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

<span data-ttu-id="1baa7-160">Hive podporuje i vlastní **serializátor/deserializers (SerDe)** složitý nebo nepravidelně strukturovaných dat.</span><span class="sxs-lookup"><span data-stu-id="1baa7-160">Hive also supports custom **serializer/deserializers (SerDe)** for complex or irregularly structured data.</span></span> <span data-ttu-id="1baa7-161">Další informace najdete v tématu hello [jak toouse vlastní SerDe JSON s HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-161">For more information, see hello [How toouse a custom JSON SerDe with HDInsight](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) document.</span></span>

<span data-ttu-id="1baa7-162">Další informace o souboru formátů podporovaných Hive naleznete v tématu hello [jazyk ručně (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span><span class="sxs-lookup"><span data-stu-id="1baa7-162">For more information on file formats supported by Hive, see hello [Language manual (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)</span></span>

## <a name="hive-internal-tables-vs-external-tables"></a><span data-ttu-id="1baa7-163">Interní tabulky vs externích tabulek Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-163">Hive internal tables vs external tables</span></span>

<span data-ttu-id="1baa7-164">Existují dva typy tabulek, které můžete vytvořit pomocí Hive:</span><span class="sxs-lookup"><span data-stu-id="1baa7-164">There are two types of tables that you can create with Hive:</span></span>

* <span data-ttu-id="1baa7-165">__Interní__: Data jsou uložena v hello Hive datového skladu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-165">__Internal__: Data is stored in hello Hive data warehouse.</span></span> <span data-ttu-id="1baa7-166">Hello datového skladu je umístěn v `/hive/warehouse/` na hello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1baa7-166">hello data warehouse is located at `/hive/warehouse/` on hello default storage for hello cluster.</span></span>

    <span data-ttu-id="1baa7-167">Použití interní tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="1baa7-167">Use internal tables when:</span></span>

    * <span data-ttu-id="1baa7-168">Data jsou dočasné.</span><span class="sxs-lookup"><span data-stu-id="1baa7-168">Data is temporary.</span></span>
    * <span data-ttu-id="1baa7-169">Chcete Hive toomanage hello životní cyklus hello tabulky a data.</span><span class="sxs-lookup"><span data-stu-id="1baa7-169">You want Hive toomanage hello lifecycle of hello table and data.</span></span>

* <span data-ttu-id="1baa7-170">__Externí__: Data jsou uložena mimo hello datového skladu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-170">__External__: Data is stored outside hello data warehouse.</span></span> <span data-ttu-id="1baa7-171">Hello data se uloží na jakékoli úložiště dostupné hello cluster.</span><span class="sxs-lookup"><span data-stu-id="1baa7-171">hello data can be stored on any storage accessible by hello cluster.</span></span>

    <span data-ttu-id="1baa7-172">Použijte externí tabulky, když:</span><span class="sxs-lookup"><span data-stu-id="1baa7-172">Use external tables when:</span></span>

    * <span data-ttu-id="1baa7-173">Hello dat se používá i mimo Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-173">hello data is also used outside of Hive.</span></span> <span data-ttu-id="1baa7-174">Například hello datové soubory jsou aktualizovány jiným procesem (které nenutí hello soubory.)</span><span class="sxs-lookup"><span data-stu-id="1baa7-174">For example, hello data files are updated by another process (that does not lock hello files.)</span></span>
    * <span data-ttu-id="1baa7-175">Data je tooremain v hello základní umístění, i po vyřazení hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="1baa7-175">Data needs tooremain in hello underlying location, even after dropping hello table.</span></span>
    * <span data-ttu-id="1baa7-176">Je nutné do vlastního umístění, jako je například účet jiné než výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="1baa7-176">You need a custom location, such as a non-default storage account.</span></span>
    * <span data-ttu-id="1baa7-177">Program než hive spravuje hello formát dat, umístění atd.</span><span class="sxs-lookup"><span data-stu-id="1baa7-177">A program other than hive manages hello data format, location, etc.</span></span>

<span data-ttu-id="1baa7-178">Další informace najdete v tématu hello [Hive interní a externí tabulky ÚVOD] [ cindygross-hive-tables] příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-178">For more information, see hello [Hive Internal and External Tables Intro][cindygross-hive-tables] blog post.</span></span>

## <a name="user-defined-functions-udf"></a><span data-ttu-id="1baa7-179">Uživatelem definované funkce (UDF)</span><span class="sxs-lookup"><span data-stu-id="1baa7-179">User-defined functions (UDF)</span></span>

<span data-ttu-id="1baa7-180">Hive lze také prostřednictvím rozšířit **uživatelsky definované funkce (UDF)**.</span><span class="sxs-lookup"><span data-stu-id="1baa7-180">Hive can also be extended through **user-defined functions (UDF)**.</span></span> <span data-ttu-id="1baa7-181">Uživatelem definovanou FUNKCI umožňuje tooimplement funkce nebo logiky, která není snadno modelován v HiveQL.</span><span class="sxs-lookup"><span data-stu-id="1baa7-181">A UDF allows you tooimplement functionality or logic that isn't easily modeled in HiveQL.</span></span> <span data-ttu-id="1baa7-182">Příklad použití UDF s Hive naleznete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="1baa7-182">For an example of using UDFs with Hive, see hello following documents:</span></span>

* [<span data-ttu-id="1baa7-183">Uživatelem definované funkce Java pomocí Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-183">Use a Java user-defined function with Hive</span></span>](hdinsight-hadoop-hive-java-udf.md)

* [<span data-ttu-id="1baa7-184">Uživatelem definované funkce Python pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="1baa7-184">Use a Python user-defined function with Hive and Pig</span></span>](hdinsight-python.md)

* [<span data-ttu-id="1baa7-185">C# uživatelem definované funkce pomocí Hive a Pig</span><span class="sxs-lookup"><span data-stu-id="1baa7-185">Use a C# user-defined function with Hive and Pig</span></span>](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [<span data-ttu-id="1baa7-186">Jak tooadd vlastní Hive uživatelem definované funkce tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="1baa7-186">How tooadd a custom Hive user-defined function tooHDInsight</span></span>](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [<span data-ttu-id="1baa7-187">Na příkladu Hive uživatelsky definované funkce tooconvert formátu data a času tooHive časové razítko</span><span class="sxs-lookup"><span data-stu-id="1baa7-187">An example Hive user-defined function tooconvert date/time formats tooHive timestamp</span></span>](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <span data-ttu-id="1baa7-188"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="1baa7-188"><a id="data"></a>Example data</span></span>

<span data-ttu-id="1baa7-189">Hive v HDInsight obsahuje předem načtený vnitřní tabulku s názvem `hivesampletable`.</span><span class="sxs-lookup"><span data-stu-id="1baa7-189">Hive on HDInsight comes pre-loaded with an internal table named `hivesampletable`.</span></span> <span data-ttu-id="1baa7-190">HDInsight také poskytuje příklad datových sad, které lze použít s Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-190">HDInsight also provides example data sets that can be used with Hive.</span></span> <span data-ttu-id="1baa7-191">Tyto sady dat se ukládají do hello `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="1baa7-191">These data sets are stored in hello `/example/data` and `/HdiSamples` directories.</span></span> <span data-ttu-id="1baa7-192">Tyto adresáře neexistují v hello výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="1baa7-192">These directories exist in hello default storage for your cluster.</span></span>

## <span data-ttu-id="1baa7-193"><a id="job"></a>Příklad dotazu Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-193"><a id="job"></a>Example Hive query</span></span>

<span data-ttu-id="1baa7-194">Hello následující sloupce projektu příkazy HiveQL do hello `/example/data/sample.log` souboru:</span><span class="sxs-lookup"><span data-stu-id="1baa7-194">hello following HiveQL statements project columns onto hello `/example/data/sample.log` file:</span></span>

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

<span data-ttu-id="1baa7-195">V předchozím příkladu hello provádět příkazy HiveQL hello hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="1baa7-195">In hello previous example, hello HiveQL statements perform hello following actions:</span></span>

* <span data-ttu-id="1baa7-196">`set hive.execution.engine=tez;`: Nastaví hello toouse spuštění modulu Tez.</span><span class="sxs-lookup"><span data-stu-id="1baa7-196">`set hive.execution.engine=tez;`: Sets hello execution engine toouse Tez.</span></span> <span data-ttu-id="1baa7-197">Pomocí Tez místo MapReduce můžete poskytnout zvýšení výkonu dotazů.</span><span class="sxs-lookup"><span data-stu-id="1baa7-197">Using Tez instead of MapReduce can provide an increase in query performance.</span></span> <span data-ttu-id="1baa7-198">Další informace o Tez naleznete v tématu hello [použití rozhraní Apache Tez pro zlepšení výkonu](#usetez) části.</span><span class="sxs-lookup"><span data-stu-id="1baa7-198">For more information on Tez, see hello [Use Apache Tez for improved performance](#usetez) section.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1baa7-199">Tento příkaz je jenom když používáte cluster HDInsight se systémem Windows je vyžadováno.</span><span class="sxs-lookup"><span data-stu-id="1baa7-199">This statement is only required when using a Windows-based HDInsight cluster.</span></span> <span data-ttu-id="1baa7-200">Tez je hello výchozí spouštěcí modul pro HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="1baa7-200">Tez is hello default execution engine for Linux-based HDInsight.</span></span>

* <span data-ttu-id="1baa7-201">`DROP TABLE`: Pokud hello tabulka již existuje, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="1baa7-201">`DROP TABLE`: If hello table already exists, delete it.</span></span>

* <span data-ttu-id="1baa7-202">`CREATE EXTERNAL TABLE`: Vytvoří novou **externí** tabulky v Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-202">`CREATE EXTERNAL TABLE`: Creates a new **external** table in Hive.</span></span> <span data-ttu-id="1baa7-203">Externí tabulky pouze uložit definici tabulky hello Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-203">External tables only store hello table definition in Hive.</span></span> <span data-ttu-id="1baa7-204">Hello data je ponechán v původním umístění hello a v původním formátu hello.</span><span class="sxs-lookup"><span data-stu-id="1baa7-204">hello data is left in hello original location and in hello original format.</span></span>

* <span data-ttu-id="1baa7-205">`ROW FORMAT`: Určuje způsob formátování hello data Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-205">`ROW FORMAT`: Tells Hive how hello data is formatted.</span></span> <span data-ttu-id="1baa7-206">V takovém případě hello pole v každém protokolu jsou oddělené mezerou.</span><span class="sxs-lookup"><span data-stu-id="1baa7-206">In this case, hello fields in each log are separated by a space.</span></span>

* <span data-ttu-id="1baa7-207">`STORED AS TEXTFILE LOCATION`: Informuje Hive kde hello data se ukládají (hello `example/data` adresáře) a která je uložena jako text.</span><span class="sxs-lookup"><span data-stu-id="1baa7-207">`STORED AS TEXTFILE LOCATION`: Tells Hive where hello data is stored (hello `example/data` directory) and that it is stored as text.</span></span> <span data-ttu-id="1baa7-208">Hello dat může být v jednom souboru nebo rozloženy více souborů v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="1baa7-208">hello data can be in one file or spread across multiple files within hello directory.</span></span>

* <span data-ttu-id="1baa7-209">`SELECT`: Vybere počet všech řádků kde hello sloupec **t4** obsahuje hodnotu hello **[Chyba]**.</span><span class="sxs-lookup"><span data-stu-id="1baa7-209">`SELECT`: Selects a count of all rows where hello column **t4** contains hello value **[ERROR]**.</span></span> <span data-ttu-id="1baa7-210">Tento příkaz vrátí hodnotu **3** vzhledem k tomu, že existují tři řádky, které obsahují tuto hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-210">This statement returns a value of **3** because there are three rows that contain this value.</span></span>

* <span data-ttu-id="1baa7-211">`INPUT__FILE__NAME LIKE '%.log'`-Hive pokusí tooapply hello schématu tooall soubory v adresáři hello.</span><span class="sxs-lookup"><span data-stu-id="1baa7-211">`INPUT__FILE__NAME LIKE '%.log'` - Hive attempts tooapply hello schema tooall files in hello directory.</span></span> <span data-ttu-id="1baa7-212">V takovém případě hello adresář obsahuje soubory, které neodpovídají schématu hello.</span><span class="sxs-lookup"><span data-stu-id="1baa7-212">In this case, hello directory contains files that do not match hello schema.</span></span> <span data-ttu-id="1baa7-213">tooprevent paměti data ve výsledcích hello, tento příkaz informuje Hive, jsme by měl vrátit pouze data ze souborů končící na. log.</span><span class="sxs-lookup"><span data-stu-id="1baa7-213">tooprevent garbage data in hello results, this statement tells Hive that we should only return data from files ending in .log.</span></span>

> [!NOTE]
> <span data-ttu-id="1baa7-214">Externí tabulky by měl použít, pokud očekáváte, že hello základní data toobe aktualizovat externího zdroje.</span><span class="sxs-lookup"><span data-stu-id="1baa7-214">External tables should be used when you expect hello underlying data toobe updated by an external source.</span></span> <span data-ttu-id="1baa7-215">Například procesu nahrávání automatizované dat, nebo operaci MapReduce.</span><span class="sxs-lookup"><span data-stu-id="1baa7-215">For example, an automated data upload process, or MapReduce operation.</span></span>
>
> <span data-ttu-id="1baa7-216">Vyřazení externí tabulku nemá **není** odstranit hello data, odstraní pouze hello definici tabulky.</span><span class="sxs-lookup"><span data-stu-id="1baa7-216">Dropping an external table does **not** delete hello data, it only deletes hello table definition.</span></span>

<span data-ttu-id="1baa7-217">toocreate **interní** místo externí tabulky, použijte následující HiveQL hello:</span><span class="sxs-lookup"><span data-stu-id="1baa7-217">toocreate an **internal** table instead of external, use hello following HiveQL:</span></span>

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

<span data-ttu-id="1baa7-218">Tyto příkazy provádět hello následující akce:</span><span class="sxs-lookup"><span data-stu-id="1baa7-218">These statements perform hello following actions:</span></span>

* <span data-ttu-id="1baa7-219">`CREATE TABLE IF NOT EXISTS`: Pokud hello tabulka neexistuje, vytvořte ho.</span><span class="sxs-lookup"><span data-stu-id="1baa7-219">`CREATE TABLE IF NOT EXISTS`: If hello table does not exist, create it.</span></span> <span data-ttu-id="1baa7-220">Protože hello **externí** – klíčové slovo není, tento příkaz vytvoří interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="1baa7-220">Because hello **EXTERNAL** keyword is not used, this statement creates an internal table.</span></span> <span data-ttu-id="1baa7-221">Tabulka Hello je uložena v datovém skladu Hive hello a je zcela spravuje Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-221">hello table is stored in hello Hive data warehouse and is managed completely by Hive.</span></span>

* <span data-ttu-id="1baa7-222">`STORED AS ORC`: Ukládá hello data ve formátu optimalizované řádek sloupcovém (ORC).</span><span class="sxs-lookup"><span data-stu-id="1baa7-222">`STORED AS ORC`: Stores hello data in Optimized Row Columnar (ORC) format.</span></span> <span data-ttu-id="1baa7-223">ORC je vysoce optimalizovaný a efektivní formát pro ukládání dat Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-223">ORC is a highly optimized and efficient format for storing Hive data.</span></span>

* <span data-ttu-id="1baa7-224">`INSERT OVERWRITE ... SELECT`: Vybere řádky z hello **log4jLogs** tabulku, která obsahuje **[Chyba]**, a poté vloží hello data do hello **errorLogs** tabulky.</span><span class="sxs-lookup"><span data-stu-id="1baa7-224">`INSERT OVERWRITE ... SELECT`: Selects rows from hello **log4jLogs** table that contains **[ERROR]**, and then inserts hello data into hello **errorLogs** table.</span></span>

> [!NOTE]
> <span data-ttu-id="1baa7-225">Na rozdíl od externích tabulek se odstranit interní tabulku také odstraní hello základní data.</span><span class="sxs-lookup"><span data-stu-id="1baa7-225">Unlike external tables, dropping an internal table also deletes hello underlying data.</span></span>

## <a name="improve-hive-query-performance"></a><span data-ttu-id="1baa7-226">Zlepšení výkonu dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="1baa7-226">Improve Hive query performance</span></span>

### <span data-ttu-id="1baa7-227"><a id="usetez"></a>Apache Tez</span><span class="sxs-lookup"><span data-stu-id="1baa7-227"><a id="usetez"></a>Apache Tez</span></span>

<span data-ttu-id="1baa7-228">[Apache Tez](http://tez.apache.org) je rozhraní, které umožňuje data náročné aplikace, například Hive, toorun mnohem efektivněji ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="1baa7-228">[Apache Tez](http://tez.apache.org) is a framework that allows data intensive applications, such as Hive, toorun much more efficiently at scale.</span></span> <span data-ttu-id="1baa7-229">Ve výchozím nastavení pro clustery HDInsight se systémem Linux je povolené tez.</span><span class="sxs-lookup"><span data-stu-id="1baa7-229">Tez is enabled by default for Linux-based HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="1baa7-230">Tez není momentálně vypnuto ve výchozím nastavení pro clustery HDInsight se systémem Windows a musí být povolena.</span><span class="sxs-lookup"><span data-stu-id="1baa7-230">Tez is currently off by default for Windows-based HDInsight clusters and it must be enabled.</span></span> <span data-ttu-id="1baa7-231">tootake výhod Tez hello následující hodnota musí být nastavena pro dotaz Hive:</span><span class="sxs-lookup"><span data-stu-id="1baa7-231">tootake advantage of Tez, hello following value must be set for a Hive query:</span></span>
>
> `set hive.execution.engine=tez;`
>
> <span data-ttu-id="1baa7-232">Tez je hello výchozí modul pro clustery HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="1baa7-232">Tez is hello default engine for Linux-based HDInsight clusters.</span></span>

<span data-ttu-id="1baa7-233">Hello [Hive na dokumentech návrhu Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) obsahuje podrobnosti o hello volby implementace a vyladění konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1baa7-233">hello [Hive on Tez design documents](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) contains details about hello implementation choices and tuning configurations.</span></span>

<span data-ttu-id="1baa7-234">tooaid při ladění úloh proběhla za použití Tez, HDInsight poskytuje následující uživatelská webové rozhraní, které vám umožňují tooview podrobnosti o úlohách Tez hello:</span><span class="sxs-lookup"><span data-stu-id="1baa7-234">tooaid in debugging jobs ran using Tez, HDInsight provides hello following web UIs that allow you tooview details of Tez jobs:</span></span>

* [<span data-ttu-id="1baa7-235">Použití hello zobrazení Ambari Tez na HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="1baa7-235">Use hello Ambari Tez view on Linux-based HDInsight</span></span>](hdinsight-debug-ambari-tez-view.md)

* [<span data-ttu-id="1baa7-236">Použití hello Tez uživatelského rozhraní na HDInsight se systémem Windows</span><span class="sxs-lookup"><span data-stu-id="1baa7-236">Use hello Tez UI on Windows-based HDInsight</span></span>](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a><span data-ttu-id="1baa7-237">Nízká latence analytického zpracování (LLAP)</span><span class="sxs-lookup"><span data-stu-id="1baa7-237">Low Latency Analytical Processing (LLAP)</span></span>

<span data-ttu-id="1baa7-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (někdy označované jako Live dlouhé a proces) je nová funkce v podregistru 2.0, který umožňuje ukládání do mezipaměti v paměti dotazů.</span><span class="sxs-lookup"><span data-stu-id="1baa7-238">[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (sometimes known as Live Long and Process) is a new feature in Hive 2.0 that allows in-memory caching of queries.</span></span> <span data-ttu-id="1baa7-239">LLAP umožňuje mnohem rychlejší, až dotazů Hive příliš[26 x rychlejší než Hive 1.x v některých případech](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span><span class="sxs-lookup"><span data-stu-id="1baa7-239">LLAP makes Hive queries much faster, up too[26x faster than Hive 1.x in some cases](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).</span></span>

<span data-ttu-id="1baa7-240">HDInsight poskytuje LLAP v hello interaktivní Hive typ clusteru.</span><span class="sxs-lookup"><span data-stu-id="1baa7-240">HDInsight provides LLAP in hello Interactive Hive cluster type.</span></span> <span data-ttu-id="1baa7-241">Další informace najdete v tématu hello [začínat interaktivní Hive](hdinsight-hadoop-use-interactive-hive.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="1baa7-241">For more information, see hello [Start with Interactive Hive](hdinsight-hadoop-use-interactive-hive.md) document.</span></span>

## <a name="hive-jobs-and-sql-server-integration-services"></a><span data-ttu-id="1baa7-242">Úlohy Hive a SQL Server Integration Services</span><span class="sxs-lookup"><span data-stu-id="1baa7-242">Hive jobs and SQL Server Integration Services</span></span>

<span data-ttu-id="1baa7-243">Pomocí integrace služby SSIS (SQL Server) toorun úlohy Hive.</span><span class="sxs-lookup"><span data-stu-id="1baa7-243">You can use SQL Server Integration Services (SSIS) toorun a Hive job.</span></span> <span data-ttu-id="1baa7-244">Hello Azure Feature Pack pro službu SSIS poskytuje hello následující součásti, které pracují se úlohy Hive v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1baa7-244">hello Azure Feature Pack for SSIS provides hello following components that work with Hive jobs on HDInsight.</span></span>

* <span data-ttu-id="1baa7-245">[Úloha Azure HDInsight Hive][hivetask]</span><span class="sxs-lookup"><span data-stu-id="1baa7-245">[Azure HDInsight Hive Task][hivetask]</span></span>

* <span data-ttu-id="1baa7-246">[Správce připojení k Azure předplatného][connectionmanager]</span><span class="sxs-lookup"><span data-stu-id="1baa7-246">[Azure Subscription Connection Manager][connectionmanager]</span></span>

<span data-ttu-id="1baa7-247">Další informace o hello Azure Feature Pack pro služby SSIS [sem][ssispack].</span><span class="sxs-lookup"><span data-stu-id="1baa7-247">Learn more about hello Azure Feature Pack for SSIS [here][ssispack].</span></span>

## <span data-ttu-id="1baa7-248"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="1baa7-248"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="1baa7-249">Teď, když jste se naučili novinky Hive a jak toouse ho s Hadoop v HDInsight, hello použijte následující odkazy tooexplore jiné způsoby toowork s Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1baa7-249">Now that you've learned what Hive is and how toouse it with Hadoop in HDInsight, use hello following links tooexplore other ways toowork with Azure HDInsight.</span></span>

* <span data-ttu-id="1baa7-250">[Nahrání dat tooHDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="1baa7-250">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="1baa7-251">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="1baa7-251">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="1baa7-252">[Použijte úlohy MapReduce s HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="1baa7-252">[Use MapReduce jobs with HDInsight][hdinsight-use-mapreduce]</span></span>

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
