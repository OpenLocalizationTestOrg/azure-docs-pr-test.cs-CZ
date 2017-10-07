---
title: "Ukázky aaaRun hello Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Začněte pomocí služby Azure HDInsight hello hello ukázky zadat. Použití skriptů prostředí PowerShell, které spouštět programy MapReduce na clustery s daty."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bf76d452-abb4-4210-87bd-a2067778c6ed
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 544856a2cdfe5154cbd9bf1fb05db081af86cd46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Spuštění ukázky MapReduce s Hadoop v HDInsight se systémem Windows
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Sadu vzorků, které jsou k dispozici toohelp, které můžete začít a spustit úlohy MapReduce na clusterů systému Hadoop pomocí Azure HDInsight. Tyto ukázky jsou k dispozici na každém hello HDInsight spravované clustery, které vytvoříte. Spuštění tyto ukázky vám seznámit se s použitím úlohy toorun rutin prostředí Azure PowerShell na clusterů systému Hadoop.

* [**Aplikace Word počet**][hdinsight-sample-wordcount]: počty výskytů slova v textovém souboru.
* [**C# streamování počet slov**][hdinsight-sample-csharp-streaming]: počty výskytů slova v textového souboru pomocí hello streamování rozhraní Hadoop.
* [**Pi odhadu**][hdinsight-sample-pi-estimator]: používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí.
* [**10 GB Graysort**][hdinsight-sample-10gb-graysort]: spuštění pro obecné účely GraySort na 10 GB souboru pomocí HDInsight. Existují tři úlohy toorun: Teragen toogenerate hello data, Terasort toosort hello data a Teravalidate tooconfirm správně seřadit hello data.

> [!NOTE]
> Hello zdrojový kód najdete v příloze hello.

Existuje mnohem další dokumentaci na webu hello pro technologie související s Hadoop, jako je například programování založené na jazyce Java MapReduce, streamování a dokumentaci o rutinách hello, které se používají v prostředí Windows PowerShell skriptování. Další informace o těchto prostředků najdete v tématu:

* [Vývoj aplikací Java MapReduce pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)
* [Odesílání úloh Hadoop do služby HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Úvod tooAzure HDInsight][hdinsight-introduction]

Spousta lidí v současné době zvolte Hive a Pig přes MapReduce.  Další informace naleznete v tématu:

* [Používání Hive v HDInsight](hdinsight-use-hive.md)
* [Použijte Pig v HDInsight](hdinsight-use-pig.md)

**Požadavky**:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **cluster služby HDInsight**. Pokyny hello různé způsoby, ve kterých lze vytvořit tyto clustery najdete v části [vytvoření Hadoop clusterů v HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* **Pracovní stanice s prostředím Azure PowerShell**.

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="hdinsight-sample-wordcount"></a>Počet - Java aplikace Word
toosubmit projektu MapReduce, nejdřív vytvořit definici úlohy MapReduce. V definici úlohy hello, zadáte soubor jar programu hello MapReduce a hello umístění soubor jar hello, což je **wasb:///example/jars/hadoop-mapreduce-examples.jar**hello název třídy a hello argumenty.  Hello wordcount MapReduce program má dva argumenty: hello zdrojový soubor, který je použité toocount slova a hello umístění pro výstup.

Hello zdrojový kód najdete v hello [příloha A](#apendix-a---the-word-count-MapReduce-program-in-java).

Pro proceduru hello vývoje Java MapReduce programu, najdete v části - [vyvíjet MapReduce Java programy pro Hadoop v HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

**toosubmit úlohu MapReduce počet aplikace word**

1. Otevřete **Windows PowerShell ISE**. Pokyny najdete v tématu [nainstalovat a nakonfigurovat Azure PowerShell][powershell-install-configure].
2. Vložte následující skript prostředí PowerShell hello:

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define hello MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit hello job and wait for job completion
    $cred = Get-Credential -Message "Enter hello HDInsight cluster HTTP user credential:"
    $mrJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $clusterName `
                        -HttpCredential $cred `
                        -JobDefinition $mrJobDefinition

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -JobId $mrJob.JobId

    # Get hello job output
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -DefaultStorageAccountName $defaultStorageAccount `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultStorageContainer  `
        -JobId $mrJob.JobId `
        -DisplayOutputType StandardError

    # Download hello job output toohello workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display hello output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    Hello úlohu MapReduce vytvoří soubor s názvem *část r-00000*, který obsahuje slova, a počty hello. skript Hello používá hello **findstr** příkaz toolist všechny hello slova, která obsahuje *"existuje"*.
3. Nastavení hello prvních tří proměnných a spusťte skript hello.

## <a name="hdinsight-sample-csharp-streaming"></a>Aplikace Word počet - streamování C#
Hadoop poskytuje streamování tooMapReduce rozhraní API, která umožňuje toowrite mapy a omezit funkce v jiných jazyků než Java.

> [!NOTE]
> Hello kroky v tomto kurzu použít pouze na základě tooWindows clusterů HDInsight. Příklad streamování pro clustery HDInsight se systémem Linux naleznete v části [vyvíjet Python streamování programy pro HDInsight](hdinsight-hadoop-streaming-python.md).

V příkladu hello, hello mapper a hello reduktorem jsou spustitelné soubory, které číst hello vstup z [stdin –] [ stdin-stdout-stderr] (řádek po řádku) a výstup hello příliš[stdout] [stdin-stdout-stderr]. Hello program počty všechna hello slova v textu hello.

Pokud je zadán parametr spustitelný soubor pro **mappers**, při inicializaci hello mapper spustí každý úkol mapper hello spustitelného souboru jako samostatný proces. Spuštění úlohy mapper hello, se převede vstupní řádky a kanály hello toohello řádky [stdin –] [ stdin-stdout-stderr] hello procesu.

V hello té doby hello mapper shromažďuje hello řádkový výstup z výstupu stdout hello hello procesu. Každý řádek je převede na dvojice klíč/hodnota, která se shromažďují jako výstup hello hello mapper. Ve výchozím nastavení předpona hello řádku až toohello první kartě znak je hello klíč a hello zbytek hello řádku (s výjimkou hello znak tabulátoru) je hodnota hello. Pokud neexistuje žádný karta znak v řádku hello, považuje za celý řádek jako hello klíč a hodnotu hello má hodnotu null.

Pokud je zadán parametr spustitelný soubor pro **přechodky**, při inicializaci hello reduktorem spustí každý úkol reduktorem hello spustitelného souboru jako samostatný proces. Spuštění úlohy reduktorem hello, převede ji jeho páry vstupní klíč/hodnota do řádky a ho kanály hello řádky toohello [stdin –] [ stdin-stdout-stderr] hello procesu.

V hello té doby hello reduktorem shromažďuje hello řádkový výstup z hello [stdout] [ stdin-stdout-stderr] hello procesu. Převede každého řádku tooa dvojice klíč/hodnota, která se shromažďují jako výstup hello reduktorem hello. Ve výchozím nastavení předpona hello řádku až toohello první kartě znak je hello klíč a hello zbytek hello řádku (s výjimkou hello znak tabulátoru) je hodnota hello.

**počet úlohu streamování word toosubmit C#**

* Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy hello hello následující řádek:

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    Hello výstupní soubor musí být:

        example/data/StreamingOutput/wc.txt/part-00000

## <a name="hdinsight-sample-pi-estimator"></a>PI odhadu
pi odhadu Hello používá statistické (jako Monte Carlo) metoda tooestimate hello hodnotu čísla pí. Body, které jsou umístěny v náhodných uvnitř jednotku čtvercovou také spadal do kruh zapsány v rámci této hranaté s prostorem rovna toohello pravděpodobnosti hello kruh pí/4. z hodnoty hello 4R, kde R je poměr hello hello počet bodů, které jsou uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello se dá odhadnout Hello hodnotu čísla pí. Hello větší hello ukázka bodů použít, je lepší odhad hello hello.

zadaná pro tento ukázkový skript Hello odešle úlohu jar Hadoop a nastavení toorun s hodnotou 16 maps, z nichž každý je požadovaná toocompute 10 milionů ukázka bodů pomocí hodnoty parametrů hello. Tyto parametr hodnoty lze změnit tooimprove hello odhadované hodnotu čísla pí. Pro referenci hello prvních 10 desetinných míst pí jsou 3.1415926535.

**toosubmit úlohu odhadu platformy**

* Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java)a nahraďte definici úlohy hello hello následující řádek:

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <a name="hdinsight-sample-10gb-graysort"></a>Graysort 10 GB
Tato ukázka používá mírné 10GB dat tak, aby mohl být program spuštěn poměrně rychle. Používá hello MapReduce aplikace vyvinuté tak, že Owen O'Malley a Arun Murthy získané hello roční pro obecné účely ("daytona") terabajt řazení srovnávací test v 2009 míru 0.578 TB za minutu (100 TB 173 minut). Další informace o tomto a dalších řazení srovnávacích testů najdete v tématu hello [Sortbenchmark](http://sortbenchmark.org/) lokality.

Tato ukázka používá tři sady MapReduce programů:

1. **TeraGen** je program MapReduce, které můžete použít toogenerate hello řádky data toosort.
2. **TeraSort** ukázky hello vstupní data a používá MapReduce toosort hello data do celkové pořadí. TeraSort je standardní řazení funkcí MapReduce, s výjimkou vlastní dělicí metody, která používá seřazený seznam klíčů N-1 vzorků, které definují hello klíče rozsah pro každý snižte. Konkrétně, všechny klíče takové které ukázkové [i-1] < = klíč < ukázka [i] jsou odesílány tooreduce i. To zaručuje, že hello výstupy snížit i jsou všechny menší než výstup hello snížit i + 1.
3. **TeraValidate** je globálně MapReduce program, který ověří tento výstup hello řazena. Vytvoří jedna mapa každý soubor v adresáři výstup hello a každé mapování zajišťuje, že každý klíč je menší než nebo rovna toohello předchozí. Hello mapy funkce také generuje záznamy hello první a poslední klíče každého souboru a hello reduce – funkce zajišťuje, že hello první klíč souboru i je větší než hello poslední klíč i-1 souboru. Všechny problémy, které jsou hlášeny jako výstup hello snížit hello klíče, které jsou mimo pořadí.

Hello vstupní a výstupní formát, použít všechny tři aplikací, čte a zapisuje hello textových souborů ve správném formátu hello. Snižte Hello výstup hello replikace nastavil too1 místo výchozí hello 3, protože hello srovnávacího testu kontextu nevyžaduje, aby na uzlech toomultiple replikovat hello výstupní data.

Ukázka hello, každý odpovídající tooone programů MapReduce hello popsané v hello Úvod vyžadují tři úkoly:

1. Generovat hello data pro řazení spuštěním hello **TeraGen** úlohu MapReduce.
2. Řazení dat hello spuštěním hello **TeraSort** úlohu MapReduce.
3. Potvrďte, že byla hello data správně seřazená spuštěním hello **TeraValidate** úlohu MapReduce.

**toosubmit hello úlohy**

* Postupujte podle postupu hello v [aplikace Word počet - Java](#word-count-java), a použijte hello následující definice úlohy:

    ```powershell
    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teragen" `
                                -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "terasort" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teravalidate" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"
    ```

## <a name="next-steps"></a>Další kroky
Z tohoto článku a hello články v každé hello vzorků, které jste se dozvěděli, jak toorun hello ukázky součástí hello clusterů HDInsight pomocí prostředí Azure PowerShell. Kurzech k používání Pig, Hive a MapReduce s HDInsight najdete v části hello následující témata:

* [Začínáme používat Hadoop s Hive v HDInsight tooanalyze mobilního telefonu použití][hdinsight-get-started]
* [Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]
* [Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]
* [Odesílání úloh Hadoop v HDInsight][hdinsight-submit-jobs]
* [Dokumentace k Azure HDInsight SDK][hdinsight-sdk-documentation]

## <a name="appendix-a---hello-word-count-source-code"></a>Příloha A - hello Word počet zdrojového kódu

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

## <a name="appendix-b---hello-word-count-streaming-source-code"></a>Dodatek B – počet slov hello streamování zdrojového kódu
Hello MapReduce program používá hello cat.exe aplikace jako mapování rozhraní toostream hello text do konzoly hello a hello wc.exe aplikace jako hello snižte počet hello toocount rozhraní slov, která jsou datového proudu z dokumentu. Mapovač hello i reduktorem čtení řádek po řádku, znaky z hello standardní vstupní proud (stdin) a zápisu toohello standardního výstupního datového proudu (stdout).

```csharp
// hello source code for hello cat.exe (Mapper).

using System;
using System.IO;

namespace cat
{
    class cat
    {
        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            char[] separators = { ' ', '\n'};
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split(separators);
                foreach (var word in words)
                {
                    Console.WriteLine("{0}\t1", word);
                }
            }
        }
    }
}
```

Hello mapper kód používá soubor cat.cs hello [StreamReader] [ streamreader] objektu tooread hello znaků hello příchozí datový proud toohello konzoly, který potom provede zápis hello datového proudu toohello standardní výstupní proud s hello statické [Console.Writeline] [ console-writeline] metoda.

```csharp
// hello source code for wc.exe (Reducer) is:

using System;
using System.IO;
using System.Linq;
using System.Collections;

namespace wc
{
    class wc
    {
        static void Main(string[] args)
        {
            string line;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            Hashtable wordCount = new Hashtable();
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split('\t');

                string key = words[0];

                if (wordCount.ContainsKey(key) == true)
                {
                    int n = Convert.ToInt32(wordCount[key]);
                    wordCount[key] = Convert.ToString(n + 1);
                }
                else
                {
                    wordCount[key] = words[1];
                }
            }
            foreach (var key in wordCount.Keys)
            {
                Console.WriteLine("{0} {1}", key, wordCount[key]);
            }
        }
    }
}
```

Hello reduktorem kód používá soubor wc.cs hello [StreamReader] [ streamreader] objektu tooread znaky z hello standardní vstupní datový proud, který byl výstup mapovačem cat.exe hello. Jako přečte hello znaky s hello [Console.Writeline] [ console-writeline] metoda, počítá hello slova roven mezer a konec řádku znaků na konci hello jednotlivých slov. Pak zapíše hello celkový toohello standardní výstupní datový proud s hello [Console.Writeline] [ console-writeline] metoda.

## <a name="appendix-c---hello-pi-estimator-source-code"></a>Příloha C – hello pí odhadu zdrojového kódu
Hello pí odhadu Java kód, který obsahuje funkce mapper a reduktorem hello k dispozici pro kontrolu níže. Hello mapper program generuje zadaný počet bodů, které jsou umístěny v náhodných uvnitř čtverce jednotky a pak počty hello číslo z těchto bodů, které jsou uvnitř hello kruhu. program reduktorem Hello shromažďuje body zjištěné službou hello mappers a potom odhadne hello hodnotu čísla pí z vzorce 4R hello, kde R je poměr hello hello počet bodů počítá uvnitř hello kruh toohello celkový počet bodů, které jsou v rámci hranaté hello.

```java
/**
* Licensed toohello Apache Software Foundation (ASF) under one
* or more contributor license agreements. See hello NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. hello ASF licenses this file
* tooyou under hello Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with hello License. You may obtain a copy of hello License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed tooin writing, software
* distributed under hello License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See hello License for hello specific language governing permissions and
* limitations under hello License.
*/

package org.apache.hadoop.examples;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Iterator;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.BooleanWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;
import org.apache.hadoop.io.SequenceFile.CompressionType;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.SequenceFileInputFormat;
import org.apache.hadoop.mapred.SequenceFileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

//A Map-reduce program tooestimate hello value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of hello inscribed circle of hello square.
//
//Reducer:
//Accumulate points inside/outside results from hello mappers.
//Let numTotal = numInside + numOutside.
//hello fraction numInside/numTotal is a rational approximation of
//hello value (Area of hello circle)/(Area of hello square),
//where hello area of hello inscribed circle is Pi/4
//and hello area of unit square is 1.
//Then, Pi is estimated value toobe 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is hello index.
//Halton sequence is used toogenerate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize tooH(startindex),
//so hello sequence begins with H(startindex+1).
HaltonSequence(long startindex) {
index = startindex;
x = new double[K.length];
q = new double[K.length][];
d = new int[K.length][];
for(int i = 0; i < K.length; i++) {
q[i] = new double[K[i]];
d[i] = new int[K[i]];
}

for(int i = 0; i < K.length; i++) {
long k = index;
x[i] = 0;

for(int j = 0; j < K[i]; j++) {
q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
d[i][j] = (int)(k % P[i]);
k = (k - d[i][j])/P[i];
x[i] += d[i][j] * q[i][j];
}
}
}

//Compute next point.
//Assume hello current point is H(index).
//Compute H(index+1).
//@return a 2-dimensional point with coordinates in [0,1)^2
double[] nextPoint() {
index++;
for(int i = 0; i < K.length; i++) {
for(int j = 0; j < K[i]; j++) {
d[i][j]++;
x[i] += q[i][j];
if (d[i][j] < P[i]) {
break;
}
d[i][j] = 0;
x[i] -= (j == 0? 1.0: q[i][j-1]);
}
}
return x;
}
}

