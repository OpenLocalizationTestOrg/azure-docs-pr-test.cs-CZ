---
title: "Vytvoření aplikace Java HBase pro HDInsight se systémem Windows Azure | Microsoft Docs"
description: "Další informace o použití Apache Maven k sestavení aplikace založené na jazyce Java Apache HBase a potom ji nasadit do clusteru HDInsight se systémem Windows Azure."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f4a4e02-45ab-40dd-842b-3ec034f256c9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 59c9af5a91b107e68a676f02fe5a936f955b22fa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a><span data-ttu-id="dcaae-103">Používání Maven k sestavení aplikací Java, které používají HBase s HDInsight se systémem Windows (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="dcaae-103">Use Maven to build Java applications that use HBase with Windows-based HDInsight (Hadoop)</span></span>
<span data-ttu-id="dcaae-104">Zjistěte, jak vytvořte a sestavte [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java pomocí Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="dcaae-104">Learn how to create and build an [Apache HBase](http://hbase.apache.org/) application in Java by using Apache Maven.</span></span> <span data-ttu-id="dcaae-105">Pak použijete aplikaci s Azure HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="dcaae-105">Then use the application with Azure HDInsight (Hadoop).</span></span>

<span data-ttu-id="dcaae-106">[Maven](http://maven.apache.org/) je software projektu správy a míru porozumění nástroj, který umožňuje vytvářet softwaru, dokumentace a sestav pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="dcaae-106">[Maven](http://maven.apache.org/) is a software project management and comprehension tool that allows you to build software, documentation, and reports for Java projects.</span></span> <span data-ttu-id="dcaae-107">V tomto článku zjistěte, jak použít jej k vytvořit základní aplikaci Java, která, které vytváří, dotazy a odstraní tabulky HBase v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-107">In this article, you learn how to use it to create a basic Java application that that creates, queries, and deletes an HBase table on an Azure HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcaae-108">Kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="dcaae-108">The steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="dcaae-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="dcaae-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="dcaae-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="dcaae-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="dcaae-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="dcaae-111">Requirements</span></span>
* <span data-ttu-id="dcaae-112">[Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="dcaae-112">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 or later</span></span>
* [<span data-ttu-id="dcaae-113">Maven</span><span class="sxs-lookup"><span data-stu-id="dcaae-113">Maven</span></span>](http://maven.apache.org/)
* <span data-ttu-id="dcaae-114">Cluster HDInsight se systémem Windows s HBase</span><span class="sxs-lookup"><span data-stu-id="dcaae-114">A Windows-based HDInsight cluster with HBase</span></span>

    > [!NOTE]
    > <span data-ttu-id="dcaae-115">Kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.2 a 3.3.</span><span class="sxs-lookup"><span data-stu-id="dcaae-115">The steps in this document have been tested with HDInsight cluster versions 3.2 and 3.3.</span></span> <span data-ttu-id="dcaae-116">Výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.3.</span><span class="sxs-lookup"><span data-stu-id="dcaae-116">The default values provided in examples are for a HDInsight 3.3 cluster.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="dcaae-117">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="dcaae-117">Create the project</span></span>
1. <span data-ttu-id="dcaae-118">Z příkazového řádku ve vašem vývojovém prostředí, změňte adresáře na umístění, kde chcete vytvořit projekt, například `cd code\hdinsight`.</span><span class="sxs-lookup"><span data-stu-id="dcaae-118">From the command line in your development environment, change directories to the location where you want to create the project, for example, `cd code\hdinsight`.</span></span>
2. <span data-ttu-id="dcaae-119">Použití **mvn** příkazu, který se instaluje s Maven, vygenerujte generování uživatelského rozhraní pro projekt.</span><span class="sxs-lookup"><span data-stu-id="dcaae-119">Use the **mvn** command, which is installed with Maven, to generate the scaffolding for the project.</span></span>

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    <span data-ttu-id="dcaae-120">Tento příkaz vytvoří adresář v aktuálním umístění s názvem zadaným **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje následující položky:</span><span class="sxs-lookup"><span data-stu-id="dcaae-120">This command creates a directory in the current location, with the name specified by the **artifactID** parameter (**hbaseapp** in this example.) This directory contains the following items:</span></span>

   * <span data-ttu-id="dcaae-121">**pom.xml**: projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje podrobnosti o informace a konfigurace použít k sestavení projektu.</span><span class="sxs-lookup"><span data-stu-id="dcaae-121">**pom.xml**:  The Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used to build the project.</span></span>
   * <span data-ttu-id="dcaae-122">**src**: adresář, který obsahuje **main\java\com\microsoft\examples** adresáře, kde bude vytvářet aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcaae-122">**src**: The directory that contains the **main\java\com\microsoft\examples** directory, where you will author the application.</span></span>
3. <span data-ttu-id="dcaae-123">Odstranit **src\test\java\com\microsoft\examples\apptest.java** souboru, protože není použit v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="dcaae-123">Delete the **src\test\java\com\microsoft\examples\apptest.java** file because it is not used in this example.</span></span>

## <a name="update-the-project-object-model"></a><span data-ttu-id="dcaae-124">Aktualizace projektu objektový Model</span><span class="sxs-lookup"><span data-stu-id="dcaae-124">Update the Project Object Model</span></span>
1. <span data-ttu-id="dcaae-125">Upravit **pom.xml** souboru a přidejte následující kód do `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="dcaae-125">Edit the **pom.xml** file and add the following code inside the `<dependencies>` section:</span></span>

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    <span data-ttu-id="dcaae-126">Tato část informuje Maven, že projekt vyžaduje **hbase-client** verze **1.1.2**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-126">This section tells Maven that the project requires **hbase-client** version **1.1.2**.</span></span> <span data-ttu-id="dcaae-127">Při kompilaci je stažen tuto závislost z úložiště Maven výchozí.</span><span class="sxs-lookup"><span data-stu-id="dcaae-127">At compile time, this dependency is downloaded from the default Maven repository.</span></span> <span data-ttu-id="dcaae-128">Můžete použít [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) Další informace o této závislosti.</span><span class="sxs-lookup"><span data-stu-id="dcaae-128">You can use the [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) to learn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="dcaae-129">Číslo verze musí odpovídat verzi HBase, který je zadán v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-129">The version number must match the version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="dcaae-130">Pomocí následující tabulky najít číslo správnou verzi.</span><span class="sxs-lookup"><span data-stu-id="dcaae-130">Use the following table to find the correct version number.</span></span>
   >
   >

   | <span data-ttu-id="dcaae-131">Verze clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="dcaae-131">HDInsight cluster version</span></span> | <span data-ttu-id="dcaae-132">HBase verze se má použít</span><span class="sxs-lookup"><span data-stu-id="dcaae-132">HBase version to use</span></span> |
   | --- | --- |
   | <span data-ttu-id="dcaae-133">3.2</span><span class="sxs-lookup"><span data-stu-id="dcaae-133">3.2</span></span> |<span data-ttu-id="dcaae-134">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="dcaae-134">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="dcaae-135">3.3</span><span class="sxs-lookup"><span data-stu-id="dcaae-135">3.3</span></span> |<span data-ttu-id="dcaae-136">1.1.2</span><span class="sxs-lookup"><span data-stu-id="dcaae-136">1.1.2</span></span> |

    <span data-ttu-id="dcaae-137">Další informace o verzích HDInsight a součásti najdete v tématu [co jsou různé součásti Hadoop, která je k dispozici s HDInsight](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="dcaae-137">For more information on HDInsight versions and components, see [What are the different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>
2. <span data-ttu-id="dcaae-138">Pokud používáte cluster služby HDInsight 3.3, musíte taky přidat následující `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="dcaae-138">If you are using an HDInsight 3.3 cluster, you must also add the following to the `<dependencies>` section:</span></span>

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    <span data-ttu-id="dcaae-139">Tuto závislost načte phoenix základních komponent, které používají Hbase verze 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="dcaae-139">This dependency will load the phoenix-core components, which are used by Hbase version 1.1.x.</span></span>
3. <span data-ttu-id="dcaae-140">Přidejte následující kód, který **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-140">Add the following code to the **pom.xml** file.</span></span> <span data-ttu-id="dcaae-141">Tato část musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="dcaae-141">This section must be inside the `<project>...</project>` tags in the file, for example, between `</dependencies>` and `</project>`.</span></span>

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
                    <source>1.7</source>
                    <target>1.7</target>
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

    <span data-ttu-id="dcaae-142">`<resources>` Části nakonfiguruje prostředku (**conf\hbase-site.xml**) obsahující informace o konfiguraci pro HBase.</span><span class="sxs-lookup"><span data-stu-id="dcaae-142">The `<resources>` section configures a resource (**conf\hbase-site.xml**) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dcaae-143">Můžete také nastavit hodnoty konfigurace prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="dcaae-143">You can also set configuration values via code.</span></span> <span data-ttu-id="dcaae-144">Zobrazte komentáře ve **CreateTable** příklad, který následuje pro jak na to.</span><span class="sxs-lookup"><span data-stu-id="dcaae-144">See the comments in the **CreateTable** example that follows for how to do this.</span></span>
   >
   >

    <span data-ttu-id="dcaae-145">To `<plugins>` části nakonfiguruje [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="dcaae-145">This `<plugins>` section configures the [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="dcaae-146">Modul plug-in kompilátoru se používá ke kompilaci topologii.</span><span class="sxs-lookup"><span data-stu-id="dcaae-146">The compiler plug-in is used to compile the topology.</span></span> <span data-ttu-id="dcaae-147">Modul plug-in stín se používá při prevenci licence duplikace v JAR balíček, který je sestavena Maven.</span><span class="sxs-lookup"><span data-stu-id="dcaae-147">The shade plug-in is used to prevent license duplication in the JAR package that is built by Maven.</span></span> <span data-ttu-id="dcaae-148">Používá se důvodem je, že duplicitní licenčních souborů dojít k chybě za běhu v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-148">The reason this is used is that the duplicate license files cause an error at run time on the HDInsight cluster.</span></span> <span data-ttu-id="dcaae-149">Používání maven stín – modul plug-in s `ApacheLicenseResourceTransformer` implementace brání této chybě.</span><span class="sxs-lookup"><span data-stu-id="dcaae-149">Using maven-shade-plugin with the `ApacheLicenseResourceTransformer` implementation prevents this error.</span></span>

    <span data-ttu-id="dcaae-150">Plugin stín maven také vytváří uber jar (nebo fat jar), který obsahuje všechny závislosti, které jsou požadované aplikací.</span><span class="sxs-lookup"><span data-stu-id="dcaae-150">The maven-shade-plugin also produces an uber jar (or fat jar) that contains all the dependencies required by the application.</span></span>
4. <span data-ttu-id="dcaae-151">Uložit **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-151">Save the **pom.xml** file.</span></span>
5. <span data-ttu-id="dcaae-152">Vytvořte nový adresář s názvem **conf** v **hbaseapp** adresáře.</span><span class="sxs-lookup"><span data-stu-id="dcaae-152">Create a new directory named **conf** in the **hbaseapp** directory.</span></span> <span data-ttu-id="dcaae-153">V **conf** adresáře, vytvořte soubor s názvem **hbase-site.xml**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-153">In the **conf** directory, create a file named **hbase-site.xml**.</span></span> <span data-ttu-id="dcaae-154">Použijte následující postupy jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="dcaae-154">Use the following as the contents of the file:</span></span>

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 The Apache Software Foundation
          *
          * Licensed to the Apache Software Foundation (ASF) under one
          * or more contributor license agreements.  See the NOTICE file
          * distributed with this work for additional information
          * regarding copyright ownership.  The ASF licenses this file
          * to you under the Apache License, Version 2.0 (the
          * "License"); you may not use this file except in compliance
          * with the License.  You may obtain a copy of the License at
          *
          *     http://www.apache.org/licenses/LICENSE-2.0
          *
          * Unless required by applicable law or agreed to in writing, software
          * distributed under the License is distributed on an "AS IS" BASIS,
          * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
          * See the License for the specific language governing permissions and
          * limitations under the License.
          */
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    <span data-ttu-id="dcaae-155">Tento soubor se použije načíst konfiguraci HBase pro cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-155">This file will be used to load the HBase configuration for an HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dcaae-156">Toto je soubor minimální hbase-site.xml, a obsahuje úplné minimální nastavení clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-156">This is a minimal hbase-site.xml file, and it contains the bare minimum settings for the HDInsight cluster.</span></span>

6. <span data-ttu-id="dcaae-157">Uložit **hbase-site.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-157">Save the **hbase-site.xml** file.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="dcaae-158">Vytvoření aplikace</span><span class="sxs-lookup"><span data-stu-id="dcaae-158">Create the application</span></span>
1. <span data-ttu-id="dcaae-159">Přejděte na **hbaseapp\src\main\java\com\microsoft\examples** adresáře a přejmenujte app.java soubor na **CreateTable.java**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-159">Go to the **hbaseapp\src\main\java\com\microsoft\examples** directory and rename the app.java file to **CreateTable.java**.</span></span>
2. <span data-ttu-id="dcaae-160">Otevřete **CreateTable.java** souboru a nahradit existující obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="dcaae-160">Open the **CreateTable.java** file and replace the existing contents with the following code:</span></span>

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
            // The following sets the znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    <span data-ttu-id="dcaae-161">Toto je **CreateTable** třída, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.</span><span class="sxs-lookup"><span data-stu-id="dcaae-161">This is the **CreateTable** class, which will create a table named **people** and populate it with some predefined users.</span></span>
3. <span data-ttu-id="dcaae-162">Uložit **CreateTable.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-162">Save the **CreateTable.java** file.</span></span>
4. <span data-ttu-id="dcaae-163">V **hbaseapp\src\main\java\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **SearchByEmail.java**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-163">In the **hbaseapp\src\main\java\com\microsoft\examples** directory, create a new file named **SearchByEmail.java**.</span></span> <span data-ttu-id="dcaae-164">Použijte následující kód jako obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="dcaae-164">Use the following code as the contents of this file:</span></span>

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

            // Create a new regex filter
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

    <span data-ttu-id="dcaae-165">**SearchByEmail** třídu lze použít k dotazu pro řádků a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="dcaae-165">The **SearchByEmail** class can be used to query for rows by email address.</span></span> <span data-ttu-id="dcaae-166">Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při používání třídy.</span><span class="sxs-lookup"><span data-stu-id="dcaae-166">Because it uses a regular expression filter, you can provide either a string or a regular expression when using the class.</span></span>
5. <span data-ttu-id="dcaae-167">Uložit **SearchByEmail.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-167">Save the **SearchByEmail.java** file.</span></span>
6. <span data-ttu-id="dcaae-168">V **hbaseapp\src\main\hava\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **DeleteTable.java**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-168">In the **hbaseapp\src\main\hava\com\microsoft\examples** directory, create a new file named **DeleteTable.java**.</span></span> <span data-ttu-id="dcaae-169">Použijte následující kód jako obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="dcaae-169">Use the following code as the contents of this file:</span></span>

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

    <span data-ttu-id="dcaae-170">Tato třída je pro čištění v tomto příkladu zakázáním a vyřazení tabulky vytvořené **CreateTable** třídy.</span><span class="sxs-lookup"><span data-stu-id="dcaae-170">This class is for cleaning up this example by disabling and dropping the table created by the **CreateTable** class.</span></span>
7. <span data-ttu-id="dcaae-171">Uložit **DeleteTable.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-171">Save the **DeleteTable.java** file.</span></span>

## <a name="build-and-package-the-application"></a><span data-ttu-id="dcaae-172">Sestavení a balíček aplikace</span><span class="sxs-lookup"><span data-stu-id="dcaae-172">Build and package the application</span></span>
1. <span data-ttu-id="dcaae-173">Otevřete příkazový řádek a přejděte do adresáře **hbaseapp** adresáře.</span><span class="sxs-lookup"><span data-stu-id="dcaae-173">Open a command prompt and change directories to the **hbaseapp** directory.</span></span>
2. <span data-ttu-id="dcaae-174">Vytvořit soubor JAR obsahující aplikaci, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dcaae-174">Use the following command to build a JAR file that contains the application:</span></span>

        mvn clean package

    <span data-ttu-id="dcaae-175">To vyčistí artefakty předchozí sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány, pak sestavení a balíčky aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcaae-175">This cleans any previous build artifacts, downloads any dependencies that have not already been installed, then builds and packages the application.</span></span>
3. <span data-ttu-id="dcaae-176">Po dokončení příkazu **hbaseapp\target** adresář obsahuje soubor s názvem **hbaseapp. 1.0 SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-176">When the command completes, the **hbaseapp\target** directory contains a file named **hbaseapp-1.0-SNAPSHOT.jar**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dcaae-177">**Hbaseapp. 1.0 SNAPSHOT.jar** uber je soubor jar (někdy nazývané fat jar,), který obsahuje všechny závislosti potřebné ke spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="dcaae-177">The **hbaseapp-1.0-SNAPSHOT.jar** file is an uber jar (sometimes called a fat jar,) which contains all the dependencies required to run the application.</span></span>

## <a name="upload-the-jar-file-and-start-a-job"></a><span data-ttu-id="dcaae-178">Nahrát soubor JAR a spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="dcaae-178">Upload the JAR file and start a job</span></span>
<span data-ttu-id="dcaae-179">Existuje mnoho způsobů, jak nahrát soubor ke svému clusteru HDInsight, jak je popsáno v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="dcaae-179">There are many ways to upload a file to your HDInsight cluster, as described in [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span> <span data-ttu-id="dcaae-180">Následující kroky pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcaae-180">The following steps use Azure PowerShell.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. <span data-ttu-id="dcaae-181">Po instalaci a konfiguraci Azure Powershellu, vytvořte nový soubor s názvem **hbase runner.psm1**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-181">After installing and configuring Azure PowerShell, create a new file named **hbase-runner.psm1**.</span></span> <span data-ttu-id="dcaae-182">Následující tabulka obsahuje obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="dcaae-182">Use the following as the contents of this file:</span></span>

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

    <span data-ttu-id="dcaae-183">Tento soubor obsahuje dva moduly:</span><span class="sxs-lookup"><span data-stu-id="dcaae-183">This file contains two modules:</span></span>

   * <span data-ttu-id="dcaae-184">**Přidat HDInsightFile** – použít k nahrání souborů do HDInsight</span><span class="sxs-lookup"><span data-stu-id="dcaae-184">**Add-HDInsightFile** - used to upload files to HDInsight</span></span>
   * <span data-ttu-id="dcaae-185">**Spuštění HBaseExample** – používá ke spuštění třídy vytvořený</span><span class="sxs-lookup"><span data-stu-id="dcaae-185">**Start-HBaseExample** - used to run the classes created earlier</span></span>
2. <span data-ttu-id="dcaae-186">Uložit **hbase runner.psm1** souboru.</span><span class="sxs-lookup"><span data-stu-id="dcaae-186">Save the **hbase-runner.psm1** file.</span></span>
3. <span data-ttu-id="dcaae-187">Otevřete nové okno Azure PowerShell, přejděte do adresáře **hbaseapp** adresář a potom spusťte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="dcaae-187">Open a new Azure PowerShell window, change directories to the **hbaseapp** directory, and then run the following command.</span></span>

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    <span data-ttu-id="dcaae-188">Změňte cestu k umístění **hbase runner.psm1** soubor vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="dcaae-188">Change the path to the location of the **hbase-runner.psm1** file created earlier.</span></span> <span data-ttu-id="dcaae-189">Takovém postupu zaregistruje modul pro tuto relaci prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="dcaae-189">This registers the module for this Azure PowerShell session.</span></span>
4. <span data-ttu-id="dcaae-190">Použijte následující příkaz pro nahrání **hbaseapp. 1.0 SNAPSHOT.jar** ke svému clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-190">Use the following command to upload the **hbaseapp-1.0-SNAPSHOT.jar** to your HDInsight cluster.</span></span>

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    <span data-ttu-id="dcaae-191">Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-191">Replace **hdinsightclustername** with the name of your HDInsight cluster.</span></span> <span data-ttu-id="dcaae-192">Odešle příkaz **hbaseapp. 1.0 SNAPSHOT.jar** k **příklad/JAR** umístění primárního úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-192">The command uploads the **hbaseapp-1.0-SNAPSHOT.jar** to the **example/jars** location in the primary storage for your HDInsight cluster.</span></span>
5. <span data-ttu-id="dcaae-193">Jakmile soubory odešlete, použijte následující kód k vytvoření tabulky pomocí **hbaseapp**:</span><span class="sxs-lookup"><span data-stu-id="dcaae-193">After the files are uploaded, use the following code to create a table using the **hbaseapp**:</span></span>

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    <span data-ttu-id="dcaae-194">Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-194">Replace **hdinsightclustername** with the name of your HDInsight cluster.</span></span>

    <span data-ttu-id="dcaae-195">Tento příkaz vytvoří novou tabulku s názvem **osoby** v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-195">This command creates a new table named **people** in your HDInsight cluster.</span></span> <span data-ttu-id="dcaae-196">Tento příkaz nezobrazuje žádný výstup v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="dcaae-196">This command does not show any output in the console window.</span></span>
6. <span data-ttu-id="dcaae-197">K vyhledání položky v tabulce, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="dcaae-197">To search for entries in the table, use the following command:</span></span>

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    <span data-ttu-id="dcaae-198">Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-198">Replace **hdinsightclustername** with the name of your HDInsight cluster.</span></span>

    <span data-ttu-id="dcaae-199">Tento příkaz používá **SearchByEmail** třídy pro vyhledávání pro všechny řádky, kde **contactinformation** rodin sloupců a **e-mailu** sloupec obsahuje řetězec **contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="dcaae-199">This command uses the **SearchByEmail** class to search for any rows where the **contactinformation** column family and the **email** column, contains the string **contoso.com**.</span></span> <span data-ttu-id="dcaae-200">Mělo by se zobrazit následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="dcaae-200">You should receive the following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="dcaae-201">Pomocí **fabrikam.com** pro `-emailRegex` hodnota vrátí uživatele, kteří mají **fabrikam.com** v poli e-mailu.</span><span class="sxs-lookup"><span data-stu-id="dcaae-201">Using **fabrikam.com** for the `-emailRegex` value returns the users that have **fabrikam.com** in the email field.</span></span> <span data-ttu-id="dcaae-202">Vzhledem k tomu, že toto hledání se implementuje pomocí regulárních založené na výrazu filtru, můžete také zadat regulární výrazy, například **^ r**, které vrátí položky e-mailu, kde začíná písmenem "r".</span><span class="sxs-lookup"><span data-stu-id="dcaae-202">Since this search is implemented by using a regular expression-based filter, you can also enter regular expressions, such as **^r**, which returns entries where the email begins with the letter 'r'.</span></span>

## <a name="delete-the-table"></a><span data-ttu-id="dcaae-203">Odstranit tabulku</span><span class="sxs-lookup"><span data-stu-id="dcaae-203">Delete the table</span></span>
<span data-ttu-id="dcaae-204">Až skončíte se na příklad, použijte následující příkaz z relace prostředí Azure PowerShell k odstranění **osoby** tabulka použitá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="dcaae-204">When you are done with the example, use the following command from the Azure PowerShell session to delete the **people** table used in this example:</span></span>

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

<span data-ttu-id="dcaae-205">Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="dcaae-205">Replace **hdinsightclustername** with the name of your HDInsight cluster.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="dcaae-206">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="dcaae-206">Troubleshooting</span></span>
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="dcaae-207">Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="dcaae-207">No results or unexpected results when using Start-HBaseExample</span></span>
<span data-ttu-id="dcaae-208">Použití `-showErr` parametr zobrazíte standardní chyba (STDERR), která je vytvořena při spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="dcaae-208">Use the `-showErr` parameter to view the standard error (STDERR) that is produced while running the job.</span></span>
