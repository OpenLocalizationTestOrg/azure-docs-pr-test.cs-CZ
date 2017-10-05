---
title: "Vytvoření Java MapReduce pro Hadoop - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Apache Maven k vytvoření aplikace založené na jazyce Java MapReduce a potom ji spustit s Hadoop na Azure HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
ms.assetid: 9ee6384c-cb61-4087-8273-fb53fa27c1c3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 11d63f22204eb2acb530378f53ac72f16a35a4f2
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a><span data-ttu-id="cd9e8-103">Vývoj aplikací Java MapReduce pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="cd9e8-103">Develop Java MapReduce programs for Hadoop on HDInsight</span></span>

<span data-ttu-id="cd9e8-104">Další informace o použití Apache Maven k vytvoření aplikace založené na jazyce Java MapReduce a potom ji spustit s Hadoop na Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-104">Learn how to use Apache Maven to create a Java-based MapReduce application, then run it with Hadoop on Azure HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="cd9e8-105">Tento příklad byl naposledy otestovali na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-105">This example was most recently tested on HDInsight 3.6.</span></span>

## <span data-ttu-id="cd9e8-106"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd9e8-106"><a name="prerequisites"></a>Prerequisites</span></span>

* <span data-ttu-id="cd9e8-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="cd9e8-108">HDInsight verze 3.4 a starší pomocí Java 7.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-108">HDInsight versions 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="cd9e8-109">HDInsight 3.5 a vyšší používá Java 8.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-109">HDInsight 3.5 and greater uses Java 8.</span></span>

