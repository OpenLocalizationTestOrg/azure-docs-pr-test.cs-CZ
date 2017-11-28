---
title: aaaCreate Java MapReduce pro Hadoop - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse Apache Maven toocreate založené na jazyce Java MapReduce aplikace, spusťte ji s Hadoop v Azure HDInsight."
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
ms.openlocfilehash: 903a57a482395f7da79002188399a4d6288ff0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a><span data-ttu-id="8b8a2-103">Vývoj aplikací Java MapReduce pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b8a2-103">Develop Java MapReduce programs for Hadoop on HDInsight</span></span>

<span data-ttu-id="8b8a2-104">Zjistěte, jak toouse Apache Maven toocreate založené na jazyce Java MapReduce aplikace, spusťte ji s Hadoop v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-104">Learn how toouse Apache Maven toocreate a Java-based MapReduce application, then run it with Hadoop on Azure HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="8b8a2-105">Tento příklad byl naposledy otestovali na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-105">This example was most recently tested on HDInsight 3.6.</span></span>

## <span data-ttu-id="8b8a2-106"><a name="prerequisites"></a>Požadavky</span><span class="sxs-lookup"><span data-stu-id="8b8a2-106"><a name="prerequisites"></a>Prerequisites</span></span>

* <span data-ttu-id="8b8a2-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="8b8a2-108">HDInsight verze 3.4 a starší pomocí Java 7.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-108">HDInsight versions 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="8b8a2-109">HDInsight 3.5 a vyšší používá Java 8.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-109">HDInsight 3.5 and greater uses Java 8.</span></span>

