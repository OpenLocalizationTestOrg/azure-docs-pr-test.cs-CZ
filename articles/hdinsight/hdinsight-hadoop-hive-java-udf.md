---
title: "aaaJava uživatelem definované funkce (UDF) s Hive v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate založené na jazyce Java uživatelem definované funkce (UDF), který funguje s Hive. Tento příklad UDF převede tabulku toolowercase textového řetězce."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 392b4cfb73299d2f6c1e8e825a4201b48d501388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="29bc8-104">Použít Java UDF s Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="29bc8-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="29bc8-105">Zjistěte, jak toocreate založené na jazyce Java uživatelem definované funkce (UDF), který funguje s Hive.</span><span class="sxs-lookup"><span data-stu-id="29bc8-105">Learn how toocreate a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="29bc8-106">v tomto příkladu Hello Java UDF převede tabulku textových řetězců tooall malá znaků.</span><span class="sxs-lookup"><span data-stu-id="29bc8-106">hello Java UDF in this example converts a table of text strings tooall-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="29bc8-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="29bc8-107">Requirements</span></span>

* <span data-ttu-id="29bc8-108">Cluster služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="29bc8-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="29bc8-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="29bc8-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="29bc8-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="29bc8-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="29bc8-111">Většina kroky v tomto dokumentu fungovat na obou clusterech se systémem Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="29bc8-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="29bc8-112">Ale hello postupem tooupload hello zkompilovat UDF toohello clusteru a spusťte ho jsou konkrétní clustery založené na tooLinux.</span><span class="sxs-lookup"><span data-stu-id="29bc8-112">However, hello steps used tooupload hello compiled UDF toohello cluster and run it are specific tooLinux-based clusters.</span></span> <span data-ttu-id="29bc8-113">Tooinformation, který lze použít s clustery se systémem Windows jsou k dispozici odkazy.</span><span class="sxs-lookup"><span data-stu-id="29bc8-113">Links are provided tooinformation that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="29bc8-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="29bc8-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="29bc8-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="29bc8-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="29bc8-116">Textového editoru nebo Java IDE</span><span class="sxs-lookup"><span data-stu-id="29bc8-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="29bc8-117">Pokud vytvoříte soubory Python v klientovi Windows hello, musíte použít editor, který používá LF jako ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="29bc8-117">If you create hello Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="29bc8-118">Pokud si nejste jisti, zda vaše editor používá LF nebo Line FEED, přečtěte si téma hello [Poradce při potížích s](#troubleshooting) části pro postup odebrání hello CR znak.</span><span class="sxs-lookup"><span data-stu-id="29bc8-118">If you are not sure whether your editor uses LF or CRLF, see hello [Troubleshooting](#troubleshooting) section for steps on removing hello CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="29bc8-119">Vytvořte například Java UDF</span><span class="sxs-lookup"><span data-stu-id="29bc8-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="29bc8-120">Z příkazového řádku použijte následující toocreate nový projekt Maven hello:</span><span class="sxs-lookup"><span data-stu-id="29bc8-120">From a command line, use hello following toocreate a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="29bc8-121">Pokud používáte prostředí PowerShell, je nutné umístit hello parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="29bc8-121">If you are using PowerShell, you must put quotes around hello parameters.</span></span> <span data-ttu-id="29bc8-122">Například, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="29bc8-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="29bc8-123">Tento příkaz vytvoří adresář s názvem **exampleudf**, který obsahuje projekt Maven hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-123">This command creates a directory named **exampleudf**, which contains hello Maven project.</span></span>

2. <span data-ttu-id="29bc8-124">Po vytvoření projektu hello odstranit hello **exampleudf/src/testování** adresář, který byl vytvořen jako součást hello projektu.</span><span class="sxs-lookup"><span data-stu-id="29bc8-124">Once hello project has been created, delete hello **exampleudf/src/test** directory that was created as part of hello project.</span></span>

