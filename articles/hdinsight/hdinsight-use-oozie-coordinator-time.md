---
title: "v prostředí HDInsight založené na čase aaaUse Hadoop Oozie coordinator | Microsoft Docs"
description: "V prostředí HDInsight, Cloudová služba velkých dat pomocí Hadoop Oozie coordinator založené na čase. Zjistěte, jak toodefine Oozie pracovní postupy a koordinátory a odesílání úloh."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a>Použití založené na čase Oozie coordinator se systémem Hadoop v HDInsight toodefine pracovních a koordinovat úlohy
V tomto článku se dozvíte, jak toodefine pracovní postupy a koordinátoři a jak tootrigger hello koordinátor úlohy podle času. Je užitečné toogo prostřednictvím [Oozie použití s HDInsight] [ hdinsight-use-oozie] před přečtěte si tento článek. Kromě toho tooOozie, můžete také naplánovat úlohy pomocí Azure Data Factory. toolearn Azure Data Factory najdete v části [použijte Pig a Hive pomocí služby Data Factory](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> Tento článek vyžaduje cluster HDInsight se systémem Windows. Informace o používání Oozie, včetně úloh založené na čase, v clusteru se systémem Linux naleznete v části [Oozie použití s Hadoop toodefine a spuštění pracovního postupu na HDInsight se systémem Linux](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Co je Oozie
Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop. Je integrován se hello zásobníku Hadoop a podporuje úloh Hadoop pro Apache MapReduce, Apache Pig, Apache Hive a Apache Sqoop. Lze také použít tooschedule úlohy, které jsou specifické tooa systému, například programy v jazyce Java nebo skripty prostředí.

Hello následující obrázek ukazuje pracovní postup hello, který implementujete:

![Diagram pracovního postupu][img-workflow-diagram]

pracovní postup Hello obsahuje dvě akce:

1. Akce Hive spouští hello toocount skript HiveQL výskyty každý typ úroveň protokolu v souboru protokolu log4j. Každý log4j protokol se skládá z řádku pole, které obsahuje [úroveň protokolu] pole tooshow hello typu a hello závažnost, například:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    je podobná Hello výstup skriptu Hive:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].
2. Akce Sqoop exportuje hello HiveQL akce výstupní tooa tabulku v databázi Azure SQL. Další informace o Sqoop najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop].

> [!NOTE]
> Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů hello poskytovaných v HDInsight?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Pracovní stanice s prostředím Azure PowerShell**.

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a 1. ledna 2017 dojde k jejímu odebrání. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.

* **Cluster služby HDInsight**. Informace o vytváření clusteru služby HDInsight najdete v tématu [Tvorba clusterů HDInsight][hdinsight-provision], nebo [Začínáme s HDInsight][hdinsight-get-started]. Hello následující toogo data prostřednictvím hello kurzu budete potřebovat:

    <table border = "1">
    <tr><th>Vlastnost clusteru</th><th>Název proměnné prostředí Windows PowerShell</th><th>Hodnota</th><th>Popis</th></tr>
    <tr><td>Název clusteru HDInsight</td><td>$clusterName</td><td></td><td>cluster HDInsight Hello, na kterém budete spouštět v tomto kurzu.</td></tr>
    <tr><td>Uživatelské jméno clusteru HDInsight</td><td>$clusterUsername</td><td></td><td>Hello HDInsight clusteru uživatelské jméno. </td></tr>
    <tr><td>Heslo uživatele clusteru HDInsight </td><td>$clusterPassword</td><td></td><td>Hello heslo uživatele clusteru HDInsight.</td></tr>
    <tr><td>Název účtu úložiště Azure</td><td>$storageAccountName</td><td></td><td>Azure Storage účet k dispozici toohello clusteru služby HDInsight. V tomto kurzu použijte hello výchozí úložiště účet, který jste zadali během procesu zřizování clusteru hello.</td></tr>
    <tr><td>Název kontejneru Azure Blob</td><td>$containerName</td><td></td><td>V tomto příkladu použijte hello kontejner úložiště objektů Blob v Azure, který se používá pro hello výchozí systém souborů clusteru HDInsight. Ve výchozím nastavení má stejný název jako hello HDInsight cluster hello.</td></tr>
    </table>
