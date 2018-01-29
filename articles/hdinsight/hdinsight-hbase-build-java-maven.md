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
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="use-maven-to-build-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Používání Maven k sestavení aplikací Java, které používají HBase s HDInsight se systémem Windows (Hadoop)
Zjistěte, jak vytvořte a sestavte [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java pomocí Apache Maven. Pak použijete aplikaci s Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) je software projektu správy a míru porozumění nástroj, který umožňuje vytvářet softwaru, dokumentace a sestav pro projekty Java. V tomto článku zjistěte, jak použít jej k vytvořit základní aplikaci Java, která, které vytváří, dotazy a odstraní tabulky HBase v clusteru Azure HDInsight.

> [!IMPORTANT]
> Kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows. HDInsight od verze 3.4 výše používá výhradně operační systém Linux. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Požadavky
* [Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější
* [Maven](http://maven.apache.org/)
* Cluster HDInsight se systémem Windows s HBase

    > [!NOTE]
    > Kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.2 a 3.3. Výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.3.

## <a name="create-the-project"></a>Vytvoření projektu
1. Z příkazového řádku ve vašem vývojovém prostředí, změňte adresáře na umístění, kde chcete vytvořit projekt, například `cd code\hdinsight`.
2. Použití **mvn** příkazu, který se instaluje s Maven, vygenerujte generování uživatelského rozhraní pro projekt.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tento příkaz vytvoří adresář v aktuálním umístění s názvem zadaným **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje následující položky:

   * **pom.xml**: projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje podrobnosti o informace a konfigurace použít k sestavení projektu.
   * **src**: adresář, který obsahuje **main\java\com\microsoft\examples** adresáře, kde bude vytvářet aplikace.
3. Odstranit **src\test\java\com\microsoft\examples\apptest.java** souboru, protože není použit v tomto příkladu.

## <a name="update-the-project-object-model"></a>Aktualizace projektu objektový Model
1. Upravit **pom.xml** souboru a přidejte následující kód do `<dependencies>` části:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Tato část informuje Maven, že projekt vyžaduje **hbase-client** verze **1.1.2**. Při kompilaci je stažen tuto závislost z úložiště Maven výchozí. Můžete použít [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) Další informace o této závislosti.

   > [!IMPORTANT]
   > Číslo verze musí odpovídat verzi HBase, který je zadán v rámci clusteru HDInsight. Pomocí následující tabulky najít číslo správnou verzi.
   >
   >

   | Verze clusteru HDInsight | HBase verze se má použít |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    Další informace o verzích HDInsight a součásti najdete v tématu [co jsou různé součásti Hadoop, která je k dispozici s HDInsight](hdinsight-component-versioning.md).
2. Pokud používáte cluster služby HDInsight 3.3, musíte taky přidat následující `<dependencies>` části:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Tuto závislost načte phoenix základních komponent, které používají Hbase verze 1.1.x.
3. Přidejte následující kód, který **pom.xml** souboru. Tato část musí být uvnitř `<project>...</project>` značky v souboru, například mezi `</dependencies>` a `</project>`.

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

    `<resources>` Části nakonfiguruje prostředku (**conf\hbase-site.xml**) obsahující informace o konfiguraci pro HBase.

   > [!NOTE]
   > Můžete také nastavit hodnoty konfigurace prostřednictvím kódu. Zobrazte komentáře ve **CreateTable** příklad, který následuje pro jak na to.
   >
   >

    To `<plugins>` části nakonfiguruje [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/). Modul plug-in kompilátoru se používá ke kompilaci topologii. Modul plug-in stín se používá při prevenci licence duplikace v JAR balíček, který je sestavena Maven. Používá se důvodem je, že duplicitní licenčních souborů dojít k chybě za běhu v clusteru HDInsight. Používání maven stín – modul plug-in s `ApacheLicenseResourceTransformer` implementace brání této chybě.

    Plugin stín maven také vytváří uber jar (nebo fat jar), který obsahuje všechny závislosti, které jsou požadované aplikací.
4. Uložit **pom.xml** souboru.
5. Vytvořte nový adresář s názvem **conf** v **hbaseapp** adresáře. V **conf** adresáře, vytvořte soubor s názvem **hbase-site.xml**. Použijte následující postupy jako obsah souboru:

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

    Tento soubor se použije načíst konfiguraci HBase pro cluster služby HDInsight.

   > [!NOTE]
   > Toto je soubor minimální hbase-site.xml, a obsahuje úplné minimální nastavení clusteru HDInsight.

6. Uložit **hbase-site.xml** souboru.

## <a name="create-the-application"></a>Vytvoření aplikace
1. Přejděte na **hbaseapp\src\main\java\com\microsoft\examples** adresáře a přejmenujte app.java soubor na **CreateTable.java**.
2. Otevřete **CreateTable.java** souboru a nahradit existující obsah následujícím kódem:

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

    Toto je **CreateTable** třída, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.
3. Uložit **CreateTable.java** souboru.
4. V **hbaseapp\src\main\java\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **SearchByEmail.java**. Použijte následující kód jako obsah tohoto souboru:

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

    **SearchByEmail** třídu lze použít k dotazu pro řádků a e-mailovou adresu. Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při používání třídy.
5. Uložit **SearchByEmail.java** souboru.
6. V **hbaseapp\src\main\hava\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **DeleteTable.java**. Použijte následující kód jako obsah tohoto souboru:

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

    Tato třída je pro čištění v tomto příkladu zakázáním a vyřazení tabulky vytvořené **CreateTable** třídy.
7. Uložit **DeleteTable.java** souboru.

## <a name="build-and-package-the-application"></a>Sestavení a balíček aplikace
1. Otevřete příkazový řádek a přejděte do adresáře **hbaseapp** adresáře.
2. Vytvořit soubor JAR obsahující aplikaci, použijte následující příkaz:

        mvn clean package

    To vyčistí artefakty předchozí sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány, pak sestavení a balíčky aplikace.
3. Po dokončení příkazu **hbaseapp\target** adresář obsahuje soubor s názvem **hbaseapp. 1.0 SNAPSHOT.jar**.

   > [!NOTE]
   > **Hbaseapp. 1.0 SNAPSHOT.jar** uber je soubor jar (někdy nazývané fat jar,), který obsahuje všechny závislosti potřebné ke spuštění aplikace.

## <a name="upload-the-jar-file-and-start-a-job"></a>Nahrát soubor JAR a spustit úlohu
Existuje mnoho způsobů, jak nahrát soubor ke svému clusteru HDInsight, jak je popsáno v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md). Následující kroky pomocí prostředí Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Po instalaci a konfiguraci Azure Powershellu, vytvořte nový soubor s názvem **hbase runner.psm1**. Následující tabulka obsahuje obsah tohoto souboru:

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

    Tento soubor obsahuje dva moduly:

   * **Přidat HDInsightFile** – použít k nahrání souborů do HDInsight
   * **Spuštění HBaseExample** – používá ke spuštění třídy vytvořený
2. Uložit **hbase runner.psm1** souboru.
3. Otevřete nové okno Azure PowerShell, přejděte do adresáře **hbaseapp** adresář a potom spusťte následující příkaz.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Změňte cestu k umístění **hbase runner.psm1** soubor vytvořený dříve. Takovém postupu zaregistruje modul pro tuto relaci prostředí Azure PowerShell.
4. Použijte následující příkaz pro nahrání **hbaseapp. 1.0 SNAPSHOT.jar** ke svému clusteru HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Nahraďte **hdinsightclustername** s názvem clusteru HDInsight. Odešle příkaz **hbaseapp. 1.0 SNAPSHOT.jar** k **příklad/JAR** umístění primárního úložiště pro váš cluster HDInsight.
5. Jakmile soubory odešlete, použijte následující kód k vytvoření tabulky pomocí **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.

    Tento příkaz vytvoří novou tabulku s názvem **osoby** v clusteru HDInsight. Tento příkaz nezobrazuje žádný výstup v okně konzoly.
6. K vyhledání položky v tabulce, použijte následující příkaz:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.

    Tento příkaz používá **SearchByEmail** třídy pro vyhledávání pro všechny řádky, kde **contactinformation** rodin sloupců a **e-mailu** sloupec obsahuje řetězec **contoso.com**. Mělo by se zobrazit následující výsledky:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Pomocí **fabrikam.com** pro `-emailRegex` hodnota vrátí uživatele, kteří mají **fabrikam.com** v poli e-mailu. Vzhledem k tomu, že toto hledání se implementuje pomocí regulárních založené na výrazu filtru, můžete také zadat regulární výrazy, například **^ r**, které vrátí položky e-mailu, kde začíná písmenem "r".

## <a name="delete-the-table"></a>Odstranit tabulku
Až skončíte se na příklad, použijte následující příkaz z relace prostředí Azure PowerShell k odstranění **osoby** tabulka použitá v tomto příkladu:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Nahraďte **hdinsightclustername** s názvem clusteru HDInsight.

## <a name="troubleshooting"></a>Řešení potíží
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample
Použití `-showErr` parametr zobrazíte standardní chyba (STDERR), která je vytvořena při spuštění úlohy.