3. <span data-ttu-id="29bc8-125">Otevřete hello **exampleudf/pom.xml**a nahradit stávající hello `<dependencies>` položka se hello následující XML:</span><span class="sxs-lookup"><span data-stu-id="29bc8-125">Open hello **exampleudf/pom.xml**, and replace hello existing `<dependencies>` entry with hello following XML:</span></span>

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    <span data-ttu-id="29bc8-126">Tyto položky zadejte hello verzi systému Hadoop a Hive součástí HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="29bc8-126">These entries specify hello version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="29bc8-127">Můžete najít informace o verzích hello Hadoop a Hive s HDInsight získané z hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="29bc8-127">You can find information on hello versions of Hadoop and Hive provided with HDInsight from hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="29bc8-128">Přidat `<build>` část před syntaxí hello `</project>` řádku na konci hello hello souboru.</span><span class="sxs-lookup"><span data-stu-id="29bc8-128">Add a `<build>` section before hello `</project>` line at hello end of hello file.</span></span> <span data-ttu-id="29bc8-129">V této části by měl obsahovat hello následující XML:</span><span class="sxs-lookup"><span data-stu-id="29bc8-129">This section should contain hello following XML:</span></span>

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.5  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
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
        </plugins>
    </build>
    ```

    <span data-ttu-id="29bc8-130">Tyto položky definovat, jak toobuild hello projektu.</span><span class="sxs-lookup"><span data-stu-id="29bc8-130">These entries define how toobuild hello project.</span></span> <span data-ttu-id="29bc8-131">Konkrétně hello verzi jazyka Java, která hello používá projektu a jak toobuild uberjar pro cluster toohello nasazení.</span><span class="sxs-lookup"><span data-stu-id="29bc8-131">Specifically, hello version of Java that hello project uses and how toobuild an uberjar for deployment toohello cluster.</span></span>

    <span data-ttu-id="29bc8-132">Uložte soubor hello po provedení změn hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-132">Save hello file once hello changes have been made.</span></span>

4. <span data-ttu-id="29bc8-133">Přejmenování **exampleudf/src/main/java/com/microsoft/examples/App.java** příliš**ExampleUDF.java**a pak otevřete soubor hello ve svém editoru.</span><span class="sxs-lookup"><span data-stu-id="29bc8-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** too**ExampleUDF.java**, and then open hello file in your editor.</span></span>

5. <span data-ttu-id="29bc8-134">Nahraďte obsah hello hello **ExampleUDF.java** soubor s hello následující a potom uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-134">Replace hello contents of hello **ExampleUDF.java** file with hello following, then save hello file.</span></span>

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of hello UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of hello input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If hello value is null, return a null
            if(input == null)
                return null;
            // Lowercase hello input string and return it
            return input.toLowerCase();
        }
    }
    ```

    <span data-ttu-id="29bc8-135">Tento kód implementuje UDF, který přijímá hodnotu řetězce a vrátí malá verzi hello řetězec.</span><span class="sxs-lookup"><span data-stu-id="29bc8-135">This code implements a UDF that accepts a string value, and returns a lowercase version of hello string.</span></span>

## <a name="build-and-install-hello-udf"></a><span data-ttu-id="29bc8-136">Sestavení a nainstalujte hello UDF</span><span class="sxs-lookup"><span data-stu-id="29bc8-136">Build and install hello UDF</span></span>