* [<span data-ttu-id="8b8a2-110">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="8b8a2-110">Apache Maven</span></span>](http://maven.apache.org/)

## <a name="configure-development-environment"></a><span data-ttu-id="8b8a2-111">Konfigurace vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="8b8a2-111">Configure development environment</span></span>

<span data-ttu-id="8b8a2-112">Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-112">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="8b8a2-113">Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-113">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="8b8a2-114">`JAVA_HOME`-by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-114">`JAVA_HOME` - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="8b8a2-115">Například v systému OS X, Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-115">For example, on an OS X, Unix or Linux system, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="8b8a2-116">Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="8b8a2-116">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="8b8a2-117">`PATH`-musí obsahovat hello následující cesty:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-117">`PATH` - should contain hello following paths:</span></span>
  
  * <span data-ttu-id="8b8a2-118">`JAVA_HOME`(nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="8b8a2-118">`JAVA_HOME` (or hello equivalent path)</span></span>

  * <span data-ttu-id="8b8a2-119">`JAVA_HOME\bin`(nebo ekvivalentní cesta hello)</span><span class="sxs-lookup"><span data-stu-id="8b8a2-119">`JAVA_HOME\bin` (or hello equivalent path)</span></span>

  * <span data-ttu-id="8b8a2-120">Hello adresáře, kde je nainstalován Maven</span><span class="sxs-lookup"><span data-stu-id="8b8a2-120">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="8b8a2-121">Vytvořte projekt Maven</span><span class="sxs-lookup"><span data-stu-id="8b8a2-121">Create a Maven project</span></span>

1. <span data-ttu-id="8b8a2-122">Z relace Terminálové služby nebo příkazového řádku ve vašem vývojovém prostředí změňte umístění adresáře toohello chcete toostore tento projekt.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-122">From a terminal session, or command line in your development environment, change directories toohello location you want toostore this project.</span></span>

2. <span data-ttu-id="8b8a2-123">Použití hello `mvn` příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-123">Use hello `mvn` command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > <span data-ttu-id="8b8a2-124">Pokud používáte prostředí PowerShell, je nutné uzavřít hello `-D` parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-124">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="8b8a2-125">Tento příkaz vytvoří adresář s názvem hello určeného hello `artifactID` parametr (**wordcountjava** v tomto příkladu.) Tento adresář obsahuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-125">This command creates a directory with hello name specified by hello `artifactID` parameter (**wordcountjava** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="8b8a2-126">`pom.xml`-hello [projektu objektu modelu (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) obsahující informace a podrobnosti o konfiguraci použít toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-126">`pom.xml` - hello [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) that contains information and configuration details used toobuild hello project.</span></span>

   * <span data-ttu-id="8b8a2-127">`src`-hello adresář, který obsahuje aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-127">`src` - hello directory that contains hello application.</span></span>

3. <span data-ttu-id="8b8a2-128">Odstranit hello `src/test/java/org/apache/hadoop/examples/apptest.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-128">Delete hello `src/test/java/org/apache/hadoop/examples/apptest.java` file.</span></span> <span data-ttu-id="8b8a2-129">V tomto příkladu se nepoužívá.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-129">It is not used in this example.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="8b8a2-130">Přidat závislosti</span><span class="sxs-lookup"><span data-stu-id="8b8a2-130">Add dependencies</span></span>

1. <span data-ttu-id="8b8a2-131">Upravit hello `pom.xml` souboru a přidejte následující text v rámci hello hello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-131">Edit hello `pom.xml` file and add hello following text inside hello `<dependencies>` section:</span></span>
   
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

    <span data-ttu-id="8b8a2-132">Definuje požadované knihovny (uvedené v &lt;artifactId\>) s určitou verzí (uvedené v &lt;verze\>).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-132">This defines required libraries (listed within &lt;artifactId\>) with a specific version (listed within &lt;version\>).</span></span> <span data-ttu-id="8b8a2-133">Při kompilaci se stáhnou tyto závislosti z hello výchozí Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-133">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="8b8a2-134">Můžete použít hello [hledání úložiště Maven](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview Další.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-134">You can use hello [Maven repository search](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview more.</span></span>
   
    <span data-ttu-id="8b8a2-135">Hello `<scope>provided</scope>` informuje Maven, nesmí tyto závislosti zabalit s hello aplikace, jako jsou poskytovány hello clusteru HDInsight se při spuštění.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-135">hello `<scope>provided</scope>` tells Maven that these dependencies should not be packaged with hello application, as they are provided by hello HDInsight cluster at run-time.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8b8a2-136">verze Hello používá by měl odpovídat hello verzi nachází v clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-136">hello version used should match hello version of Hadoop present on your cluster.</span></span> <span data-ttu-id="8b8a2-137">Další informace o verzích, najdete v části hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-137">For more information on versions, see hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

2. <span data-ttu-id="8b8a2-138">Přidejte následující toohello hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-138">Add hello following toohello `pom.xml` file.</span></span> <span data-ttu-id="8b8a2-139">Tento text musí být uvnitř hello `<project>...</project>` značky v souboru hello, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-139">This text must be inside hello `<project>...</project>` tags in hello file; for example, between `</dependencies>` and `</project>`.</span></span>

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

    <span data-ttu-id="8b8a2-140">Hello první modul plug-in nakonfiguruje hello [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/), což je použité toobuild uberjar (někdy nazývané fatjar), která obsahuje závislosti požadované aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-140">hello first plugin configures hello [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), which is used toobuild an uberjar (sometimes called a fatjar), which contains dependencies required by hello application.</span></span> <span data-ttu-id="8b8a2-141">Rovněž zamezí duplikace licencí v rámci balíčku jar hello, což může způsobit problémy na některé systémy.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-141">It also prevents duplication of licenses within hello jar package, which can cause problems on some systems.</span></span>

    <span data-ttu-id="8b8a2-142">druhý modul plug-in Hello nakonfiguruje hello cílová Java verze.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-142">hello second plugin configures hello target Java version.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b8a2-143">HDInsight 3.4 a starší používání Java 7.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-143">HDInsight 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="8b8a2-144">HDInsight 3.5 a vyšší používá Java 8.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-144">HDInsight 3.5 and greater uses Java 8.</span></span>

3. <span data-ttu-id="8b8a2-145">Uložit hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-145">Save hello `pom.xml` file.</span></span>

## <a name="create-hello-mapreduce-application"></a><span data-ttu-id="8b8a2-146">Vytvoření aplikace MapReduce hello</span><span class="sxs-lookup"><span data-stu-id="8b8a2-146">Create hello MapReduce application</span></span>

1. <span data-ttu-id="8b8a2-147">Přejděte toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` adresáře a přejmenujte hello `App.java` souboru příliš`WordCount.java`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-147">Go toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` directory and rename hello `App.java` file too`WordCount.java`.</span></span>

2. <span data-ttu-id="8b8a2-148">Otevřete hello `WordCount.java` soubor v textovém editoru a nahraďte obsah hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-148">Open hello `WordCount.java` file in a text editor and replace hello contents with hello following text:</span></span>
   
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
   
    <span data-ttu-id="8b8a2-149">Název balíčku hello oznámení je `org.apache.hadoop.examples` a název třídy hello je `WordCount`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-149">Notice hello package name is `org.apache.hadoop.examples` and hello class name is `WordCount`.</span></span> <span data-ttu-id="8b8a2-150">Tyto názvy se používají, když odeslat úlohu MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-150">You use these names when you submit hello MapReduce job.</span></span>

3. <span data-ttu-id="8b8a2-151">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-151">Save hello file.</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="8b8a2-152">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="8b8a2-152">Build hello application</span></span>

1. <span data-ttu-id="8b8a2-153">Změnit toohello `wordcountjava` adresáře, pokud si nejste již existuje.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-153">Change toohello `wordcountjava` directory, if you are not already there.</span></span>

2. <span data-ttu-id="8b8a2-154">Použijte následující příkaz toobuild JAR soubor obsahující aplikace hello hello:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-154">Use hello following command toobuild a JAR file containing hello application:</span></span>

   ```
   mvn clean package
   ```

    <span data-ttu-id="8b8a2-155">Tento příkaz odstraní všechny předchozí artefakty sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány a pak sestavení a aplikace hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-155">This command cleans any previous build artifacts, downloads any dependencies that have not already been installed, and then builds and package hello application.</span></span>

3. <span data-ttu-id="8b8a2-156">Jakmile hello dokončení příkazu hello `wordcountjava/target` adresář obsahuje soubor s názvem `wordcountjava-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-156">Once hello command finishes, hello `wordcountjava/target` directory contains a file named `wordcountjava-1.0-SNAPSHOT.jar`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="8b8a2-157">Hello `wordcountjava-1.0-SNAPSHOT.jar` soubor je uberjar, obsahující nejen hello WordCount úlohy, ale je potřeba závislosti, které hello úlohy za běhu.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-157">hello `wordcountjava-1.0-SNAPSHOT.jar` file is an uberjar, which contains not only hello WordCount job, but also dependencies that hello job requires at runtime.</span></span>

## <span data-ttu-id="8b8a2-158"><a id="upload"></a>Nahrát hello jar</span><span class="sxs-lookup"><span data-stu-id="8b8a2-158"><a id="upload"></a>Upload hello jar</span></span>

<span data-ttu-id="8b8a2-159">Použijte následující příkaz tooupload hello jar souboru toohello HDInsight headnode hello:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-159">Use hello following command tooupload hello jar file toohello HDInsight headnode:</span></span>

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

<span data-ttu-id="8b8a2-160">Tento příkaz zkopíruje soubory hello z hlavního uzlu toohello hello místní systém.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-160">This command copies hello files from hello local system toohello head node.</span></span> <span data-ttu-id="8b8a2-161">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-161">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="8b8a2-162"><a name="run"></a>Spustit úlohu MapReduce hello na Hadoop</span><span class="sxs-lookup"><span data-stu-id="8b8a2-162"><a name="run"></a>Run hello MapReduce job on Hadoop</span></span>

1. <span data-ttu-id="8b8a2-163">Připojte tooHDInsight pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-163">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="8b8a2-164">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-164">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8b8a2-165">Z relace SSH hello použijte následující příkaz toorun hello MapReduce aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-165">From hello SSH session, use hello following command toorun hello MapReduce application:</span></span>
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    <span data-ttu-id="8b8a2-166">Tento příkaz spustí hello aplikace WordCount MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-166">This command starts hello WordCount MapReduce application.</span></span> <span data-ttu-id="8b8a2-167">vstupní soubor Hello je `/example/data/gutenberg/davinci.txt`, a adresář výstup hello je `/example/data/wordcountout`.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-167">hello input file is `/example/data/gutenberg/davinci.txt`, and hello output directory is `/example/data/wordcountout`.</span></span> <span data-ttu-id="8b8a2-168">Vstupní soubor hello i výstupní jsou uložené toohello výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-168">Both hello input file and output are stored toohello default storage for hello cluster.</span></span>

3. <span data-ttu-id="8b8a2-169">Po dokončení úlohy hello, použijte následující příkaz tooview hello výsledky hello:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-169">Once hello job completes, use hello following command tooview hello results:</span></span>
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    <span data-ttu-id="8b8a2-170">Seznam slova a počty, mělo by se zobrazit s hodnotami podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="8b8a2-170">You should receive a list of words and counts, with values similar toohello following text:</span></span>
   
        zeal    1
        zelus   1
        zenith  2

## <span data-ttu-id="8b8a2-171"><a id="nextsteps"></a>Další kroky</span><span class="sxs-lookup"><span data-stu-id="8b8a2-171"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="8b8a2-172">V tomto dokumentu jste se naučili jak toodevelop úlohu Java MapReduce.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-172">In this document, you have learned how toodevelop a Java MapReduce job.</span></span> <span data-ttu-id="8b8a2-173">V tématu hello následující dokumenty, kde najdete další způsoby toowork s HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8b8a2-173">See hello following documents for other ways toowork with HDInsight.</span></span>

* <span data-ttu-id="8b8a2-174">[Použití Hivu se službou HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="8b8a2-174">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="8b8a2-175">[Použití Pigu se službou HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="8b8a2-175">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* [<span data-ttu-id="8b8a2-176">Používání nástroje MapReduce s HDInsight</span><span class="sxs-lookup"><span data-stu-id="8b8a2-176">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="8b8a2-177">Další informace najdete v tématu taky hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="8b8a2-177">For more information, see also hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

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