//Mapper class for Pi estimation.
//Generate points in a unit square and then
//count points inside/outside of hello inscribed circle of hello square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from hello (offset+1)th sample.
//@param size hello number of samples for this map
//@param out output {ture->numInside, false->numOutside}
//@param reporter
public void map(LongWritable offset,
LongWritable size,
OutputCollector<BooleanWritable, LongWritable> out,
Reporter reporter) throws IOException {

final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
long numInside = 0L;
long numOutside = 0L;

for(long i = 0; i < size.get(); ) {
//generate points in a unit square
final double[] point = haltonsequence.nextPoint();

//count points inside/outside of hello inscribed circle of hello square
final double x = point[0] - 0.5;
final double y = point[1] - 0.5;
if (x*x + y*y > 0.25) {
numOutside++;
} else {
numInside++;
}

//report status
i++;
if (i % 1000 == 0) {
reporter.setStatus("Generated " + i + " samples.");
}
}

//output map results
out.collect(new BooleanWritable(true), new LongWritable(numInside));
out.collect(new BooleanWritable(false), new LongWritable(numOutside));
}
}

//Reducer class for Pi estimation.
//Accumulate points inside/outside results from hello mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing hello file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from hello mappers.
// @param isInside Is hello points inside?
// @param values An iterator tooa list of point counts
// @param output dummy, not used here.
// @param reporter

