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
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>Používání nástroje MapReduce v Hadoop v HDInsight

Zjistěte, jak toorun MapReduce úlohy v clusterech prostředí HDInsight. Použijte následující tabulku toodiscover hello různé způsoby, že lze použít MapReduce s HDInsight hello:

| **Použít**... | **.. .toodo to** | .. při to **clusteru operačního systému** | .. .from to **klientský operační systém** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Použijte příkaz Hadoop hello prostřednictvím **SSH** |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [REST](hdinsight-hadoop-use-mapreduce-curl.md) |Odeslat úlohu hello vzdáleně pomocí **REST** (příklady použití cURL) |Linux nebo Windows |Linux, Unix, Mac OS X nebo systému Windows |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Odeslat úlohu hello vzdáleně pomocí **prostředí Windows PowerShell** |Linux nebo Windows |Windows |
| [Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3) |Použijte příkaz Hadoop hello prostřednictvím **vzdálené plochy** |Windows |Windows |

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="whatis"></a>Co je MapReduce

Hadoop MapReduce je framework software pro zápis úlohy, které zpracovávají velké množství dat. Vstupní data je rozdělená do nezávislé bloků, které se pak zpracovávají paralelně mezi hello uzly v clusteru. Úloha MapReduce se skládá z dvě funkce:

* **Mapovač**: spotřebovává vstupních dat, analyzuje (obvykle s filtrování a řazení operations) a vysílá řazených kolekcí členů (páry klíč hodnota)

* **Reduktorem**: využívá řazených kolekcí členů vysílaných hello Mapper a provádí operaci souhrn, která vytvoří výsledek menší, kombinované z hello Mapovač dat

V příkladu úlohy MapReduce počet základní word je znázorněno v následujícím diagramu hello:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

výstup Hello této úlohy je počet kolikrát jednotlivých slov došlo k chybě v textu hello, který byl analyzován.

* Mapovač Hello používá každý řádek ze vstupního textu hello jako vstup a dělí na slova. Ho vysílá klíč/hodnota pár pokaždé, když slovo dojde v aplikaci hello word následuje 1. výstup Hello je řazen před odesláním tooreducer.
* Hello reduktorem sčítá tyto počty jednotlivých pro každou aplikaci word a vysílá pár jeden klíč/hodnota obsahující slovo hello a hello součet jeho výskytů.

MapReduce můžou se implementovat v různých jazycích. Java hello nejběžnější implementace a slouží pro účely ukázky v tomto dokumentu.

## <a name="development-languages"></a>Programovacích jazyků

Jazyky nebo rozhraní, které jsou založené na jazyce Java a hello virtuální počítač Java můžete spustili přímo jako úlohu MapReduce. Hello příkladu v tomto dokumentu je aplikace Java MapReduce. Jazyky Java jiného typu, jako je C#, Python nebo samostatné spustitelné soubory, musíte použít, streamování Hadoop.

Streamování Hadoop komunikuje s hello mapper a reduktorem pomocí standardního vstupu a výstupu STDOUT. Mapovač Hello reduktorem číst data řádku současně z stdin – a zápisu tooSTDOUT výstup hello. Každý řádek čtení nebo vysílaných hello mapper a reduktorem musí být ve formátu hello dvojic klíč hodnota oddělená tabulátorem:

    [key]/t[value]

Další informace najdete v tématu [streamování Hadoop](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Příklady použití streamování s HDInsight Hadoop najdete v části hello následující dokumenty:

* [Vývoj úloh MapReduce C#](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Vývoj úloh Python MapReduce](hdinsight-hadoop-streaming-python.md)

## <a id="data"></a>Příklad dat

HDInsight poskytuje různé příklad datových sad, které jsou uloženy v hello `/example/data` a `/HdiSamples` adresáře. Tyto adresáře jsou hello výchozí úložiště pro cluster. V tomto dokumentu používáme hello `/example/data/gutenberg/davinci.txt` souboru. Tento soubor obsahuje poznámkových bloků hello z Leonardo Da Vinci.

## <a id="job"></a>Příklad MapReduce

Příklad počet dokumentů aplikace word MapReduce je součástí clusteru HDInsight. V tomto příkladu je umístěn v `/example/jars/hadoop-mapreduce-examples.jar` na hello výchozí úložiště pro cluster.

Následující kód v jazyce Java Hello je hello zdroj hello MapReduce aplikace obsažené v hello `hadoop-mapreduce-examples.jar` souboru:

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

Vaše vlastní aplikace MapReduce, najdete pokyny toowrite hello následující dokumenty:

* [Vývoj aplikací Java MapReduce pro HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Vývoj aplikací Python MapReduce pro HDInsight](hdinsight-hadoop-streaming-python.md)

## <a id="run"></a>Spustit hello MapReduce

HDInsight HiveQL úlohy můžete spustit pomocí různých metod. Pomocí hello následující toodecide tabulku, která metoda je pro vás nejvhodnější a potom postupujte podle hello odkaz návod.

| **Použít**... | **.. .toodo to** | .. při to **clusteru operačního systému** | .. .from to **klientský operační systém** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Použijte příkaz Hadoop hello prostřednictvím **SSH** |Linux |Linux, Unix, Mac OS X nebo systému Windows |
| [Curl](hdinsight-hadoop-use-mapreduce-curl.md) |Odeslat úlohu hello vzdáleně pomocí **REST** |Linux nebo Windows |Linux, Unix, Mac OS X nebo systému Windows |
| [Prostředí Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Odeslat úlohu hello vzdáleně pomocí **prostředí Windows PowerShell** |Linux nebo Windows |Windows |
| [Vzdálená plocha](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 a 3.3) |Použijte příkaz Hadoop hello prostřednictvím **vzdálené plochy** |Windows |Windows |

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="nextsteps"></a>Další kroky

toolearn Další informace o práci s daty v HDInsight, najdete v části hello následující dokumenty:

* [Vývoj aplikací Java MapReduce pro HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [Vývoj Python streamování MapReduce programy pro HDInsight](hdinsight-hadoop-streaming-python.md)

* [Vývoj úloh paření MapReduce s Apache Hadoop v HDInsight](hdinsight-hadoop-mapreduce-scalding.md)

* [Použití Hivu se službou HDInsight][hdinsight-use-hive]

* [Použití Pigu se službou HDInsight][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
