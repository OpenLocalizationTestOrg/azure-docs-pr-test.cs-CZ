---
title: Klient aaaJava HBase - Azure HDInsight | Microsoft Docs
description: "Zjistěte, jak toouse aplikaci Apache HBase Apache Maven toobuild založené na jazyce Java, pak je nasadit tooHBase v Azure HDInsight."
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
ms.openlocfilehash: 41ef92b2900280dd59089c4fa40686c44133b337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="build-java-applications-for-apache-hbase"></a><span data-ttu-id="5f887-103">Sestavení aplikací Java pro Apache HBase</span><span class="sxs-lookup"><span data-stu-id="5f887-103">Build Java applications for Apache HBase</span></span>

<span data-ttu-id="5f887-104">Zjistěte, jak toocreate [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java.</span><span class="sxs-lookup"><span data-stu-id="5f887-104">Learn how toocreate an [Apache HBase](http://hbase.apache.org/) application in Java.</span></span> <span data-ttu-id="5f887-105">Potom pomocí aplikace hello s HBase v Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-105">Then use hello application with HBase on Azure HDInsight.</span></span>

<span data-ttu-id="5f887-106">Hello kroky v tomto dokumentu [Maven](http://maven.apache.org/) toocreate a sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-106">hello steps in this document use [Maven](http://maven.apache.org/) toocreate and build hello project.</span></span> <span data-ttu-id="5f887-107">Maven je správa softwaru projektu a míru porozumění nástroj, který umožňuje toobuild softwaru, dokumentace a sestav pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="5f887-107">Maven is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span>

> [!NOTE]
> <span data-ttu-id="5f887-108">Hello kroky v tomto dokumentu byly naposledy testovány s HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="5f887-108">hello steps in this document were most recently tested with HDInsight 3.6.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f887-109">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="5f887-109">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="5f887-110">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5f887-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5f887-111">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5f887-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="5f887-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f887-112">Requirements</span></span>

* <span data-ttu-id="5f887-113">[Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5f887-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 or later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5f887-114">HDInsight 3.5 a vyšší vyžaduje Java 8.</span><span class="sxs-lookup"><span data-stu-id="5f887-114">HDInsight 3.5 and greater requires Java 8.</span></span> <span data-ttu-id="5f887-115">Dřívějších verzích HDInsight vyžadují Java 7.</span><span class="sxs-lookup"><span data-stu-id="5f887-115">Earlier versions of HDInsight require Java 7.</span></span>

* [<span data-ttu-id="5f887-116">Maven</span><span class="sxs-lookup"><span data-stu-id="5f887-116">Maven</span></span>](http://maven.apache.org/)

* [<span data-ttu-id="5f887-117">Cluster Azure HDInsight se systémem Linux s HBase</span><span class="sxs-lookup"><span data-stu-id="5f887-117">A Linux-based Azure HDInsight cluster with HBase</span></span>](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > <span data-ttu-id="5f887-118">Hello kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.4 a 3.5.</span><span class="sxs-lookup"><span data-stu-id="5f887-118">hello steps in this document have been tested with HDInsight cluster versions 3.4 and 3.5.</span></span> <span data-ttu-id="5f887-119">Hello výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="5f887-119">hello default values provided in examples are for a HDInsight 3.5 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="5f887-120">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="5f887-120">Create hello project</span></span>

1. <span data-ttu-id="5f887-121">Z příkazového řádku hello ve vašem vývojovém prostředí, změňte adresáře toohello umístění, kam má toocreate hello projektu, například `cd code\hbase`.</span><span class="sxs-lookup"><span data-stu-id="5f887-121">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hbase`.</span></span>

2. <span data-ttu-id="5f887-122">Použití hello **mvn** příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-122">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > <span data-ttu-id="5f887-123">Pokud používáte prostředí PowerShell, je nutné uzavřít hello `-D` parametry v uvozovkách.</span><span class="sxs-lookup"><span data-stu-id="5f887-123">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="5f887-124">Tento příkaz vytvoří adresář s hello stejný název jako hello **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="5f887-124">This command creates a directory with hello same name as hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="5f887-125">**pom.xml**: hello projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje informace a konfigurace projektu hello toobuild podrobnosti použít.</span><span class="sxs-lookup"><span data-stu-id="5f887-125">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="5f887-126">**src**: hello adresář, který obsahuje hello **main/java/com/microsoft/příklady** adresáře, kde můžete vytvářet aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-126">**src**: hello directory that contains hello **main/java/com/microsoft/examples** directory, where you author hello application.</span></span>

3. <span data-ttu-id="5f887-127">Odstranit hello `src/test/java/com/microsoft/examples/apptest.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-127">Delete hello `src/test/java/com/microsoft/examples/apptest.java` file.</span></span> <span data-ttu-id="5f887-128">Není možné použít v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="5f887-128">It is not be used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="5f887-129">Aktualizace hello projektu objektový Model</span><span class="sxs-lookup"><span data-stu-id="5f887-129">Update hello Project Object Model</span></span>

1. <span data-ttu-id="5f887-130">Upravit hello `pom.xml` souboru a přidejte následující kód do hello hello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="5f887-130">Edit hello `pom.xml` file and add hello following code inside hello `<dependencies>` section:</span></span>

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

    <span data-ttu-id="5f887-131">V této části určuje projektu hello musí **hbase-client** a **phoenix základní** součásti.</span><span class="sxs-lookup"><span data-stu-id="5f887-131">This section indicates that hello project needs **hbase-client** and **phoenix-core** components.</span></span> <span data-ttu-id="5f887-132">Při kompilaci se stáhnou tyto závislosti z hello výchozí Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="5f887-132">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="5f887-133">Můžete použít hello [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn Další informace o této závislosti.</span><span class="sxs-lookup"><span data-stu-id="5f887-133">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5f887-134">číslo verze Hello hello hbase-client musí odpovídat verzi hello hbase, který je zadán v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-134">hello version number of hello hbase-client must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="5f887-135">Pomocí následující tabulky toofind hello správnou verzi číslo hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-135">Use hello following table toofind hello correct version number.</span></span>

   | <span data-ttu-id="5f887-136">Verze clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="5f887-136">HDInsight cluster version</span></span> | <span data-ttu-id="5f887-137">Verze toouse HBase</span><span class="sxs-lookup"><span data-stu-id="5f887-137">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="5f887-138">3.2</span><span class="sxs-lookup"><span data-stu-id="5f887-138">3.2</span></span> |<span data-ttu-id="5f887-139">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="5f887-139">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="5f887-140">3.3, 3.4, 3.5 a 3.6</span><span class="sxs-lookup"><span data-stu-id="5f887-140">3.3, 3.4, 3.5, and 3.6</span></span> |<span data-ttu-id="5f887-141">1.1.2</span><span class="sxs-lookup"><span data-stu-id="5f887-141">1.1.2</span></span> |

    <span data-ttu-id="5f887-142">Další informace o verzích HDInsight a součásti najdete v tématu [co jsou hello různých komponent systému Hadoop HDInsight k dispozici](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="5f887-142">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>

3. <span data-ttu-id="5f887-143">Přidejte následující kód toohello hello **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-143">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="5f887-144">Tento text musí být uvnitř hello `<project>...</project>` značky v hello souboru, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="5f887-144">This text must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

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

    <span data-ttu-id="5f887-145">Tato část nakonfiguruje prostředku (`conf/hbase-site.xml`) obsahující informace o konfiguraci pro HBase.</span><span class="sxs-lookup"><span data-stu-id="5f887-145">This section configures a resource (`conf/hbase-site.xml`) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f887-146">Můžete také nastavit hodnoty konfigurace prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="5f887-146">You can also set configuration values via code.</span></span> <span data-ttu-id="5f887-147">V tématu hello komentáře v hello `CreateTable` příklad.</span><span class="sxs-lookup"><span data-stu-id="5f887-147">See hello comments in hello `CreateTable` example.</span></span>

    <span data-ttu-id="5f887-148">Tato část také nakonfiguruje hello [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="5f887-148">This section also configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="5f887-149">modul plug-in Hello kompilátoru je použité toocompile hello topologie.</span><span class="sxs-lookup"><span data-stu-id="5f887-149">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="5f887-150">Hello stín modulu plug-in se použité tooprevent licence duplikace v hello JAR balíček, který je sestavena Maven.</span><span class="sxs-lookup"><span data-stu-id="5f887-150">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="5f887-151">Tento modul plug-in je použité tooprevent "duplicitní souborů s licencí" Chyba za běhu v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-151">This plugin is used tooprevent a "duplicate license files" error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="5f887-152">Pomocí modulu plug-in maven stín hello `ApacheLicenseResourceTransformer` implementace brání chyba hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-152">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents hello error.</span></span>

    <span data-ttu-id="5f887-153">Hello maven stín – modul plug-in vytvoří také uber jar, který obsahuje všechny závislosti hello požadované aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-153">hello maven-shade-plugin also produces an uber jar that contains all hello dependencies required by hello application.</span></span>

4. <span data-ttu-id="5f887-154">Uložit hello `pom.xml` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-154">Save hello `pom.xml` file.</span></span>

5. <span data-ttu-id="5f887-155">Vytvořte adresář s názvem `conf` v hello `hbaseapp` adresáře.</span><span class="sxs-lookup"><span data-stu-id="5f887-155">Create a directory named `conf` in hello `hbaseapp` directory.</span></span> <span data-ttu-id="5f887-156">Tento adresář se informace o konfiguraci toohold použité pro připojování tooHBase.</span><span class="sxs-lookup"><span data-stu-id="5f887-156">This directory is used toohold configuration information for connecting tooHBase.</span></span>

6. <span data-ttu-id="5f887-157">Použití hello následující příkaz toocopy hello HBase konfiguraci z toohello clusteru HBase hello `conf` adresáře.</span><span class="sxs-lookup"><span data-stu-id="5f887-157">Use hello following command toocopy hello HBase configuration from hello HBase cluster toohello `conf` directory.</span></span> <span data-ttu-id="5f887-158">Nahraďte `USERNAME` s názvem hello vaše přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="5f887-158">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="5f887-159">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="5f887-159">Replace `CLUSTERNAME` with your HDInsight cluster name:</span></span>

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   <span data-ttu-id="5f887-160">Další informace o používání `ssh` a `scp`, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5f887-160">For more information on using `ssh` and `scp`, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="5f887-161">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5f887-161">Create hello application</span></span>

1. <span data-ttu-id="5f887-162">Přejděte toohello `hbaseapp/src/main/java/com/microsoft/examples` adresáře a přejmenujte hello soubor app.java příliš`CreateTable.java`.</span><span class="sxs-lookup"><span data-stu-id="5f887-162">Go toohello `hbaseapp/src/main/java/com/microsoft/examples` directory and rename hello app.java file too`CreateTable.java`.</span></span>

2. <span data-ttu-id="5f887-163">Otevřete hello `CreateTable.java` souboru a nahradit existující obsah hello hello následující text:</span><span class="sxs-lookup"><span data-stu-id="5f887-163">Open hello `CreateTable.java` file and replace hello existing contents with hello following text:</span></span>

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

        //Linux-based HDInsight clusters use /hbase-unsecure as hello znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using hello config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create hello table...
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

        // Add each person toohello table
        //   Use hello `name` column family for hello name
        //   Use hello `contactinfo` column family for hello email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close hello table
        table.flushCommits();
        table.close();
        }
    }
   ```

    <span data-ttu-id="5f887-164">Tento kód je hello **CreateTable** třídy, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.</span><span class="sxs-lookup"><span data-stu-id="5f887-164">This code is hello **CreateTable** class, which creates a table named **people** and populate it with some predefined users.</span></span>

3. <span data-ttu-id="5f887-165">Uložit hello `CreateTable.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-165">Save hello `CreateTable.java` file.</span></span>

4. <span data-ttu-id="5f887-166">V hello `hbaseapp/src/main/java/com/microsoft/examples` adresáře, vytvořte soubor s názvem `SearchByEmail.java`.</span><span class="sxs-lookup"><span data-stu-id="5f887-166">In hello `hbaseapp/src/main/java/com/microsoft/examples` directory, create a file named `SearchByEmail.java`.</span></span> <span data-ttu-id="5f887-167">Použijte hello následující text jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="5f887-167">Use hello following text as hello contents of this file:</span></span>

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

        // Use GenericOptionsParser tooget only hello parameters toohello class
        // and not all hello parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open hello table
        HTable table = new HTable(config, "people");

        // Define hello family and qualifiers toobe used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach hello regex filter tooa filter
        //   for hello email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set hello filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get hello results
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

    <span data-ttu-id="5f887-168">Hello **SearchByEmail** třídy lze použít tooquery pro řádků a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="5f887-168">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="5f887-169">Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při použití třídy hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-169">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>

5. <span data-ttu-id="5f887-170">Uložit hello `SearchByEmail.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-170">Save hello `SearchByEmail.java` file.</span></span>

6. <span data-ttu-id="5f887-171">V hello `hbaseapp/src/main/hava/com/microsoft/examples` adresáře, vytvořte soubor s názvem `DeleteTable.java`.</span><span class="sxs-lookup"><span data-stu-id="5f887-171">In hello `hbaseapp/src/main/hava/com/microsoft/examples` directory, create a file named `DeleteTable.java`.</span></span> <span data-ttu-id="5f887-172">Použijte hello následující text jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="5f887-172">Use hello following text as hello contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using hello config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete hello table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    <span data-ttu-id="5f887-173">Tato třída vyčistí hello vytvořené v tomto příkladu zakázáním tabulky HBase a vyřazení hello tabulky vytvořené hello `CreateTable` třídy.</span><span class="sxs-lookup"><span data-stu-id="5f887-173">This class cleans up hello HBase tables created in this example by disabling and dropping hello table created by hello `CreateTable` class.</span></span>

7. <span data-ttu-id="5f887-174">Uložit hello `DeleteTable.java` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-174">Save hello `DeleteTable.java` file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="5f887-175">Sestavení a balíček aplikace hello</span><span class="sxs-lookup"><span data-stu-id="5f887-175">Build and package hello application</span></span>

1. <span data-ttu-id="5f887-176">Z hello `hbaseapp` adresář, použijte hello následující příkaz toobuild soubor JAR obsahující aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="5f887-176">From hello `hbaseapp` directory, use hello following command toobuild a JAR file that contains hello application:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="5f887-177">Tento příkaz vytvoří a balíčky hello aplikace do souboru .jar.</span><span class="sxs-lookup"><span data-stu-id="5f887-177">This command builds and packages hello application into a .jar file.</span></span>

2. <span data-ttu-id="5f887-178">Když hello dokončení příkazu hello `hbaseapp/target` adresář obsahuje soubor s názvem `hbaseapp-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="5f887-178">When hello command completes, hello `hbaseapp/target` directory contains a file named `hbaseapp-1.0-SNAPSHOT.jar`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f887-179">Hello `hbaseapp-1.0-SNAPSHOT.jar` je soubor jar uber.</span><span class="sxs-lookup"><span data-stu-id="5f887-179">hello `hbaseapp-1.0-SNAPSHOT.jar` file is an uber jar.</span></span> <span data-ttu-id="5f887-180">Obsahuje všechny aplikace hello požadované toorun hello závislosti.</span><span class="sxs-lookup"><span data-stu-id="5f887-180">It contains all hello dependencies required toorun hello application.</span></span>


## <a name="upload-hello-jar-and-run-jobs-ssh"></a><span data-ttu-id="5f887-181">Nahrát hello JAR a spouštění úloh (SSH)</span><span class="sxs-lookup"><span data-stu-id="5f887-181">Upload hello JAR and run jobs (SSH)</span></span>

<span data-ttu-id="5f887-182">Následující postup použijte Hello `scp` toocopy hello JAR toohello primární hlavního uzlu vaší databáze hbase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-182">hello following steps use `scp` toocopy hello JAR toohello primary head node of your HBase on HDInsight cluster.</span></span> <span data-ttu-id="5f887-183">Hello `ssh` příkaz je pak použit tooconnect toohello clusteru a spusťte hello příklad přímo na hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="5f887-183">hello `ssh` command is then used tooconnect toohello cluster and run hello example directly on hello head node.</span></span>

1. <span data-ttu-id="5f887-184">tooupload hello jar toohello cluster, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-184">tooupload hello jar toohello cluster, use hello following command:</span></span>

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="5f887-185">Nahraďte `USERNAME` s názvem hello vaše přihlášení SSH.</span><span class="sxs-lookup"><span data-stu-id="5f887-185">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="5f887-186">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-186">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

2. <span data-ttu-id="5f887-187">tooconnect toohello clusteru HBase pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-187">tooconnect toohello HBase cluster, use hello following command:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="5f887-188">Nahraďte `USERNAME` hello název vaší přihlašování přes SSH.</span><span class="sxs-lookup"><span data-stu-id="5f887-188">Replace `USERNAME` hello name of your SSH login.</span></span> <span data-ttu-id="5f887-189">Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-189">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

3. <span data-ttu-id="5f887-190">toocreate tabulky HBase pomocí hello aplikaci Java, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-190">toocreate an HBase table using hello Java application, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    <span data-ttu-id="5f887-191">Tento příkaz vytvoří tabulku HBase s názvem **osoby**a ta je s daty.</span><span class="sxs-lookup"><span data-stu-id="5f887-191">This command creates a HBase table named **people**, and populates it with data.</span></span>

4. <span data-ttu-id="5f887-192">toosearch pro e-mailové adresy, které jsou uložené v tabulce hello hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-192">toosearch for email addresses stored in hello table, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    <span data-ttu-id="5f887-193">Zobrazí hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="5f887-193">You receive hello following results:</span></span>

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. <span data-ttu-id="5f887-194">toodelete hello tabulky, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-194">toodelete hello table, use hello following command:</span></span>

    

## <a name="upload-hello-jar-and-run-jobs-powershell"></a><span data-ttu-id="5f887-195">Nahrát hello JAR a spouštění úloh (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="5f887-195">Upload hello JAR and run jobs (PowerShell)</span></span>

<span data-ttu-id="5f887-196">Hello následující kroky pomocí prostředí Azure PowerShell tooupload hello JAR toohello výchozí úložiště pro váš cluster HBase.</span><span class="sxs-lookup"><span data-stu-id="5f887-196">hello following steps use Azure PowerShell tooupload hello JAR toohello default storage for your HBase cluster.</span></span> <span data-ttu-id="5f887-197">Rutiny služby HDInsight se pak použít toorun hello příklady vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="5f887-197">HDInsight cmdlets are then used toorun hello examples remotely.</span></span>

1. <span data-ttu-id="5f887-198">Po instalaci a konfiguraci Azure Powershellu, vytvořte soubor s názvem `hbase-runner.psm1`.</span><span class="sxs-lookup"><span data-stu-id="5f887-198">After installing and configuring Azure PowerShell, create a file named `hbase-runner.psm1`.</span></span> <span data-ttu-id="5f887-199">Použijte hello následující text jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="5f887-199">Use hello following text as hello contents of this file:</span></span>

   ```powershell
    <#
    .SYNOPSIS
    Copies a file toohello primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory toohello blob container for
    hello HDInsight cluster.
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
    #hello class toorun
    [Parameter(Mandatory = $true)]
    [String]$className,

    #hello name of hello HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want toosee stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is hello Azure module installed?
    FindAzure

    # Get hello login for hello HDInsight cluster
    $creds=Get-Credential -Message "Enter hello login for hello cluster" -UserName "admin"

    # hello JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # hello job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get hello job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
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
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file toohello primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory toohello blob container for
    hello HDInsight cluster.
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
            #hello path toohello local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #hello destination path and file name, relative toohello root of hello container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #hello name of hello HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is hello Azure module installed?
        FindAzure

        # Get authentication for hello cluster
        $creds=Get-Credential

        # Does hello local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get hello primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file toostorage, overwriting existing files if -force was used.
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
            throw "No active Azure subscription found! If you have a subscription, use hello Login-AzureRmAccount cmdlet toologin tooyour subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does hello cluster exist?
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
        # Get hello resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get hello storage context, as we can't depend
        # on using hello default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get hello container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts toosupport finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export hello verb-phrase things
    export-modulemember *-*
   ```

    <span data-ttu-id="5f887-200">Tento soubor obsahuje dva moduly:</span><span class="sxs-lookup"><span data-stu-id="5f887-200">This file contains two modules:</span></span>

   * <span data-ttu-id="5f887-201">**Přidat HDInsightFile** – použité tooupload soubory toohello clusteru</span><span class="sxs-lookup"><span data-stu-id="5f887-201">**Add-HDInsightFile** - used tooupload files toohello cluster</span></span>
   * <span data-ttu-id="5f887-202">**Spuštění HBaseExample** -používané třídy hello toorun vytvořený</span><span class="sxs-lookup"><span data-stu-id="5f887-202">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>

2. <span data-ttu-id="5f887-203">Uložit hello `hbase-runner.psm1` souboru.</span><span class="sxs-lookup"><span data-stu-id="5f887-203">Save hello `hbase-runner.psm1` file.</span></span>

3. <span data-ttu-id="5f887-204">Otevřete nové okno Azure PowerShell, změňte adresáře toohello `hbaseapp` adresář, a pak spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-204">Open a new Azure PowerShell window, change directories toohello `hbaseapp` directory, and then run hello following command:</span></span>

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    <span data-ttu-id="5f887-205">Změnit umístění toohello cesta hello hello `hbase-runner.psm1` soubor vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="5f887-205">Change hello path toohello location of hello `hbase-runner.psm1` file created earlier.</span></span> <span data-ttu-id="5f887-206">Tento příkaz registruje hello modulu Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5f887-206">This command registers hello module with Azure PowerShell.</span></span>

4. <span data-ttu-id="5f887-207">Použití hello následující příkaz tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f887-207">Use hello following command tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour cluster.</span></span>

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    <span data-ttu-id="5f887-208">Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f887-208">Replace `hdinsightclustername` with hello name of your cluster.</span></span> <span data-ttu-id="5f887-209">příkaz Hello odešle hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` umístění v hello primárního úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="5f887-209">hello command uploads hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` location in hello primary storage for your cluster.</span></span>

5. <span data-ttu-id="5f887-210">hello toocreate tabulky pomocí `hbaseapp`, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="5f887-210">toocreate a table using hello `hbaseapp`, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    <span data-ttu-id="5f887-211">Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f887-211">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="5f887-212">Tento příkaz vytvoří tabulku s názvem **osoby** v HBase v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f887-212">This command creates a table named **people** in HBase on your HDInsight cluster.</span></span> <span data-ttu-id="5f887-213">Tento příkaz v okně konzoly hello nezobrazuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="5f887-213">This command does not show any output in hello console window.</span></span>

6. <span data-ttu-id="5f887-214">toosearch pro položky v tabulce hello hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5f887-214">toosearch for entries in hello table, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    <span data-ttu-id="5f887-215">Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="5f887-215">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="5f887-216">Tento příkaz používá hello `SearchByEmail` třídy toosearch pro všechny řádky, kde hello `contactinformation` rodin sloupců a hello `email` sloupec obsahuje řetězec hello `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="5f887-216">This command uses hello `SearchByEmail` class toosearch for any rows where hello `contactinformation` column family and hello `email` column, contains hello string `contoso.com`.</span></span> <span data-ttu-id="5f887-217">Měli byste obdržet hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="5f887-217">You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="5f887-218">Pomocí **fabrikam.com** pro hello `-emailRegex` hodnota vrátí hello uživatele, kteří mají **fabrikam.com** v poli hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="5f887-218">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="5f887-219">Regulární výrazy můžete použít také jako hello hledaný termín.</span><span class="sxs-lookup"><span data-stu-id="5f887-219">You can also use regular expressions as hello search term.</span></span> <span data-ttu-id="5f887-220">Například **^ r** vrátí e-mailové adresy, které začínají hello písmeno "r".</span><span class="sxs-lookup"><span data-stu-id="5f887-220">For example, **^r** returns email addresses that begin with hello letter 'r'.</span></span>

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="5f887-221">Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="5f887-221">No results or unexpected results when using Start-HBaseExample</span></span>

<span data-ttu-id="5f887-222">Použití hello `-showErr` parametr tooview hello standardní chyba (STDERR) vytvořený při spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="5f887-222">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="5f887-223">Odstranit tabulku hello</span><span class="sxs-lookup"><span data-stu-id="5f887-223">Delete hello table</span></span>

<span data-ttu-id="5f887-224">Až skončíte s hello příklad, použijte následující toodelete hello hello **osoby** tabulka použitá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="5f887-224">When you are done with hello example, use hello following toodelete hello **people** table used in this example:</span></span>

<span data-ttu-id="5f887-225">__Ze `ssh` relace__:</span><span class="sxs-lookup"><span data-stu-id="5f887-225">__From an `ssh` session__:</span></span>

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

<span data-ttu-id="5f887-226">__Z prostředí Azure PowerShell__:</span><span class="sxs-lookup"><span data-stu-id="5f887-226">__From Azure PowerShell__:</span></span>

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a><span data-ttu-id="5f887-227">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5f887-227">Next steps</span></span>

[<span data-ttu-id="5f887-228">Zjistěte, jak toouse SQL SQuirreL s HBase</span><span class="sxs-lookup"><span data-stu-id="5f887-228">Learn how toouse SQuirreL SQL with HBase</span></span>](hdinsight-hbase-phoenix-squirrel-linux.md)