public void reduce(BooleanWritable isInside,
Iterator<LongWritable> values,
OutputCollector<WritableComparable<?>, Writable> output,
Reporter reporter) throws IOException {
if (isInside.get()) {
for(; values.hasNext(); numInside += values.next().get());
} else {
for(; values.hasNext(); numOutside += values.next().get());
}
}

//Reduce task done, write output tooa file.
@Override
public void close() throws IOException {
//write output tooa file
Path outDir = new Path(TMP_DIR, "out");
Path outFile = new Path(outDir, "reduce-out");
FileSystem fileSys = FileSystem.get(conf);
SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
outFile, LongWritable.class, LongWritable.class,
CompressionType.NONE);
writer.append(new LongWritable(numInside), new LongWritable(numOutside));
writer.close();
}
}

//Run a map/reduce job for estimating Pi.
//@return hello estimated value of Pi.
public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
)
throws IOException {
//setup job conf
jobConf.setJobName(PiEstimator.class.getSimpleName());

jobConf.setInputFormat(SequenceFileInputFormat.class);

jobConf.setOutputKeyClass(BooleanWritable.class);
jobConf.setOutputValueClass(LongWritable.class);
jobConf.setOutputFormat(SequenceFileOutputFormat.class);

jobConf.setMapperClass(PiMapper.class);
jobConf.setNumMapTasks(numMaps);

jobConf.setReducerClass(PiReducer.class);
jobConf.setNumReduceTasks(1);

// turn off speculative execution, because DFS doesn't handle
// multiple writers toohello same file.
jobConf.setSpeculativeExecution(false);

//setup input/output directories
final Path inDir = new Path(TMP_DIR, "in");
final Path outDir = new Path(TMP_DIR, "out");
FileInputFormat.setInputPaths(jobConf, inDir);
FileOutputFormat.setOutputPath(jobConf, outDir);

final FileSystem fs = FileSystem.get(jobConf);
if (fs.exists(TMP_DIR)) {
throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
+ " already exists. Remove it first.");
}
if (!fs.mkdirs(inDir)) {
throw new IOException("Cannot create input directory " + inDir);
}

