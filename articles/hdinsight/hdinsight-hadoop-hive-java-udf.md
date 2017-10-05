---
title: "Java uživatelem definované funkce (UDF) s Hive v HDInsight - Azure | Microsoft Docs"
description: "Postup vytvoření založené na jazyce Java uživatelem definované funkce (UDF), funguje s Hive. Tento příklad UDF převede tabulku textové řetězce na malá písmena."
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
ms.openlocfilehash: 481d234eaf88bdb210821084ee4154159470eda0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="8228d-104">Použít Java UDF s Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="8228d-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="8228d-105">Postup vytvoření založené na jazyce Java uživatelem definované funkce (UDF), funguje s Hive.</span><span class="sxs-lookup"><span data-stu-id="8228d-105">Learn how to create a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="8228d-106">Java UDF v tomto příkladu převede tabulku textové řetězce na malá všechny znaky.</span><span class="sxs-lookup"><span data-stu-id="8228d-106">The Java UDF in this example converts a table of text strings to all-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="8228d-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8228d-107">Requirements</span></span>

* <span data-ttu-id="8228d-108">Cluster služby HDInsight</span><span class="sxs-lookup"><span data-stu-id="8228d-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="8228d-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="8228d-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8228d-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8228d-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="8228d-111">Většina kroky v tomto dokumentu fungovat na obou clusterech se systémem Windows a Linux.</span><span class="sxs-lookup"><span data-stu-id="8228d-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="8228d-112">Nicméně jsou specifické pro clustery se systémem Linux s postupem nahrání kompilované UDF do clusteru a potom ho spusťte.</span><span class="sxs-lookup"><span data-stu-id="8228d-112">However, the steps used to upload the compiled UDF to the cluster and run it are specific to Linux-based clusters.</span></span> <span data-ttu-id="8228d-113">Jsou uvedeny odkazy na informace, které lze použít s clustery se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="8228d-113">Links are provided to information that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="8228d-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="8228d-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="8228d-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="8228d-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="8228d-116">Textového editoru nebo Java IDE</span><span class="sxs-lookup"><span data-stu-id="8228d-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8228d-117">Pokud vytvoříte soubory Python v klientovi Windows, musíte použít editor, který používá LF jako ukončování řádků.</span><span class="sxs-lookup"><span data-stu-id="8228d-117">If you create the Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="8228d-118">Pokud si nejste jisti, zda vaše editor používá LF nebo Line FEED, přečtěte si téma [Poradce při potížích s](#troubleshooting) na odebrání znak CR kroky v sekci.</span><span class="sxs-lookup"><span data-stu-id="8228d-118">If you are not sure whether your editor uses LF or CRLF, see the [Troubleshooting](#troubleshooting) section for steps on removing the CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="8228d-119">Vytvořte například Java UDF</span><span class="sxs-lookup"><span data-stu-id="8228d-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="8228d-120">Z příkazového řádku použijte následující postupy k vytvoření nového projektu Maven:</span><span class="sxs-lookup"><span data-stu-id="8228d-120">From a command line, use the following to create a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="8228d-121">Pokud používáte prostředí PowerShell, je nutné umístit parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="8228d-121">If you are using PowerShell, you must put quotes around the parameters.</span></span> <span data-ttu-id="8228d-122">Například, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="8228d-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="8228d-123">Tento příkaz vytvoří adresář s názvem **exampleudf**, který obsahuje projekt Maven.</span><span class="sxs-lookup"><span data-stu-id="8228d-123">This command creates a directory named **exampleudf**, which contains the Maven project.</span></span>

2. <span data-ttu-id="8228d-124">Po vytvoření projektu odstranit **exampleudf/src/testování** adresář, který byl vytvořen jako součást projektu.</span><span class="sxs-lookup"><span data-stu-id="8228d-124">Once the project has been created, delete the **exampleudf/src/test** directory that was created as part of the project.</span></span>

3. <span data-ttu-id="8228d-125">Otevřete **exampleudf/pom.xml**a nahradit existující `<dependencies>` položka se následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="8228d-125">Open the **exampleudf/pom.xml**, and replace the existing `<dependencies>` entry with the following XML:</span></span>

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

    <span data-ttu-id="8228d-126">Tyto položky zadejte verzi systému Hadoop a Hive součástí HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="8228d-126">These entries specify the version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="8228d-127">Informace o verzích Hadoop a Hive s HDInsight z můžete najít [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8228d-127">You can find information on the versions of Hadoop and Hive provided with HDInsight from the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="8228d-128">Přidat `<build>` části před `</project>` řádek na konec souboru.</span><span class="sxs-lookup"><span data-stu-id="8228d-128">Add a `<build>` section before the `</project>` line at the end of the file.</span></span> <span data-ttu-id="8228d-129">V této části by měl obsahovat následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="8228d-129">This section should contain the following XML:</span></span>

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

    <span data-ttu-id="8228d-130">Tyto položky definovat, jak sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="8228d-130">These entries define how to build the project.</span></span> <span data-ttu-id="8228d-131">Konkrétně verze Java, která používá projektu a jak sestavit uberjar pro nasazení do clusteru.</span><span class="sxs-lookup"><span data-stu-id="8228d-131">Specifically, the version of Java that the project uses and how to build an uberjar for deployment to the cluster.</span></span>

    <span data-ttu-id="8228d-132">Po provedení změn uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="8228d-132">Save the file once the changes have been made.</span></span>

4. <span data-ttu-id="8228d-133">Přejmenování **exampleudf/src/main/java/com/microsoft/examples/App.java** k **ExampleUDF.java**a pak otevřete soubor ve svém editoru.</span><span class="sxs-lookup"><span data-stu-id="8228d-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** to **ExampleUDF.java**, and then open the file in your editor.</span></span>

5. <span data-ttu-id="8228d-134">Nahraďte obsah **ExampleUDF.java** soubor s následující a pak soubor uložte.</span><span class="sxs-lookup"><span data-stu-id="8228d-134">Replace the contents of the **ExampleUDF.java** file with the following, then save the file.</span></span>

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    <span data-ttu-id="8228d-135">Tento kód implementuje UDF, který přijímá hodnotu řetězce a vrátí malá verzi řetězce.</span><span class="sxs-lookup"><span data-stu-id="8228d-135">This code implements a UDF that accepts a string value, and returns a lowercase version of the string.</span></span>

## <a name="build-and-install-the-udf"></a><span data-ttu-id="8228d-136">Sestavení a nainstalujte UDF</span><span class="sxs-lookup"><span data-stu-id="8228d-136">Build and install the UDF</span></span>

1. <span data-ttu-id="8228d-137">Pro zkompilování a balíček UDF použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8228d-137">Use the following command to compile and package the UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="8228d-138">Tento příkaz vytvoří a balíčky UDF do `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` souboru.</span><span class="sxs-lookup"><span data-stu-id="8228d-138">This command builds and packages the UDF into the `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="8228d-139">Použití `scp` příkaz pro kopírování souboru do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8228d-139">Use the `scp` command to copy the file to the HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="8228d-140">Nahraďte `myuser` pomocí uživatelského účtu SSH pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="8228d-140">Replace `myuser` with the SSH user account for your cluster.</span></span> <span data-ttu-id="8228d-141">Nahraďte `mycluster` se název clusteru.</span><span class="sxs-lookup"><span data-stu-id="8228d-141">Replace `mycluster` with the cluster name.</span></span> <span data-ttu-id="8228d-142">Pokud jste použili heslo k zabezpečení účtu SSH, zobrazí se výzva k zadání hesla.</span><span class="sxs-lookup"><span data-stu-id="8228d-142">If you used a password to secure the SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="8228d-143">Pokud jste použili certifikát, budete možná muset použít `-i` parametr k určení souboru privátního klíče.</span><span class="sxs-lookup"><span data-stu-id="8228d-143">If you used a certificate, you may need to use the `-i` parameter to specify the private key file.</span></span>

3. <span data-ttu-id="8228d-144">Připojte se ke clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="8228d-144">Connect to the cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="8228d-145">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8228d-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="8228d-146">Z relace SSH zkopírujte soubor jar do úložiště HDInsight.</span><span class="sxs-lookup"><span data-stu-id="8228d-146">From the SSH session, copy the jar file to HDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a><span data-ttu-id="8228d-147">Použití UDF z Hive</span><span class="sxs-lookup"><span data-stu-id="8228d-147">Use the UDF from Hive</span></span>

1. <span data-ttu-id="8228d-148">Použijte následující spuštění klienta Beeline z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="8228d-148">Use the following to start the Beeline client from the SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="8228d-149">Tento příkaz předpokládá, že jste použili výchozí **správce** pro přihlašovací účet pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="8228d-149">This command assumes that you used the default of **admin** for the login account for your cluster.</span></span>

2. <span data-ttu-id="8228d-150">Až přijedete do `jdbc:hive2://localhost:10001/>` výzva, zadejte následující příkaz a přidejte UDF do Hive a vystavit jako funkce.</span><span class="sxs-lookup"><span data-stu-id="8228d-150">Once you arrive at the `jdbc:hive2://localhost:10001/>` prompt, enter the following to add the UDF to Hive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="8228d-151">Tento příklad předpokládá, že Azure Storage je výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="8228d-151">This example assumes that Azure Storage is default storage for the cluster.</span></span> <span data-ttu-id="8228d-152">Pokud váš cluster používá místo Data Lake Store, změňte `wasb:///` hodnotu `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="8228d-152">If your cluster uses Data Lake Store instead, change the `wasb:///` value to `adl:///`.</span></span>

3. <span data-ttu-id="8228d-153">Použijte UDF k převedení hodnoty získané z tabulky na řetězce na malá písmena.</span><span class="sxs-lookup"><span data-stu-id="8228d-153">Use the UDF to convert values retrieved from a table to lower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="8228d-154">Tento dotaz vybere platforem zařízení (Android, Windows, iOS, atd.) z tabulky, převést řetězec, který má nižší případ a jejich zobrazení.</span><span class="sxs-lookup"><span data-stu-id="8228d-154">This query selects the device platform (Android, Windows, iOS, etc.) from the table, convert the string to lower case, and then display them.</span></span> <span data-ttu-id="8228d-155">Výstup se zobrazí podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="8228d-155">The output appears similar to the following text:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="8228d-156">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8228d-156">Next steps</span></span>

<span data-ttu-id="8228d-157">Další způsoby, jak pracovat s Hive, najdete v části [používání Hive s HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="8228d-157">For other ways to work with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="8228d-158">Další informace týkající se Hive User-Defined funkcí najdete v tématu [Hive operátory a funkce definované uživatelem](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) části wiki Hive na apache.org.</span><span class="sxs-lookup"><span data-stu-id="8228d-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of the Hive wiki at apache.org.</span></span>
