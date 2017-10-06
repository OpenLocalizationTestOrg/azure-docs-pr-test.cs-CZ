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
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a>Vývoj aplikací Java MapReduce pro Hadoop v HDInsight

Zjistěte, jak toouse Apache Maven toocreate založené na jazyce Java MapReduce aplikace, spusťte ji s Hadoop v Azure HDInsight.

> [!NOTE]
> Tento příklad byl naposledy otestovali na HDInsight 3.6.

## <a name="prerequisites"></a>Požadavky

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK).
    
    > [!NOTE]
    > HDInsight verze 3.4 a starší pomocí Java 7. HDInsight 3.5 a vyšší používá Java 8.

* [Apache Maven](http://maven.apache.org/)

## <a name="configure-development-environment"></a>Konfigurace vývojového prostředí

Hello následující proměnné prostředí může být nastaven při instalaci Java a hello JDK. Ale byste měli zkontrolovat, že existují a že obsahují hello správné hodnoty pro váš systém.

* `JAVA_HOME`-by měla odkazovat toohello adresáře, kde je nainstalován hello prostředí Java runtime (JRE). Například v systému OS X, Unix nebo Linux, musí mít hodnotu podobnou příliš`/usr/lib/jvm/java-7-oracle`. Windows neměl by mít hodnotu podobnou příliš`c:\Program Files (x86)\Java\jre1.7`

* `PATH`-musí obsahovat hello následující cesty:
  
  * `JAVA_HOME`(nebo ekvivalentní cesta hello)

  * `JAVA_HOME\bin`(nebo ekvivalentní cesta hello)

  * Hello adresáře, kde je nainstalován Maven

## <a name="create-a-maven-project"></a>Vytvořte projekt Maven

1. Z relace Terminálové služby nebo příkazového řádku ve vašem vývojovém prostředí změňte umístění adresáře toohello chcete toostore tento projekt.

2. Použití hello `mvn` příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > Pokud používáte prostředí PowerShell, je nutné uzavřít hello `-D` parametry v uvozovkách.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Tento příkaz vytvoří adresář s názvem hello určeného hello `artifactID` parametr (**wordcountjava** v tomto příkladu.) Tento adresář obsahuje hello následující položky:

   * `pom.xml`-hello [projektu objektu modelu (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) obsahující informace a podrobnosti o konfiguraci použít toobuild hello projektu.

   * `src`-hello adresář, který obsahuje aplikace hello.

3. Odstranit hello `src/test/java/org/apache/hadoop/examples/apptest.java` souboru. V tomto příkladu se nepoužívá.

## <a name="add-dependencies"></a>Přidat závislosti

1. Upravit hello `pom.xml` souboru a přidejte následující text v rámci hello hello `<dependencies>` části:
   
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

    Definuje požadované knihovny (uvedené v &lt;artifactId\>) s určitou verzí (uvedené v &lt;verze\>). Při kompilaci se stáhnou tyto závislosti z hello výchozí Maven úložiště. Můžete použít hello [hledání úložiště Maven](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview Další.
   
    Hello `<scope>provided</scope>` informuje Maven, nesmí tyto závislosti zabalit s hello aplikace, jako jsou poskytovány hello clusteru HDInsight se při spuštění.

    > [!IMPORTANT]
    > verze Hello používá by měl odpovídat hello verzi nachází v clusteru Hadoop. Další informace o verzích, najdete v části hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.

2. Přidejte následující toohello hello `pom.xml` souboru. Tento text musí být uvnitř hello `<project>...</project>` značky v souboru hello, například mezi `</dependencies>` a `</project>`.

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

    Hello první modul plug-in nakonfiguruje hello [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/), což je použité toobuild uberjar (někdy nazývané fatjar), která obsahuje závislosti požadované aplikace hello. Rovněž zamezí duplikace licencí v rámci balíčku jar hello, což může způsobit problémy na některé systémy.

    druhý modul plug-in Hello nakonfiguruje hello cílová Java verze.

    > [!NOTE]
    > HDInsight 3.4 a starší používání Java 7. HDInsight 3.5 a vyšší používá Java 8.

3. Uložit hello `pom.xml` souboru.

## <a name="create-hello-mapreduce-application"></a>Vytvoření aplikace MapReduce hello

1. Přejděte toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` adresáře a přejmenujte hello `App.java` souboru příliš`WordCount.java`.

2. Otevřete hello `WordCount.java` soubor v textovém editoru a nahraďte obsah hello hello následující text:
   
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
   
    Název balíčku hello oznámení je `org.apache.hadoop.examples` a název třídy hello je `WordCount`. Tyto názvy se používají, když odeslat úlohu MapReduce hello.

3. Uložte soubor hello.

## <a name="build-hello-application"></a>Vytvoření aplikace hello

1. Změnit toohello `wordcountjava` adresáře, pokud si nejste již existuje.

2. Použijte následující příkaz toobuild JAR soubor obsahující aplikace hello hello:

   ```
   mvn clean package
   ```

    Tento příkaz odstraní všechny předchozí artefakty sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány a pak sestavení a aplikace hello balíčku.

3. Jakmile hello dokončení příkazu hello `wordcountjava/target` adresář obsahuje soubor s názvem `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > Hello `wordcountjava-1.0-SNAPSHOT.jar` soubor je uberjar, obsahující nejen hello WordCount úlohy, ale je potřeba závislosti, které hello úlohy za běhu.

## <a id="upload"></a>Nahrát hello jar

Použijte následující příkaz tooupload hello jar souboru toohello HDInsight headnode hello:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

Tento příkaz zkopíruje soubory hello z hlavního uzlu toohello hello místní systém. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run"></a>Spustit úlohu MapReduce hello na Hadoop

1. Připojte tooHDInsight pomocí protokolu SSH. Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Z relace SSH hello použijte následující příkaz toorun hello MapReduce aplikace hello:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Tento příkaz spustí hello aplikace WordCount MapReduce. vstupní soubor Hello je `/example/data/gutenberg/davinci.txt`, a adresář výstup hello je `/example/data/wordcountout`. Vstupní soubor hello i výstupní jsou uložené toohello výchozí úložiště pro hello cluster.

3. Po dokončení úlohy hello, použijte následující příkaz tooview hello výsledky hello:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    Seznam slova a počty, mělo by se zobrazit s hodnotami podobné toohello následující text:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Další kroky

V tomto dokumentu jste se naučili jak toodevelop úlohu Java MapReduce. V tématu hello následující dokumenty, kde najdete další způsoby toowork s HDInsight.

* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Používání nástroje MapReduce s HDInsight](hdinsight-use-mapreduce.md)

Další informace najdete v tématu taky hello [středisko pro vývojáře Java](https://azure.microsoft.com/develop/java/).

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