//generate an input file for each map task
try {
for(int i=0; i < numMaps; ++i) {
final Path file = new Path(inDir, "part"+i);
final LongWritable offset = new LongWritable(i * numPoints);
final LongWritable size = new LongWritable(numPoints);
final SequenceFile.Writer writer = SequenceFile.createWriter(
fs, jobConf, file,
LongWritable.class, LongWritable.class, CompressionType.NONE);
try {
writer.append(offset, size);
} finally {
writer.close();
}
System.out.println("Wrote input for Map #"+i);
}

//start a map/reduce job
System.out.println("Starting Job");
final long startTime = System.currentTimeMillis();
JobClient.runJob(jobConf);
final double duration = (System.currentTimeMillis() - startTime)/1000.0;
System.out.println("Job Finished in " + duration + " seconds");

//read outputs
Path inFile = new Path(outDir, "reduce-out");
LongWritable numInside = new LongWritable();
LongWritable numOutside = new LongWritable();
SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
try {
reader.next(numInside, numOutside);
} finally {
reader.close();
}

//compute estimated value
return BigDecimal.valueOf(4).setScale(20)
.multiply(BigDecimal.valueOf(numInside.get()))
.divide(BigDecimal.valueOf(numMaps))
.divide(BigDecimal.valueOf(numPoints));
} finally {
fs.delete(TMP_DIR, true);
}
}

