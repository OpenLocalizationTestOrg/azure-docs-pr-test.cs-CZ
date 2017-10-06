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
# <a name="build-java-applications-for-apache-hbase"></a>Sestavení aplikací Java pro Apache HBase

Zjistěte, jak toocreate [Apache HBase](http://hbase.apache.org/) aplikace v jazyce Java. Potom pomocí aplikace hello s HBase v Azure HDInsight.

Hello kroky v tomto dokumentu [Maven](http://maven.apache.org/) toocreate a sestavení projektu hello. Maven je správa softwaru projektu a míru porozumění nástroj, který umožňuje toobuild softwaru, dokumentace a sestav pro projekty Java.

> [!NOTE]
> Hello kroky v tomto dokumentu byly naposledy testovány s HDInsight 3.6.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Požadavky

* [Platforma Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 nebo novější.

    > [!NOTE]
    > HDInsight 3.5 a vyšší vyžaduje Java 8. Dřívějších verzích HDInsight vyžadují Java 7.

* [Maven](http://maven.apache.org/)

* [Cluster Azure HDInsight se systémem Linux s HBase](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > Hello kroky v tomto dokumentu byly testovány s verze clusteru HDInsight 3.4 a 3.5. Hello výchozí hodnoty zadané v příkladech jsou pro cluster HDInsight 3.5.

## <a name="create-hello-project"></a>Vytvoření projektu hello

1. Z příkazového řádku hello ve vašem vývojovém prostředí, změňte adresáře toohello umístění, kam má toocreate hello projektu, například `cd code\hbase`.

2. Použití hello **mvn** příkaz, který se instaluje s Maven, toogenerate hello generování uživatelského rozhraní pro projekt hello.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > Pokud používáte prostředí PowerShell, je nutné uzavřít hello `-D` parametry v uvozovkách.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Tento příkaz vytvoří adresář s hello stejný název jako hello **artifactID** parametr (**hbaseapp** v tomto příkladu.) Tento adresář obsahuje hello následující položky:

   * **pom.xml**: hello projektu objektový Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) obsahuje informace a konfigurace projektu hello toobuild podrobnosti použít.
   * **src**: hello adresář, který obsahuje hello **main/java/com/microsoft/příklady** adresáře, kde můžete vytvářet aplikace hello.

3. Odstranit hello `src/test/java/com/microsoft/examples/apptest.java` souboru. Není možné použít v tomto příkladu.

## <a name="update-hello-project-object-model"></a>Aktualizace hello projektu objektový Model

1. Upravit hello `pom.xml` souboru a přidejte následující kód do hello hello `<dependencies>` části:

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

    V této části určuje projektu hello musí **hbase-client** a **phoenix základní** součásti. Při kompilaci se stáhnou tyto závislosti z hello výchozí Maven úložiště. Můžete použít hello [Maven centrální úložiště hledání](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn Další informace o této závislosti.

   > [!IMPORTANT]
   > číslo verze Hello hello hbase-client musí odpovídat verzi hello hbase, který je zadán v rámci clusteru HDInsight. Pomocí následující tabulky toofind hello správnou verzi číslo hello.

   | Verze clusteru HDInsight | Verze toouse HBase |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4, 3.5 a 3.6 |1.1.2 |

    Další informace o verzích HDInsight a součásti najdete v tématu [co jsou hello různých komponent systému Hadoop HDInsight k dispozici](hdinsight-component-versioning.md).

3. Přidejte následující kód toohello hello **pom.xml** souboru. Tento text musí být uvnitř hello `<project>...</project>` značky v hello souboru, například mezi `</dependencies>` a `</project>`.

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

    Tato část nakonfiguruje prostředku (`conf/hbase-site.xml`) obsahující informace o konfiguraci pro HBase.

   > [!NOTE]
   > Můžete také nastavit hodnoty konfigurace prostřednictvím kódu. V tématu hello komentáře v hello `CreateTable` příklad.

    Tato část také nakonfiguruje hello [modulu plug-in kompilátoru Maven](http://maven.apache.org/plugins/maven-compiler-plugin/) a [modulu plug-in stín Maven](http://maven.apache.org/plugins/maven-shade-plugin/). modul plug-in Hello kompilátoru je použité toocompile hello topologie. Hello stín modulu plug-in se použité tooprevent licence duplikace v hello JAR balíček, který je sestavena Maven. Tento modul plug-in je použité tooprevent "duplicitní souborů s licencí" Chyba za běhu v clusteru HDInsight hello. Pomocí modulu plug-in maven stín hello `ApacheLicenseResourceTransformer` implementace brání chyba hello.

    Hello maven stín – modul plug-in vytvoří také uber jar, který obsahuje všechny závislosti hello požadované aplikace hello.

4. Uložit hello `pom.xml` souboru.

5. Vytvořte adresář s názvem `conf` v hello `hbaseapp` adresáře. Tento adresář se informace o konfiguraci toohold použité pro připojování tooHBase.

6. Použití hello následující příkaz toocopy hello HBase konfiguraci z toohello clusteru HBase hello `conf` adresáře. Nahraďte `USERNAME` s názvem hello vaše přihlášení SSH. Nahraďte `CLUSTERNAME` názvem clusteru HDInsight:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Další informace o používání `ssh` a `scp`, najdete v části [použití SSH s HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-hello-application"></a>Vytvoření aplikace hello

1. Přejděte toohello `hbaseapp/src/main/java/com/microsoft/examples` adresáře a přejmenujte hello soubor app.java příliš`CreateTable.java`.

2. Otevřete hello `CreateTable.java` souboru a nahradit existující obsah hello hello následující text:

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

    Tento kód je hello **CreateTable** třídy, která vytvoří tabulku s názvem **osoby** a jeho naplnění některé předdefinované uživatele.

3. Uložit hello `CreateTable.java` souboru.

4. V hello `hbaseapp/src/main/java/com/microsoft/examples` adresáře, vytvořte soubor s názvem `SearchByEmail.java`. Použijte hello následující text jako hello obsah tohoto souboru:

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

    Hello **SearchByEmail** třídy lze použít tooquery pro řádků a e-mailovou adresu. Protože používá regulární výraz filtru, můžete zadat řetězce nebo regulárního výrazu při použití třídy hello.

5. Uložit hello `SearchByEmail.java` souboru.

6. V hello `hbaseapp/src/main/hava/com/microsoft/examples` adresáře, vytvořte soubor s názvem `DeleteTable.java`. Použijte hello následující text jako hello obsah tohoto souboru:

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

    Tato třída vyčistí hello vytvořené v tomto příkladu zakázáním tabulky HBase a vyřazení hello tabulky vytvořené hello `CreateTable` třídy.

7. Uložit hello `DeleteTable.java` souboru.

## <a name="build-and-package-hello-application"></a>Sestavení a balíček aplikace hello

1. Z hello `hbaseapp` adresář, použijte hello následující příkaz toobuild soubor JAR obsahující aplikace hello:

    ```bash
    mvn clean package
    ```

    Tento příkaz vytvoří a balíčky hello aplikace do souboru .jar.

2. Když hello dokončení příkazu hello `hbaseapp/target` adresář obsahuje soubor s názvem `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > Hello `hbaseapp-1.0-SNAPSHOT.jar` je soubor jar uber. Obsahuje všechny aplikace hello požadované toorun hello závislosti.


## <a name="upload-hello-jar-and-run-jobs-ssh"></a>Nahrát hello JAR a spouštění úloh (SSH)

Následující postup použijte Hello `scp` toocopy hello JAR toohello primární hlavního uzlu vaší databáze hbase v clusteru HDInsight. Hello `ssh` příkaz je pak použit tooconnect toohello clusteru a spusťte hello příklad přímo na hello hlavního uzlu.

1. tooupload hello jar toohello cluster, hello použijte následující příkaz:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Nahraďte `USERNAME` s názvem hello vaše přihlášení SSH. Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.

2. tooconnect toohello clusteru HBase pomocí hello následující příkaz:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Nahraďte `USERNAME` hello název vaší přihlašování přes SSH. Nahraďte `CLUSTERNAME` názvem clusteru HDInsight.

3. toocreate tabulky HBase pomocí hello aplikaci Java, hello použijte následující příkaz:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Tento příkaz vytvoří tabulku HBase s názvem **osoby**a ta je s daty.

4. toosearch pro e-mailové adresy, které jsou uložené v tabulce hello hello použijte následující příkaz:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    Zobrazí hello následující výsledky:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. toodelete hello tabulky, hello použijte následující příkaz:

    

## <a name="upload-hello-jar-and-run-jobs-powershell"></a>Nahrát hello JAR a spouštění úloh (PowerShell)

Hello následující kroky pomocí prostředí Azure PowerShell tooupload hello JAR toohello výchozí úložiště pro váš cluster HBase. Rutiny služby HDInsight se pak použít toorun hello příklady vzdáleně.

1. Po instalaci a konfiguraci Azure Powershellu, vytvořte soubor s názvem `hbase-runner.psm1`. Použijte hello následující text jako hello obsah tohoto souboru:

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

    Tento soubor obsahuje dva moduly:

   * **Přidat HDInsightFile** – použité tooupload soubory toohello clusteru
   * **Spuštění HBaseExample** -používané třídy hello toorun vytvořený

2. Uložit hello `hbase-runner.psm1` souboru.

3. Otevřete nové okno Azure PowerShell, změňte adresáře toohello `hbaseapp` adresář, a pak spusťte hello následující příkaz:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Změnit umístění toohello cesta hello hello `hbase-runner.psm1` soubor vytvořený dříve. Tento příkaz registruje hello modulu Azure PowerShell.

4. Použití hello následující příkaz tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour clusteru.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru. příkaz Hello odešle hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` umístění v hello primárního úložiště pro cluster.

5. hello toocreate tabulky pomocí `hbaseapp`, použijte následující příkaz hello:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru.

    Tento příkaz vytvoří tabulku s názvem **osoby** v HBase v clusteru HDInsight. Tento příkaz v okně konzoly hello nezobrazuje žádný výstup.

6. toosearch pro položky v tabulce hello hello použijte následující příkaz:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    Nahraďte `hdinsightclustername` s hello názvem vašeho clusteru.

    Tento příkaz používá hello `SearchByEmail` třídy toosearch pro všechny řádky, kde hello `contactinformation` rodin sloupců a hello `email` sloupec obsahuje řetězec hello `contoso.com`. Měli byste obdržet hello následující výsledky:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Pomocí **fabrikam.com** pro hello `-emailRegex` hodnota vrátí hello uživatele, kteří mají **fabrikam.com** v poli hello e-mailu. Regulární výrazy můžete použít také jako hello hledaný termín. Například **^ r** vrátí e-mailové adresy, které začínají hello písmeno "r".

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Žádné výsledky nebo neočekávané výsledky při použití Start HBaseExample

Použití hello `-showErr` parametr tooview hello standardní chyba (STDERR) vytvořený při spuštěná úloha hello.

## <a name="delete-hello-table"></a>Odstranit tabulku hello

Až skončíte s hello příklad, použijte následující toodelete hello hello **osoby** tabulka použitá v tomto příkladu:

__Ze `ssh` relace__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Z prostředí Azure PowerShell__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Další kroky

[Zjistěte, jak toouse SQL SQuirreL s HBase](hdinsight-hbase-phoenix-squirrel-linux.md)
