---
title: aaaMapReduce s Hadoop v HDInsight | Microsoft Docs
description: "Zjistěte, jak toorun MapReduce úlohy systému Hadoop v clusterech HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0cf7ad0e6769e678be64f9e4ec8ed7a214ab7af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a><span data-ttu-id="7ec09-103">Používání nástroje MapReduce v Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-103">Use MapReduce in Hadoop on HDInsight</span></span>

<span data-ttu-id="7ec09-104">Zjistěte, jak toorun MapReduce úlohy v clusterech prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7ec09-104">Learn how toorun MapReduce jobs on HDInsight clusters.</span></span> <span data-ttu-id="7ec09-105">Použijte následující tabulku toodiscover hello různé způsoby, že lze použít MapReduce s HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="7ec09-105">Use hello following table toodiscover hello various ways that MapReduce can be used with HDInsight:</span></span>

| <span data-ttu-id="7ec09-106">**Použít**...</span><span class="sxs-lookup"><span data-stu-id="7ec09-106">**Use this**...</span></span> | <span data-ttu-id="7ec09-107">**.. .toodo to**</span><span class="sxs-lookup"><span data-stu-id="7ec09-107">**...toodo this**</span></span> | <span data-ttu-id="7ec09-108">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="7ec09-108">...with this **cluster operating system**</span></span> | <span data-ttu-id="7ec09-109">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="7ec09-109">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="7ec09-110">SSH</span><span class="sxs-lookup"><span data-stu-id="7ec09-110">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="7ec09-111">Použijte příkaz Hadoop hello prostřednictvím **SSH**</span><span class="sxs-lookup"><span data-stu-id="7ec09-111">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="7ec09-112">Linux</span><span class="sxs-lookup"><span data-stu-id="7ec09-112">Linux</span></span> |<span data-ttu-id="7ec09-113">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-113">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="7ec09-114">REST</span><span class="sxs-lookup"><span data-stu-id="7ec09-114">REST</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="7ec09-115">Odeslat úlohu hello vzdáleně pomocí **REST** (příklady použití cURL)</span><span class="sxs-lookup"><span data-stu-id="7ec09-115">Submit hello job remotely by using **REST** (examples use cURL)</span></span> |<span data-ttu-id="7ec09-116">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-116">Linux or Windows</span></span> |<span data-ttu-id="7ec09-117">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="7ec09-118">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ec09-118">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="7ec09-119">Odeslat úlohu hello vzdáleně pomocí **prostředí Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="7ec09-119">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="7ec09-120">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-120">Linux or Windows</span></span> |<span data-ttu-id="7ec09-121">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-121">Windows</span></span> |
| <span data-ttu-id="7ec09-122">[Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="7ec09-122">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="7ec09-123">Použijte příkaz Hadoop hello prostřednictvím **vzdálené plochy**</span><span class="sxs-lookup"><span data-stu-id="7ec09-123">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="7ec09-124">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-124">Windows</span></span> |<span data-ttu-id="7ec09-125">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-125">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="7ec09-126">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7ec09-126">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7ec09-127">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7ec09-127">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="7ec09-128"><a id="whatis"></a>Co je MapReduce</span><span class="sxs-lookup"><span data-stu-id="7ec09-128"><a id="whatis"></a>What is MapReduce</span></span>

<span data-ttu-id="7ec09-129">Hadoop MapReduce je framework software pro zápis úlohy, které zpracovávají velké množství dat.</span><span class="sxs-lookup"><span data-stu-id="7ec09-129">Hadoop MapReduce is a software framework for writing jobs that process vast amounts of data.</span></span> <span data-ttu-id="7ec09-130">Vstupní data je rozdělená do nezávislé bloků, které se pak zpracovávají paralelně mezi hello uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="7ec09-130">Input data is split into independent chunks, which are then processed in parallel across hello nodes in your cluster.</span></span> <span data-ttu-id="7ec09-131">Úloha MapReduce se skládá z dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="7ec09-131">A MapReduce job consists of two functions:</span></span>

* <span data-ttu-id="7ec09-132">**Mapovač**: spotřebovává vstupních dat, analyzuje (obvykle s filtrování a řazení operations) a vysílá řazených kolekcí členů (páry klíč hodnota)</span><span class="sxs-lookup"><span data-stu-id="7ec09-132">**Mapper**: Consumes input data, analyzes it (usually with filter and sorting operations), and emits tuples (key-value pairs)</span></span>

* <span data-ttu-id="7ec09-133">**Reduktorem**: využívá řazených kolekcí členů vysílaných hello Mapper a provádí operaci souhrn, která vytvoří výsledek menší, kombinované z hello Mapovač dat</span><span class="sxs-lookup"><span data-stu-id="7ec09-133">**Reducer**: Consumes tuples emitted by hello Mapper and performs a summary operation that creates a smaller, combined result from hello Mapper data</span></span>

<span data-ttu-id="7ec09-134">V příkladu úlohy MapReduce počet základní word je znázorněno v následujícím diagramu hello:</span><span class="sxs-lookup"><span data-stu-id="7ec09-134">A basic word count MapReduce job example is illustrated in hello following diagram:</span></span>

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

<span data-ttu-id="7ec09-136">výstup Hello této úlohy je počet kolikrát jednotlivých slov došlo k chybě v textu hello, který byl analyzován.</span><span class="sxs-lookup"><span data-stu-id="7ec09-136">hello output of this job is a count of how many times each word occurred in hello text that was analyzed.</span></span>

* <span data-ttu-id="7ec09-137">Mapovač Hello používá každý řádek ze vstupního textu hello jako vstup a dělí na slova.</span><span class="sxs-lookup"><span data-stu-id="7ec09-137">hello mapper takes each line from hello input text as an input and breaks it into words.</span></span> <span data-ttu-id="7ec09-138">Ho vysílá klíč/hodnota pár pokaždé, když slovo dojde v aplikaci hello word následuje 1.</span><span class="sxs-lookup"><span data-stu-id="7ec09-138">It emits a key/value pair each time a word occurs of hello word is followed by a 1.</span></span> <span data-ttu-id="7ec09-139">výstup Hello je řazen před odesláním tooreducer.</span><span class="sxs-lookup"><span data-stu-id="7ec09-139">hello output is sorted before sending it tooreducer.</span></span>
* <span data-ttu-id="7ec09-140">Hello reduktorem sčítá tyto počty jednotlivých pro každou aplikaci word a vysílá pár jeden klíč/hodnota obsahující slovo hello a hello součet jeho výskytů.</span><span class="sxs-lookup"><span data-stu-id="7ec09-140">hello reducer sums these individual counts for each word and emits a single key/value pair that contains hello word followed by hello sum of its occurrences.</span></span>

<span data-ttu-id="7ec09-141">MapReduce můžou se implementovat v různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="7ec09-141">MapReduce can be implemented in various languages.</span></span> <span data-ttu-id="7ec09-142">Java hello nejběžnější implementace a slouží pro účely ukázky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7ec09-142">Java is hello most common implementation, and is used for demonstration purposes in this document.</span></span>

## <a name="development-languages"></a><span data-ttu-id="7ec09-143">Programovacích jazyků</span><span class="sxs-lookup"><span data-stu-id="7ec09-143">Development languages</span></span>

<span data-ttu-id="7ec09-144">Jazyky nebo rozhraní, které jsou založené na jazyce Java a hello virtuální počítač Java můžete spustili přímo jako úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="7ec09-144">Languages or frameworks that are based on Java and hello Java Virtual Machine can be ran directly as a MapReduce job.</span></span> <span data-ttu-id="7ec09-145">Hello příkladu v tomto dokumentu je aplikace Java MapReduce.</span><span class="sxs-lookup"><span data-stu-id="7ec09-145">hello example used in this document is a Java MapReduce application.</span></span> <span data-ttu-id="7ec09-146">Jazyky Java jiného typu, jako je C#, Python nebo samostatné spustitelné soubory, musíte použít, streamování Hadoop.</span><span class="sxs-lookup"><span data-stu-id="7ec09-146">Non-Java languages, such as C#, Python, or standalone executables, must use Hadoop streaming.</span></span>

<span data-ttu-id="7ec09-147">Streamování Hadoop komunikuje s hello mapper a reduktorem pomocí standardního vstupu a výstupu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="7ec09-147">Hadoop streaming communicates with hello mapper and reducer over STDIN and STDOUT.</span></span> <span data-ttu-id="7ec09-148">Mapovač Hello reduktorem číst data řádku současně z stdin – a zápisu tooSTDOUT výstup hello.</span><span class="sxs-lookup"><span data-stu-id="7ec09-148">hello mapper and reducer read data a line at a time from STDIN, and write hello output tooSTDOUT.</span></span> <span data-ttu-id="7ec09-149">Každý řádek čtení nebo vysílaných hello mapper a reduktorem musí být ve formátu hello dvojic klíč hodnota oddělená tabulátorem:</span><span class="sxs-lookup"><span data-stu-id="7ec09-149">Each line read or emitted by hello mapper and reducer must be in hello format of a key/value pair, delimited by a tab character:</span></span>

    [key]/t[value]

<span data-ttu-id="7ec09-150">Další informace najdete v tématu [streamování Hadoop](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span><span class="sxs-lookup"><span data-stu-id="7ec09-150">For more information, see [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span></span>

<span data-ttu-id="7ec09-151">Příklady použití streamování s HDInsight Hadoop najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="7ec09-151">For examples of using Hadoop streaming with HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="7ec09-152">Vývoj úloh MapReduce C#</span><span class="sxs-lookup"><span data-stu-id="7ec09-152">Develop C# MapReduce jobs</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="7ec09-153">Vývoj úloh Python MapReduce</span><span class="sxs-lookup"><span data-stu-id="7ec09-153">Develop Python MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="7ec09-154"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="7ec09-154"><a id="data"></a>Example data</span></span>

<span data-ttu-id="7ec09-155">HDInsight poskytuje různé příklad datových sad, které jsou uloženy v hello `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="7ec09-155">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directory.</span></span> <span data-ttu-id="7ec09-156">Tyto adresáře jsou hello výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="7ec09-156">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="7ec09-157">V tomto dokumentu používáme hello `/example/data/gutenberg/davinci.txt` souboru.</span><span class="sxs-lookup"><span data-stu-id="7ec09-157">In this document, we use hello `/example/data/gutenberg/davinci.txt` file.</span></span> <span data-ttu-id="7ec09-158">Tento soubor obsahuje poznámkových bloků hello z Leonardo Da Vinci.</span><span class="sxs-lookup"><span data-stu-id="7ec09-158">This file contains hello notebooks of Leonardo Da Vinci.</span></span>

## <span data-ttu-id="7ec09-159"><a id="job"></a>Příklad MapReduce</span><span class="sxs-lookup"><span data-stu-id="7ec09-159"><a id="job"></a>Example MapReduce</span></span>

<span data-ttu-id="7ec09-160">Příklad počet dokumentů aplikace word MapReduce je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7ec09-160">An example MapReduce word count application is included with your HDInsight cluster.</span></span> <span data-ttu-id="7ec09-161">V tomto příkladu je umístěn v `/example/jars/hadoop-mapreduce-examples.jar` na hello výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="7ec09-161">This example is located at `/example/jars/hadoop-mapreduce-examples.jar` on hello default storage for your cluster.</span></span>

<span data-ttu-id="7ec09-162">Následující kód v jazyce Java Hello je hello zdroj hello MapReduce aplikace obsažené v hello `hadoop-mapreduce-examples.jar` souboru:</span><span class="sxs-lookup"><span data-stu-id="7ec09-162">hello following Java code is hello source of hello MapReduce application contained in hello `hadoop-mapreduce-examples.jar` file:</span></span>

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

<span data-ttu-id="7ec09-163">Vaše vlastní aplikace MapReduce, najdete pokyny toowrite hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="7ec09-163">For instructions toowrite your own MapReduce applications, see hello following documents:</span></span>

* [<span data-ttu-id="7ec09-164">Vývoj aplikací Java MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-164">Develop Java MapReduce applications for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="7ec09-165">Vývoj aplikací Python MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-165">Develop Python MapReduce applications for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="7ec09-166"><a id="run"></a>Spustit hello MapReduce</span><span class="sxs-lookup"><span data-stu-id="7ec09-166"><a id="run"></a>Run hello MapReduce</span></span>

<span data-ttu-id="7ec09-167">HDInsight HiveQL úlohy můžete spustit pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="7ec09-167">HDInsight can run HiveQL jobs by using various methods.</span></span> <span data-ttu-id="7ec09-168">Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="7ec09-168">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="7ec09-169">**Použít**...</span><span class="sxs-lookup"><span data-stu-id="7ec09-169">**Use this**...</span></span> | <span data-ttu-id="7ec09-170">**.. .toodo to**</span><span class="sxs-lookup"><span data-stu-id="7ec09-170">**...toodo this**</span></span> | <span data-ttu-id="7ec09-171">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="7ec09-171">...with this **cluster operating system**</span></span> | <span data-ttu-id="7ec09-172">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="7ec09-172">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="7ec09-173">SSH</span><span class="sxs-lookup"><span data-stu-id="7ec09-173">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="7ec09-174">Použijte příkaz Hadoop hello prostřednictvím **SSH**</span><span class="sxs-lookup"><span data-stu-id="7ec09-174">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="7ec09-175">Linux</span><span class="sxs-lookup"><span data-stu-id="7ec09-175">Linux</span></span> |<span data-ttu-id="7ec09-176">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-176">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="7ec09-177">Curl</span><span class="sxs-lookup"><span data-stu-id="7ec09-177">Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="7ec09-178">Odeslat úlohu hello vzdáleně pomocí **REST**</span><span class="sxs-lookup"><span data-stu-id="7ec09-178">Submit hello job remotely by using **REST**</span></span> |<span data-ttu-id="7ec09-179">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-179">Linux or Windows</span></span> |<span data-ttu-id="7ec09-180">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="7ec09-181">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="7ec09-181">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="7ec09-182">Odeslat úlohu hello vzdáleně pomocí **prostředí Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="7ec09-182">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="7ec09-183">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-183">Linux or Windows</span></span> |<span data-ttu-id="7ec09-184">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-184">Windows</span></span> |
| <span data-ttu-id="7ec09-185">[Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="7ec09-185">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="7ec09-186">Použijte příkaz Hadoop hello prostřednictvím **vzdálené plochy**</span><span class="sxs-lookup"><span data-stu-id="7ec09-186">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="7ec09-187">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-187">Windows</span></span> |<span data-ttu-id="7ec09-188">Windows</span><span class="sxs-lookup"><span data-stu-id="7ec09-188">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="7ec09-189">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="7ec09-189">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7ec09-190">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7ec09-190">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="7ec09-191"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="7ec09-191"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="7ec09-192">toolearn Další informace o práci s daty v HDInsight, najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="7ec09-192">toolearn more about working with data in HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="7ec09-193">Vývoj aplikací Java MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-193">Develop Java MapReduce programs for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="7ec09-194">Vývoj Python streamování MapReduce programy pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-194">Develop Python streaming MapReduce programs for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

* [<span data-ttu-id="7ec09-195">Vývoj úloh paření MapReduce s Apache Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="7ec09-195">Develop Scalding MapReduce jobs with Apache Hadoop on HDInsight</span></span>](hdinsight-hadoop-mapreduce-scalding.md)

* <span data-ttu-id="7ec09-196">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="7ec09-196">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>

* <span data-ttu-id="7ec09-197">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="7ec09-197">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
