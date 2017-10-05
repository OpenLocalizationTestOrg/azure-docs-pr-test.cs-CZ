---
title: Klient Java HBase - Azure HDInsight | Microsoft Docs
description: "Další informace o použití Apache Maven k sestavení aplikace založené na jazyce Java Apache HBase a potom ji nasadit do HBase v Azure HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 03c88397e36c0fc7f19410e49f6b6f1a607659f8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="build-java-applications-for-apache-hbase"></a><span data-ttu-id="ccf53-103">Sestavení aplikací Java pro Apache HBase</span><span class="sxs-lookup"><span data-stu-id="ccf53-103">Build Java applications for Apache HBase</span></span>

<span data-ttu-id="ccf53-104">Naučte se vytvářet [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="ccf53-104">Learn how to create an [Apache HBase](http://hbase.apache.org/) application in Java.</span></span> <span data-ttu-id="ccf53-105">Pak použijete aplikaci s HBase v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-105">Then use the application with HBase on Azure HDInsight.</span></span>

<span data-ttu-id="ccf53-106">Kroky v tomto dokumentu použití [Maven](http://maven.apache.org/) vytvořte a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="ccf53-106">The steps in this document use [Maven](http://maven.apache.org/) to create and build the project.</span></span> <span data-ttu-id="ccf53-107">Maven je správa softwaru projektu a míru porozumění nástroj, který umožňuje vytvářet softwaru, dokumentace a sestav pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="ccf53-107">Maven is a software project management and comprehension tool that allows you to build software, documentation, and reports for Java projects.</span></span>

> [!NOTE]
> <span data-ttu-id="ccf53-108">Kroky v tomto dokumentu byly naposledy testovány s HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="ccf53-108">The steps in this document were most recently tested with HDInsight 3.6.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ccf53-109">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="ccf53-109">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="ccf53-110">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="ccf53-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ccf53-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ccf53-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="ccf53-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ccf53-112">Requirements</span></span>

* <span data-ttu-id="ccf53-113">[Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ccf53-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 or later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ccf53-114">HDInsight 3.5 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="ccf53-114">HDInsight 3.5 and greater requires Java 8.</span></span> <span data-ttu-id="ccf53-115">Dřívějších verzích HDInsight vyžadují Java 7.</span><span class="sxs-lookup"><span data-stu-id="ccf53-115">Earlier versions of HDInsight require Java 7.</span></span>

* [<span data-ttu-id="ccf53-116">Maven</span><span class="sxs-lookup"><span data-stu-id="ccf53-116">Maven</span></span>](http://maven.apache.org/)

* [<span data-ttu-id="ccf53-117">Cluster Azure HDInsight se systémem Linux s HBase</span><span class="sxs-lookup"><span data-stu-id="ccf53-117">A Linux-based Azure HDInsight cluster with HBase</span></span>](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > <span data-ttu-id="ccf53-118">Kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.4 a 3.5.</span><span class="sxs-lookup"><span data-stu-id="ccf53-118">The steps in this document have been tested with HDInsight cluster versions 3.4 and 3.5.</span></span> <span data-ttu-id="ccf53-119">Výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="ccf53-119">The default values provided in examples are for a HDInsight 3.5 cluster.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="ccf53-120">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="ccf53-120">Create the project</span></span>

1. <span data-ttu-id="ccf53-121">Z příkazového řádku ve vašem vývojovém prostředí, změňte adresáře na umístění, kde chcete vytvořit projekt, například `cd code\hbase`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-121">From the command line in your development environment, change directories to the location where you want to create the project, for example, `cd code\hbase`.</span></span>

2. <span data-ttu-id="ccf53-122">Použití **mvn** příkazu, který se instaluje s Maven, vygenerujte generování uživatelského rozhraní pro projekt.</span><span class="sxs-lookup"><span data-stu-id="ccf53-122">Use the **mvn** command, which is installed with Maven, to generate the scaffolding for the project.</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > <span data-ttu-id="ccf53-123">Pokud používáte prostředí PowerShell, je nutné uzavřít `-D` parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="ccf53-123">If you are using PowerShell, you must enclose the `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="ccf53-124">Tento příkaz vytvoří adresář se stejným názvem jako **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="ccf53-124">This command creates a directory with the same name as the **artifactID** parameter (**hbaseapp** in this example.) This directory contains the following items:</span></span>

   * <span data-ttu-id="ccf53-125">**pom.xml**: projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje podrobnosti o informace a konfigurace použít k sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-125">**pom.xml**:  The Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used to build the project.</span></span>
   * <span data-ttu-id="ccf53-126">**src**: adresář, který obsahuje **main/java/com/microsoft/příklady** adresáře, kde můžete vytvářet aplikace.</span><span class="sxs-lookup"><span data-stu-id="ccf53-126">**src**: The directory that contains the **main/java/com/microsoft/examples** directory, where you author the application.</span></span>

3. <span data-ttu-id="ccf53-127">Odstranit `src/test/java/com/microsoft/examples/apptest.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-127">Delete the `src/test/java/com/microsoft/examples/apptest.java` file.</span></span> <span data-ttu-id="ccf53-128">Není možné použít v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-128">It is not be used in this example.</span></span>

## <a name="update-the-project-object-model"></a><span data-ttu-id="ccf53-129">Aktualizace projektu objektový Model</span><span class="sxs-lookup"><span data-stu-id="ccf53-129">Update the Project Object Model</span></span>

1. <span data-ttu-id="ccf53-130">Upravit `pom.xml` souboru a přidejte následující kód do `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="ccf53-130">Edit the `pom.xml` file and add the following code inside the `<dependencies>` section:</span></span>

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    <span data-ttu-id="ccf53-131">V této části ukazuje, že projekt vyžaduje **hbase-client** a **phoenix základní** součásti.</span><span class="sxs-lookup"><span data-stu-id="ccf53-131">This section indicates that the project needs **hbase-client** and **phoenix-core** components.</span></span> <span data-ttu-id="ccf53-132">Při kompilaci se stáhnou tyto závislosti z úložiště Maven výchozí.</span><span class="sxs-lookup"><span data-stu-id="ccf53-132">At compile time, these dependencies are downloaded from the default Maven repository.</span></span> <span data-ttu-id="ccf53-133">Můžete použít [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) Další informace o této závislosti.</span><span class="sxs-lookup"><span data-stu-id="ccf53-133">You can use the [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) to learn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="ccf53-134">Číslo verze klienta hbase musí odpovídat verzi HBase, který je zadán v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-134">The version number of the hbase-client must match the version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="ccf53-135">Pomocí následující tabulky najít číslo správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="ccf53-135">Use the following table to find the correct version number.</span></span>

   | <span data-ttu-id="ccf53-136">Verze clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="ccf53-136">HDInsight cluster version</span></span> | <span data-ttu-id="ccf53-137">HBase verze se má použít</span><span class="sxs-lookup"><span data-stu-id="ccf53-137">HBase version to use</span></span> |
   | --- | --- |
   | <span data-ttu-id="ccf53-138">3.2</span><span class="sxs-lookup"><span data-stu-id="ccf53-138">3.2</span></span> |<span data-ttu-id="ccf53-139">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="ccf53-139">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="ccf53-140">3.3, 3.4, 3.5 a 3.6</span><span class="sxs-lookup"><span data-stu-id="ccf53-140">3.3, 3.4, 3.5, and 3.6</span></span> |<span data-ttu-id="ccf53-141">1.1.2</span><span class="sxs-lookup"><span data-stu-id="ccf53-141">1.1.2</span></span> |

    <span data-ttu-id="ccf53-142">Další informace o verzích HDInsight a součásti najdete v tématu [co jsou různé součásti Hadoop, která je k dispozici s HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="ccf53-142">For more information on HDInsight versions and components, see [What are the different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>

3. <span data-ttu-id="ccf53-143">Přidejte následující kód, který **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-143">Add the following code to the **pom.xml** file.</span></span> <span data-ttu-id="ccf53-144">Tento text musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-144">This text must be inside the `<project>...</project>` tags in the file, for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
        <resource>
            <directory>${basedir}/conf</directory>
            <filtering>false</filtering>
            <includes>
            <include>hbase-site.xml</include>
            </includes>
        </resource>
        </resources>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
            </plugin>
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
        </plugins>
    </build>
   ```

    <span data-ttu-id="ccf53-145">Tato část nakonfiguruje prostředku (`conf/hbase-site.xml`) obsahující informace o konfiguraci pro HBase.</span><span class="sxs-lookup"><span data-stu-id="ccf53-145">This section configures a resource (`conf/hbase-site.xml`) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ccf53-146">Můžete také nastavit hodnoty konfigurace prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-146">You can also set configuration values via code.</span></span> <span data-ttu-id="ccf53-147">Zobrazte komentáře ve `CreateTable` příklad.</span><span class="sxs-lookup"><span data-stu-id="ccf53-147">See the comments in the `CreateTable` example.</span></span>

    <span data-ttu-id="ccf53-148">Tato část také nakonfiguruje [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="ccf53-148">This section also configures the [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="ccf53-149">Modul plug-in kompilátoru se používá ke kompilaci topologii.</span><span class="sxs-lookup"><span data-stu-id="ccf53-149">The compiler plug-in is used to compile the topology.</span></span> <span data-ttu-id="ccf53-150">Modul plug-in stín se používá při prevenci licence duplikace v JAR balíček, který je sestavena Maven.</span><span class="sxs-lookup"><span data-stu-id="ccf53-150">The shade plug-in is used to prevent license duplication in the JAR package that is built by Maven.</span></span> <span data-ttu-id="ccf53-151">Tento modul plug-in se používá při prevenci "duplicitní licence soubory" Chyba za běhu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-151">This plugin is used to prevent a "duplicate license files" error at run time on the HDInsight cluster.</span></span> <span data-ttu-id="ccf53-152">Používání maven stín – modul plug-in s `ApacheLicenseResourceTransformer` implementace brání chyba.</span><span class="sxs-lookup"><span data-stu-id="ccf53-152">Using maven-shade-plugin with the `ApacheLicenseResourceTransformer` implementation prevents the error.</span></span>

    <span data-ttu-id="ccf53-153">Plugin stín maven vytvoří také uber jar, který obsahuje všechny závislosti, které jsou požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="ccf53-153">The maven-shade-plugin also produces an uber jar that contains all the dependencies required by the application.</span></span>

4. <span data-ttu-id="ccf53-154">Uložit `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-154">Save the `pom.xml` file.</span></span>

5. <span data-ttu-id="ccf53-155">Vytvořte adresář s názvem `conf` v `hbaseapp` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ccf53-155">Create a directory named `conf` in the `hbaseapp` directory.</span></span> <span data-ttu-id="ccf53-156">Tento adresář se používá k uložení konfigurační informace pro připojení k HBase.</span><span class="sxs-lookup"><span data-stu-id="ccf53-156">This directory is used to hold configuration information for connecting to HBase.</span></span>

6. <span data-ttu-id="ccf53-157">Použijte následující příkaz pro kopírování HBase konfigurace z clusteru HBase `conf` adresáře.</span><span class="sxs-lookup"><span data-stu-id="ccf53-157">Use the following command to copy the HBase configuration from the HBase cluster to the `conf` directory.</span></span> <span data-ttu-id="ccf53-158">Nahraďte `USERNAME` s názvem vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="ccf53-158">Replace `USERNAME` with the name of your SSH login.</span></span> <span data-ttu-id="ccf53-159">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ccf53-159">Replace `CLUSTERNAME` with your HDInsight cluster name:</span></span>

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   <span data-ttu-id="ccf53-160">Další informace o používání `ssh` a `scp`, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="ccf53-160">For more information on using `ssh` and `scp`, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="ccf53-161">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="ccf53-161">Create the application</span></span>

1. <span data-ttu-id="ccf53-162">Přejděte na `hbaseapp/src/main/java/com/microsoft/examples` adresáře a přejmenujte app.java soubor na `CreateTable.java`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-162">Go to the `hbaseapp/src/main/java/com/microsoft/examples` directory and rename the app.java file to `CreateTable.java`.</span></span>

2. <span data-ttu-id="ccf53-163">Otevřete `CreateTable.java` souboru a nahradit existující obsah s následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="ccf53-163">Open the `CreateTable.java` file and replace the existing contents with the following text:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;
    import org.apache.hadoop.hbase.HTableDescriptor;
    import org.apache.hadoop.hbase.TableName;
    import org.apache.hadoop.hbase.HColumnDescriptor;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Put;
    import org.apache.hadoop.hbase.util.Bytes;

    public class CreateTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Example of setting zookeeper values for HDInsight
        // in code instead of an hbase-site.xml file
        //
        // config.set("hbase.zookeeper.quorum",
        //            "zookeepernode0,zookeepernode1,zookeepernode2");
        //config.set("hbase.zookeeper.property.clientPort", "2181");
        //config.set("hbase.cluster.distributed", "true");
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create the table...
        HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
        // ... with two column families
        tableDescriptor.addFamily(new HColumnDescriptor("name"));
        tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
        admin.createTable(tableDescriptor);

        // define some people
        String[][] people = {
            { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
            { "2", "Franklin", "Holtz", "franklin@contoso.com" },
            { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
            { "4", "Rae", "Schroeder", "rae@contoso.com" },
            { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
            { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

        HTable table = new HTable(config, "people");

        // Add each person to the table
        //   Use the `name` column family for the name
        //   Use the `contactinfo` column family for the email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close the table
        table.flushCommits();
        table.close();
        }
    }
   ```

    <span data-ttu-id="ccf53-164">Tento kód je **CreateTable** třídy, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.</span><span class="sxs-lookup"><span data-stu-id="ccf53-164">This code is the **CreateTable** class, which creates a table named **people** and populate it with some predefined users.</span></span>

3. <span data-ttu-id="ccf53-165">Uložit `CreateTable.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-165">Save the `CreateTable.java` file.</span></span>

4. <span data-ttu-id="ccf53-166">V `hbaseapp/src/main/java/com/microsoft/examples` adresáře, vytvořte soubor s názvem `SearchByEmail.java`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-166">In the `hbaseapp/src/main/java/com/microsoft/examples` directory, create a file named `SearchByEmail.java`.</span></span> <span data-ttu-id="ccf53-167">Jako obsah tohoto souboru použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="ccf53-167">Use the following text as the contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Scan;
    import org.apache.hadoop.hbase.client.ResultScanner;
    import org.apache.hadoop.hbase.client.Result;
    import org.apache.hadoop.hbase.filter.RegexStringComparator;
    import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
    import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
    import org.apache.hadoop.hbase.util.Bytes;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class SearchByEmail {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Use GenericOptionsParser to get only the parameters to the class
        // and not all the parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open the table
        HTable table = new HTable(config, "people");

        // Define the family and qualifiers to be used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach the regex filter to a filter
        //   for the email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set the filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get the results
        ResultScanner results = table.getScanner(scan);
        // Iterate over results and print  values
        for (Result result : results ) {
            String id = new String(result.getRow());
            byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
            String firstName = new String(firstNameObj);
            byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
            String lastName = new String(lastNameObj);
            System.out.println(firstName + " " + lastName + " - ID: " + id);
            byte[] emailObj = result.getValue(contactFamily, emailQualifier);
            String email = new String(emailObj);
            System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
        }
        results.close();
        table.close();
        }
    }
   ```

    <span data-ttu-id="ccf53-168">**SearchByEmail** třídu lze použít k dotazu pro řádků a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-168">The **SearchByEmail** class can be used to query for rows by email address.</span></span> <span data-ttu-id="ccf53-169">Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při používání třídy.</span><span class="sxs-lookup"><span data-stu-id="ccf53-169">Because it uses a regular expression filter, you can provide either a string or a regular expression when using the class.</span></span>

5. <span data-ttu-id="ccf53-170">Uložit `SearchByEmail.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-170">Save the `SearchByEmail.java` file.</span></span>

6. <span data-ttu-id="ccf53-171">V `hbaseapp/src/main/hava/com/microsoft/examples` adresáře, vytvořte soubor s názvem `DeleteTable.java`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-171">In the `hbaseapp/src/main/hava/com/microsoft/examples` directory, create a file named `DeleteTable.java`.</span></span> <span data-ttu-id="ccf53-172">Jako obsah tohoto souboru použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="ccf53-172">Use the following text as the contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete the table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    <span data-ttu-id="ccf53-173">Tato třída vyčistí tabulek HBase vytvořené zakázání a vkládání v tabulce vytvořené v tomto příkladu `CreateTable` třídy.</span><span class="sxs-lookup"><span data-stu-id="ccf53-173">This class cleans up the HBase tables created in this example by disabling and dropping the table created by the `CreateTable` class.</span></span>

7. <span data-ttu-id="ccf53-174">Uložit `DeleteTable.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-174">Save the `DeleteTable.java` file.</span></span>

## <a name="build-and-package-the-application"></a><span data-ttu-id="ccf53-175">Sestavení a balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="ccf53-175">Build and package the application</span></span>

1. <span data-ttu-id="ccf53-176">Z `hbaseapp` adresáře, použijte následující příkaz k vytvoření soubor JAR, která obsahuje aplikaci:</span><span class="sxs-lookup"><span data-stu-id="ccf53-176">From the `hbaseapp` directory, use the following command to build a JAR file that contains the application:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="ccf53-177">Tento příkaz vytvoří a balíčky aplikací do souboru .jar.</span><span class="sxs-lookup"><span data-stu-id="ccf53-177">This command builds and packages the application into a .jar file.</span></span>

2. <span data-ttu-id="ccf53-178">Po dokončení příkazu `hbaseapp/target` adresář obsahuje soubor s názvem `hbaseapp-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-178">When the command completes, the `hbaseapp/target` directory contains a file named `hbaseapp-1.0-SNAPSHOT.jar`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ccf53-179">`hbaseapp-1.0-SNAPSHOT.jar` Je soubor jar uber.</span><span class="sxs-lookup"><span data-stu-id="ccf53-179">The `hbaseapp-1.0-SNAPSHOT.jar` file is an uber jar.</span></span> <span data-ttu-id="ccf53-180">Obsahuje všechny závislosti potřebné ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="ccf53-180">It contains all the dependencies required to run the application.</span></span>


## <a name="upload-the-jar-and-run-jobs-ssh"></a><span data-ttu-id="ccf53-181">Nahrát JAR a spouštění úloh (SSH)</span><span class="sxs-lookup"><span data-stu-id="ccf53-181">Upload the JAR and run jobs (SSH)</span></span>

<span data-ttu-id="ccf53-182">Následující postup použijte `scp` zkopírovat JAR do hlavního uzlu primární vaší databáze hbase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-182">The following steps use `scp` to copy the JAR to the primary head node of your HBase on HDInsight cluster.</span></span> <span data-ttu-id="ccf53-183">`ssh` Příkaz se pak používá k připojení ke clusteru a spusťte v příkladu přímo na hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-183">The `ssh` command is then used to connect to the cluster and run the example directly on the head node.</span></span>

1. <span data-ttu-id="ccf53-184">Nahrát jar do clusteru, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-184">To upload the jar to the cluster, use the following command:</span></span>

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="ccf53-185">Nahraďte `USERNAME` s názvem vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="ccf53-185">Replace `USERNAME` with the name of your SSH login.</span></span> <span data-ttu-id="ccf53-186">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-186">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

2. <span data-ttu-id="ccf53-187">Pokud chcete připojit ke clusteru HBase, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-187">To connect to the HBase cluster, use the following command:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="ccf53-188">Nahraďte `USERNAME` název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="ccf53-188">Replace `USERNAME` the name of your SSH login.</span></span> <span data-ttu-id="ccf53-189">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-189">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

3. <span data-ttu-id="ccf53-190">K vytvoření tabulky HBase pomocí aplikace Java, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-190">To create an HBase table using the Java application, use the following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    <span data-ttu-id="ccf53-191">Tento příkaz vytvoří tabulku HBase s názvem **osoby**a ta je s daty.</span><span class="sxs-lookup"><span data-stu-id="ccf53-191">This command creates a HBase table named **people**, and populates it with data.</span></span>

4. <span data-ttu-id="ccf53-192">K vyhledání e-mailové adresy, které jsou uložené v tabulce, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-192">To search for email addresses stored in the table, use the following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    <span data-ttu-id="ccf53-193">Zobrazí se následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="ccf53-193">You receive the following results:</span></span>

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. <span data-ttu-id="ccf53-194">Chcete-li odstranit tabulku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-194">To delete the table, use the following command:</span></span>

    

## <a name="upload-the-jar-and-run-jobs-powershell"></a><span data-ttu-id="ccf53-195">Nahrát JAR a spouštění úloh (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="ccf53-195">Upload the JAR and run jobs (PowerShell)</span></span>

<span data-ttu-id="ccf53-196">Následující kroky pomocí prostředí Azure PowerShell pro nahrání JAR do výchozího úložiště pro váš cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="ccf53-196">The following steps use Azure PowerShell to upload the JAR to the default storage for your HBase cluster.</span></span> <span data-ttu-id="ccf53-197">Rutiny služby HDInsight se pak používají k příklady spustit vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="ccf53-197">HDInsight cmdlets are then used to run the examples remotely.</span></span>

1. <span data-ttu-id="ccf53-198">Po instalaci a konfiguraci Azure Powershellu, vytvořte soubor s názvem `hbase-runner.psm1`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-198">After installing and configuring Azure PowerShell, create a file named `hbase-runner.psm1`.</span></span> <span data-ttu-id="ccf53-199">Jako obsah tohoto souboru použijte následující text:</span><span class="sxs-lookup"><span data-stu-id="ccf53-199">Use the following text as the contents of this file:</span></span>

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzureStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use the Login-AzureRmAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    <span data-ttu-id="ccf53-200">Tento soubor obsahuje dva moduly:</span><span class="sxs-lookup"><span data-stu-id="ccf53-200">This file contains two modules:</span></span>

   * <span data-ttu-id="ccf53-201">**Přidat HDInsightFile** – použít k nahrání souborů do clusteru</span><span class="sxs-lookup"><span data-stu-id="ccf53-201">**Add-HDInsightFile** - used to upload files to the cluster</span></span>
   * <span data-ttu-id="ccf53-202">**Spuštění HBaseExample** – používá ke spuštění třídy vytvořený</span><span class="sxs-lookup"><span data-stu-id="ccf53-202">**Start-HBaseExample** - used to run the classes created earlier</span></span>

2. <span data-ttu-id="ccf53-203">Uložit `hbase-runner.psm1` souboru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-203">Save the `hbase-runner.psm1` file.</span></span>

3. <span data-ttu-id="ccf53-204">Otevřete nové okno Azure PowerShell, přejděte do adresáře `hbaseapp` adresář a potom spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-204">Open a new Azure PowerShell window, change directories to the `hbaseapp` directory, and then run the following command:</span></span>

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    <span data-ttu-id="ccf53-205">Změňte cestu k umístění `hbase-runner.psm1` soubor vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="ccf53-205">Change the path to the location of the `hbase-runner.psm1` file created earlier.</span></span> <span data-ttu-id="ccf53-206">Tento příkaz registruje modul Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccf53-206">This command registers the module with Azure PowerShell.</span></span>

4. <span data-ttu-id="ccf53-207">Použijte následující příkaz pro nahrání `hbaseapp-1.0-SNAPSHOT.jar` do clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-207">Use the following command to upload the `hbaseapp-1.0-SNAPSHOT.jar` to your cluster.</span></span>

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    <span data-ttu-id="ccf53-208">Nahraďte `hdinsightclustername` názvem svého clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-208">Replace `hdinsightclustername` with the name of your cluster.</span></span> <span data-ttu-id="ccf53-209">Odešle příkaz `hbaseapp-1.0-SNAPSHOT.jar` k `example/jars` umístění primárního úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="ccf53-209">The command uploads the `hbaseapp-1.0-SNAPSHOT.jar` to the `example/jars` location in the primary storage for your cluster.</span></span>

5. <span data-ttu-id="ccf53-210">K vytvoření tabulky pomocí `hbaseapp`, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-210">To create a table using the `hbaseapp`, use the following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    <span data-ttu-id="ccf53-211">Nahraďte `hdinsightclustername` názvem svého clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-211">Replace `hdinsightclustername` with the name of your cluster.</span></span>

    <span data-ttu-id="ccf53-212">Tento příkaz vytvoří tabulku s názvem **osoby** v HBase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ccf53-212">This command creates a table named **people** in HBase on your HDInsight cluster.</span></span> <span data-ttu-id="ccf53-213">Tento příkaz nezobrazuje žádný výstup v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="ccf53-213">This command does not show any output in the console window.</span></span>

6. <span data-ttu-id="ccf53-214">K vyhledání položky v tabulce, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ccf53-214">To search for entries in the table, use the following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    <span data-ttu-id="ccf53-215">Nahraďte `hdinsightclustername` názvem svého clusteru.</span><span class="sxs-lookup"><span data-stu-id="ccf53-215">Replace `hdinsightclustername` with the name of your cluster.</span></span>

    <span data-ttu-id="ccf53-216">Tento příkaz používá `SearchByEmail` třídy pro vyhledávání pro všechny řádky, kde `contactinformation` rodin sloupců a `email` sloupec obsahuje řetězec `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="ccf53-216">This command uses the `SearchByEmail` class to search for any rows where the `contactinformation` column family and the `email` column, contains the string `contoso.com`.</span></span> <span data-ttu-id="ccf53-217">Mělo by se zobrazit následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="ccf53-217">You should receive the following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="ccf53-218">Pomocí **fabrikam.com** pro `-emailRegex` hodnota vrátí uživatele, kteří mají **fabrikam.com** v poli e-mailu.</span><span class="sxs-lookup"><span data-stu-id="ccf53-218">Using **fabrikam.com** for the `-emailRegex` value returns the users that have **fabrikam.com** in the email field.</span></span> <span data-ttu-id="ccf53-219">Regulární výrazy můžete použít také jako hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="ccf53-219">You can also use regular expressions as the search term.</span></span> <span data-ttu-id="ccf53-220">Například **^ r** vrátí e-mailové adresy, které začínají písmeno "r".</span><span class="sxs-lookup"><span data-stu-id="ccf53-220">For example, **^r** returns email addresses that begin with the letter 'r'.</span></span>

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="ccf53-221">Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="ccf53-221">No results or unexpected results when using Start-HBaseExample</span></span>

<span data-ttu-id="ccf53-222">Použití `-showErr` parametr zobrazíte standardní chyba (STDERR), která je vytvořena při spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="ccf53-222">Use the `-showErr` parameter to view the standard error (STDERR) that is produced while running the job.</span></span>

## <a name="delete-the-table"></a><span data-ttu-id="ccf53-223">Odstranit tabulku</span><span class="sxs-lookup"><span data-stu-id="ccf53-223">Delete the table</span></span>

<span data-ttu-id="ccf53-224">Až skončíte se na příklad, použijte následující odstranit **osoby** tabulka použitá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="ccf53-224">When you are done with the example, use the following to delete the **people** table used in this example:</span></span>

<span data-ttu-id="ccf53-225">__Ze `ssh` relace__:</span><span class="sxs-lookup"><span data-stu-id="ccf53-225">__From an `ssh` session__:</span></span>

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

<span data-ttu-id="ccf53-226">__Z prostředí Azure PowerShell__:</span><span class="sxs-lookup"><span data-stu-id="ccf53-226">__From Azure PowerShell__:</span></span>

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a><span data-ttu-id="ccf53-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ccf53-227">Next steps</span></span>

[<span data-ttu-id="ccf53-228">Naučte se používat SQuirreL SQL s HBase</span><span class="sxs-lookup"><span data-stu-id="ccf53-228">Learn how to use SQuirreL SQL with HBase</span></span>](hdinsight-hbase-phoenix-squirrel-linux.md)