//Parse arguments and then runs a map/reduce job.
//Print output in standard out.
//@return a non-zero if there is an error. Otherwise, return 0.
public int run(String[] args) throws Exception {
if (args.length != 2) {
System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
ToolRunner.printGenericCommandUsage(System.err);
return -1;
}

final int nMaps = Integer.parseInt(args[0]);
final long nSamples = Long.parseLong(args[1]);

System.out.println("Number of Maps = " + nMaps);
System.out.println("Samples per Map = " + nSamples);

final JobConf jobConf = new JobConf(getConf(), getClass());
System.out.println("Estimated value of Pi is "
+ estimate(nMaps, nSamples, jobConf));
return 0;
}

//main method for running it as a stand alone command.
public static void main(String[] argv) throws Exception {
System.exit(ToolRunner.run(null, new PiEstimator(), argv));
}
}
```

## <a name="appendix-d---hello-10gb-graysort-source-code"></a>Dodatek D – hello 10gb graysort zdrojového kódu
pro kontroly v této části se zobrazí kód Hello hello TeraSort MapReduce programu.

```java
/**
    * Licensed toohello Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See hello NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  hello ASF licenses this file
    * tooyou under hello Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with hello License.  You may obtain a copy of hello License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed tooin writing, software
    * distributed under hello License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See hello License for hello specific language governing permissions and
    * limitations under hello License.
    */