* **Azure SQL database**. Je nutné nakonfigurovat pravidlo brány firewall pro přístup k databázi SQL serveru tooallow hello z pracovní stanice. Pokyny týkající se vytváření databáze Azure SQL a konfiguraci brány firewall hello najdete v tématu [začít používat Azure SQL database] [databáze_sql get-started]. Tento článek obsahuje skript prostředí Windows PowerShell pro vytvoření tabulky databáze Azure SQL hello, které potřebujete pro účely tohoto kurzu.

    <table border = "1">
    <tr><th>Vlastnost databáze SQL</th><th>Název proměnné prostředí Windows PowerShell</th><th>Hodnota</th><th>Popis</th></tr>
    <tr><td>Název databáze serveru SQL</td><td>$sqlDatabaseServer</td><td></td><td>Hello SQL databáze serveru toowhich Sqoop bude exportovat data. </td></tr>
    <tr><td>Přihlašovací jméno SQL databáze</td><td>$sqlDatabaseLogin</td><td></td><td>Přihlašovací jméno SQL Database.</td></tr>
    <tr><td>Heslo pro přihlášení databáze SQL</td><td>$sqlDatabaseLoginPassword</td><td></td><td>Heslo přihlášení k databázi SQL.</td></tr>
    <tr><td>Název databáze SQL</td><td>$sqlDatabaseName</td><td></td><td>Hello Azure SQL database toowhich Sqoop bude exportovat data. </td></tr>
    </table>

  > [!NOTE]
  > Ve výchozím nastavení Azure SQL database umožňuje připojení z Azure služby, jako je Azure HDInsight. Pokud toto nastavení brány firewall je zakázáno, musíte ji povolit z portálu Azure hello. Pokyny týkající se vytváření databáze SQL a konfigurace pravidel brány firewall, najdete v části [vytvořit a nakonfigurovat databázi SQL][sqldatabase-get-started].

> [!NOTE]
> Vyplňování hello hodnoty v tabulkách hello. Je užitečné při procházení tohoto kurzu.

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a>Definice pracovního postupu Oozie a hello související skript HiveQL
Definice Oozie pracovní postupy jsou zapsány ve hPDL (jazyka definice proces XML). Hello výchozí název souboru pracovního postupu je *workflow.xml*.  Budete uložte místně soubor hello pracovního postupu a poté ji nasadit toohello clusteru HDInsight pomocí prostředí Azure PowerShell později v tomto kurzu.

Hello Hive akce v pracovním postupu hello volá soubor skriptu HiveQL. Tento soubor skriptu obsahuje tři příkazy HiveQL:

1. **příkaz DROP TABLE Hello** odstranění hello tabulku Hive log4j, pokud existuje.
2. **příkaz CREATE TABLE Hello** vytvoří log4j externí tabulku Hive, který ukazuje toohello umístění souboru protokolu log4j hello;
3. **umístění souboru protokolu log4j hello Hello**. Oddělovač polí Hello je ",". oddělovač řádku výchozí Hello je "\n". Externí tabulku Hive je použité tooavoid hello datový soubor odebírán z hello původního umístění, v případě, že má toorun hello Oozie pracovní vícekrát.
4. **Hello vložit PŘEPSAT příkaz** počty hello výskyty každý typ úroveň protokolu z hello log4j tabulku Hive a ukládá umístění úložiště objektů Blob v Azure tooan výstup hello.

> [!NOTE]
> Je známý problém cesta Hive. Budete spouštět na tento problém při odesílání úlohu Oozie. Hello pokyny k opravě problému hello najdete na webu TechNet Wiki hello: [HDInsight Hive Chyba: nelze toorename][technetwiki-hive-error].

**toodefine hello HiveQL skriptu souboru toobe volá pracovní postup hello**

1. Vytvořte textový soubor s hello následující obsah:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Existují tři proměnné používané ve skriptu hello:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     Soubor definice pracovního postupu Hello (workflow.xml v tomto kurzu) předá tyto hodnoty toothis skript HiveQL v době běhu.
2. Uložte soubor hello jako **C:\Tutorials\UseOozie\useooziewf.hql** pomocí kódování ANSI (ASCII). (Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.) Tento soubor skriptu bude cluster HDInsight nasazené toohello později v kurzu hello.

