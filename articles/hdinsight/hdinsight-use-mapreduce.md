---
title: MapReduce s Hadoop v HDInsight | Microsoft Docs
description: "Informace o spouštění úloh MapReduce systému Hadoop v clusterech HDInsight."
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
ms.openlocfilehash: df8ac578a56de72df667b1fa7f90f981c79d9999
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a><span data-ttu-id="e6c35-103">Používání nástroje MapReduce v Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-103">Use MapReduce in Hadoop on HDInsight</span></span>

<span data-ttu-id="e6c35-104">Informace o spouštění úloh MapReduce clustery prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e6c35-104">Learn how to run MapReduce jobs on HDInsight clusters.</span></span> <span data-ttu-id="e6c35-105">Následující tabulku použijte ke zjištění různých způsobů, jak lze MapReduce s HDInsight:</span><span class="sxs-lookup"><span data-stu-id="e6c35-105">Use the following table to discover the various ways that MapReduce can be used with HDInsight:</span></span>

| <span data-ttu-id="e6c35-106">**Použít**...</span><span class="sxs-lookup"><span data-stu-id="e6c35-106">**Use this**...</span></span> | <span data-ttu-id="e6c35-107">**.. .a tomu**</span><span class="sxs-lookup"><span data-stu-id="e6c35-107">**...to do this**</span></span> | <span data-ttu-id="e6c35-108">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="e6c35-108">...with this **cluster operating system**</span></span> | <span data-ttu-id="e6c35-109">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="e6c35-109">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="e6c35-110">SSH</span><span class="sxs-lookup"><span data-stu-id="e6c35-110">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="e6c35-111">Použijte příkaz Hadoop prostřednictvím **SSH**</span><span class="sxs-lookup"><span data-stu-id="e6c35-111">Use the Hadoop command through **SSH**</span></span> |<span data-ttu-id="e6c35-112">Linux</span><span class="sxs-lookup"><span data-stu-id="e6c35-112">Linux</span></span> |<span data-ttu-id="e6c35-113">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-113">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="e6c35-114">REST</span><span class="sxs-lookup"><span data-stu-id="e6c35-114">REST</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="e6c35-115">Odeslání úlohy vzdáleně pomocí **REST** (příklady použití cURL)</span><span class="sxs-lookup"><span data-stu-id="e6c35-115">Submit the job remotely by using **REST** (examples use cURL)</span></span> |<span data-ttu-id="e6c35-116">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-116">Linux or Windows</span></span> |<span data-ttu-id="e6c35-117">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="e6c35-118">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6c35-118">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="e6c35-119">Odeslání úlohy vzdáleně pomocí **prostředí Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e6c35-119">Submit the job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="e6c35-120">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-120">Linux or Windows</span></span> |<span data-ttu-id="e6c35-121">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-121">Windows</span></span> |
| <span data-ttu-id="e6c35-122">[Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="e6c35-122">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="e6c35-123">Použijte příkaz Hadoop prostřednictvím **vzdálené plochy**</span><span class="sxs-lookup"><span data-stu-id="e6c35-123">Use the Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="e6c35-124">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-124">Windows</span></span> |<span data-ttu-id="e6c35-125">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-125">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="e6c35-126">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="e6c35-126">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e6c35-127">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e6c35-127">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="e6c35-128"><a id="whatis"></a>Co je MapReduce</span><span class="sxs-lookup"><span data-stu-id="e6c35-128"><a id="whatis"></a>What is MapReduce</span></span>

<span data-ttu-id="e6c35-129">Hadoop MapReduce je framework software pro zápis úlohy, které zpracovávají velké množství dat.</span><span class="sxs-lookup"><span data-stu-id="e6c35-129">Hadoop MapReduce is a software framework for writing jobs that process vast amounts of data.</span></span> <span data-ttu-id="e6c35-130">Vstupní data je rozdělená do nezávislé bloků, které se pak zpracovávají paralelně mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="e6c35-130">Input data is split into independent chunks, which are then processed in parallel across the nodes in your cluster.</span></span> <span data-ttu-id="e6c35-131">Úloha MapReduce se skládá z dvě funkce:</span><span class="sxs-lookup"><span data-stu-id="e6c35-131">A MapReduce job consists of two functions:</span></span>

* <span data-ttu-id="e6c35-132">**Mapovač**: spotřebovává vstupních dat, analyzuje (obvykle s filtrování a řazení operations) a vysílá řazených kolekcí členů (páry klíč hodnota)</span><span class="sxs-lookup"><span data-stu-id="e6c35-132">**Mapper**: Consumes input data, analyzes it (usually with filter and sorting operations), and emits tuples (key-value pairs)</span></span>

* <span data-ttu-id="e6c35-133">**Reduktorem**: využívá řazených kolekcí členů vygenerované mapovačem a provádí operaci souhrn, která vytvoří výsledek menší, kombinované z Mapovač dat</span><span class="sxs-lookup"><span data-stu-id="e6c35-133">**Reducer**: Consumes tuples emitted by the Mapper and performs a summary operation that creates a smaller, combined result from the Mapper data</span></span>

<span data-ttu-id="e6c35-134">V příkladu úlohy MapReduce počet základní word je znázorněno v následujícím diagramu:</span><span class="sxs-lookup"><span data-stu-id="e6c35-134">A basic word count MapReduce job example is illustrated in the following diagram:</span></span>

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

<span data-ttu-id="e6c35-136">Výstup této úlohy je počet kolikrát jednotlivých slov došlo k chybě v textu, který byl analyzován.</span><span class="sxs-lookup"><span data-stu-id="e6c35-136">The output of this job is a count of how many times each word occurred in the text that was analyzed.</span></span>

* <span data-ttu-id="e6c35-137">Mapper trvá každý řádek ze vstupního textu jako vstup a dělí na slova.</span><span class="sxs-lookup"><span data-stu-id="e6c35-137">The mapper takes each line from the input text as an input and breaks it into words.</span></span> <span data-ttu-id="e6c35-138">Ho vysílá klíč/hodnota pár pokaždé, když dojde k slovo aplikace word následuje 1.</span><span class="sxs-lookup"><span data-stu-id="e6c35-138">It emits a key/value pair each time a word occurs of the word is followed by a 1.</span></span> <span data-ttu-id="e6c35-139">Výstup je řazen před odesláním reduktorem.</span><span class="sxs-lookup"><span data-stu-id="e6c35-139">The output is sorted before sending it to reducer.</span></span>
* <span data-ttu-id="e6c35-140">Reduktorem sčítá tyto počty jednotlivých pro každou aplikaci word a vysílá pár jeden klíč/hodnota obsahující slovo a součet jeho výskytů.</span><span class="sxs-lookup"><span data-stu-id="e6c35-140">The reducer sums these individual counts for each word and emits a single key/value pair that contains the word followed by the sum of its occurrences.</span></span>

<span data-ttu-id="e6c35-141">MapReduce můžou se implementovat v různých jazycích.</span><span class="sxs-lookup"><span data-stu-id="e6c35-141">MapReduce can be implemented in various languages.</span></span> <span data-ttu-id="e6c35-142">Java nejběžnější implementace a slouží pro účely ukázky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="e6c35-142">Java is the most common implementation, and is used for demonstration purposes in this document.</span></span>

## <a name="development-languages"></a><span data-ttu-id="e6c35-143">Programovacích jazyků</span><span class="sxs-lookup"><span data-stu-id="e6c35-143">Development languages</span></span>

<span data-ttu-id="e6c35-144">Jazyky nebo rozhraní, které jsou založeny na jazyce Java a virtuální počítač Java můžete spustili přímo jako úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="e6c35-144">Languages or frameworks that are based on Java and the Java Virtual Machine can be ran directly as a MapReduce job.</span></span> <span data-ttu-id="e6c35-145">V příkladu v tomto dokumentu je aplikace Java MapReduce.</span><span class="sxs-lookup"><span data-stu-id="e6c35-145">The example used in this document is a Java MapReduce application.</span></span> <span data-ttu-id="e6c35-146">Jazyky Java jiného typu, jako je C#, Python nebo samostatné spustitelné soubory, musíte použít, streamování Hadoop.</span><span class="sxs-lookup"><span data-stu-id="e6c35-146">Non-Java languages, such as C#, Python, or standalone executables, must use Hadoop streaming.</span></span>

<span data-ttu-id="e6c35-147">Streamování Hadoop komunikuje s mapper a reduktorem pomocí standardního vstupu a výstupu STDOUT.</span><span class="sxs-lookup"><span data-stu-id="e6c35-147">Hadoop streaming communicates with the mapper and reducer over STDIN and STDOUT.</span></span> <span data-ttu-id="e6c35-148">Mapovač a reduktorem číst data řádku současně z stdin – a zapisovat výstup STDOUT.</span><span class="sxs-lookup"><span data-stu-id="e6c35-148">The mapper and reducer read data a line at a time from STDIN, and write the output to STDOUT.</span></span> <span data-ttu-id="e6c35-149">Každý řádek čtení nebo vysílaných mapper a reduktorem musí být ve formátu pár klíč hodnota oddělená tabulátorem:</span><span class="sxs-lookup"><span data-stu-id="e6c35-149">Each line read or emitted by the mapper and reducer must be in the format of a key/value pair, delimited by a tab character:</span></span>

    [key]/t[value]

<span data-ttu-id="e6c35-150">Další informace najdete v tématu [streamování Hadoop](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span><span class="sxs-lookup"><span data-stu-id="e6c35-150">For more information, see [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span></span>

<span data-ttu-id="e6c35-151">Příklady použití streamování s HDInsight Hadoop najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="e6c35-151">For examples of using Hadoop streaming with HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="e6c35-152">Vývoj úloh MapReduce C#</span><span class="sxs-lookup"><span data-stu-id="e6c35-152">Develop C# MapReduce jobs</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="e6c35-153">Vývoj úloh Python MapReduce</span><span class="sxs-lookup"><span data-stu-id="e6c35-153">Develop Python MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="e6c35-154"><a id="data"></a>Příklad dat</span><span class="sxs-lookup"><span data-stu-id="e6c35-154"><a id="data"></a>Example data</span></span>

<span data-ttu-id="e6c35-155">HDInsight poskytuje různé příklad datových sad, které jsou uloženy v `/example/data` a `/HdiSamples` adresáře.</span><span class="sxs-lookup"><span data-stu-id="e6c35-155">HDInsight provides various example data sets, which are stored in the `/example/data` and `/HdiSamples` directory.</span></span> <span data-ttu-id="e6c35-156">Tyto adresáře jsou ve výchozím nastavení úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="e6c35-156">These directories are in the default storage for your cluster.</span></span> <span data-ttu-id="e6c35-157">V tomto dokumentu budeme používat `/example/data/gutenberg/davinci.txt` souboru.</span><span class="sxs-lookup"><span data-stu-id="e6c35-157">In this document, we use the `/example/data/gutenberg/davinci.txt` file.</span></span> <span data-ttu-id="e6c35-158">Tento soubor obsahuje poznámkových bloků Leonardo Da Vinci.</span><span class="sxs-lookup"><span data-stu-id="e6c35-158">This file contains the notebooks of Leonardo Da Vinci.</span></span>

## <span data-ttu-id="e6c35-159"><a id="job"></a>Příklad MapReduce</span><span class="sxs-lookup"><span data-stu-id="e6c35-159"><a id="job"></a>Example MapReduce</span></span>

<span data-ttu-id="e6c35-160">Příklad počet dokumentů aplikace word MapReduce je součástí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="e6c35-160">An example MapReduce word count application is included with your HDInsight cluster.</span></span> <span data-ttu-id="e6c35-161">V tomto příkladu je umístěn v `/example/jars/hadoop-mapreduce-examples.jar` na výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="e6c35-161">This example is located at `/example/jars/hadoop-mapreduce-examples.jar` on the default storage for your cluster.</span></span>

<span data-ttu-id="e6c35-162">Následující kód Java je zdrojem MapReduce aplikace součástí `hadoop-mapreduce-examples.jar` souboru:</span><span class="sxs-lookup"><span data-stu-id="e6c35-162">The following Java code is the source of the MapReduce application contained in the `hadoop-mapreduce-examples.jar` file:</span></span>

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

<span data-ttu-id="e6c35-163">Pokyny pro zápis aplikace MapReduce najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="e6c35-163">For instructions to write your own MapReduce applications, see the following documents:</span></span>

* [<span data-ttu-id="e6c35-164">Vývoj aplikací Java MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-164">Develop Java MapReduce applications for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="e6c35-165">Vývoj aplikací Python MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-165">Develop Python MapReduce applications for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="e6c35-166"><a id="run"></a>Spuštění MapReduce</span><span class="sxs-lookup"><span data-stu-id="e6c35-166"><a id="run"></a>Run the MapReduce</span></span>

<span data-ttu-id="e6c35-167">HDInsight HiveQL úlohy můžete spustit pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="e6c35-167">HDInsight can run HiveQL jobs by using various methods.</span></span> <span data-ttu-id="e6c35-168">Následující tabulku použijte k rozhodování, jakou metodu je pro vás nejvhodnější a potom klepněte na odkaz návod.</span><span class="sxs-lookup"><span data-stu-id="e6c35-168">Use the following table to decide which method is right for you, then follow the link for a walkthrough.</span></span>

| <span data-ttu-id="e6c35-169">**Použít**...</span><span class="sxs-lookup"><span data-stu-id="e6c35-169">**Use this**...</span></span> | <span data-ttu-id="e6c35-170">**.. .a tomu**</span><span class="sxs-lookup"><span data-stu-id="e6c35-170">**...to do this**</span></span> | <span data-ttu-id="e6c35-171">.. při to **clusteru operačního systému**</span><span class="sxs-lookup"><span data-stu-id="e6c35-171">...with this **cluster operating system**</span></span> | <span data-ttu-id="e6c35-172">.. .from to **klientský operační systém**</span><span class="sxs-lookup"><span data-stu-id="e6c35-172">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="e6c35-173">SSH</span><span class="sxs-lookup"><span data-stu-id="e6c35-173">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="e6c35-174">Použijte příkaz Hadoop prostřednictvím **SSH**</span><span class="sxs-lookup"><span data-stu-id="e6c35-174">Use the Hadoop command through **SSH**</span></span> |<span data-ttu-id="e6c35-175">Linux</span><span class="sxs-lookup"><span data-stu-id="e6c35-175">Linux</span></span> |<span data-ttu-id="e6c35-176">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-176">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="e6c35-177">Curl</span><span class="sxs-lookup"><span data-stu-id="e6c35-177">Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="e6c35-178">Odeslání úlohy vzdáleně pomocí **REST**</span><span class="sxs-lookup"><span data-stu-id="e6c35-178">Submit the job remotely by using **REST**</span></span> |<span data-ttu-id="e6c35-179">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-179">Linux or Windows</span></span> |<span data-ttu-id="e6c35-180">Linux, Unix, Mac OS X nebo systému Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="e6c35-181">Prostředí Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6c35-181">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="e6c35-182">Odeslání úlohy vzdáleně pomocí **prostředí Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="e6c35-182">Submit the job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="e6c35-183">Linux nebo Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-183">Linux or Windows</span></span> |<span data-ttu-id="e6c35-184">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-184">Windows</span></span> |
| <span data-ttu-id="e6c35-185">[Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3)</span><span class="sxs-lookup"><span data-stu-id="e6c35-185">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="e6c35-186">Použijte příkaz Hadoop prostřednictvím **vzdálené plochy**</span><span class="sxs-lookup"><span data-stu-id="e6c35-186">Use the Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="e6c35-187">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-187">Windows</span></span> |<span data-ttu-id="e6c35-188">Windows</span><span class="sxs-lookup"><span data-stu-id="e6c35-188">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="e6c35-189">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="e6c35-189">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e6c35-190">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e6c35-190">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="e6c35-191"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="e6c35-191"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="e6c35-192">Další informace o práci s daty v HDInsight, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="e6c35-192">To learn more about working with data in HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="e6c35-193">Vývoj aplikací Java MapReduce pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-193">Develop Java MapReduce programs for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="e6c35-194">Vývoj Python streamování MapReduce programy pro HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-194">Develop Python streaming MapReduce programs for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

* [<span data-ttu-id="e6c35-195">Vývoj úloh paření MapReduce s Apache Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="e6c35-195">Develop Scalding MapReduce jobs with Apache Hadoop on HDInsight</span></span>](hdinsight-hadoop-mapreduce-scalding.md)

* <span data-ttu-id="e6c35-196">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="e6c35-196">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>

* <span data-ttu-id="e6c35-197">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="e6c35-197">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