package org.apache.hadoop.examples.terasort;

import java.io.IOException;
import java.io.PrintStream;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.filecache.DistributedCache;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.Partitioner;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

/**
    * Generates hello sampled split points, launches hello job,
    * and waits for it toofinish.
    * <p>
    * toorun hello program:
    * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
    */

public class TeraSort extends Configured implements Tool {
    private static final Log LOG = LogFactory.getLog(TeraSort.class);

    /**
    * A partitioner that splits text keys into roughly equal
    * partitions in a global sorted order.
    */

    static class TotalOrderPartitioner implements Partitioner<Text,Text>{
    private TrieNode trie;
    private Text[] splitPoints;

    /**
        * A generic trie node
        */
    static abstract class TrieNode {
        private int level;
        TrieNode(int level) {
        this.level = level;
        }
        abstract int findPartition(Text key);
        abstract void print(PrintStream strm) throws IOException;
        int getLevel() {
        return level;
        }
    }

    /**
        * An inner trie node that contains 256 children based on hello next
        * character.
        */
    static class InnerTrieNode extends TrieNode {
        private TrieNode[] child = new TrieNode[256];

        InnerTrieNode(int level) {
        super(level);
        }
        int findPartition(Text key) {
        int level = getLevel();
        if (key.getLength() <= level) {
            return child[0].findPartition(key);
        }
        return child[key.getBytes()[level]].findPartition(key);
        }
        void setChild(int idx, TrieNode child) {
        this.child[idx] = child;
        }
        void print(PrintStream strm) throws IOException {
        for(int ch=0; ch < 255; ++ch) {
            for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
            }
            strm.print(ch);
            strm.println(" ->");
            if (child[ch] != null) {
            child[ch].print(strm);
            }
        }
        }
    }

