---
title: "pracovní postupy aaaUse Hadoop Oozie v HDInsight se systémem Linux | Microsoft Docs"
description: "Použijte Hadoop Oozie v HDInsight se systémem Linux. Zjistěte, jak toodefine pracovním postupu Oozie a odešlete úlohu Oozie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: cb5682837543312621e3424b7a9341b5d2a00bf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a><span data-ttu-id="5644d-104">Pomocí Oozie Hadoop toodefine a spuštění workflowu v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="5644d-104">Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight</span></span>

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="5644d-105">Zjistěte, jak toouse Apache Oozie s Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5644d-105">Learn how toouse Apache Oozie with Hadoop on HDInsight.</span></span> <span data-ttu-id="5644d-106">Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop.</span><span class="sxs-lookup"><span data-stu-id="5644d-106">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="5644d-107">Oozie je integrována hello zásobníku Hadoop a podporuje hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="5644d-107">Oozie is integrated with hello Hadoop stack, and it supports hello following jobs:</span></span>

* <span data-ttu-id="5644d-108">Apache MapReduce</span><span class="sxs-lookup"><span data-stu-id="5644d-108">Apache MapReduce</span></span>
* <span data-ttu-id="5644d-109">Apache Pig</span><span class="sxs-lookup"><span data-stu-id="5644d-109">Apache Pig</span></span>
* <span data-ttu-id="5644d-110">Apache Hive</span><span class="sxs-lookup"><span data-stu-id="5644d-110">Apache Hive</span></span>
* <span data-ttu-id="5644d-111">Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="5644d-111">Apache Sqoop</span></span>

<span data-ttu-id="5644d-112">Oozie může být také použít tooschedule úlohy, které jsou specifické tooa systém, jako jsou programy v jazyce Java nebo skripty prostředí</span><span class="sxs-lookup"><span data-stu-id="5644d-112">Oozie can also be used tooschedule jobs that are specific tooa system, like Java programs or shell scripts</span></span>

> [!NOTE]
> <span data-ttu-id="5644d-113">Další možností pro definování pracovních postupů v prostředí HDInsight je Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5644d-113">Another option for defining workflows with HDInsight is Azure Data Factory.</span></span> <span data-ttu-id="5644d-114">toolearn Další informace o Azure Data Factory najdete v části [použijte Pig a Hive pomocí služby Data Factory][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="5644d-114">toolearn more about Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5644d-115">Oozie není povoleno v doméně HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5644d-115">Oozie is not enabled on domain-joined HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5644d-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5644d-116">Prerequisites</span></span>

