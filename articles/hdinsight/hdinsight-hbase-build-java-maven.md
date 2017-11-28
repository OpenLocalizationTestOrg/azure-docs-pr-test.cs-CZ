---
title: "aaaBuild aplikace Java HBase pro HDInsight se systémem Windows Azure | Microsoft Docs"
description: "Zjistěte, jak toouse aplikaci Apache HBase Apache Maven toobuild založené na jazyce Java, pak je nasadit tooa clusteru HDInsight se systémem Windows Azure."
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
ms.openlocfilehash: 33c2f3d12cb6a17b5406817e8bcd3accff239517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a><span data-ttu-id="eafc5-103">Používání Maven toobuild Java aplikací, které používají HBase s HDInsight se systémem Windows (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="eafc5-103">Use Maven toobuild Java applications that use HBase with Windows-based HDInsight (Hadoop)</span></span>
<span data-ttu-id="eafc5-104">Zjistěte, jak toocreate a vytvoření [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java pomocí Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="eafc5-104">Learn how toocreate and build an [Apache HBase](http://hbase.apache.org/) application in Java by using Apache Maven.</span></span> <span data-ttu-id="eafc5-105">Pak použijete hello aplikací s Azure HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="eafc5-105">Then use hello application with Azure HDInsight (Hadoop).</span></span>

<span data-ttu-id="eafc5-106">[Maven](http://maven.apache.org/) je software projektu správy a míru porozumění nástroj, který vám umožní toobuild softwaru, dokumentace a sestav pro projekty Java.</span><span class="sxs-lookup"><span data-stu-id="eafc5-106">[Maven](http://maven.apache.org/) is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span> <span data-ttu-id="eafc5-107">V tomto článku se dozvíte, jak toouse ho toocreate základní aplikaci Java, která vytvoří, dotazy a odstraní HBase, které tabulky v clusteru Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-107">In this article, you learn how toouse it toocreate a basic Java application that that creates, queries, and deletes an HBase table on an Azure HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eafc5-108">Hello kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="eafc5-108">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="eafc5-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="eafc5-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="eafc5-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="eafc5-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="eafc5-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eafc5-111">Requirements</span></span>
* <span data-ttu-id="eafc5-112">[Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější</span><span class="sxs-lookup"><span data-stu-id="eafc5-112">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 or later</span></span>
* [<span data-ttu-id="eafc5-113">Maven</span><span class="sxs-lookup"><span data-stu-id="eafc5-113">Maven</span></span>](http://maven.apache.org/)
* <span data-ttu-id="eafc5-114">Cluster HDInsight se systémem Windows s HBase</span><span class="sxs-lookup"><span data-stu-id="eafc5-114">A Windows-based HDInsight cluster with HBase</span></span>

    > [!NOTE]
    > <span data-ttu-id="eafc5-115">Hello kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.2 a 3.3.</span><span class="sxs-lookup"><span data-stu-id="eafc5-115">hello steps in this document have been tested with HDInsight cluster versions 3.2 and 3.3.</span></span> <span data-ttu-id="eafc5-116">Hello výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.3.</span><span class="sxs-lookup"><span data-stu-id="eafc5-116">hello default values provided in examples are for a HDInsight 3.3 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="eafc5-117">Vytvoření projektu hello</span><span class="sxs-lookup"><span data-stu-id="eafc5-117">Create hello project</span></span>
1. <span data-ttu-id="eafc5-118">Z příkazového řádku hello ve vašem vývojovém prostředí, změňte adresáře toohello umístění, kam má toocreate hello projektu, například `cd code\hdinsight`.</span><span class="sxs-lookup"><span data-stu-id="eafc5-118">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hdinsight`.</span></span>
2. <span data-ttu-id="eafc5-119">Použití hello **mvn** příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-119">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    <span data-ttu-id="eafc5-120">Tento příkaz vytvoří adresář s názvem hello určeného hello v aktuální umístění hello, **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje hello následující položky:</span><span class="sxs-lookup"><span data-stu-id="eafc5-120">This command creates a directory in hello current location, with hello name specified by hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="eafc5-121">**pom.xml**: hello projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje informace a konfigurace projektu hello toobuild podrobnosti použít.</span><span class="sxs-lookup"><span data-stu-id="eafc5-121">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="eafc5-122">**src**: hello adresář, který obsahuje hello **main\java\com\microsoft\examples** adresáře, kde bude vytvářet aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-122">**src**: hello directory that contains hello **main\java\com\microsoft\examples** directory, where you will author hello application.</span></span>
3. <span data-ttu-id="eafc5-123">Odstranit hello **src\test\java\com\microsoft\examples\apptest.java** souboru, protože není použit v tomto příkladu.</span><span class="sxs-lookup"><span data-stu-id="eafc5-123">Delete hello **src\test\java\com\microsoft\examples\apptest.java** file because it is not used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="eafc5-124">Aktualizace hello projektu objektový Model</span><span class="sxs-lookup"><span data-stu-id="eafc5-124">Update hello Project Object Model</span></span>
1. <span data-ttu-id="eafc5-125">Upravit hello **pom.xml** souboru a přidejte následující kód do hello hello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="eafc5-125">Edit hello **pom.xml** file and add hello following code inside hello `<dependencies>` section:</span></span>

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    <span data-ttu-id="eafc5-126">Tato část informuje Maven, který hello projektu vyžaduje **hbase-client** verze **1.1.2**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-126">This section tells Maven that hello project requires **hbase-client** version **1.1.2**.</span></span> <span data-ttu-id="eafc5-127">Při kompilaci je stažen tuto závislost z hello výchozí Maven úložiště.</span><span class="sxs-lookup"><span data-stu-id="eafc5-127">At compile time, this dependency is downloaded from hello default Maven repository.</span></span> <span data-ttu-id="eafc5-128">Můžete použít hello [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn Další informace o této závislosti.</span><span class="sxs-lookup"><span data-stu-id="eafc5-128">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="eafc5-129">Hello číslo verze musí odpovídat verzi hello hbase, který je zadán v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-129">hello version number must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="eafc5-130">Pomocí následující tabulky toofind hello správnou verzi číslo hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-130">Use hello following table toofind hello correct version number.</span></span>
   >
   >

   | <span data-ttu-id="eafc5-131">Verze clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="eafc5-131">HDInsight cluster version</span></span> | <span data-ttu-id="eafc5-132">Verze toouse HBase</span><span class="sxs-lookup"><span data-stu-id="eafc5-132">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="eafc5-133">3.2</span><span class="sxs-lookup"><span data-stu-id="eafc5-133">3.2</span></span> |<span data-ttu-id="eafc5-134">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="eafc5-134">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="eafc5-135">3.3</span><span class="sxs-lookup"><span data-stu-id="eafc5-135">3.3</span></span> |<span data-ttu-id="eafc5-136">1.1.2</span><span class="sxs-lookup"><span data-stu-id="eafc5-136">1.1.2</span></span> |

    <span data-ttu-id="eafc5-137">Další informace o verzích HDInsight a součásti najdete v tématu [co jsou hello různých komponent systému Hadoop HDInsight k dispozici](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="eafc5-137">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>
2. <span data-ttu-id="eafc5-138">Pokud používáte cluster služby HDInsight 3.3, musíte taky přidat hello následující toohello `<dependencies>` části:</span><span class="sxs-lookup"><span data-stu-id="eafc5-138">If you are using an HDInsight 3.3 cluster, you must also add hello following toohello `<dependencies>` section:</span></span>

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    <span data-ttu-id="eafc5-139">Tuto závislost načte hello phoenix – základní komponenty, které používají Hbase verze 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="eafc5-139">This dependency will load hello phoenix-core components, which are used by Hbase version 1.1.x.</span></span>
3. <span data-ttu-id="eafc5-140">Přidejte následující kód toohello hello **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-140">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="eafc5-141">Tato část musí být uvnitř hello `<project>...</project>` značky v hello souboru, například mezi `</dependencies>` a `</project>`.</span><span class="sxs-lookup"><span data-stu-id="eafc5-141">This section must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

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

    <span data-ttu-id="eafc5-142">Hello `<resources>` části nakonfiguruje prostředku (**conf\hbase-site.xml**) obsahující informace o konfiguraci pro HBase.</span><span class="sxs-lookup"><span data-stu-id="eafc5-142">hello `<resources>` section configures a resource (**conf\hbase-site.xml**) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="eafc5-143">Můžete také nastavit hodnoty konfigurace prostřednictvím kódu.</span><span class="sxs-lookup"><span data-stu-id="eafc5-143">You can also set configuration values via code.</span></span> <span data-ttu-id="eafc5-144">V tématu hello komentáře v hello **CreateTable** příklad, který následuje jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="eafc5-144">See hello comments in hello **CreateTable** example that follows for how toodo this.</span></span>
   >
   >

    <span data-ttu-id="eafc5-145">To `<plugins>` části nakonfiguruje hello [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="eafc5-145">This `<plugins>` section configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="eafc5-146">modul plug-in Hello kompilátoru je použité toocompile hello topologie.</span><span class="sxs-lookup"><span data-stu-id="eafc5-146">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="eafc5-147">Hello stín modulu plug-in se použité tooprevent licence duplikace v hello JAR balíček, který je sestavena Maven.</span><span class="sxs-lookup"><span data-stu-id="eafc5-147">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="eafc5-148">Hello používá skutečnost, že hello duplicitní licenčních souborů dojít k chybě za běhu v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-148">hello reason this is used is that hello duplicate license files cause an error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="eafc5-149">Pomocí modulu plug-in maven stín hello `ApacheLicenseResourceTransformer` implementace brání této chybě.</span><span class="sxs-lookup"><span data-stu-id="eafc5-149">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents this error.</span></span>

    <span data-ttu-id="eafc5-150">Hello modul plug-in maven stín také vytváří uber jar (nebo fat jar), který obsahuje všechny závislosti hello požadované aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-150">hello maven-shade-plugin also produces an uber jar (or fat jar) that contains all hello dependencies required by hello application.</span></span>
4. <span data-ttu-id="eafc5-151">Uložit hello **pom.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-151">Save hello **pom.xml** file.</span></span>
5. <span data-ttu-id="eafc5-152">Vytvořte nový adresář s názvem **conf** v hello **hbaseapp** adresáře.</span><span class="sxs-lookup"><span data-stu-id="eafc5-152">Create a new directory named **conf** in hello **hbaseapp** directory.</span></span> <span data-ttu-id="eafc5-153">V hello **conf** adresáře, vytvořte soubor s názvem **hbase-site.xml**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-153">In hello **conf** directory, create a file named **hbase-site.xml**.</span></span> <span data-ttu-id="eafc5-154">Použijte následující hello jako hello obsah souboru hello:</span><span class="sxs-lookup"><span data-stu-id="eafc5-154">Use hello following as hello contents of hello file:</span></span>

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 hello Apache Software Foundation
          *
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

    <span data-ttu-id="eafc5-155">Tento soubor bude použité tooload hello HBase konfigurace clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-155">This file will be used tooload hello HBase configuration for an HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="eafc5-156">Toto je soubor minimální hbase-site.xml, a obsahuje hello úplné minimální nastavení pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="eafc5-156">This is a minimal hbase-site.xml file, and it contains hello bare minimum settings for hello HDInsight cluster.</span></span>

6. <span data-ttu-id="eafc5-157">Uložit hello **hbase-site.xml** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-157">Save hello **hbase-site.xml** file.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="eafc5-158">Vytvoření aplikace hello</span><span class="sxs-lookup"><span data-stu-id="eafc5-158">Create hello application</span></span>
1. <span data-ttu-id="eafc5-159">Přejděte toohello **hbaseapp\src\main\java\com\microsoft\examples** adresáře a přejmenujte hello soubor app.java příliš**CreateTable.java**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-159">Go toohello **hbaseapp\src\main\java\com\microsoft\examples** directory and rename hello app.java file too**CreateTable.java**.</span></span>
2. <span data-ttu-id="eafc5-160">Otevřete hello **CreateTable.java** souboru a nahradit existující obsah hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="eafc5-160">Open hello **CreateTable.java** file and replace hello existing contents with hello following code:</span></span>

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
            // hello following sets hello znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    <span data-ttu-id="eafc5-161">Toto je hello **CreateTable** třída, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.</span><span class="sxs-lookup"><span data-stu-id="eafc5-161">This is hello **CreateTable** class, which will create a table named **people** and populate it with some predefined users.</span></span>
3. <span data-ttu-id="eafc5-162">Uložit hello **CreateTable.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-162">Save hello **CreateTable.java** file.</span></span>
4. <span data-ttu-id="eafc5-163">V hello **hbaseapp\src\main\java\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **SearchByEmail.java**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-163">In hello **hbaseapp\src\main\java\com\microsoft\examples** directory, create a new file named **SearchByEmail.java**.</span></span> <span data-ttu-id="eafc5-164">Použijte následující kód jako obsah tohoto souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="eafc5-164">Use hello following code as hello contents of this file:</span></span>

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

            // Create a new regex filter
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

    <span data-ttu-id="eafc5-165">Hello **SearchByEmail** třídy lze použít tooquery pro řádků a e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="eafc5-165">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="eafc5-166">Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při použití třídy hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-166">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>
5. <span data-ttu-id="eafc5-167">Uložit hello **SearchByEmail.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-167">Save hello **SearchByEmail.java** file.</span></span>
6. <span data-ttu-id="eafc5-168">V hello **hbaseapp\src\main\hava\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **DeleteTable.java**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-168">In hello **hbaseapp\src\main\hava\com\microsoft\examples** directory, create a new file named **DeleteTable.java**.</span></span> <span data-ttu-id="eafc5-169">Použijte následující kód jako obsah tohoto souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="eafc5-169">Use hello following code as hello contents of this file:</span></span>

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

    <span data-ttu-id="eafc5-170">Tato třída je pro čištění v tomto příkladu zakázáním a vyřazení hello tabulky vytvořené hello **CreateTable** třídy.</span><span class="sxs-lookup"><span data-stu-id="eafc5-170">This class is for cleaning up this example by disabling and dropping hello table created by hello **CreateTable** class.</span></span>
7. <span data-ttu-id="eafc5-171">Uložit hello **DeleteTable.java** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-171">Save hello **DeleteTable.java** file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="eafc5-172">Sestavení a balíček aplikace hello</span><span class="sxs-lookup"><span data-stu-id="eafc5-172">Build and package hello application</span></span>
1. <span data-ttu-id="eafc5-173">Otevřete příkazový řádek a změňte adresáře toohello **hbaseapp** adresáře.</span><span class="sxs-lookup"><span data-stu-id="eafc5-173">Open a command prompt and change directories toohello **hbaseapp** directory.</span></span>
2. <span data-ttu-id="eafc5-174">Použijte následující příkaz toobuild soubor JAR obsahující hello aplikace hello:</span><span class="sxs-lookup"><span data-stu-id="eafc5-174">Use hello following command toobuild a JAR file that contains hello application:</span></span>

        mvn clean package

    <span data-ttu-id="eafc5-175">To vyčistí artefakty předchozí sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány, pak sestavení a balíčky aplikací hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-175">This cleans any previous build artifacts, downloads any dependencies that have not already been installed, then builds and packages hello application.</span></span>
3. <span data-ttu-id="eafc5-176">Když hello dokončení příkazu hello **hbaseapp\target** adresář obsahuje soubor s názvem **hbaseapp. 1.0 SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-176">When hello command completes, hello **hbaseapp\target** directory contains a file named **hbaseapp-1.0-SNAPSHOT.jar**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="eafc5-177">Hello **hbaseapp. 1.0 SNAPSHOT.jar** uber je soubor jar (někdy nazývané fat jar,), který obsahuje všechny závislosti hello požadované aplikace hello toorun.</span><span class="sxs-lookup"><span data-stu-id="eafc5-177">hello **hbaseapp-1.0-SNAPSHOT.jar** file is an uber jar (sometimes called a fat jar,) which contains all hello dependencies required toorun hello application.</span></span>

## <a name="upload-hello-jar-file-and-start-a-job"></a><span data-ttu-id="eafc5-178">Nahrát soubor JAR hello a spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="eafc5-178">Upload hello JAR file and start a job</span></span>
<span data-ttu-id="eafc5-179">Existuje mnoho způsobů tooupload cluster HDInsight tooyour souboru jak je popsáno v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="eafc5-179">There are many ways tooupload a file tooyour HDInsight cluster, as described in [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span> <span data-ttu-id="eafc5-180">Hello následující kroky pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eafc5-180">hello following steps use Azure PowerShell.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. <span data-ttu-id="eafc5-181">Po instalaci a konfiguraci Azure Powershellu, vytvořte nový soubor s názvem **hbase runner.psm1**.</span><span class="sxs-lookup"><span data-stu-id="eafc5-181">After installing and configuring Azure PowerShell, create a new file named **hbase-runner.psm1**.</span></span> <span data-ttu-id="eafc5-182">Použijte následující hello jako hello obsah tohoto souboru:</span><span class="sxs-lookup"><span data-stu-id="eafc5-182">Use hello following as hello contents of this file:</span></span>

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

    <span data-ttu-id="eafc5-183">Tento soubor obsahuje dva moduly:</span><span class="sxs-lookup"><span data-stu-id="eafc5-183">This file contains two modules:</span></span>

   * <span data-ttu-id="eafc5-184">**Přidat HDInsightFile** – používá soubory tooHDInsight tooupload</span><span class="sxs-lookup"><span data-stu-id="eafc5-184">**Add-HDInsightFile** - used tooupload files tooHDInsight</span></span>
   * <span data-ttu-id="eafc5-185">**Spuštění HBaseExample** -používané třídy hello toorun vytvořený</span><span class="sxs-lookup"><span data-stu-id="eafc5-185">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>
2. <span data-ttu-id="eafc5-186">Uložit hello **hbase runner.psm1** souboru.</span><span class="sxs-lookup"><span data-stu-id="eafc5-186">Save hello **hbase-runner.psm1** file.</span></span>
3. <span data-ttu-id="eafc5-187">Otevřete nové okno Azure PowerShell, změňte adresáře toohello **hbaseapp** adresář, a pak spusťte hello následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="eafc5-187">Open a new Azure PowerShell window, change directories toohello **hbaseapp** directory, and then run hello following command.</span></span>

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    <span data-ttu-id="eafc5-188">Změnit umístění toohello cesta hello hello **hbase runner.psm1** soubor vytvořený dříve.</span><span class="sxs-lookup"><span data-stu-id="eafc5-188">Change hello path toohello location of hello **hbase-runner.psm1** file created earlier.</span></span> <span data-ttu-id="eafc5-189">Tento modul hello zaregistruje pro tuto relaci prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eafc5-189">This registers hello module for this Azure PowerShell session.</span></span>
4. <span data-ttu-id="eafc5-190">Použití hello následující příkaz tooupload hello **hbaseapp. 1.0 SNAPSHOT.jar** tooyour clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-190">Use hello following command tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight cluster.</span></span>

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    <span data-ttu-id="eafc5-191">Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-191">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="eafc5-192">příkaz Hello odešle hello **hbaseapp. 1.0 SNAPSHOT.jar** toohello **příklad/JAR** umístění v hello primárního úložiště pro váš cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-192">hello command uploads hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **example/jars** location in hello primary storage for your HDInsight cluster.</span></span>
5. <span data-ttu-id="eafc5-193">Po hello soubory jsou odeslány, použijte hello následující kód toocreate tabulky s použitím hello **hbaseapp**:</span><span class="sxs-lookup"><span data-stu-id="eafc5-193">After hello files are uploaded, use hello following code toocreate a table using hello **hbaseapp**:</span></span>

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    <span data-ttu-id="eafc5-194">Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-194">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="eafc5-195">Tento příkaz vytvoří novou tabulku s názvem **osoby** v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-195">This command creates a new table named **people** in your HDInsight cluster.</span></span> <span data-ttu-id="eafc5-196">Tento příkaz v okně konzoly hello nezobrazuje žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eafc5-196">This command does not show any output in hello console window.</span></span>
6. <span data-ttu-id="eafc5-197">toosearch pro položky v tabulce hello hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="eafc5-197">toosearch for entries in hello table, use hello following command:</span></span>

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    <span data-ttu-id="eafc5-198">Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-198">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="eafc5-199">Tento příkaz používá hello **SearchByEmail** třídy toosearch pro všechny řádky, kde hello **contactinformation** rodin sloupců a hello **e-mailu** sloupec obsahuje řetězec hello **contoso.com**. Měli byste obdržet hello následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="eafc5-199">This command uses hello **SearchByEmail** class toosearch for any rows where hello **contactinformation** column family and hello **email** column, contains hello string **contoso.com**. You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="eafc5-200">Pomocí **fabrikam.com** pro hello `-emailRegex` hodnota vrátí hello uživatele, kteří mají **fabrikam.com** v poli hello e-mailu.</span><span class="sxs-lookup"><span data-stu-id="eafc5-200">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="eafc5-201">Vzhledem k tomu, že toto hledání se implementuje pomocí regulárních založené na výrazu filtru, můžete také zadat regulární výrazy, například **^ r**, které vrátí položky, kde e-mailu hello začíná textem hello písmeno "r".</span><span class="sxs-lookup"><span data-stu-id="eafc5-201">Since this search is implemented by using a regular expression-based filter, you can also enter regular expressions, such as **^r**, which returns entries where hello email begins with hello letter 'r'.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="eafc5-202">Odstranit tabulku hello</span><span class="sxs-lookup"><span data-stu-id="eafc5-202">Delete hello table</span></span>
<span data-ttu-id="eafc5-203">Když jste hotovi s hello příklad použití hello následující příkaz z hello prostředí Azure PowerShell relace toodelete hello **osoby** tabulka použitá v tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="eafc5-203">When you are done with hello example, use hello following command from hello Azure PowerShell session toodelete hello **people** table used in this example:</span></span>

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

<span data-ttu-id="eafc5-204">Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eafc5-204">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="eafc5-205">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="eafc5-205">Troubleshooting</span></span>
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="eafc5-206">Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample</span><span class="sxs-lookup"><span data-stu-id="eafc5-206">No results or unexpected results when using Start-HBaseExample</span></span>
<span data-ttu-id="eafc5-207">Použití hello `-showErr` parametr tooview hello standardní chyba (STDERR) vytvořený při spuštěná úloha hello.</span><span class="sxs-lookup"><span data-stu-id="eafc5-207">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>