    /**
        * A leaf trie node that does string compares toofigure out where hello given
        * key belongs between lower..upper.
        */
    static class LeafTrieNode extends TrieNode {
        int lower;
        int upper;
        Text[] splitPoints;
        LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
        super(level);
        this.splitPoints = splitPoints;
        this.lower = lower;
        this.upper = upper;
        }
        int findPartition(Text key) {
        for(int i=lower; i<upper; ++i) {
            if (splitPoints[i].compareTo(key) >= 0) {
            return i;
            }
        }
        return upper;
        }
        void print(PrintStream strm) throws IOException {
        for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
        }
        strm.print(lower);
        strm.print(", ");
        strm.println(upper);
        }
    }

    /**
        * Read hello cut points from hello given sequence file.
        * @param fs hello file system
        * @param p hello path tooread
        * @param job hello job config
        * @return hello strings toosplit hello partitions on
        * @throws IOException
        */
    private static Text[] readPartitions(FileSystem fs, Path p,
                                            JobConf job) throws IOException {
        SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
        List<Text> parts = new ArrayList<Text>();
        Text key = new Text();
        NullWritable value = NullWritable.get();
        while (reader.next(key, value)) {
        parts.add(key);
        key = new Text();
        }
        reader.close();
        return parts.toArray(new Text[parts.size()]);
    }

    /**
        * Given a sorted set of cut points, build a trie that will find hello correct
        * partition quickly.
        * @param splits hello list of cut points
        * @param lower hello lower bound of partitions 0..numPartitions-1
        * @param upper hello upper bound of partitions 0..numPartitions-1
        * @param prefix hello prefix that we have already checked against
        * @param maxDepth hello maximum depth we will build a trie for
        * @return hello trie node that will divide hello splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on toohello prefix
        trial.append(new byte[1], 0, 1);
        int currentBound = lower;
        for(int ch = 0; ch < 255; ++ch) {
        trial.getBytes()[depth] = (byte) (ch + 1);
        lower = currentBound;
        while (currentBound < upper) {
            if (splits[currentBound].compareTo(trial) >= 0) {
            break;
            }
            currentBound += 1;
        }
        trial.getBytes()[depth] = (byte) ch;
        result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                        maxDepth);
        }
        // pick up hello rest
        trial.getBytes()[depth] = 127;
        result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                    maxDepth);
        return result;
    }

    public void configure(JobConf job) {
        try {
        FileSystem fs = FileSystem.getLocal(job);
        Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
        splitPoints = readPartitions(fs, partFile, job);
        trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
        } catch (IOException ie) {
        throw new IllegalArgumentException("can't read paritions file", ie);
        }
    }

    public TotalOrderPartitioner() {
    }

    public int getPartition(Text key, Text value, int numPartitions) {
        return trie.findPartition(key);
    }

    }

    public int run(String[] args) throws Exception {
    LOG.info("starting");
    JobConf job = (JobConf) getConf();
    Path inputDir = new Path(args[0]);
    inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
    Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
    URI partitionUri = new URI(partitionFile.toString() +
                                "#" + TeraInputFormat.PARTITION_FILENAME);
    TeraInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.setJobName("TeraSort");
    job.setJarByClass(TeraSort.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    job.setInputFormat(TeraInputFormat.class);
    job.setOutputFormat(TeraOutputFormat.class);
    job.setPartitionerClass(TotalOrderPartitioner.class);
    TeraInputFormat.writePartitionFile(job, partitionFile);
    DistributedCache.addCacheFile(partitionUri, job);
    DistributedCache.createSymlink(job);
    job.setInt("dfs.replication", 1);
    TeraOutputFormat.setFinalSync(job, true);
    JobClient.runJob(job);
    LOG.info("done");
    return 0;
    }

    /**
    * @param args
    */

    public static void main(String[] args) throws Exception {
    int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
    System.exit(res);
    }
}
```

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