* <span data-ttu-id="5644d-117">**Cluster služby HDInsight**: najdete v části [Začínáme s prostředím HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="5644d-117">**An HDInsight cluster**: See [Get Started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="5644d-118">Hello kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="5644d-118">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="5644d-119">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5644d-119">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="5644d-120">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="5644d-120">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="example-workflow"></a><span data-ttu-id="5644d-121">Příklad pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="5644d-121">Example workflow</span></span>

<span data-ttu-id="5644d-122">Hello pracovního postupu v tomto dokumentu obsahuje dvě akce.</span><span class="sxs-lookup"><span data-stu-id="5644d-122">hello workflow used in this document contains two actions.</span></span> <span data-ttu-id="5644d-123">Akce jsou definice pro úlohy, jako je například spuštění Hive, Sqoop, MapReduce nebo jiný proces:</span><span class="sxs-lookup"><span data-stu-id="5644d-123">Actions are definitions for tasks, such as running Hive, Sqoop, MapReduce, or other process:</span></span>

![Diagram pracovního postupu][img-workflow-diagram]

1. <span data-ttu-id="5644d-125">Akce Hive spouští skript HiveQL tooextract záznamů z hello **hivesampletable** součástí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5644d-125">A Hive action runs a HiveQL script tooextract records from hello **hivesampletable** included with HDInsight.</span></span> <span data-ttu-id="5644d-126">Každý řádek dat popisuje návštěvu z určité mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="5644d-126">Each row of data describes a visit from a specific mobile device.</span></span> <span data-ttu-id="5644d-127">Formát záznamu Hello se zobrazí podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-127">hello record format appears similar toohello following text:</span></span>

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    <span data-ttu-id="5644d-128">Hello skriptu Hive v tomto dokumentu počítá hello celkový počet návštěv pro každou platformu (třeba na Android nebo iPhone) a ukládá hello počty tooa novou tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="5644d-128">hello Hive script used in this document counts hello total visits for each platform (such as Android or iPhone) and stores hello counts tooa new Hive table.</span></span>

    <span data-ttu-id="5644d-129">Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="5644d-129">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

2. <span data-ttu-id="5644d-130">Akce Sqoop exportuje obsah hello hello nové Hive tooa tabulku v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="5644d-130">A Sqoop action exports hello contents of hello new Hive table tooa table in an Azure SQL database.</span></span> <span data-ttu-id="5644d-131">Další informace o Sqoop najdete v tématu [Sqoop pomocí Hadoop v prostředí HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="5644d-131">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="5644d-132">Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů systému Hadoop hello poskytovaných v HDInsight][hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="5644d-132">For supported Oozie versions on HDInsight clusters, see [What's new in hello Hadoop cluster versions provided by HDInsight][hdinsight-versions].</span></span>

## <a name="create-hello-working-directory"></a><span data-ttu-id="5644d-133">Vytvoření hello pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="5644d-133">Create hello working directory</span></span>

<span data-ttu-id="5644d-134">Oozie očekává prostředky potřebné pro úlohy toobe uložené v hello stejný adresář.</span><span class="sxs-lookup"><span data-stu-id="5644d-134">Oozie expects resources required for a job toobe stored in hello same directory.</span></span> <span data-ttu-id="5644d-135">Tento příklad používá **wasb: / / / kurzy/useoozie**.</span><span class="sxs-lookup"><span data-stu-id="5644d-135">This example uses **wasb:///tutorials/useoozie**.</span></span> <span data-ttu-id="5644d-136">Tento adresář a hello datový adresář, který obsahuje novou tabulku Hive hello vytvořené tento pracovní postup, použijte následující příkaz toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-136">Use hello following command toocreate this directory, and hello data directory that holds hello new Hive table created by this workflow:</span></span>

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> <span data-ttu-id="5644d-137">Hello `-p` parametr způsobí, že všechny adresáře v toobe cesta hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="5644d-137">hello `-p` parameter causes all directories in hello path toobe created.</span></span> <span data-ttu-id="5644d-138">Hello **data** adresář je použité toohold dat používá hello **useooziewf.hql** skriptu.</span><span class="sxs-lookup"><span data-stu-id="5644d-138">hello **data** directory is used toohold data used by hello **useooziewf.hql** script.</span></span>

<span data-ttu-id="5644d-139">Taky spusťte následující příkaz, který zajistí, že Oozie může zosobnit váš uživatelský účet při spuštění úlohy Hive a Sqoop hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-139">Also run hello following command, which ensures that Oozie can impersonate your user account when running Hive and Sqoop jobs.</span></span> <span data-ttu-id="5644d-140">Nahraďte **uživatelské jméno** s vaše přihlašovací jméno:</span><span class="sxs-lookup"><span data-stu-id="5644d-140">Replace **USERNAME** with your login name:</span></span>

```
sudo adduser USERNAME users
```

> [!NOTE]
> <span data-ttu-id="5644d-141">Můžete ignorovat chyby tohoto uživatele hello je již členem hello `users` skupiny.</span><span class="sxs-lookup"><span data-stu-id="5644d-141">You can ignore errors that hello user is already a member of hello `users` group.</span></span>

## <a name="add-a-database-driver"></a><span data-ttu-id="5644d-142">Přidat ovladač databáze</span><span class="sxs-lookup"><span data-stu-id="5644d-142">Add a database driver</span></span>

<span data-ttu-id="5644d-143">Vzhledem k tomu, že tento pracovní postup používá Sqoop tooexport data tooSQL databáze, je nutné zadat, že kopii ovladač JDBC hello používá tootalk tooSQL databáze.</span><span class="sxs-lookup"><span data-stu-id="5644d-143">Since this workflow uses Sqoop tooexport data tooSQL Database, you must provide a copy of hello JDBC driver used tootalk tooSQL Database.</span></span> <span data-ttu-id="5644d-144">Použití hello následující příkaz toocopy ho toohello pracovní adresář:</span><span class="sxs-lookup"><span data-stu-id="5644d-144">Use hello following command toocopy it toohello working directory:</span></span>

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

<span data-ttu-id="5644d-145">Pokud pracovní postup používá jiné prostředky, jako je například jar obsahující aplikaci MapReduce, potřebovali byste tooadd také tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="5644d-145">If your workflow used other resources, such as a jar containing a MapReduce application, you would need tooadd these resources as well.</span></span>

## <a name="define-hello-hive-query"></a><span data-ttu-id="5644d-146">Definování dotazu Hive hello</span><span class="sxs-lookup"><span data-stu-id="5644d-146">Define hello Hive query</span></span>

<span data-ttu-id="5644d-147">Pomocí následujících kroků toocreate HiveQL skript, který definuje dotaz, který se používá v pracovním postupu Oozie později v tomto dokumentu hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-147">Use hello following steps toocreate a HiveQL script that defines a query, which is used in an Oozie workflow later in this document.</span></span>

1. <span data-ttu-id="5644d-148">Připojte toohello clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="5644d-148">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="5644d-149">Hello následujícího příkazu je příklad použití hello `ssh` příkaz.</span><span class="sxs-lookup"><span data-stu-id="5644d-149">hello following command is an example of using hello `ssh` command.</span></span> <span data-ttu-id="5644d-150">Nahraďte __uživatelské jméno__ hello uživatele SSH pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="5644d-150">Replace __USERNAME__ with hello SSH user for hello cluster.</span></span> <span data-ttu-id="5644d-151">Nahraďte __CLUSTERNAME__ s názvem hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5644d-151">Replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span>

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="5644d-152">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="5644d-152">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="5644d-153">Z hello připojení SSH použijte následující příkaz toocreate soubor hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-153">From hello SSH connection, use hello following command toocreate a file:</span></span>

    ```
    nano useooziewf.hql
    ```

3. <span data-ttu-id="5644d-154">Jakmile se otevře hello nano editor, použijte následující dotaz jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-154">Once hello nano editor opens, use hello following query as hello contents of hello file:</span></span>

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    <span data-ttu-id="5644d-155">Existují dvě proměnné používané ve skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-155">There are two variables used in hello script:</span></span>

    * <span data-ttu-id="5644d-156">**${hiveTableName}**: obsahuje název hello toobe tabulky hello vytvořen</span><span class="sxs-lookup"><span data-stu-id="5644d-156">**${hiveTableName}**: contains hello name of hello table toobe created</span></span>

    * <span data-ttu-id="5644d-157">**${hiveDataFolder}**: obsahuje hello umístění toostore hello datové soubory pro tabulku hello</span><span class="sxs-lookup"><span data-stu-id="5644d-157">**${hiveDataFolder}**: contains hello location toostore hello data files for hello table</span></span>

    <span data-ttu-id="5644d-158">Soubor definice pracovního postupu Hello (workflow.xml v tomto kurzu) předává tyto hodnoty toothis skript HiveQL v době běhu</span><span class="sxs-lookup"><span data-stu-id="5644d-158">hello workflow definition file (workflow.xml in this tutorial) passes these values toothis HiveQL script at run time</span></span>

4. <span data-ttu-id="5644d-159">tooexit hello editor, stiskněte kombinaci kláves Ctrl-X.</span><span class="sxs-lookup"><span data-stu-id="5644d-159">tooexit hello editor, press Ctrl-X.</span></span> <span data-ttu-id="5644d-160">Po zobrazení výzvy vyberte **Y** toosave hello souboru, potom použijte **Enter** toouse hello **useooziewf.hql** název souboru.</span><span class="sxs-lookup"><span data-stu-id="5644d-160">When prompted, select **Y** toosave hello file, then use **Enter** toouse hello **useooziewf.hql** file name.</span></span>

5. <span data-ttu-id="5644d-161">Použití hello následující příkazy toocopy **useooziewf.hql** příliš**wasb:///tutorials/useoozie/useooziewf.hql**:</span><span class="sxs-lookup"><span data-stu-id="5644d-161">Use hello following commands toocopy **useooziewf.hql** too**wasb:///tutorials/useoozie/useooziewf.hql**:</span></span>

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    <span data-ttu-id="5644d-162">Tyto příkazy ukládání hello **useooziewf.hql** souboru na hello HDFS kompatibilní úložiště pro hello cluster.</span><span class="sxs-lookup"><span data-stu-id="5644d-162">These commands store hello **useooziewf.hql** file on hello HDFS-compatible storage for hello cluster.</span></span>

## <a name="define-hello-workflow"></a><span data-ttu-id="5644d-163">Definice pracovního postupu hello</span><span class="sxs-lookup"><span data-stu-id="5644d-163">Define hello workflow</span></span>

<span data-ttu-id="5644d-164">Definice Oozie pracovní postupy jsou zapsány ve hPDL (XML proces Definition Language).</span><span class="sxs-lookup"><span data-stu-id="5644d-164">Oozie workflows definitions are written in hPDL (an XML Process Definition Language).</span></span> <span data-ttu-id="5644d-165">Hello použijte následující postup toodefine hello pracovního postupu:</span><span class="sxs-lookup"><span data-stu-id="5644d-165">Use hello following steps toodefine hello workflow:</span></span>

1. <span data-ttu-id="5644d-166">Použijte následující příkaz toocreate hello a upravit nový soubor:</span><span class="sxs-lookup"><span data-stu-id="5644d-166">Use hello following statement toocreate and edit a new file:</span></span>

    ```
    nano workflow.xml
    ```

2. <span data-ttu-id="5644d-167">Jakmile se otevře hello nano editor, zadejte následující XML jako obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-167">Once hello nano editor opens, enter hello following XML as hello file contents:</span></span>

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
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
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

    <span data-ttu-id="5644d-168">Existují dvě akce definované v pracovním postupu hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-168">There are two actions defined in hello workflow:</span></span>

   * <span data-ttu-id="5644d-169">**RunHiveScript**: Tato akce je hello spuštění akce a spouští hello **useooziewf.hql** skript podregistru</span><span class="sxs-lookup"><span data-stu-id="5644d-169">**RunHiveScript**: This action is hello start action, and runs hello **useooziewf.hql** Hive script</span></span>

   * <span data-ttu-id="5644d-170">**RunSqoopExport**: Tato akce exportuje hello data vytvořená z tooSQL skriptu Hive hello databáze pomocí Sqoop.</span><span class="sxs-lookup"><span data-stu-id="5644d-170">**RunSqoopExport**: This action exports hello data created from hello Hive script tooSQL Database using Sqoop.</span></span> <span data-ttu-id="5644d-171">Tato akce je spuštěna pouze pokud hello **RunHiveScript** akce je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="5644d-171">This action only runs if hello **RunHiveScript** action is successful.</span></span>

     <span data-ttu-id="5644d-172">pracovní postup Hello má několik položek, jako například `${jobTracker}`.</span><span class="sxs-lookup"><span data-stu-id="5644d-172">hello workflow has several entries, such as `${jobTracker}`.</span></span> <span data-ttu-id="5644d-173">Tyto položky jsou nahrazovány hodnoty, které můžete použít v definici úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-173">These entries are replaced by values you use in hello job definition.</span></span> <span data-ttu-id="5644d-174">Vytvoří se definice úlohy Hello později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5644d-174">hello job definition is created later in this document.</span></span>

     <span data-ttu-id="5644d-175">Také Poznámka hello `<archive>sqljdbc4.jar</arcive>` položku v hello Sqoop části.</span><span class="sxs-lookup"><span data-stu-id="5644d-175">Also note hello `<archive>sqljdbc4.jar</arcive>` entry in hello Sqoop section.</span></span> <span data-ttu-id="5644d-176">Tato položka dá pokyn, Oozie toomake archivu k dispozici pro Sqoop po spuštění této akce.</span><span class="sxs-lookup"><span data-stu-id="5644d-176">This entry instructs Oozie toomake this archive available for Sqoop when this action runs.</span></span>

3. <span data-ttu-id="5644d-177">Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="5644d-177">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

4. <span data-ttu-id="5644d-178">Použití hello následující příkaz toocopy hello **workflow.xml** souboru příliš**/tutorials/useoozie/workflow.xml**:</span><span class="sxs-lookup"><span data-stu-id="5644d-178">Use hello following command toocopy hello **workflow.xml** file too**/tutorials/useoozie/workflow.xml**:</span></span>

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a><span data-ttu-id="5644d-179">Vytvoření databáze hello</span><span class="sxs-lookup"><span data-stu-id="5644d-179">Create hello database</span></span>

<span data-ttu-id="5644d-180">toocreate Azure SQL Database, postupujte podle kroků hello v hello [vytvoření databáze SQL](../sql-database/sql-database-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5644d-180">toocreate an Azure SQL Database, follow hello steps in hello [Create a SQL Database](../sql-database/sql-database-get-started.md) document.</span></span> <span data-ttu-id="5644d-181">Při vytváření hello databáze, použijte `oozietest` jako název databáze hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-181">When creating hello database, use `oozietest` as hello database name.</span></span> <span data-ttu-id="5644d-182">Také si poznamenejte název hello hello databázového serveru.</span><span class="sxs-lookup"><span data-stu-id="5644d-182">Also make a note of hello name of hello database server.</span></span>

### <a name="create-hello-table"></a><span data-ttu-id="5644d-183">Vytvoření tabulky hello</span><span class="sxs-lookup"><span data-stu-id="5644d-183">Create hello table</span></span>

> [!NOTE]
> <span data-ttu-id="5644d-184">Existuje mnoho způsobů tooconnect tooSQL databáze toocreate tabulku.</span><span class="sxs-lookup"><span data-stu-id="5644d-184">There are many ways tooconnect tooSQL Database toocreate a table.</span></span> <span data-ttu-id="5644d-185">Následující postup použijte Hello [FreeTDS](http://www.freetds.org/) z clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-185">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="5644d-186">Použijte následující příkaz tooinstall FreeTDS na clusteru HDInsight hello hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-186">Use hello following command tooinstall FreeTDS on hello HDInsight cluster:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. <span data-ttu-id="5644d-187">Po instalaci FreeTDS, použijte následující příkaz tooconnect toohello databáze SQL serveru, kterou jste vytvořili dříve hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-187">Once FreeTDS has been installed, use hello following command tooconnect toohello SQL Database server you created previously:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="5644d-188">Zobrazí se výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-188">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. <span data-ttu-id="5644d-189">V hello `1>` výzva, zadejte hello následující řádky:</span><span class="sxs-lookup"><span data-stu-id="5644d-189">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    <span data-ttu-id="5644d-190">Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="5644d-190">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="5644d-191">Tyto příkazy vytvořit tabulku s názvem **mobiledata** používané v pracovním postupu hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-191">These statements create a table named **mobiledata** that is used by hello workflow.</span></span>

    <span data-ttu-id="5644d-192">Použití hello následující tooverify, který hello tabulka byla vytvořena:</span><span class="sxs-lookup"><span data-stu-id="5644d-192">Use hello following tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="5644d-193">Zobrazí výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-193">You see output similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. <span data-ttu-id="5644d-194">Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.</span><span class="sxs-lookup"><span data-stu-id="5644d-194">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="create-hello-job-definition"></a><span data-ttu-id="5644d-195">Vytvořit definici úlohy hello</span><span class="sxs-lookup"><span data-stu-id="5644d-195">Create hello job definition</span></span>

<span data-ttu-id="5644d-196">definice úlohy Hello popisuje, kde toofind hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="5644d-196">hello job definition describes where toofind hello workflow.xml.</span></span> <span data-ttu-id="5644d-197">Také popisuje, kde toofind další soubory, které používá pracovní postup hello (například useooziewf.hql.) Také definuje hello hodnoty pro vlastnosti používaných v rámci pracovního postupu hello a související soubory.</span><span class="sxs-lookup"><span data-stu-id="5644d-197">It also describes where toofind other files used by hello workflow (such as useooziewf.hql.) It also defines hello values for properties used within hello workflow and associated files.</span></span>

1. <span data-ttu-id="5644d-198">Použijte následující příkaz tooget hello úplná adresa hello výchozí úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-198">Use hello following command tooget hello full address of hello default storage.</span></span> <span data-ttu-id="5644d-199">Tato adresa se používá v konfiguračním souboru hello za chvíli:</span><span class="sxs-lookup"><span data-stu-id="5644d-199">This address is used in hello configuration file in a moment:</span></span>

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    <span data-ttu-id="5644d-200">Tento příkaz vrátí informace podobné toohello následující XML:</span><span class="sxs-lookup"><span data-stu-id="5644d-200">This command returns information similar toohello following XML:</span></span>

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > <span data-ttu-id="5644d-201">Pokud hello HDInsight cluster používá jako hello výchozí úložiště Azure Storage, hello `<value>` obsah elementu začínat `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="5644d-201">If hello HDInsight cluster uses Azure Storage as hello default storage, hello `<value>` element contents begin with `wasb://`.</span></span> <span data-ttu-id="5644d-202">Pokud je použita Azure Data Lake Store, začne s `adl://`.</span><span class="sxs-lookup"><span data-stu-id="5644d-202">If Azure Data Lake Store is used instead, it begins with `adl://`.</span></span>

    <span data-ttu-id="5644d-203">Uložit obsah hello hello `<value>` element, protože se používá v dalších krocích hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-203">Save hello contents of hello `<value>` element, as it is used in hello next steps.</span></span>

2. <span data-ttu-id="5644d-204">Použijte následující příkaz tooget plně kvalifikovaný název domény clusteru headnode hello hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-204">Use hello following command tooget FQDN of hello cluster headnode.</span></span> <span data-ttu-id="5644d-205">Tyto informace se používají pro hello JobTracker adresu pro hello cluster:</span><span class="sxs-lookup"><span data-stu-id="5644d-205">This information is used for hello JobTracker address for hello cluster:</span></span>

    ```
    hostname -f
    ```

    <span data-ttu-id="5644d-206">Tento příkaz vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-206">This returns information similar toohello following text:</span></span>

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    <span data-ttu-id="5644d-207">Hello port používaný pro hello JobTracker je 8050, takže je hello toouse úplná adresa pro hello JobTracker `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span><span class="sxs-lookup"><span data-stu-id="5644d-207">hello port used for hello JobTracker is 8050, so hello full address toouse for hello JobTracker is `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span></span>

3. <span data-ttu-id="5644d-208">Použijte následující toocreate hello Oozie úlohy definice konfigurace hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-208">Use hello following toocreate hello Oozie job definition configuration:</span></span>

    ```
    nano job.xml
    ```

4. <span data-ttu-id="5644d-209">Jakmile se otevře hello nano editor, použijte následující XML jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-209">Once hello nano editor opens, use hello following XML as hello contents of hello file:</span></span>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>JOBTRACKERADDRESS</value>
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
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * <span data-ttu-id="5644d-210">Nahraďte všechny výskyty  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  s hodnotou hello jste obdrželi dříve pro výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="5644d-210">Replace all instances of **wasb://mycontainer@mystorageaccount.blob.core.windows.net** with hello value you received earlier for default storage.</span></span>

     > [!WARNING]
     > <span data-ttu-id="5644d-211">Pokud se cesta hello `wasb` cestu, musíte použít úplnou cestu hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-211">If hello path is a `wasb` path, you must use hello full path.</span></span> <span data-ttu-id="5644d-212">Není Zkraťte ho toojust `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="5644d-212">Do not shorten it toojust `wasb:///`.</span></span>

   * <span data-ttu-id="5644d-213">Nahraďte **JOBTRACKERADDRESS** s hello JobTracker/ResourceManager adresu jste obdrželi dříve.</span><span class="sxs-lookup"><span data-stu-id="5644d-213">Replace **JOBTRACKERADDRESS** with hello JobTracker/ResourceManager address you received earlier.</span></span>
   * <span data-ttu-id="5644d-214">Nahraďte **jméno** s vaše přihlašovací jméno pro hello HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="5644d-214">Replace **YourName** with your login name for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="5644d-215">Nahraďte **serverName**, **adminLogin**, a **adminPassword** s hello informace o vaší databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="5644d-215">Replace **serverName**, **adminLogin**, and **adminPassword** with hello information for your Azure SQL Database.</span></span>

     <span data-ttu-id="5644d-216">Většina hello informace v tomto souboru je použité toopopulate hello hodnoty používané v hello workflow.xml nebo ooziewf.hql soubory (např. ${nameNode}.)</span><span class="sxs-lookup"><span data-stu-id="5644d-216">Most of hello information in this file is used toopopulate hello values used in hello workflow.xml or ooziewf.hql files (such as ${nameNode}.)</span></span>

     > [!NOTE]
     > <span data-ttu-id="5644d-217">Hello **oozie.wf.application.path** položka udává kde toofind hello workflow.xml soubor, který obsahuje pracovní postup hello spuštěné prostřednictvím této úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-217">hello **oozie.wf.application.path** entry defines where toofind hello workflow.xml file, which contains hello workflow ran by this job.</span></span>

5. <span data-ttu-id="5644d-218">Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="5644d-218">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

## <a name="submit-and-manage-hello-job"></a><span data-ttu-id="5644d-219">Odesílat a spravovat úlohy hello</span><span class="sxs-lookup"><span data-stu-id="5644d-219">Submit and manage hello job</span></span>

<span data-ttu-id="5644d-220">Hello následující kroky použijte hello Oozie příkaz toosubmit a správa pracovních postupů Oozie v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-220">hello following steps use hello Oozie command toosubmit and manage Oozie workflows on hello cluster.</span></span> <span data-ttu-id="5644d-221">příkaz Oozie Hello je popisný rozhraní přes hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="5644d-221">hello Oozie command is a friendly interface over hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5644d-222">Při použití příkazu hello Oozie, je nutné použít hello plně kvalifikovaný název domény pro hello HDInsight headnode.</span><span class="sxs-lookup"><span data-stu-id="5644d-222">When using hello Oozie command, you must use hello FQDN for hello HDInsight headnode.</span></span> <span data-ttu-id="5644d-223">Tento plně kvalifikovaný název domény je k dispozici pouze z clusteru hello nebo pokud hello clusteru je na virtuální síť Azure, z jiných počítačů v hello stejné síti.</span><span class="sxs-lookup"><span data-stu-id="5644d-223">This FQDN is only accessible from hello cluster, or if hello cluster is on an Azure Virtual Network, from other machines on hello same network.</span></span>


1. <span data-ttu-id="5644d-224">Použijte následující tooobtain hello URL toohello Oozie služby hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-224">Use hello following tooobtain hello URL toohello Oozie service:</span></span>

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    <span data-ttu-id="5644d-225">Tento příkaz vrátí informace podobné toohello následující XML:</span><span class="sxs-lookup"><span data-stu-id="5644d-225">This returns information similar toohello following XML:</span></span>

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    <span data-ttu-id="5644d-226">Hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` část je hello toouse adresu URL s hello Oozie příkaz.</span><span class="sxs-lookup"><span data-stu-id="5644d-226">hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` portion is hello URL toouse with hello Oozie command.</span></span>

2. <span data-ttu-id="5644d-227">Použití hello následující toocreate proměnné prostředí pro adresu URL hello, takže není nutné tootype pro každý příkaz:</span><span class="sxs-lookup"><span data-stu-id="5644d-227">Use hello following toocreate an environment variable for hello URL, so you don't have tootype it for every command:</span></span>

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    <span data-ttu-id="5644d-228">Nahraďte adresu URL hello hello jeden, který jste dostali dříve.</span><span class="sxs-lookup"><span data-stu-id="5644d-228">Replace hello URL with hello one you received earlier.</span></span>
3. <span data-ttu-id="5644d-229">Použijte následující úlohy hello toosubmit hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-229">Use hello following toosubmit hello job:</span></span>

    ```
    oozie job -config job.xml -submit
    ```

    <span data-ttu-id="5644d-230">Tento příkaz načte informace o úlohách hello z **job.xml** a odešle ji tooOozie, ale nespustí se nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="5644d-230">This command loads hello job information from **job.xml** and submits it tooOozie, but does not run it.</span></span>

    <span data-ttu-id="5644d-231">Po dokončení příkazu hello by měl vrátit hello ID úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-231">Once hello command completes, it should return hello ID of hello job.</span></span> <span data-ttu-id="5644d-232">Například, `0000005-150622124850154-oozie-oozi-W`.</span><span class="sxs-lookup"><span data-stu-id="5644d-232">For example, `0000005-150622124850154-oozie-oozi-W`.</span></span> <span data-ttu-id="5644d-233">Toto ID je použité toomanage hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-233">This ID is used toomanage hello job.</span></span>

4. <span data-ttu-id="5644d-234">Zobrazit stav hello hello úlohy pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5644d-234">View hello status of hello job using hello following command:</span></span>

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > <span data-ttu-id="5644d-235">Nahraďte `<JOBID>` s hello ID vrácené v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-235">Replace `<JOBID>` with hello ID returned in hello previous step.</span></span>

    <span data-ttu-id="5644d-236">Tento příkaz vrátí informace podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-236">This returns information similar toohello following text:</span></span>

    ```
    Job ID : 0000005-150622124850154-oozie-oozi-W
    ------------------------------------------------------------------------------------------------------------------------------------
    Workflow Name : useooziewf
    App Path      : wasb:///tutorials/useoozie
    Status        : PREP
    Run           : 0
    User          : USERNAME
    Group         : -
    Created       : 2015-06-22 15:06 GMT
    Started       : -
    Last Modified : 2015-06-22 15:06 GMT
    Ended         : -
    CoordAction ID: -
    ------------------------------------------------------------------------------------------------------------------------------------
    ```

    <span data-ttu-id="5644d-237">Tato úloha je ve stavu `PREP`.</span><span class="sxs-lookup"><span data-stu-id="5644d-237">This job has a status of `PREP`.</span></span> <span data-ttu-id="5644d-238">Tento stav indikuje tuto úlohu hello byla vytvořena, ale není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="5644d-238">This status indicates that hello job was created, but not started.</span></span>

5. <span data-ttu-id="5644d-239">Použijte následující příkaz toostart hello úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-239">Use hello following command toostart hello job:</span></span>

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > <span data-ttu-id="5644d-240">Nahraďte `<JOBID>` s hello ID vrácené dříve.</span><span class="sxs-lookup"><span data-stu-id="5644d-240">Replace `<JOBID>` with hello ID returned previously.</span></span>

    <span data-ttu-id="5644d-241">Pokud zaškrtnete hello stav po tento příkaz, je v běžícím stavu a pro hello akce v rámci úlohy hello se vrátí informace.</span><span class="sxs-lookup"><span data-stu-id="5644d-241">If you check hello status after this command, it is in a running state, and information is returned for hello actions within hello job.</span></span>

6. <span data-ttu-id="5644d-242">Po úspěšném dokončení úkolů hello můžete ověřit, že hello data byla vygenerována a exportovali toohello tabulka databáze SQL pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="5644d-242">Once hello task completes successfully, you can verify that hello data was generated and exported toohello SQL Database table by using hello following commands:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="5644d-243">V hello `1>` výzva, zadejte hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="5644d-243">At hello `1>` prompt, enter hello following query:</span></span>

    ```
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="5644d-244">vrácené informace Hello je podobné toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="5644d-244">hello information returned is similar toohello following text:</span></span>

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

<span data-ttu-id="5644d-245">Další informace o hello Oozie příkaz najdete v tématu [nástroj příkazového řádku Oozie](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span><span class="sxs-lookup"><span data-stu-id="5644d-245">For more information on hello Oozie command, see [Oozie command-line tool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span></span>

## <a name="oozie-rest-api"></a><span data-ttu-id="5644d-246">Oozie REST API</span><span class="sxs-lookup"><span data-stu-id="5644d-246">Oozie REST API</span></span>

<span data-ttu-id="5644d-247">Hello Oozie REST API vám umožní toobuild vlastní nástroje, které pracují s Oozie.</span><span class="sxs-lookup"><span data-stu-id="5644d-247">hello Oozie REST API allows you toobuild your own tools that work with Oozie.</span></span> <span data-ttu-id="5644d-248">Hello následují HDInsight konkrétní informace o používání hello Oozie REST API:</span><span class="sxs-lookup"><span data-stu-id="5644d-248">hello following are HDInsight specific information about using hello Oozie REST API:</span></span>

* <span data-ttu-id="5644d-249">**Identifikátor URI**: hello REST API je přístupná z clusteru mimo hello`https://CLUSTERNAME.azurehdinsight.net/oozie`</span><span class="sxs-lookup"><span data-stu-id="5644d-249">**URI**: hello REST API can be accessed from outside hello cluster at `https://CLUSTERNAME.azurehdinsight.net/oozie`</span></span>

* <span data-ttu-id="5644d-250">**Ověřování**: ověření toohello rozhraní API pomocí účet clusteru HTTP hello (správce) a hesla.</span><span class="sxs-lookup"><span data-stu-id="5644d-250">**Authentication**: Authenticate toohello API using hello cluster HTTP account (admin) and password.</span></span> <span data-ttu-id="5644d-251">Například:</span><span class="sxs-lookup"><span data-stu-id="5644d-251">For example:</span></span>

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

<span data-ttu-id="5644d-252">Další informace o používání hello Oozie REST API najdete v tématu [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="5644d-252">For more information on using hello Oozie REST API, see [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

## <a name="oozie-web-ui"></a><span data-ttu-id="5644d-253">Oozie webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="5644d-253">Oozie Web UI</span></span>

<span data-ttu-id="5644d-254">Hello Oozie webového uživatelského rozhraní poskytuje webové pohled na stav hello Oozie úloh na clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-254">hello Oozie Web UI provides a web-based view into hello status of Oozie jobs on hello cluster.</span></span> <span data-ttu-id="5644d-255">Hello webového uživatelského rozhraní umožňuje tooview hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="5644d-255">hello web UI allows you tooview hello following information:</span></span>

* <span data-ttu-id="5644d-256">Stav úlohy</span><span class="sxs-lookup"><span data-stu-id="5644d-256">Job status</span></span>
* <span data-ttu-id="5644d-257">Definice úlohy</span><span class="sxs-lookup"><span data-stu-id="5644d-257">Job definition</span></span>
* <span data-ttu-id="5644d-258">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="5644d-258">Configuration</span></span>
* <span data-ttu-id="5644d-259">Graf hello akcí v úloze hello</span><span class="sxs-lookup"><span data-stu-id="5644d-259">A graph of hello actions in hello job</span></span>
* <span data-ttu-id="5644d-260">Protokoly pro úlohu hello</span><span class="sxs-lookup"><span data-stu-id="5644d-260">Logs for hello job</span></span>

<span data-ttu-id="5644d-261">Můžete také zobrazit podrobnosti pro akce v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-261">You can also view details for actions within a job.</span></span>

<span data-ttu-id="5644d-262">tooaccess hello Oozie webového uživatelského rozhraní, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5644d-262">tooaccess hello Oozie Web UI, use hello following steps:</span></span>

1. <span data-ttu-id="5644d-263">Vytvoření clusteru HDInsight toohello tunelového propojení SSH.</span><span class="sxs-lookup"><span data-stu-id="5644d-263">Create an SSH tunnel toohello HDInsight cluster.</span></span> <span data-ttu-id="5644d-264">Informace najdete v tématu hello [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="5644d-264">For information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="5644d-265">Po vytvoření tunelu, otevřete ve webovém prohlížeči webovému uživatelskému rozhraní Ambari hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-265">Once a tunnel has been created, open hello Ambari web UI in your web browser.</span></span> <span data-ttu-id="5644d-266">Hello identifikátor URI pro lokalitu Ambari hello je **https://CLUSTERNAME.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="5644d-266">hello URI for hello Ambari site is **https://CLUSTERNAME.azurehdinsight.net**.</span></span> <span data-ttu-id="5644d-267">Nahraďte **CLUSTERNAME** s názvem hello clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="5644d-267">Replace **CLUSTERNAME** with hello name of your Linux-based HDInsight cluster.</span></span>

3. <span data-ttu-id="5644d-268">Hello levé straně stránky hello, vyberte **Oozie**, pak **rychlé odkazy**a v neposlední řadě **Oozie webového uživatelského rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="5644d-268">From hello left side of hello page, select **Oozie**, then **Quick Links**, and finally **Oozie Web UI**.</span></span>

    ![Obrázek nabídky hello](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. <span data-ttu-id="5644d-270">výchozí hodnoty toodisplaying Oozie webového uživatelského rozhraní Hello spuštěné úlohy pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5644d-270">hello Oozie Web UI defaults toodisplaying running Workflow Jobs.</span></span> <span data-ttu-id="5644d-271">Vyberte všechny úlohy pracovního postupu, toosee **všechny úlohy**.</span><span class="sxs-lookup"><span data-stu-id="5644d-271">toosee all workflow jobs, select **All Jobs**.</span></span>

    ![Zobrazí všechny úlohy](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. <span data-ttu-id="5644d-273">Vyberte úlohy tooview Další informace o úloze hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-273">Select a job tooview more information about hello job.</span></span>

    ![Informace o úloze](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. <span data-ttu-id="5644d-275">Z karty hello informace o úloze můžete zobrazit informace o základní úlohy a hello jednotlivé akce v rámci úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-275">From hello Job Info tab, you can see basic job information and hello individual actions within hello job.</span></span> <span data-ttu-id="5644d-276">V horní části hello, které můžete zobrazit pomocí karty hello hello definice úlohy, úlohy konfigurace přístup hello protokol úlohy nebo zobrazení směrované Acyklické grafu (DAG) hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-276">Using hello tabs at hello top you can view hello Job Definition, Job Configuration, access hello Job Log or view a Directed Acyclic Graph (DAG) of hello job.</span></span>

   * <span data-ttu-id="5644d-277">**Protokol úlohy**: Vyberte hello **GetLogs** tlačítko tooget všechny protokoly pro úlohu hello, nebo použijte hello **zadejte vyhledávací filtr** pole toofilter protokoly</span><span class="sxs-lookup"><span data-stu-id="5644d-277">**Job Log**: Select hello **GetLogs** button tooget all logs for hello job, or use hello **Enter Search Filter** field toofilter logs</span></span>

       ![Protokol úlohy](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * <span data-ttu-id="5644d-279">**JobDAG**: hello DAG je grafické zobrazení cesty k datům hello prováděné prostřednictvím hello workflowu</span><span class="sxs-lookup"><span data-stu-id="5644d-279">**JobDAG**: hello DAG is a graphical overview of hello data paths taken through hello workflow</span></span>

       ![Úloha DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. <span data-ttu-id="5644d-281">Vyberte jednu z akcí hello z hello **informace o úloze** kartě vyvoláte informace pro akce hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-281">Selecting one of hello actions from hello **Job Info** tab brings up information for hello action.</span></span> <span data-ttu-id="5644d-282">Vyberte například hello **RunHiveScript** akce.</span><span class="sxs-lookup"><span data-stu-id="5644d-282">For example, select hello **RunHiveScript** action.</span></span>

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/action.png)

8. <span data-ttu-id="5644d-284">Zobrazí podrobnosti o hello akce, například odkaz toohello **adresa URL konzoly**.</span><span class="sxs-lookup"><span data-stu-id="5644d-284">You can see details for hello action, such as a link toohello **Console URL**.</span></span> <span data-ttu-id="5644d-285">Tento odkaz může být použité tooview JobTracker informace pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-285">This link can be used tooview JobTracker information for hello job.</span></span>

## <a name="scheduling-jobs"></a><span data-ttu-id="5644d-286">Plánování úloh</span><span class="sxs-lookup"><span data-stu-id="5644d-286">Scheduling jobs</span></span>

<span data-ttu-id="5644d-287">Koordinátor Hello umožňuje toospecify začátek, konec a výskyt frekvence pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-287">hello coordinator allows you toospecify a start, end, and occurrence frequency for jobs.</span></span> <span data-ttu-id="5644d-288">toodefine plán pro pracovní postup hello, hello použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5644d-288">toodefine a schedule for hello workflow, use hello following steps:</span></span>

1. <span data-ttu-id="5644d-289">Použití hello následující toocreate soubor s názvem **coordinator.xml**:</span><span class="sxs-lookup"><span data-stu-id="5644d-289">Use hello following toocreate a file named **coordinator.xml**:</span></span>

    ```
    nano coordinator.xml
    ```

    <span data-ttu-id="5644d-290">Použijte následující XML jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-290">Use hello following XML as hello contents of hello file:</span></span>

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > <span data-ttu-id="5644d-291">Hello `${...}` proměnné jsou nahrazena hodnotami v definici úlohy hello za běhu.</span><span class="sxs-lookup"><span data-stu-id="5644d-291">hello `${...}` variables are replaced by values in hello job definition at run-time.</span></span> <span data-ttu-id="5644d-292">Hello proměnnými jsou:</span><span class="sxs-lookup"><span data-stu-id="5644d-292">hello variables are:</span></span>
    >
    > * <span data-ttu-id="5644d-293">`${coordFrequency}`: Doba mezi spuštěním instancí hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="5644d-293">`${coordFrequency}`: Time between running instances of hello job.</span></span>
    > <span data-ttu-id="5644d-294">** `${coordStart}`: Úloha hello počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="5644d-294">** `${coordStart}`: hello job start time.</span></span>
    > * <span data-ttu-id="5644d-295">`${coordEnd}`: času ukončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-295">`${coordEnd}`: hello job end time.</span></span>
    > * <span data-ttu-id="5644d-296">`${coordTimezone}`: Koordinátor úlohy jsou v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC).</span><span class="sxs-lookup"><span data-stu-id="5644d-296">`${coordTimezone}`: Coordinator jobs are in a fixed time zone with no daylight savings time (typically represented by using UTC).</span></span> <span data-ttu-id="5644d-297">Toto časové pásmo se označuje jako hello "Oozie zpracování časové pásmo."</span><span class="sxs-lookup"><span data-stu-id="5644d-297">This time zone is referred as hello "Oozie processing timezone."</span></span>
    > * <span data-ttu-id="5644d-298">`${wfPath}`: hello workflow.xml toohello cesta.</span><span class="sxs-lookup"><span data-stu-id="5644d-298">`${wfPath}`: hello path toohello workflow.xml.</span></span>

2. <span data-ttu-id="5644d-299">toosave hello soubor, použijte kombinaci kláves Ctrl-X **Y**, a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5644d-299">toosave hello file, use Ctrl-X, **Y**, and **Enter**.</span></span>

3. <span data-ttu-id="5644d-300">Použijte následující příkaz toocopy hello souboru toohello pracovní adresář pro tuto úlohu hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-300">Use hello following command toocopy hello file toohello working directory for this job:</span></span>

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. <span data-ttu-id="5644d-301">Použití hello následující toomodify hello **job.xml** souboru:</span><span class="sxs-lookup"><span data-stu-id="5644d-301">Use hello following toomodify hello **job.xml** file:</span></span>

    ```
    nano job.xml
    ```

    <span data-ttu-id="5644d-302">Ujistěte se, hello následující změny:</span><span class="sxs-lookup"><span data-stu-id="5644d-302">Make hello following changes:</span></span>

   * <span data-ttu-id="5644d-303">tooinstruct oozie toorun hello coordinator souboru místo hello pracovního postupu, změna `<name>oozie.wf.application.path</name>` příliš`<name>oozie.coord.application.path</name>`.</span><span class="sxs-lookup"><span data-stu-id="5644d-303">tooinstruct oozie toorun hello coordinator file instead of hello workflow, change `<name>oozie.wf.application.path</name>` too`<name>oozie.coord.application.path</name>`.</span></span>

   * <span data-ttu-id="5644d-304">tooset hello `workflowPath` proměnné používané hello coordinator, přidejte následující XML hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-304">tooset hello `workflowPath` variable used by hello coordinator, add hello following XML:</span></span>

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       <span data-ttu-id="5644d-305">Nahraďte hello `wasb://mycontainer@mystorageaccount.blob.core.windows` textu s hodnotou hello používá v jiné položky v souboru job.xml hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-305">Replace hello `wasb://mycontainer@mystorageaccount.blob.core.windows` text with hello value used in other entries in hello job.xml file.</span></span>

   * <span data-ttu-id="5644d-306">spuštění hello toodefine, end a četnost pro koordinátora hello, přidejte následující XML hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-306">toodefine hello start, end, and frequency for hello coordinator, add hello following XML:</span></span>

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       <span data-ttu-id="5644d-307">Tyto hodnoty nastaveny hello počáteční čas too12: 00 PM na 10 může 2017 hello koncový čas tooMay 12, 2017.</span><span class="sxs-lookup"><span data-stu-id="5644d-307">These values set hello start time too12:00PM on May 10, 2017, hello end time tooMay 12, 2017.</span></span> <span data-ttu-id="5644d-308">Hello interval pro spuštění tuto úlohu denně.</span><span class="sxs-lookup"><span data-stu-id="5644d-308">hello interval for running this job daily.</span></span> <span data-ttu-id="5644d-309">frekvence Hello je v minutách, takže 24 hodin x 60 minut = 1 440 minut.</span><span class="sxs-lookup"><span data-stu-id="5644d-309">hello frequency is in minutes, so 24 hours x 60 minutes = 1440 minutes.</span></span> <span data-ttu-id="5644d-310">Nakonec časové pásmo hello nastavena tooUTC.</span><span class="sxs-lookup"><span data-stu-id="5644d-310">Finally, hello timezone is set tooUTC.</span></span>

5. <span data-ttu-id="5644d-311">Použijte Ctrl-X, pak **Y** a **Enter** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="5644d-311">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

6. <span data-ttu-id="5644d-312">toorun hello úloha hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="5644d-312">toorun hello job, use hello following command:</span></span>

    ```
    oozie job -config job.xml -run
    ```

    <span data-ttu-id="5644d-313">Tento příkaz odešle a spustí úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-313">This command submits and starts hello job.</span></span>

7. <span data-ttu-id="5644d-314">Pokud navštívíte hello Oozie webového uživatelského rozhraní a vyberte hello **koordinátor úlohy** kartě uvidíte informace podobné toohello následující bitové kopie:</span><span class="sxs-lookup"><span data-stu-id="5644d-314">If you visit hello Oozie Web UI and select hello **Coordinator Jobs** tab, you see information similar toohello following image:</span></span>

    ![Karta úlohy Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    <span data-ttu-id="5644d-316">Hello **další Materialization** záznam obsahuje hello příštím hello spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="5644d-316">hello **Next Materialization** entry contains hello next time that hello job runs.</span></span>

8. <span data-ttu-id="5644d-317">Podobně jako toohello starší úlohy pracovního postupu, výběr položek úlohy hello v hello webového uživatelského rozhraní zobrazí informace o úloze hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-317">Similar toohello earlier workflow job, selecting hello job entry in hello web UI displays information on hello job:</span></span>

    ![Informace o úloze Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > <span data-ttu-id="5644d-319">Tento image se zobrazí pouze úspěšně spustí hello úlohy, ne z individuálních akce v rámci pracovního postupu hello naplánované.</span><span class="sxs-lookup"><span data-stu-id="5644d-319">This image only shows successful runs of hello job, not individual actions within hello scheduled workflow.</span></span> <span data-ttu-id="5644d-320">toosee, vyberte jednu z hello **akce** položky.</span><span class="sxs-lookup"><span data-stu-id="5644d-320">toosee that, select one of hello **Action** entries.</span></span>

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a><span data-ttu-id="5644d-322">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="5644d-322">Troubleshooting</span></span>

<span data-ttu-id="5644d-323">Hello Oozie uživatelského rozhraní umožňuje tooview Oozie protokoly.</span><span class="sxs-lookup"><span data-stu-id="5644d-323">hello Oozie UI allows you tooview Oozie logs.</span></span> <span data-ttu-id="5644d-324">Obsahuje taky odkazy tooJobTracker protokoly pro úlohy MapReduce spustí pracovní postup hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-324">It also contains links tooJobTracker logs for MapReduce tasks started by hello workflow.</span></span> <span data-ttu-id="5644d-325">musí být Hello vzor pro řešení potíží:</span><span class="sxs-lookup"><span data-stu-id="5644d-325">hello pattern for troubleshooting should be:</span></span>

1. <span data-ttu-id="5644d-326">Zobrazit úlohy hello v Oozie webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5644d-326">View hello job in Oozie Web UI.</span></span>

2. <span data-ttu-id="5644d-327">Pokud dojde k chybě nebo pro určité akce se nezdařilo, vyberte hello toosee akce, pokud hello **chybová zpráva** pole poskytuje další informace o selhání hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-327">If there is an error or failure for a specific action, select hello action toosee if hello **Error Message** field provides more information on hello failure.</span></span>

3. <span data-ttu-id="5644d-328">Pokud je k dispozici, použijte hello adresu URL z hello akce tooview další podrobnosti (třeba protokoly JobTracker) pro akci hello.</span><span class="sxs-lookup"><span data-stu-id="5644d-328">If available, use hello URL from hello action tooview more details (such as JobTracker logs) for hello action.</span></span>

<span data-ttu-id="5644d-329">Hello následují konkrétní chyby, které se můžete setkat, a jak tooresolve je.</span><span class="sxs-lookup"><span data-stu-id="5644d-329">hello following are specific errors you may encounter, and how tooresolve them.</span></span>

### <a name="ja009-cannot-initialize-cluster"></a><span data-ttu-id="5644d-330">JA009: Nelze inicializovat clusteru</span><span class="sxs-lookup"><span data-stu-id="5644d-330">JA009: Cannot initialize cluster</span></span>

<span data-ttu-id="5644d-331">**Příznaky**: hello změny stavu úlohy příliš**POZASTAVENO**.</span><span class="sxs-lookup"><span data-stu-id="5644d-331">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="5644d-332">Zobrazí podrobnosti pro úlohu hello hello RunHiveScript stav jako **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="5644d-332">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="5644d-333">Výběr akce hello zobrazí hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="5644d-333">Selecting hello action displays hello following error message:</span></span>

    JA009: Cannot initialize Cluster. Please check your configuration for map

<span data-ttu-id="5644d-334">**Příčina**: hello WASB adresy použité v hello **job.xml** soubor neobsahuje kontejner úložiště hello nebo název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5644d-334">**Cause**: hello WASB addresses used in hello **job.xml** file do not contain hello storage container or storage account name.</span></span> <span data-ttu-id="5644d-335">musí být ve formátu adresa WASB Hello `wasb://containername@storageaccountname.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="5644d-335">hello WASB address format must be `wasb://containername@storageaccountname.blob.core.windows.net`.</span></span>

<span data-ttu-id="5644d-336">**Řešení**: Změna adresy WASB hello používá hello úloha.</span><span class="sxs-lookup"><span data-stu-id="5644d-336">**Resolution**: Change hello WASB addresses used by hello job.</span></span>

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a><span data-ttu-id="5644d-337">JA002: Oozie není povolen tooimpersonate &lt;uživatele ></span><span class="sxs-lookup"><span data-stu-id="5644d-337">JA002: Oozie is not allowed tooimpersonate &lt;USER></span></span>

<span data-ttu-id="5644d-338">**Příznaky**: hello změny stavu úlohy příliš**POZASTAVENO**.</span><span class="sxs-lookup"><span data-stu-id="5644d-338">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="5644d-339">Zobrazí podrobnosti pro úlohu hello hello RunHiveScript stav jako **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="5644d-339">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="5644d-340">Výběr akce hello ukazuje hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="5644d-340">Selecting hello action shows hello following error message:</span></span>

    JA002: User: oozie is not allowed tooimpersonate <USER>

<span data-ttu-id="5644d-341">**Příčina**: nastavení aktuální oprávnění neumožňují Oozie tooimpersonate hello zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="5644d-341">**Cause**: Current permission settings do not allow Oozie tooimpersonate hello specified user account.</span></span>

<span data-ttu-id="5644d-342">**Řešení**: Oozie v hello je povolená uživatelé tooimpersonate **uživatelé** skupiny.</span><span class="sxs-lookup"><span data-stu-id="5644d-342">**Resolution**: Oozie is allowed tooimpersonate users in hello **users** group.</span></span> <span data-ttu-id="5644d-343">Použití hello `groups USERNAME` toosee hello skupiny, které hello uživatelský účet je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="5644d-343">Use hello `groups USERNAME` toosee hello groups that hello user account is a member of.</span></span> <span data-ttu-id="5644d-344">Pokud není uživatel hello členem hello **uživatelé** skupiny, použijte následující příkaz tooadd hello uživatele toohello skupiny hello:</span><span class="sxs-lookup"><span data-stu-id="5644d-344">If hello user is not a member of hello **users** group, use hello following command tooadd hello user toohello group:</span></span>

    sudo adduser USERNAME users

> [!NOTE]
> <span data-ttu-id="5644d-345">Může trvat několik minut, než HDInsight rozpozná, že tento uživatel hello přidala toohello skupiny.</span><span class="sxs-lookup"><span data-stu-id="5644d-345">It may take several minutes before HDInsight recognizes that hello user has been added toohello group.</span></span>

### <a name="launcher-error-sqoop"></a><span data-ttu-id="5644d-346">Spouštěč chyby (Sqoop)</span><span class="sxs-lookup"><span data-stu-id="5644d-346">Launcher ERROR (Sqoop)</span></span>

<span data-ttu-id="5644d-347">**Příznaky**: hello změny stavu úlohy příliš**KILLED**.</span><span class="sxs-lookup"><span data-stu-id="5644d-347">**Symptoms**: hello job status changes too**KILLED**.</span></span> <span data-ttu-id="5644d-348">Zobrazí podrobnosti pro úlohu hello hello RunSqoopExport stav jako **chyba**.</span><span class="sxs-lookup"><span data-stu-id="5644d-348">Details for hello job show hello RunSqoopExport status as **ERROR**.</span></span> <span data-ttu-id="5644d-349">Výběr akce hello ukazuje hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="5644d-349">Selecting hello action shows hello following error message:</span></span>

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

<span data-ttu-id="5644d-350">**Příčina**: Sqoop je nelze tooload hello ovladač vyžaduje tooaccess hello databáze.</span><span class="sxs-lookup"><span data-stu-id="5644d-350">**Cause**: Sqoop is unable tooload hello database driver required tooaccess hello database.</span></span>

<span data-ttu-id="5644d-351">**Řešení**: při použití Sqoop z úlohu Oozie, je nutné zahrnout hello ovladač databáze s hello jiné prostředky (třeba hello workflow.xml) používá hello úloha.</span><span class="sxs-lookup"><span data-stu-id="5644d-351">**Resolution**: When using Sqoop from an Oozie job, you must include hello database driver with hello other resources (such as hello workflow.xml) used by hello job.</span></span> <span data-ttu-id="5644d-352">Také odkazovat hello archivu obsahující hello ovladač databáze z hello `<sqoop>...</sqoop>` části hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="5644d-352">Also Reference hello archive containing hello database driver from hello `<sqoop>...</sqoop>` section of hello workflow.xml.</span></span>

<span data-ttu-id="5644d-353">Například pro úlohu hello v tomto dokumentu použijete hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5644d-353">For example, for hello job in this document, you would use hello following steps:</span></span>

1. <span data-ttu-id="5644d-354">Zkopírujte adresář /tutorials/useoozie hello sqljdbc4.1.jar souboru toohello:</span><span class="sxs-lookup"><span data-stu-id="5644d-354">Copy hello sqljdbc4.1.jar file toohello /tutorials/useoozie directory:</span></span>

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. <span data-ttu-id="5644d-355">Upravit hello workflow.xml tooadd hello následující XML na nový řádek výše `</sqoop>`:</span><span class="sxs-lookup"><span data-stu-id="5644d-355">Modify hello workflow.xml tooadd hello following XML on a new line above `</sqoop>`:</span></span>

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a><span data-ttu-id="5644d-356">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5644d-356">Next steps</span></span>

<span data-ttu-id="5644d-357">V tomto kurzu jste se naučili jak toodefine pracovním postupu Oozie a jak toorun úlohu Oozie.</span><span class="sxs-lookup"><span data-stu-id="5644d-357">In this tutorial, you learned how toodefine an Oozie workflow and how toorun an Oozie job.</span></span> <span data-ttu-id="5644d-358">toolearn Další informace o práci s HDInsight, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="5644d-358">toolearn more about working with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="5644d-359">[Použijte založené na čase Oozie Coordinator s HDInsight][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="5644d-359">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="5644d-360">[Nahrání dat pro úlohy Hadoop v HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="5644d-360">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="5644d-361">[Použití nástroje Sqoop se systémem Hadoop v HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="5644d-361">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="5644d-362">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="5644d-362">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="5644d-363">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="5644d-363">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="5644d-364">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="5644d-364">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
