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
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Používání Maven toobuild Java aplikací, které používají HBase s HDInsight se systémem Windows (Hadoop)
Zjistěte, jak toocreate a vytvoření [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java pomocí Apache Maven. Pak použijete hello aplikací s Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) je software projektu správy a míru porozumění nástroj, který vám umožní toobuild softwaru, dokumentace a sestav pro projekty Java. V tomto článku se dozvíte, jak toouse ho toocreate základní aplikaci Java, která vytvoří, dotazy a odstraní HBase, které tabulky v clusteru Azure HDInsight.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Požadavky
* [Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 nebo novější
* [Maven](http://maven.apache.org/)
* Cluster HDInsight se systémem Windows s HBase

    > [!NOTE]
    > Hello kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.2 a 3.3. Hello výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.3.

## <a name="create-hello-project"></a>Vytvoření projektu hello
1. Z příkazového řádku hello ve vašem vývojovém prostředí, změňte adresáře toohello umístění, kam má toocreate hello projektu, například `cd code\hdinsight`.
2. Použití hello **mvn** příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Tento příkaz vytvoří adresář s názvem hello určeného hello v aktuální umístění hello, **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje hello následující položky:

   * **pom.xml**: hello projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje informace a konfigurace projektu hello toobuild podrobnosti použít.
   * **src**: hello adresář, který obsahuje hello **main\java\com\microsoft\examples** adresáře, kde bude vytvářet aplikace hello.
3. Odstranit hello **src\test\java\com\microsoft\examples\apptest.java** souboru, protože není použit v tomto příkladu.

## <a name="update-hello-project-object-model"></a>Aktualizace hello projektu objektový Model
1. Upravit hello **pom.xml** souboru a přidejte následující kód do hello hello `<dependencies>` části:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Tato část informuje Maven, který hello projektu vyžaduje **hbase-client** verze **1.1.2**. Při kompilaci je stažen tuto závislost z hello výchozí Maven úložiště. Můžete použít hello [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn Další informace o této závislosti.

   > [!IMPORTANT]
   > Hello číslo verze musí odpovídat verzi hello hbase, který je zadán v rámci clusteru HDInsight. Pomocí následující tabulky toofind hello správnou verzi číslo hello.
   >
   >

   | Verze clusteru HDInsight | Verze toouse HBase |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    Další informace o verzích HDInsight a součásti najdete v tématu [co jsou hello různých komponent systému Hadoop HDInsight k dispozici](hdinsight-component-versioning.md).
2. Pokud používáte cluster služby HDInsight 3.3, musíte taky přidat hello následující toohello `<dependencies>` části:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    Tuto závislost načte hello phoenix – základní komponenty, které používají Hbase verze 1.1.x.
3. Přidejte následující kód toohello hello **pom.xml** souboru. Tato část musí být uvnitř hello `<project>...</project>` značky v hello souboru, například mezi `</dependencies>` a `</project>`.

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

    Hello `<resources>` části nakonfiguruje prostředku (**conf\hbase-site.xml**) obsahující informace o konfiguraci pro HBase.

   > [!NOTE]
   > Můžete také nastavit hodnoty konfigurace prostřednictvím kódu. V tématu hello komentáře v hello **CreateTable** příklad, který následuje jak toodo to.
   >
   >

    To `<plugins>` části nakonfiguruje hello [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/). modul plug-in Hello kompilátoru je použité toocompile hello topologie. Hello stín modulu plug-in se použité tooprevent licence duplikace v hello JAR balíček, který je sestavena Maven. Hello používá skutečnost, že hello duplicitní licenčních souborů dojít k chybě za běhu v clusteru HDInsight hello. Pomocí modulu plug-in maven stín hello `ApacheLicenseResourceTransformer` implementace brání této chybě.

    Hello modul plug-in maven stín také vytváří uber jar (nebo fat jar), který obsahuje všechny závislosti hello požadované aplikace hello.
4. Uložit hello **pom.xml** souboru.
5. Vytvořte nový adresář s názvem **conf** v hello **hbaseapp** adresáře. V hello **conf** adresáře, vytvořte soubor s názvem **hbase-site.xml**. Použijte následující hello jako hello obsah souboru hello:

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

    Tento soubor bude použité tooload hello HBase konfigurace clusteru služby HDInsight.

   > [!NOTE]
   > Toto je soubor minimální hbase-site.xml, a obsahuje hello úplné minimální nastavení pro hello HDInsight cluster.

6. Uložit hello **hbase-site.xml** souboru.

## <a name="create-hello-application"></a>Vytvoření aplikace hello
1. Přejděte toohello **hbaseapp\src\main\java\com\microsoft\examples** adresáře a přejmenujte hello soubor app.java příliš**CreateTable.java**.
2. Otevřete hello **CreateTable.java** souboru a nahradit existující obsah hello hello následující kód:

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

    Toto je hello **CreateTable** třída, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.
3. Uložit hello **CreateTable.java** souboru.
4. V hello **hbaseapp\src\main\java\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **SearchByEmail.java**. Použijte následující kód jako obsah tohoto souboru hello hello:

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

    Hello **SearchByEmail** třídy lze použít tooquery pro řádků a e-mailovou adresu. Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při použití třídy hello.
5. Uložit hello **SearchByEmail.java** souboru.
6. V hello **hbaseapp\src\main\hava\com\microsoft\examples** adresáře, vytvořte nový soubor s názvem **DeleteTable.java**. Použijte následující kód jako obsah tohoto souboru hello hello:

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

    Tato třída je pro čištění v tomto příkladu zakázáním a vyřazení hello tabulky vytvořené hello **CreateTable** třídy.
7. Uložit hello **DeleteTable.java** souboru.

## <a name="build-and-package-hello-application"></a>Sestavení a balíček aplikace hello
1. Otevřete příkazový řádek a změňte adresáře toohello **hbaseapp** adresáře.
2. Použijte následující příkaz toobuild soubor JAR obsahující hello aplikace hello:

        mvn clean package

    To vyčistí artefakty předchozí sestavení, stáhne všechny závislosti, které dosud nebyly nainstalovány, pak sestavení a balíčky aplikací hello.
3. Když hello dokončení příkazu hello **hbaseapp\target** adresář obsahuje soubor s názvem **hbaseapp. 1.0 SNAPSHOT.jar**.

   > [!NOTE]
   > Hello **hbaseapp. 1.0 SNAPSHOT.jar** uber je soubor jar (někdy nazývané fat jar,), který obsahuje všechny závislosti hello požadované aplikace hello toorun.

## <a name="upload-hello-jar-file-and-start-a-job"></a>Nahrát soubor JAR hello a spustit úlohu
Existuje mnoho způsobů tooupload cluster HDInsight tooyour souboru jak je popsáno v [nahrávání dat pro úlohy Hadoop do HDInsight](hdinsight-upload-data.md). Hello následující kroky pomocí prostředí Azure PowerShell.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Po instalaci a konfiguraci Azure Powershellu, vytvořte nový soubor s názvem **hbase runner.psm1**. Použijte následující hello jako hello obsah tohoto souboru:

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

    Tento soubor obsahuje dva moduly:

   * **Přidat HDInsightFile** – používá soubory tooHDInsight tooupload
   * **Spuštění HBaseExample** -používané třídy hello toorun vytvořený
2. Uložit hello **hbase runner.psm1** souboru.
3. Otevřete nové okno Azure PowerShell, změňte adresáře toohello **hbaseapp** adresář, a pak spusťte hello následující příkaz.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Změnit umístění toohello cesta hello hello **hbase runner.psm1** soubor vytvořený dříve. Tento modul hello zaregistruje pro tuto relaci prostředí Azure PowerShell.
4. Použití hello následující příkaz tooupload hello **hbaseapp. 1.0 SNAPSHOT.jar** tooyour clusteru HDInsight.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight. příkaz Hello odešle hello **hbaseapp. 1.0 SNAPSHOT.jar** toohello **příklad/JAR** umístění v hello primárního úložiště pro váš cluster HDInsight.
5. Po hello soubory jsou odeslány, použijte hello následující kód toocreate tabulky s použitím hello **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.

    Tento příkaz vytvoří novou tabulku s názvem **osoby** v clusteru HDInsight. Tento příkaz v okně konzoly hello nezobrazuje žádný výstup.
6. toosearch pro položky v tabulce hello hello použijte následující příkaz:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.

    Tento příkaz používá hello **SearchByEmail** třídy toosearch pro všechny řádky, kde hello **contactinformation** rodin sloupců a hello **e-mailu** sloupec obsahuje řetězec hello **contoso.com**. Měli byste obdržet hello následující výsledky:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Pomocí **fabrikam.com** pro hello `-emailRegex` hodnota vrátí hello uživatele, kteří mají **fabrikam.com** v poli hello e-mailu. Vzhledem k tomu, že toto hledání se implementuje pomocí regulárních založené na výrazu filtru, můžete také zadat regulární výrazy, například **^ r**, které vrátí položky, kde e-mailu hello začíná textem hello písmeno "r".

## <a name="delete-hello-table"></a>Odstranit tabulku hello
Když jste hotovi s hello příklad použití hello následující příkaz z hello prostředí Azure PowerShell relace toodelete hello **osoby** tabulka použitá v tomto příkladu:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Nahraďte **hdinsightclustername** s názvem hello clusteru HDInsight.

## <a name="troubleshooting"></a>Řešení potíží
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample
Použití hello `-showErr` parametr tooview hello standardní chyba (STDERR) vytvořený při spuštěná úloha hello.