1. <span data-ttu-id="29bc8-137">Použijte následující příkaz toocompile hello a balíček hello UDF:</span><span class="sxs-lookup"><span data-stu-id="29bc8-137">Use hello following command toocompile and package hello UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="29bc8-138">Tento příkaz vytvoří a balíčky hello UDF do hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` souboru.</span><span class="sxs-lookup"><span data-stu-id="29bc8-138">This command builds and packages hello UDF into hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="29bc8-139">Použití hello `scp` příkaz toocopy hello souboru toohello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="29bc8-139">Use hello `scp` command toocopy hello file toohello HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="29bc8-140">Nahraďte `myuser` s hello účtu uživatele SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="29bc8-140">Replace `myuser` with hello SSH user account for your cluster.</span></span> <span data-ttu-id="29bc8-141">Nahraďte `mycluster` s názvem clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-141">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="29bc8-142">Pokud jste použili hello toosecure a heslo účtu SSH, jste výzvami tooenter hello heslo.</span><span class="sxs-lookup"><span data-stu-id="29bc8-142">If you used a password toosecure hello SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="29bc8-143">Pokud jste použili certifikát, pravděpodobně bude třeba toouse hello `-i` parametr toospecify hello soubor privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="29bc8-143">If you used a certificate, you may need toouse hello `-i` parameter toospecify hello private key file.</span></span>

3. <span data-ttu-id="29bc8-144">Připojte toohello clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="29bc8-144">Connect toohello cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="29bc8-145">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="29bc8-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="29bc8-146">Z relace SSH hello zkopírujte úložiště tooHDInsight souborů jar hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-146">From hello SSH session, copy hello jar file tooHDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a><span data-ttu-id="29bc8-147">Použít hello UDF z Hive</span><span class="sxs-lookup"><span data-stu-id="29bc8-147">Use hello UDF from Hive</span></span>

1. <span data-ttu-id="29bc8-148">Použijte následující toostart hello Beeline klienta z relace SSH hello hello.</span><span class="sxs-lookup"><span data-stu-id="29bc8-148">Use hello following toostart hello Beeline client from hello SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="29bc8-149">Tento příkaz předpokládá, že jste použili výchozí hello **správce** pro hello přihlašovací účet pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="29bc8-149">This command assumes that you used hello default of **admin** for hello login account for your cluster.</span></span>

2. <span data-ttu-id="29bc8-150">Až přijedete do hello `jdbc:hive2://localhost:10001/>` výzva, zadejte následující tooadd hello UDF tooHive hello a vystavit jako funkce.</span><span class="sxs-lookup"><span data-stu-id="29bc8-150">Once you arrive at hello `jdbc:hive2://localhost:10001/>` prompt, enter hello following tooadd hello UDF tooHive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="29bc8-151">Tento příklad předpokládá, že Azure Storage je výchozí úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="29bc8-151">This example assumes that Azure Storage is default storage for hello cluster.</span></span> <span data-ttu-id="29bc8-152">Pokud váš cluster používá místo Data Lake Store, změňte hello `wasb:///` hodnota příliš`adl:///`.</span><span class="sxs-lookup"><span data-stu-id="29bc8-152">If your cluster uses Data Lake Store instead, change hello `wasb:///` value too`adl:///`.</span></span>

3. <span data-ttu-id="29bc8-153">Použijte hello UDF tooconvert hodnoty získané z řetězce případu tabulky toolower.</span><span class="sxs-lookup"><span data-stu-id="29bc8-153">Use hello UDF tooconvert values retrieved from a table toolower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="29bc8-154">Tento dotaz, vybere hello platforem zařízení (Android, Windows, iOS, atd.) z tabulky hello převést hello řetězec toolower případ a jejich zobrazení.</span><span class="sxs-lookup"><span data-stu-id="29bc8-154">This query selects hello device platform (Android, Windows, iOS, etc.) from hello table, convert hello string toolower case, and then display them.</span></span> <span data-ttu-id="29bc8-155">Zobrazí výstup Hello podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="29bc8-155">hello output appears similar toohello following text:</span></span>

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a><span data-ttu-id="29bc8-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="29bc8-156">Next steps</span></span>

<span data-ttu-id="29bc8-157">Jiné způsoby toowork s Hive, najdete v části [používání Hive s HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="29bc8-157">For other ways toowork with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="29bc8-158">Další informace týkající se Hive User-Defined funkcí najdete v tématu [Hive operátory a funkce definované uživatelem](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) části hello wiki Hive na apache.org.</span><span class="sxs-lookup"><span data-stu-id="29bc8-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of hello Hive wiki at apache.org.</span></span>
