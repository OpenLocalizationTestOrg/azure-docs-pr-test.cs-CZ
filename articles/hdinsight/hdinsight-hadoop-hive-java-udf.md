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
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Použít Java UDF s Hive v HDInsight

Zjistěte, jak toocreate založené na jazyce Java uživatelem definované funkce (UDF), který funguje s Hive. v tomto příkladu Hello Java UDF převede tabulku textových řetězců tooall malá znaků.

## <a name="requirements"></a>Požadavky

* Cluster služby HDInsight 

    > [!IMPORTANT]
    > Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

    Většina kroky v tomto dokumentu fungovat na obou clusterech se systémem Windows a Linux. Ale hello postupem tooupload hello zkompilovat UDF toohello clusteru a spusťte ho jsou konkrétní clustery založené na tooLinux. Tooinformation, který lze použít s clustery se systémem Windows jsou k dispozici odkazy.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 nebo novější (nebo ekvivalentní, jako je například OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Textového editoru nebo Java IDE

    > [!IMPORTANT]
    > Pokud vytvoříte soubory Python v klientovi Windows hello, musíte použít editor, který používá LF jako ukončování řádků. Pokud si nejste jisti, zda vaše editor používá LF nebo Line FEED, přečtěte si téma hello [Poradce při potížích s](#troubleshooting) části pro postup odebrání hello CR znak.

## <a name="create-an-example-java-udf"></a>Vytvořte například Java UDF 

1. Z příkazového řádku použijte následující toocreate nový projekt Maven hello:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Pokud používáte prostředí PowerShell, je nutné umístit hello parametry v uvozovkách. Například, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Tento příkaz vytvoří adresář s názvem **exampleudf**, který obsahuje projekt Maven hello.

2. Po vytvoření projektu hello odstranit hello **exampleudf/src/testování** adresář, který byl vytvořen jako součást hello projektu.

3. Otevřete hello **exampleudf/pom.xml**a nahradit stávající hello `<dependencies>` položka se hello následující XML:

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

    Tyto položky zadejte hello verzi systému Hadoop a Hive součástí HDInsight 3.5. Můžete najít informace o verzích hello Hadoop a Hive s HDInsight získané z hello [Správa verzí komponenty HDInsight](hdinsight-component-versioning.md) dokumentu.

    Přidat `<build>` část před syntaxí hello `</project>` řádku na konci hello hello souboru. V této části by měl obsahovat hello následující XML:

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

    Tyto položky definovat, jak toobuild hello projektu. Konkrétně hello verzi jazyka Java, která hello používá projektu a jak toobuild uberjar pro cluster toohello nasazení.

    Uložte soubor hello po provedení změn hello.

4. Přejmenování **exampleudf/src/main/java/com/microsoft/examples/App.java** příliš**ExampleUDF.java**a pak otevřete soubor hello ve svém editoru.

5. Nahraďte obsah hello hello **ExampleUDF.java** soubor s hello následující a potom uložte soubor hello.

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

    Tento kód implementuje UDF, který přijímá hodnotu řetězce a vrátí malá verzi hello řetězec.

## <a name="build-and-install-hello-udf"></a>Sestavení a nainstalujte hello UDF

1. Použijte následující příkaz toocompile hello a balíček hello UDF:

    ```bash
    mvn compile package
    ```

    Tento příkaz vytvoří a balíčky hello UDF do hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` souboru.

2. Použití hello `scp` příkaz toocopy hello souboru toohello clusteru HDInsight.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Nahraďte `myuser` s hello účtu uživatele SSH pro váš cluster. Nahraďte `mycluster` s názvem clusteru hello. Pokud jste použili hello toosecure a heslo účtu SSH, jste výzvami tooenter hello heslo. Pokud jste použili certifikát, pravděpodobně bude třeba toouse hello `-i` parametr toospecify hello soubor privátního klíče.

3. Připojte toohello clusteru pomocí protokolu SSH.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

4. Z relace SSH hello zkopírujte úložiště tooHDInsight souborů jar hello.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a>Použít hello UDF z Hive

1. Použijte následující toostart hello Beeline klienta z relace SSH hello hello.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    Tento příkaz předpokládá, že jste použili výchozí hello **správce** pro hello přihlašovací účet pro váš cluster.

2. Až přijedete do hello `jdbc:hive2://localhost:10001/>` výzva, zadejte následující tooadd hello UDF tooHive hello a vystavit jako funkce.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > Tento příklad předpokládá, že Azure Storage je výchozí úložiště pro hello cluster. Pokud váš cluster používá místo Data Lake Store, změňte hello `wasb:///` hodnota příliš`adl:///`.

3. Použijte hello UDF tooconvert hodnoty získané z řetězce případu tabulky toolower.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Tento dotaz, vybere hello platforem zařízení (Android, Windows, iOS, atd.) z tabulky hello převést hello řetězec toolower případ a jejich zobrazení. Zobrazí výstup Hello podobné toohello následující text:

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

## <a name="next-steps"></a>Další kroky

Jiné způsoby toowork s Hive, najdete v části [používání Hive s HDInsight](hdinsight-use-hive.md).

Další informace týkající se Hive User-Defined funkcí najdete v tématu [Hive operátory a funkce definované uživatelem](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) části hello wiki Hive na apache.org.