* [<span data-ttu-id="cd9e8-110">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="cd9e8-110">Apache Maven</span></span>](http://maven.apache.org/)

## <a name="configure-development-environment"></a><span data-ttu-id="cd9e8-111">Konfigurace vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="cd9e8-111">Configure development environment</span></span>

<span data-ttu-id="cd9e8-112">Následující proměnné prostředí může být nastaven při instalaci Java a sadu JDK.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-112">The following environment variables may be set when you install Java and the JDK.</span></span> <span data-ttu-id="cd9e8-113">Nicméně byste měli zkontrolovat, že existují a že obsahují správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-113">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="cd9e8-114">`JAVA_HOME`-by měla odkazovat na adresář, kam nainstalovat prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-114">`JAVA_HOME` - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="cd9e8-115">Například v systému OS X, Unix nebo Linux, musí mít hodnotu podobnou `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-115">For example, on an OS X, Unix or Linux system, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="cd9e8-116">V systému Windows má hodnotu podobnou`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="cd9e8-116">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="cd9e8-117">`PATH`-musí obsahovat následující cesty:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-117">`PATH` - should contain the following paths:</span></span>
  
  * <span data-ttu-id="cd9e8-118">`JAVA_HOME`(nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="cd9e8-118">`JAVA_HOME` (or the equivalent path)</span></span>

  * <span data-ttu-id="cd9e8-119">`JAVA_HOME\bin`(nebo ekvivalentní cesta)</span><span class="sxs-lookup"><span data-stu-id="cd9e8-119">`JAVA_HOME\bin` (or the equivalent path)</span></span>

  * <span data-ttu-id="cd9e8-120">Adresář, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="cd9e8-120">The directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="cd9e8-121">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="cd9e8-121">Create a Maven project</span></span>

1. <span data-ttu-id="cd9e8-122">Z relace Terminálové služby nebo příkazového řádku ve vašem vývojovém prostředí změňte adresáře na umístění, které chcete uložit tento projekt.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-122">From a terminal session, or command line in your development environment, change directories to the location you want to store this project.</span></span>

2. <span data-ttu-id="cd9e8-123">Použití `mvn` příkazu, který se instaluje s Maven, vygenerujte generování uživatelského rozhraní pro projekt.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-123">Use the `mvn` command, which is installed with Maven, to generate the scaffolding for the project.</span></span>

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > <span data-ttu-id="cd9e8-124">Pokud používáte prostředí PowerShell, je nutné uzavřít `-D` parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-124">If you are using PowerShell, you must enclose the `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="cd9e8-125">Tento příkaz vytvoří adresář s názvem zadaným `artifactID` parametr (**wordcountjava** v tomto příkladu.) Tento adresář obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-125">This command creates a directory with the name specified by the `artifactID` parameter (**wordcountjava** in this example.) This directory contains the following items:</span></span>

   * <span data-ttu-id="cd9e8-126">`pom.xml`-Na [projektu objektu modelu (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) obsahující podrobnosti o informace a konfigurace použít k sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-126">`pom.xml` - The [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) that contains information and configuration details used to build the project.</span></span>

   * <span data-ttu-id="cd9e8-127">`src`-Adresář, který obsahuje aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-127">`src` - The directory that contains the application.</span></span>

3. <span data-ttu-id="cd9e8-128">Odstranit `src/test/java/org/apache/hadoop/examples/apptest.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-128">Delete the `src/test/java/org/apache/hadoop/examples/apptest.java` file.</span></span> <span data-ttu-id="cd9e8-129">V tomto příkladu se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-129">It is not used in this example.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="cd9e8-130">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="cd9e8-130">Add dependencies</span></span>

1. <span data-ttu-id="cd9e8-131">Upravit `pom.xml` souboru a přidejte následující text v rámci `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-131">Edit the `pom.xml` file and add the following text inside the `<dependencies>` section:</span></span>
   
   ```xml
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-examples</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
   ```

    <span data-ttu-id="cd9e8-132">Definuje požadované knihovny (uvedené v &lt;artifactId\>) s určitou verzí (uvedené v &lt;verze\>).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-132">This defines required libraries (listed within &lt;artifactId\>) with a specific version (listed within &lt;version\>).</span></span> <span data-ttu-id="cd9e8-133">Při kompilaci se stáhnou tyto závislosti z úložiště Maven výchozí.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-133">At compile time, these dependencies are downloaded from the default Maven repository.</span></span> <span data-ttu-id="cd9e8-134">Můžete použít [hledání úložiště Maven](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) zobrazení.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-134">You can use the [Maven repository search](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) to view more.</span></span>
   
    <span data-ttu-id="cd9e8-135">`<scope>provided</scope>` Informuje Maven, tyto závislosti by neměl být zabalené s aplikací, jako jsou k dispozici v clusteru HDInsight za běhu.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-135">The `<scope>provided</scope>` tells Maven that these dependencies should not be packaged with the application, as they are provided by the HDInsight cluster at run-time.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="cd9e8-136">Je verze použitá by měl odpovídat verzi nachází v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-136">The version used should match the version of Hadoop present on your cluster.</span></span> <span data-ttu-id="cd9e8-137">Další informace o verzích, najdete v článku [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-137">For more information on versions, see the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

2. <span data-ttu-id="cd9e8-138">Přidejte následující `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-138">Add the following to the `pom.xml` file.</span></span> <span data-ttu-id="cd9e8-139">Tento text musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-139">This text must be inside the `<project>...</project>` tags in the file; for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
            </configuration>
            <executions>
            <execution>
                <phase>package</phase>
                    <goals>
                    <goal>shade</goal>
                    </goals>
            </execution>
            </executions>
            </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
            <source>1.8</source>
            <target>1.8</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
   ```

    <span data-ttu-id="cd9e8-140">První modul plug-in nakonfiguruje [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/), sloužící k sestavení uberjar (někdy nazývané fatjar), která obsahuje závislosti, které jsou požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-140">The first plugin configures the [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), which is used to build an uberjar (sometimes called a fatjar), which contains dependencies required by the application.</span></span> <span data-ttu-id="cd9e8-141">Rovněž zamezí duplikace licencí v rámci balíčku jar, což může způsobit problémy na některé systémy.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-141">It also prevents duplication of licenses within the jar package, which can cause problems on some systems.</span></span>

    <span data-ttu-id="cd9e8-142">Druhý modul plug-in nakonfiguruje cílová verze Java.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-142">The second plugin configures the target Java version.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cd9e8-143">HDInsight 3.4 a starší používání Java 7.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-143">HDInsight 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="cd9e8-144">HDInsight 3.5 a vyšší používá Java 8.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-144">HDInsight 3.5 and greater uses Java 8.</span></span>

3. <span data-ttu-id="cd9e8-145">Uložit `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-145">Save the `pom.xml` file.</span></span>

## <a name="create-the-mapreduce-application"></a><span data-ttu-id="cd9e8-146">Vytvoření aplikace nástroje MapReduce</span><span class="sxs-lookup"><span data-stu-id="cd9e8-146">Create the MapReduce application</span></span>

1. <span data-ttu-id="cd9e8-147">Přejděte na `wordcountjava/src/main/java/org/apache/hadoop/examples` adresáře a přejmenujte `App.java` do souboru `WordCount.java`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-147">Go to the `wordcountjava/src/main/java/org/apache/hadoop/examples` directory and rename the `App.java` file to `WordCount.java`.</span></span>

2. <span data-ttu-id="cd9e8-148">Otevřete `WordCount.java` soubor v textovém editoru a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-148">Open the `WordCount.java` file in a text editor and replace the contents with the following text:</span></span>
   
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
   
    <span data-ttu-id="cd9e8-149">Všimněte si, název balíčku je `org.apache.hadoop.examples` a název třídy je `WordCount`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-149">Notice the package name is `org.apache.hadoop.examples` and the class name is `WordCount`.</span></span> <span data-ttu-id="cd9e8-150">Můžete použít tyto názvy při odeslání úlohy MapReduce.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-150">You use these names when you submit the MapReduce job.</span></span>

3. <span data-ttu-id="cd9e8-151">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-151">Save the file.</span></span>

## <a name="build-the-application"></a><span data-ttu-id="cd9e8-152">Sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="cd9e8-152">Build the application</span></span>

1. <span data-ttu-id="cd9e8-153">Změnit na `wordcountjava` adresáře, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-153">Change to the `wordcountjava` directory, if you are not already there.</span></span>

2. <span data-ttu-id="cd9e8-154">Vytvořit soubor JAR obsahující aplikace, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-154">Use the following command to build a JAR file containing the application:</span></span>

   ```
   mvn clean package
   ```

    <span data-ttu-id="cd9e8-155">Tento příkaz odstraní všechny předchozí artefakty sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány a pak sestavení a balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-155">This command cleans any previous build artifacts, downloads any dependencies that have not already been installed, and then builds and package the application.</span></span>

3. <span data-ttu-id="cd9e8-156">Po dokončení příkazu `wordcountjava/target` adresář obsahuje soubor s názvem `wordcountjava-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-156">Once the command finishes, the `wordcountjava/target` directory contains a file named `wordcountjava-1.0-SNAPSHOT.jar`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cd9e8-157">`wordcountjava-1.0-SNAPSHOT.jar` Soubor je uberjar, který obsahuje pouze úlohu WordCount, ale také závislosti, které úloha vyžaduje za běhu.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-157">The `wordcountjava-1.0-SNAPSHOT.jar` file is an uberjar, which contains not only the WordCount job, but also dependencies that the job requires at runtime.</span></span>

## <span data-ttu-id="cd9e8-158"><a id="upload"></a>Nahrát jar</span><span class="sxs-lookup"><span data-stu-id="cd9e8-158"><a id="upload"></a>Upload the jar</span></span>

<span data-ttu-id="cd9e8-159">Použijte následující příkaz k nahrání na soubor jar do HDInsight headnode:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-159">Use the following command to upload the jar file to the HDInsight headnode:</span></span>

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

<span data-ttu-id="cd9e8-160">Tento příkaz zkopíruje soubory z místního systému k hlavnímu uzlu.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-160">This command copies the files from the local system to the head node.</span></span> <span data-ttu-id="cd9e8-161">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-161">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="cd9e8-162"><a name="run"></a>Spustit úlohu MapReduce na Hadoop</span><span class="sxs-lookup"><span data-stu-id="cd9e8-162"><a name="run"></a>Run the MapReduce job on Hadoop</span></span>

1. <span data-ttu-id="cd9e8-163">Připojení k HDInsight pomocí SSH.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-163">Connect to HDInsight using SSH.</span></span> <span data-ttu-id="cd9e8-164">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-164">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="cd9e8-165">Z relace SSH použijte následující příkaz ke spuštění aplikace MapReduce:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-165">From the SSH session, use the following command to run the MapReduce application:</span></span>
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    <span data-ttu-id="cd9e8-166">Tento příkaz spustí aplikaci WordCount MapReduce.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-166">This command starts the WordCount MapReduce application.</span></span> <span data-ttu-id="cd9e8-167">Vstupní soubor je `/example/data/gutenberg/davinci.txt`, a je k zadanému výstupnímu adresáři `/example/data/wordcountout`.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-167">The input file is `/example/data/gutenberg/davinci.txt`, and the output directory is `/example/data/wordcountout`.</span></span> <span data-ttu-id="cd9e8-168">Soubor vstupní i výstupní jsou uloženy na výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-168">Both the input file and output are stored to the default storage for the cluster.</span></span>

3. <span data-ttu-id="cd9e8-169">Po dokončení úlohy pro zobrazení výsledků použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-169">Once the job completes, use the following command to view the results:</span></span>
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    <span data-ttu-id="cd9e8-170">Mělo by se zobrazit seznam slova a počty s hodnotami podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="cd9e8-170">You should receive a list of words and counts, with values similar to the following text:</span></span>
   
        zeal    1
        zelus   1
        zenith  2

## <span data-ttu-id="cd9e8-171"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="cd9e8-171"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="cd9e8-172">V tomto dokumentu jste se naučili, jak vyvíjet úlohu Java MapReduce.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-172">In this document, you have learned how to develop a Java MapReduce job.</span></span> <span data-ttu-id="cd9e8-173">Najdete v následujících dokumentech pro další způsoby, jak pracovat s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="cd9e8-173">See the following documents for other ways to work with HDInsight.</span></span>

* <span data-ttu-id="cd9e8-174">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="cd9e8-174">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="cd9e8-175">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="cd9e8-175">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* [<span data-ttu-id="cd9e8-176">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="cd9e8-176">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="cd9e8-177">Další informace naleznete také [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="cd9e8-177">For more information, see also the [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