**toodefine pracovního postupu**

1. Vytvořte textový soubor s hello následující obsah:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    Existují dvě akce definované v pracovním postupu hello. je Hello start-tooaction *RunHiveScript*. Pokud spuštění akce hello *OK*, je další akce hello *RunSqoopExport*.

    Hello RunHiveScript má několik proměnné. Při odesílání úlohy Oozie hello z pracovní stanice pomocí prostředí Azure PowerShell, projdou hello hodnoty.

    Proměnné pracovního postupu

    <table border = "1">
    <tr><th>Proměnné pracovního postupu</th><th>Popis</th></tr>
    <tr><td>${jobTracker}</td><td>Zadejte adresu URL hello sledovací modul úlohy Hadoop hello. Použití <strong>jobtrackerhost:9010</strong> v HDInsight clusteru verze 3.0 a 2.0.</td></tr>
    <tr><td>${nameNode}</td><td>Zadejte adresu URL hello hello Hadoop název uzlu. Použít hello výchozí soubor systému wasb: / / adres, například <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>Určuje, že bude odeslána hello název fronty, který hello úlohy. Použití <strong>výchozí</strong>.</td></tr>
    </table>

    Proměnné akcí v Hive

    <table border = "1">
    <tr><th>Hive proměnné akce</th><th>Popis</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Hello zdrojový adresář pro hello příkaz Hive Create Table.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Hello výstupní složky pro hello příkaz INSERT PŘEPSAT.</td></tr>
    <tr><td>${hiveTableName}</td><td>Název Hello hello tabulku Hive, který odkazuje na hello log4j datových souborů.</td></tr>
    </table>

    Proměnné akcí v Sqoop

    <table border = "1">
    <tr><th>Sqoop proměnné akce</th><th>Popis</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>Připojovací řetězec databáze SQL.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>exportován Hello data hello toowhere tabulky v databázi Azure SQL.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>Hello výstupní složky pro hello Hive vložit PŘEPSAT příkaz. Toto je hello stejné složce, hello Sqoop export (export-dir).</td></tr>
    </table>

    Další informace o pracovním postupu Oozie a použití hello akce pracovního postupu najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [Apache Oozie 3.3.2 dokumentace] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).

1. Uložte soubor hello jako **C:\Tutorials\UseOozie\workflow.xml** pomocí kódování ANSI (ASCII). (Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)

**toodefine coordinator**

1. Vytvořte textový soubor s hello následující obsah:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Existují pět proměnné používané v souboru definice hello:

   | Proměnná | Popis |
   | --- | --- |
   | ${coordFrequency} |Čas pozastavení úlohy. Frekvence je vždy vyjádřené v minutách. |
   | ${coordStart} |Čas spuštění úlohy. |
   | ${coordEnd} |Čas ukončení úlohy. |
   | ${coordTimezone} |Oozie zpracovává koordinátor úlohy v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC). Toto časové pásmo se označuje jako hello "Oozie zpracování časové pásmo." |
   | ${wfPath} |Hello cesta pro hello workflow.xml.  Pokud název souboru pracovního postupu hello není hello výchozí název souboru (workflow.xml), je nutné zadat. |
2. Uložte soubor hello jako **C:\Tutorials\UseOozie\coordinator.xml** pomocí kódování ANSI (ASCII) hello. (Použijte Poznámkový blok, pokud textového editoru neposkytuje tuto možnost.)

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a>Nasazení projektu Oozie hello a připravte hello kurzu
Budete spouštět prostředí Azure PowerShell skriptu tooperform hello následující:

* Kopírování hello úložiště objektů Blob tooAzure HiveQL skriptu (useoozie.hql), wasb:///tutorials/useoozie/useoozie.hql.
* Zkopírujte workflow.xml toowasb:///tutorials/useoozie/workflow.xml.
* Zkopírujte coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml.
* Kopírování hello datového souboru (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.
* Vytvoření tabulky databáze Azure SQL pro ukládání dat export Sqoop. Název tabulky Hello je *log4jLogCount*.

**Pochopení úložiště HDInsight**

HDInsight používá úložiště objektů Blob v Azure pro úložiště dat. wasb: / / je implementace systému souborů Hadoop distributed (HDFS) hello v Azure Blob storage společnosti Microsoft. Další informace najdete v tématu [použití Azure Blob storage s HDInsight][hdinsight-storage].

Při zřizování clusteru služby HDInsight, účet úložiště objektů Blob v Azure a konkrétní kontejner z daného účtu je určený jako hello výchozí systém souborů, jako v HDFS. Kromě toho toothis účet úložiště, můžete přidat další úložiště účtů z hello stejné předplatné Azure nebo z různých předplatných Azure během procesu zřizování hello. Pokyny o přidání dalších účtů úložiště najdete v tématu [zřizování clusterů HDInsight][hdinsight-provision]. skript prostředí PowerShell Azure hello toosimplify použili v tomto kurzu, všechny soubory se ukládají do kontejneru systému souboru výchozí hello hello umístěné v */kurzy/useoozie*. Ve výchozím nastavení má tento kontejner hello stejný název jako název clusteru HDInsight hello.
Hello syntaxe je:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> Pouze hello *wasb: / /* syntaxe je podporován v clusteru HDInsight verze 3.0. Hello starší *asv: / /* syntaxe je podporován v HDInsight 2.1 a 1.6 clustery, ale není podporována v clusterech HDInsight 3.0.
>
> Hello wasb: / / cesta je virtuální cesta. Další informace najdete v části [použití Azure Blob storage s HDInsight][hdinsight-storage].

Soubor, který je uložen v kontejneru systému souboru výchozí hello je přístupná z prostředí HDInsight pomocí některé z hello následující identifikátory URI (používám workflow.xml jako příklad):

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Pokud chcete soubor hello tooaccess přímo z účtu úložiště hello, název objektu blob hello hello souboru je:

    tutorials/useoozie/workflow.xml

**Pochopení interních a externích tabulek Hive**

Existuje několik věcí, které je třeba tooknow o vnitřních a vnějších tabulek Hive:

* příkaz CREATE TABLE Hello vytvoří interní tabulku, také známé jako spravované tabulku. Hello datového souboru se musí nacházet v kontejneru výchozí hello.
* příkaz CREATE TABLE Hello přesune hello data soubortoohello/hive/skladu/<TableName> složky v hello výchozí kontejner.
* Hello vytvoření externí tabulky příkaz vytvoří externí tabulku. Hello datového souboru může být umístěn mimo výchozí kontejner hello.
* příkaz CREATE TABLE externí Hello nepřesouvá hello datový soubor.
* příkaz CREATE TABLE externí Hello neumožňuje všechny podsložky hello složky, která je zadána v klauzuli umístění hello. Toto je hello důvod, proč hello kurzu vytvoří kopii souboru sample.log hello.

Další informace najdete v tématu [HDInsight: Hive interní a externí tabulky ÚVOD][cindygross-hive-tables].

**kurz tooprepare hello**

1. Otevřete hello Windows PowerShell ISE (na obrazovce Start systému Windows 8 hello zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**. Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).
2. V dolním podokně hello spusťte následující příkaz tooconnect tooyour předplatného Azure hello:

    ```powershell
    Add-AzureAccount
    ```

    Můžete se výzvami tooenter přihlašovací údaje účtu Azure. Tato metoda přidávání připojení předplatného vyprší, a po 12 hodinách, bude třeba toorun hello rutinu znovu.

   > [!NOTE]
   > Pokud máte více předplatných Azure a předplatné výchozí hello není hello ten, který chcete toouse, použijte hello <strong>Select-AzureSubscription</strong> rutiny tooselect předplatné.

3. Zkopírujte následující skript do podokno skriptu hello hello a pak nastavte hello prvních šesti proměnné:

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    Další popis hello proměnných najdete v tématu hello [požadavky](#prerequisites) v tomto kurzu.

4. Připojte hello následující skript toohello v hello skript:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create hello log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** toorun hello skriptu. Hello výstup bude vypadat podobně jako:

    ![Kurz přípravy výstup][img-preparation-output]

## <a name="run-hello-oozie-project"></a>Spusťte projekt Oozie hello
Prostředí Azure PowerShell aktuálně neposkytuje žádné rutiny pro definování Oozie úloh. Můžete použít hello **Invoke-RestMethod** rutiny tooinvoke Oozie webové služby. rozhraní API webových služeb Oozie Hello je JSON rozhraní HTTP REST API. Další informace o rozhraní API hello Oozie webových služeb najdete v tématu [dokumentaci Apache Oozie 4.0] [ apache-oozie-400] (u clusteru HDInsight verze 3.0) nebo [dokumentaci Apache Oozie 3.3.2] [ apache-oozie-332] (u clusteru HDInsight verze 2.1).

**toosubmit úlohu Oozie**

1. Otevřete hello Windows PowerShell ISE (na obrazovce Start systému Windows 8, zadejte **PowerShell_ISE**a potom klikněte na **Windows PowerShell ISE**. Další informace najdete v tématu [spuštění prostředí Windows PowerShell ve Windows 8 a Windows][powershell-start]).
2. Kopírování hello následující skript do hello podokno skriptu a pak sadu hello nejprve čtrnáct proměnné (však přeskočit **$storageUri**).

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Další popis hello proměnných najdete v tématu hello [požadavky](#prerequisites) v tomto kurzu.

    $coordstart a $coordend jsou hello pracovního postupu počáteční a koncový čas. toofind na čas UTC nebo GMT hello hledání "čas utc" na vyhledávače bing.com. Hello $coordFrequency se jak často má toorun hello pracovní minut.
3. Připojí hello následující toohello skriptu. Tato část definuje datovou část Oozie hello:

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > Hello hlavní rozdíl oproti toohello pracovní postup odesílání datové části souboru je hello proměnná **oozie.coord.application.path**. Při odesílání úlohy pracovního postupu použijete **oozie.wf.application.path** místo.

4. Připojí hello následující toohello skriptu. Tato část kontroluje stav hello Oozie webové služby:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. Připojí hello následující toohello skriptu. Tato část vytvoří úlohu Oozie:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > Při odesílání úlohy pracovního postupu, musíte provést další webové služby volání toostart hello úloha po vytvoření úlohy hello. V takovém případě je spuštěna úloha coordinator hello podle času. Hello úlohy se spustí automaticky.

6. Připojí hello následující toohello skriptu. Tato část kontroluje stav úlohy Oozie hello:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. (Volitelné) Připojí hello následující toohello skriptu.

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. Připojte hello následující toohello skriptu:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    Pokud chcete, aby toorun hello další funkce, odeberte znak # hello.
9. Pokud váš clusteru HDinsight verze 2.1, nahraďte "https://$clusterName.azurehdinsight.net:443/oozie/v2/" s "https://$clusterName.azurehdinsight.net:443/oozie/v1/". Verze clusteru HDInsight 2.1 nemá podporuje verze 2 hello webové služby.
10. Klikněte na tlačítko **spustit skript** nebo stiskněte klávesu **F5** toorun hello skriptu. Hello výstup bude vypadat podobně jako:

     ![Kurz spustit výstup pracovního postupu][img-runworkflow-output]
11. Připojte tooyour SQL Database toosee hello exportovat data.

**Protokol chyb úlohy toocheck hello**

tootroubleshoot pracovního postupu, soubor protokolu Oozie hello naleznete na C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log z clusteru headnode hello. Informace o protokolu RDP, najdete v části [hello clusterů HDInsight správa pomocí portálu Azure][hdinsight-admin-portal].

**kurz toorerun hello**

pracovní postup hello toorerun, je třeba provést hello následující úlohy:

* Odstraňte soubor výstup skriptu Hive hello.
* Odstraňte hello data v tabulce log4jLogsCount hello.

Tady je ukázkový skript prostředí Windows PowerShell, který můžete použít:

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toodefine pracovním postupu Oozie a Oozie coordinator, a jak toorun Oozie coordinator úlohy pomocí prostředí Azure PowerShell. toolearn více, najdete v části hello následující články:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Používat úložiště objektů Azure Blob s HDInsight][hdinsight-storage]
* [Spravovat HDInsight pomocí prostředí Azure PowerShell][hdinsight-admin-powershell]
* [Nahrání dat tooHDInsight][hdinsight-upload-data]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
