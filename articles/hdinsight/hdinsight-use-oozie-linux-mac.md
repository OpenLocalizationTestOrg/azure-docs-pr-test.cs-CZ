---
title: "Použití pracovních postupů Hadoop Oozie v HDInsight se systémem Linux | Microsoft Docs"
description: "Použijte Hadoop Oozie v HDInsight se systémem Linux. Zjistěte, jak definovat pracovním postupu Oozie a odeslat úlohu Oozie."
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
ms.openlocfilehash: e3206078e451aefe02689bfb61ce22a20dd0fa70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a><span data-ttu-id="d2410-104">Použití Oozie se systémem Hadoop k definování a spuštění workflowu v HDInsight se systémem Linux</span><span class="sxs-lookup"><span data-stu-id="d2410-104">Use Oozie with Hadoop to define and run a workflow on Linux-based HDInsight</span></span>

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="d2410-105">Další informace o použití Apache Oozie s Hadoop v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-105">Learn how to use Apache Oozie with Hadoop on HDInsight.</span></span> <span data-ttu-id="d2410-106">Apache Oozie je pracovní postup nebo koordinaci systém, který spravuje úloh Hadoop.</span><span class="sxs-lookup"><span data-stu-id="d2410-106">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="d2410-107">Oozie je integrována do zásobníku Hadoop a podporuje následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="d2410-107">Oozie is integrated with the Hadoop stack, and it supports the following jobs:</span></span>

* <span data-ttu-id="d2410-108">Apache MapReduce</span><span class="sxs-lookup"><span data-stu-id="d2410-108">Apache MapReduce</span></span>
* <span data-ttu-id="d2410-109">Apache Pig</span><span class="sxs-lookup"><span data-stu-id="d2410-109">Apache Pig</span></span>
* <span data-ttu-id="d2410-110">Apache Hive</span><span class="sxs-lookup"><span data-stu-id="d2410-110">Apache Hive</span></span>
* <span data-ttu-id="d2410-111">Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="d2410-111">Apache Sqoop</span></span>

<span data-ttu-id="d2410-112">Oozie lze také použít k plánování úloh, které jsou specifické pro systém, jako jsou programy v jazyce Java nebo skripty prostředí</span><span class="sxs-lookup"><span data-stu-id="d2410-112">Oozie can also be used to schedule jobs that are specific to a system, like Java programs or shell scripts</span></span>

> [!NOTE]
> <span data-ttu-id="d2410-113">Další možností pro definování pracovních postupů v prostředí HDInsight je Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d2410-113">Another option for defining workflows with HDInsight is Azure Data Factory.</span></span> <span data-ttu-id="d2410-114">Další informace o Azure Data Factory najdete v tématu [použijte Pig a Hive pomocí služby Data Factory][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="d2410-114">To learn more about Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2410-115">Oozie není povoleno v doméně HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-115">Oozie is not enabled on domain-joined HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d2410-116">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d2410-116">Prerequisites</span></span>

* <span data-ttu-id="d2410-117">**Cluster služby HDInsight**: najdete v části [Začínáme s prostředím HDInsight v Linuxu](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="d2410-117">**An HDInsight cluster**: See [Get Started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="d2410-118">Kroky v tomto dokumentu vyžadují clusteru služby HDInsight, který používá Linux.</span><span class="sxs-lookup"><span data-stu-id="d2410-118">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="d2410-119">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="d2410-119">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d2410-120">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d2410-120">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="example-workflow"></a><span data-ttu-id="d2410-121">Příklad pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="d2410-121">Example workflow</span></span>

<span data-ttu-id="d2410-122">Pracovní postup v tomto dokumentu obsahuje dvě akce.</span><span class="sxs-lookup"><span data-stu-id="d2410-122">The workflow used in this document contains two actions.</span></span> <span data-ttu-id="d2410-123">Akce jsou definice pro úlohy, jako je například spuštění Hive, Sqoop, MapReduce nebo jiný proces:</span><span class="sxs-lookup"><span data-stu-id="d2410-123">Actions are definitions for tasks, such as running Hive, Sqoop, MapReduce, or other process:</span></span>

![Diagram pracovního postupu][img-workflow-diagram]

1. <span data-ttu-id="d2410-125">Akce Hive spouští skript HiveQL k extrakci záznamy ze **hivesampletable** součástí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-125">A Hive action runs a HiveQL script to extract records from the **hivesampletable** included with HDInsight.</span></span> <span data-ttu-id="d2410-126">Každý řádek dat popisuje návštěvu z určité mobilní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d2410-126">Each row of data describes a visit from a specific mobile device.</span></span> <span data-ttu-id="d2410-127">Formát záznamu se zobrazí podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d2410-127">The record format appears similar to the following text:</span></span>

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    <span data-ttu-id="d2410-128">V tomto dokumentu skriptu Hive počítá celkový počet návštěv pro každou platformu (třeba na Android nebo iPhone) a ukládá počty na novou tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="d2410-128">The Hive script used in this document counts the total visits for each platform (such as Android or iPhone) and stores the counts to a new Hive table.</span></span>

    <span data-ttu-id="d2410-129">Další informace o Hivu najdete v tématu [Použití Hivu se službou HDInsight][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="d2410-129">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

2. <span data-ttu-id="d2410-130">Akce Sqoop exportuje obsah novou tabulku Hive k tabulce v Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="d2410-130">A Sqoop action exports the contents of the new Hive table to a table in an Azure SQL database.</span></span> <span data-ttu-id="d2410-131">Další informace o Sqoop najdete v tématu [Sqoop pomocí Hadoop v prostředí HDInsight][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="d2410-131">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="d2410-132">Podporované verze Oozie v clusterech prostředí HDInsight najdete v tématu [co je nového ve verzích clusterů systému Hadoop poskytovaných v HDInsight][hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="d2410-132">For supported Oozie versions on HDInsight clusters, see [What's new in the Hadoop cluster versions provided by HDInsight][hdinsight-versions].</span></span>

## <a name="create-the-working-directory"></a><span data-ttu-id="d2410-133">Vytvořte pracovní adresář</span><span class="sxs-lookup"><span data-stu-id="d2410-133">Create the working directory</span></span>

<span data-ttu-id="d2410-134">Oozie očekává prostředky potřebné pro úlohu k uložení do stejného adresáře.</span><span class="sxs-lookup"><span data-stu-id="d2410-134">Oozie expects resources required for a job to be stored in the same directory.</span></span> <span data-ttu-id="d2410-135">Tento příklad používá **wasb: / / / kurzy/useoozie**.</span><span class="sxs-lookup"><span data-stu-id="d2410-135">This example uses **wasb:///tutorials/useoozie**.</span></span> <span data-ttu-id="d2410-136">Chcete-li vytvořit tento adresář a do adresáře dat, která obsahuje novou tabulku Hive, který byl vytvořen tento pracovní postup použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-136">Use the following command to create this directory, and the data directory that holds the new Hive table created by this workflow:</span></span>

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> <span data-ttu-id="d2410-137">`-p` Parametr způsobí, že všechny adresáře v cestě, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d2410-137">The `-p` parameter causes all directories in the path to be created.</span></span> <span data-ttu-id="d2410-138">**Data** directory se používá k ukládání dat používané **useooziewf.hql** skriptu.</span><span class="sxs-lookup"><span data-stu-id="d2410-138">The **data** directory is used to hold data used by the **useooziewf.hql** script.</span></span>

<span data-ttu-id="d2410-139">Taky spusťte následující příkaz, který zajistí, že může Oozie při spuštění úlohy Hive a Sqoop zosobnit váš uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="d2410-139">Also run the following command, which ensures that Oozie can impersonate your user account when running Hive and Sqoop jobs.</span></span> <span data-ttu-id="d2410-140">Nahraďte **uživatelské jméno** s vaše přihlašovací jméno:</span><span class="sxs-lookup"><span data-stu-id="d2410-140">Replace **USERNAME** with your login name:</span></span>

```
sudo adduser USERNAME users
```

> [!NOTE]
> <span data-ttu-id="d2410-141">Můžete ignorovat chyby, které uživatel je již členem `users` skupiny.</span><span class="sxs-lookup"><span data-stu-id="d2410-141">You can ignore errors that the user is already a member of the `users` group.</span></span>

## <a name="add-a-database-driver"></a><span data-ttu-id="d2410-142">Přidat ovladač databáze</span><span class="sxs-lookup"><span data-stu-id="d2410-142">Add a database driver</span></span>

<span data-ttu-id="d2410-143">Vzhledem k tomu, že tento pracovní postup používá Sqoop exportovat data do databáze SQL, je nutné zadat kopii ovladač JDBC používaný ke komunikaci s SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d2410-143">Since this workflow uses Sqoop to export data to SQL Database, you must provide a copy of the JDBC driver used to talk to SQL Database.</span></span> <span data-ttu-id="d2410-144">Použijte následující příkaz a zkopírujte ho do pracovního adresáře:</span><span class="sxs-lookup"><span data-stu-id="d2410-144">Use the following command to copy it to the working directory:</span></span>

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

<span data-ttu-id="d2410-145">Pokud pracovní postup používá jiné prostředky, jako je například jar obsahující aplikaci MapReduce, musíte přidat také tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="d2410-145">If your workflow used other resources, such as a jar containing a MapReduce application, you would need to add these resources as well.</span></span>

## <a name="define-the-hive-query"></a><span data-ttu-id="d2410-146">Zadejte dotaz Hive</span><span class="sxs-lookup"><span data-stu-id="d2410-146">Define the Hive query</span></span>

<span data-ttu-id="d2410-147">Pomocí následujících kroků můžete vytvořit skript HiveQL, který definuje dotaz, který se používá v pracovním postupu Oozie později v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d2410-147">Use the following steps to create a HiveQL script that defines a query, which is used in an Oozie workflow later in this document.</span></span>

1. <span data-ttu-id="d2410-148">Připojte se ke clusteru pomocí protokolu SSH.</span><span class="sxs-lookup"><span data-stu-id="d2410-148">Connect to the cluster using SSH.</span></span> <span data-ttu-id="d2410-149">Příkaz je příklad použití `ssh` příkaz.</span><span class="sxs-lookup"><span data-stu-id="d2410-149">The following command is an example of using the `ssh` command.</span></span> <span data-ttu-id="d2410-150">Nahraďte __uživatelské jméno__ s uživatelem SSH pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d2410-150">Replace __USERNAME__ with the SSH user for the cluster.</span></span> <span data-ttu-id="d2410-151">Nahraďte __CLUSTERNAME__ s názvem clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-151">Replace __CLUSTERNAME__ with the name of the HDInsight cluster.</span></span>

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="d2410-152">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="d2410-152">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="d2410-153">Z připojení SSH použijte následující příkaz k vytvoření souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-153">From the SSH connection, use the following command to create a file:</span></span>

    ```
    nano useooziewf.hql
    ```

3. <span data-ttu-id="d2410-154">Jakmile se otevře nano editor, použijte následující dotaz jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-154">Once the nano editor opens, use the following query as the contents of the file:</span></span>

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    <span data-ttu-id="d2410-155">Existují dvě proměnné používané ve skriptu:</span><span class="sxs-lookup"><span data-stu-id="d2410-155">There are two variables used in the script:</span></span>

    * <span data-ttu-id="d2410-156">**${hiveTableName}**: obsahuje název tabulky, který se má vytvořit</span><span class="sxs-lookup"><span data-stu-id="d2410-156">**${hiveTableName}**: contains the name of the table to be created</span></span>

    * <span data-ttu-id="d2410-157">**${hiveDataFolder}**: obsahuje umístění pro uložení souborů dat pro tabulku</span><span class="sxs-lookup"><span data-stu-id="d2410-157">**${hiveDataFolder}**: contains the location to store the data files for the table</span></span>

    <span data-ttu-id="d2410-158">Soubor definice pracovního postupu (workflow.xml v tomto kurzu) předává tyto hodnoty tento skript HiveQL v době běhu</span><span class="sxs-lookup"><span data-stu-id="d2410-158">The workflow definition file (workflow.xml in this tutorial) passes these values to this HiveQL script at run time</span></span>

4. <span data-ttu-id="d2410-159">Editor ukončíte stisknutím Ctrl-X.</span><span class="sxs-lookup"><span data-stu-id="d2410-159">To exit the editor, press Ctrl-X.</span></span> <span data-ttu-id="d2410-160">Po zobrazení výzvy vyberte **Y** k uložení souboru, potom použijte **Enter** používat **useooziewf.hql** název souboru.</span><span class="sxs-lookup"><span data-stu-id="d2410-160">When prompted, select **Y** to save the file, then use **Enter** to use the **useooziewf.hql** file name.</span></span>

5. <span data-ttu-id="d2410-161">Použijte následující příkazy pro kopírování **useooziewf.hql** k **wasb:///tutorials/useoozie/useooziewf.hql**:</span><span class="sxs-lookup"><span data-stu-id="d2410-161">Use the following commands to copy **useooziewf.hql** to **wasb:///tutorials/useoozie/useooziewf.hql**:</span></span>

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    <span data-ttu-id="d2410-162">Ukládání těchto příkazů **useooziewf.hql** souboru na HDFS kompatibilní úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="d2410-162">These commands store the **useooziewf.hql** file on the HDFS-compatible storage for the cluster.</span></span>

## <a name="define-the-workflow"></a><span data-ttu-id="d2410-163">Definice pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="d2410-163">Define the workflow</span></span>

<span data-ttu-id="d2410-164">Definice Oozie pracovní postupy jsou zapsány ve hPDL (XML proces Definition Language).</span><span class="sxs-lookup"><span data-stu-id="d2410-164">Oozie workflows definitions are written in hPDL (an XML Process Definition Language).</span></span> <span data-ttu-id="d2410-165">Pomocí následujících kroků můžete definovat pracovní postup:</span><span class="sxs-lookup"><span data-stu-id="d2410-165">Use the following steps to define the workflow:</span></span>

1. <span data-ttu-id="d2410-166">Můžete vytvářet a upravovat nový soubor, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-166">Use the following statement to create and edit a new file:</span></span>

    ```
    nano workflow.xml
    ```

2. <span data-ttu-id="d2410-167">Jakmile se otevře nano editor, zadejte následující kód XML jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-167">Once the nano editor opens, enter the following XML as the file contents:</span></span>

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>
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

    <span data-ttu-id="d2410-168">Existují dvě akce, které jsou definovány v pracovním postupu:</span><span class="sxs-lookup"><span data-stu-id="d2410-168">There are two actions defined in the workflow:</span></span>

   * <span data-ttu-id="d2410-169">**RunHiveScript**: Tato akce je akci spuštění a běží **useooziewf.hql** skript podregistru</span><span class="sxs-lookup"><span data-stu-id="d2410-169">**RunHiveScript**: This action is the start action, and runs the **useooziewf.hql** Hive script</span></span>

   * <span data-ttu-id="d2410-170">**RunSqoopExport**: Tato akce exportuje data vytvořená ze skriptu Hive na databázi SQL pomocí Sqoop.</span><span class="sxs-lookup"><span data-stu-id="d2410-170">**RunSqoopExport**: This action exports the data created from the Hive script to SQL Database using Sqoop.</span></span> <span data-ttu-id="d2410-171">Tato akce je spuštěna pouze pokud **RunHiveScript** akce je úspěšné.</span><span class="sxs-lookup"><span data-stu-id="d2410-171">This action only runs if the **RunHiveScript** action is successful.</span></span>

     <span data-ttu-id="d2410-172">Pracovní postup má několik položek, jako například `${jobTracker}`.</span><span class="sxs-lookup"><span data-stu-id="d2410-172">The workflow has several entries, such as `${jobTracker}`.</span></span> <span data-ttu-id="d2410-173">Tyto položky jsou nahrazovány hodnoty, které můžete použít v definici úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-173">These entries are replaced by values you use in the job definition.</span></span> <span data-ttu-id="d2410-174">Později v tomto dokumentu se vytvoří definici úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-174">The job definition is created later in this document.</span></span>

     <span data-ttu-id="d2410-175">Všimněte si také `<archive>sqljdbc4.jar</arcive>` položky v části Sqoop.</span><span class="sxs-lookup"><span data-stu-id="d2410-175">Also note the `<archive>sqljdbc4.jar</arcive>` entry in the Sqoop section.</span></span> <span data-ttu-id="d2410-176">Tato položka dá pokyn Oozie archivu k dispozici na pro Sqoop spuštění této akce.</span><span class="sxs-lookup"><span data-stu-id="d2410-176">This entry instructs Oozie to make this archive available for Sqoop when this action runs.</span></span>

3. <span data-ttu-id="d2410-177">Použijte Ctrl-X, pak **Y** a **Enter** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="d2410-177">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

4. <span data-ttu-id="d2410-178">Použijte následující příkaz pro kopírování **workflow.xml** do souboru **/tutorials/useoozie/workflow.xml**:</span><span class="sxs-lookup"><span data-stu-id="d2410-178">Use the following command to copy the **workflow.xml** file to **/tutorials/useoozie/workflow.xml**:</span></span>

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-the-database"></a><span data-ttu-id="d2410-179">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="d2410-179">Create the database</span></span>

<span data-ttu-id="d2410-180">K vytvoření databáze SQL Azure, postupujte podle kroků v [vytvoření databáze SQL](../sql-database/sql-database-get-started.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d2410-180">To create an Azure SQL Database, follow the steps in the [Create a SQL Database](../sql-database/sql-database-get-started.md) document.</span></span> <span data-ttu-id="d2410-181">Při vytváření databáze, použijte `oozietest` jako název databáze.</span><span class="sxs-lookup"><span data-stu-id="d2410-181">When creating the database, use `oozietest` as the database name.</span></span> <span data-ttu-id="d2410-182">Také si poznamenejte název databázového serveru.</span><span class="sxs-lookup"><span data-stu-id="d2410-182">Also make a note of the name of the database server.</span></span>

### <a name="create-the-table"></a><span data-ttu-id="d2410-183">Vytvoření tabulky</span><span class="sxs-lookup"><span data-stu-id="d2410-183">Create the table</span></span>

> [!NOTE]
> <span data-ttu-id="d2410-184">Pro připojení k databázi SQL a vytvořte tabulku mnoha způsoby.</span><span class="sxs-lookup"><span data-stu-id="d2410-184">There are many ways to connect to SQL Database to create a table.</span></span> <span data-ttu-id="d2410-185">Následující postup použijte [FreeTDS](http://www.freetds.org/) z clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-185">The following steps use [FreeTDS](http://www.freetds.org/) from the HDInsight cluster.</span></span>


1. <span data-ttu-id="d2410-186">Použijte následující příkaz k instalaci FreeTDS v clusteru HDInsight:</span><span class="sxs-lookup"><span data-stu-id="d2410-186">Use the following command to install FreeTDS on the HDInsight cluster:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. <span data-ttu-id="d2410-187">Jednou FreeTDS nainstalován, použijte následující příkaz pro připojení k serveru služby SQL Database, kterou jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="d2410-187">Once FreeTDS has been installed, use the following command to connect to the SQL Database server you created previously:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="d2410-188">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="d2410-188">You receive output similar to the following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. <span data-ttu-id="d2410-189">Na `1>` výzva, zadejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="d2410-189">At the `1>` prompt, enter the following lines:</span></span>

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    <span data-ttu-id="d2410-190">Když `GO` příkaz je zadán, jsou vyhodnocovány předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="d2410-190">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="d2410-191">Tyto příkazy vytvořit tabulku s názvem **mobiledata** používané v tomto pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="d2410-191">These statements create a table named **mobiledata** that is used by the workflow.</span></span>

    <span data-ttu-id="d2410-192">Chcete-li ověřit, zda byl vytvořen v tabulce použijte následující:</span><span class="sxs-lookup"><span data-stu-id="d2410-192">Use the following to verify that the table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="d2410-193">Zobrazí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="d2410-193">You see output similar to the following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. <span data-ttu-id="d2410-194">Zadejte `exit` na `1>` výzvy ukončete nástroj tsql.</span><span class="sxs-lookup"><span data-stu-id="d2410-194">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="create-the-job-definition"></a><span data-ttu-id="d2410-195">Vytvořit definici úlohy</span><span class="sxs-lookup"><span data-stu-id="d2410-195">Create the job definition</span></span>

<span data-ttu-id="d2410-196">Definice úlohy popisuje, kde najít workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="d2410-196">The job definition describes where to find the workflow.xml.</span></span> <span data-ttu-id="d2410-197">Také popisuje, kde najít další soubory, které používá pracovní postup (například useooziewf.hql.) Také definuje hodnoty pro vlastnosti používaných v rámci pracovního postupu a související soubory.</span><span class="sxs-lookup"><span data-stu-id="d2410-197">It also describes where to find other files used by the workflow (such as useooziewf.hql.) It also defines the values for properties used within the workflow and associated files.</span></span>

1. <span data-ttu-id="d2410-198">Použijte následující příkaz k získání úplné adresy výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="d2410-198">Use the following command to get the full address of the default storage.</span></span> <span data-ttu-id="d2410-199">Tato adresa se používá v konfiguračním souboru za chvíli:</span><span class="sxs-lookup"><span data-stu-id="d2410-199">This address is used in the configuration file in a moment:</span></span>

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    <span data-ttu-id="d2410-200">Tento příkaz vrátí informace podobná následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="d2410-200">This command returns information similar to the following XML:</span></span>

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > <span data-ttu-id="d2410-201">Pokud HDInsight cluster používá úložiště Azure jako výchozí úložiště, `<value>` obsah elementu začínat `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="d2410-201">If the HDInsight cluster uses Azure Storage as the default storage, the `<value>` element contents begin with `wasb://`.</span></span> <span data-ttu-id="d2410-202">Pokud je použita Azure Data Lake Store, začne s `adl://`.</span><span class="sxs-lookup"><span data-stu-id="d2410-202">If Azure Data Lake Store is used instead, it begins with `adl://`.</span></span>

    <span data-ttu-id="d2410-203">Uložení obsahu `<value>` element, protože se používá v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="d2410-203">Save the contents of the `<value>` element, as it is used in the next steps.</span></span>

2. <span data-ttu-id="d2410-204">Získat plně kvalifikovaný název domény clusteru headnode použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="d2410-204">Use the following command to get FQDN of the cluster headnode.</span></span> <span data-ttu-id="d2410-205">Tyto informace se používají pro adresu JobTracker pro cluster:</span><span class="sxs-lookup"><span data-stu-id="d2410-205">This information is used for the JobTracker address for the cluster:</span></span>

    ```
    hostname -f
    ```

    <span data-ttu-id="d2410-206">Vrátí informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d2410-206">This returns information similar to the following text:</span></span>

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    <span data-ttu-id="d2410-207">Port používaný pro jako JobTracker je 8050, takže je úplná adresa pro jako JobTracker `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span><span class="sxs-lookup"><span data-stu-id="d2410-207">The port used for the JobTracker is 8050, so the full address to use for the JobTracker is `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span></span>

3. <span data-ttu-id="d2410-208">Použijte následující postupy k vytvoření konfigurace definice úlohy Oozie:</span><span class="sxs-lookup"><span data-stu-id="d2410-208">Use the following to create the Oozie job definition configuration:</span></span>

    ```
    nano job.xml
    ```

4. <span data-ttu-id="d2410-209">Jakmile se otevře nano editor, použijte následující kód XML jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-209">Once the nano editor opens, use the following XML as the contents of the file:</span></span>

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

   * <span data-ttu-id="d2410-210">Nahraďte všechny výskyty  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  s hodnotou jste obdrželi dříve pro výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="d2410-210">Replace all instances of **wasb://mycontainer@mystorageaccount.blob.core.windows.net** with the value you received earlier for default storage.</span></span>

     > [!WARNING]
     > <span data-ttu-id="d2410-211">Pokud se cesta `wasb` cestu, musíte použít úplnou cestu.</span><span class="sxs-lookup"><span data-stu-id="d2410-211">If the path is a `wasb` path, you must use the full path.</span></span> <span data-ttu-id="d2410-212">Není jeho právě Zkraťte `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="d2410-212">Do not shorten it to just `wasb:///`.</span></span>

   * <span data-ttu-id="d2410-213">Nahraďte **JOBTRACKERADDRESS** s adresou JobTracker/ResourceManager jste obdrželi dříve.</span><span class="sxs-lookup"><span data-stu-id="d2410-213">Replace **JOBTRACKERADDRESS** with the JobTracker/ResourceManager address you received earlier.</span></span>
   * <span data-ttu-id="d2410-214">Nahraďte **jméno** s vaše přihlašovací jméno pro HDInsight cluster.</span><span class="sxs-lookup"><span data-stu-id="d2410-214">Replace **YourName** with your login name for the HDInsight cluster.</span></span>
   * <span data-ttu-id="d2410-215">Nahraďte **serverName**, **adminLogin**, a **adminPassword** s informacemi k vaší databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="d2410-215">Replace **serverName**, **adminLogin**, and **adminPassword** with the information for your Azure SQL Database.</span></span>

     <span data-ttu-id="d2410-216">Většinu informací v tomto souboru se používá k naplnění hodnoty používané v souborech workflow.xml nebo ooziewf.hql (např. ${nameNode}.)</span><span class="sxs-lookup"><span data-stu-id="d2410-216">Most of the information in this file is used to populate the values used in the workflow.xml or ooziewf.hql files (such as ${nameNode}.)</span></span>

     > [!NOTE]
     > <span data-ttu-id="d2410-217">**Oozie.wf.application.path** položka udává kde najít soubor workflow.xml, který obsahuje pracovní postup spuštěné prostřednictvím této úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-217">The **oozie.wf.application.path** entry defines where to find the workflow.xml file, which contains the workflow ran by this job.</span></span>

5. <span data-ttu-id="d2410-218">Použijte Ctrl-X, pak **Y** a **Enter** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="d2410-218">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

## <a name="submit-and-manage-the-job"></a><span data-ttu-id="d2410-219">Odesílat a spravovat úlohy</span><span class="sxs-lookup"><span data-stu-id="d2410-219">Submit and manage the job</span></span>

<span data-ttu-id="d2410-220">Následující postup použijte příkaz Oozie k odeslání a správa pracovních postupů Oozie v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d2410-220">The following steps use the Oozie command to submit and manage Oozie workflows on the cluster.</span></span> <span data-ttu-id="d2410-221">Příkaz Oozie je popisný rozhraní přes [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="d2410-221">The Oozie command is a friendly interface over the [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d2410-222">Při použití příkazu Oozie, je nutné použít plně kvalifikovaný název domény pro HDInsight headnode.</span><span class="sxs-lookup"><span data-stu-id="d2410-222">When using the Oozie command, you must use the FQDN for the HDInsight headnode.</span></span> <span data-ttu-id="d2410-223">Tento plně kvalifikovaný název domény je k dispozici pouze z clusteru, nebo pokud je clusteru na virtuální síť Azure, z jiných počítačů ve stejné síti.</span><span class="sxs-lookup"><span data-stu-id="d2410-223">This FQDN is only accessible from the cluster, or if the cluster is on an Azure Virtual Network, from other machines on the same network.</span></span>


1. <span data-ttu-id="d2410-224">Následující informace vám pomůžou získat adresu URL pro službu Oozie:</span><span class="sxs-lookup"><span data-stu-id="d2410-224">Use the following to obtain the URL to the Oozie service:</span></span>

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    <span data-ttu-id="d2410-225">Vrátí informace podobná následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="d2410-225">This returns information similar to the following XML:</span></span>

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    <span data-ttu-id="d2410-226">`http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` Část je adresa URL pro použití s příkazem Oozie.</span><span class="sxs-lookup"><span data-stu-id="d2410-226">The `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` portion is the URL to use with the Oozie command.</span></span>

2. <span data-ttu-id="d2410-227">Použijte následující postupy k vytvoření proměnné prostředí pro adresu URL, takže není nutné zadat pro každý příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-227">Use the following to create an environment variable for the URL, so you don't have to type it for every command:</span></span>

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    <span data-ttu-id="d2410-228">Nahraďte adresu URL, které jste dostali dříve.</span><span class="sxs-lookup"><span data-stu-id="d2410-228">Replace the URL with the one you received earlier.</span></span>
3. <span data-ttu-id="d2410-229">Použijte následující postup odeslání úlohy:</span><span class="sxs-lookup"><span data-stu-id="d2410-229">Use the following to submit the job:</span></span>

    ```
    oozie job -config job.xml -submit
    ```

    <span data-ttu-id="d2410-230">Tento příkaz načte informace o úloze z **job.xml** a odešle ji Oozie, ale nejde spustit.</span><span class="sxs-lookup"><span data-stu-id="d2410-230">This command loads the job information from **job.xml** and submits it to Oozie, but does not run it.</span></span>

    <span data-ttu-id="d2410-231">Po dokončení příkazu by měl vrátit ID úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-231">Once the command completes, it should return the ID of the job.</span></span> <span data-ttu-id="d2410-232">Například, `0000005-150622124850154-oozie-oozi-W`.</span><span class="sxs-lookup"><span data-stu-id="d2410-232">For example, `0000005-150622124850154-oozie-oozi-W`.</span></span> <span data-ttu-id="d2410-233">Toto ID se používá ke správě úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-233">This ID is used to manage the job.</span></span>

4. <span data-ttu-id="d2410-234">Zobrazení stavu úlohy pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="d2410-234">View the status of the job using the following command:</span></span>

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > <span data-ttu-id="d2410-235">Nahraďte `<JOBID>` s ID, vrátí se v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d2410-235">Replace `<JOBID>` with the ID returned in the previous step.</span></span>

    <span data-ttu-id="d2410-236">Vrátí informace podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d2410-236">This returns information similar to the following text:</span></span>

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

    <span data-ttu-id="d2410-237">Tato úloha je ve stavu `PREP`.</span><span class="sxs-lookup"><span data-stu-id="d2410-237">This job has a status of `PREP`.</span></span> <span data-ttu-id="d2410-238">Tento stav indikuje, že byla úloha vytvořen, ale není spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d2410-238">This status indicates that the job was created, but not started.</span></span>

5. <span data-ttu-id="d2410-239">Spustit úlohu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-239">Use the following command to start the job:</span></span>

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > <span data-ttu-id="d2410-240">Nahraďte `<JOBID>` s ID vrátil dříve.</span><span class="sxs-lookup"><span data-stu-id="d2410-240">Replace `<JOBID>` with the ID returned previously.</span></span>

    <span data-ttu-id="d2410-241">Pokud po tento příkaz Zkontrolovat stav, je v běžícím stavu a se vrátí informace pro akce v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-241">If you check the status after this command, it is in a running state, and information is returned for the actions within the job.</span></span>

6. <span data-ttu-id="d2410-242">Po úspěšném dokončení úlohy můžete ověřit, že data byla vygenerována a exportovány do tabulky databáze SQL pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="d2410-242">Once the task completes successfully, you can verify that the data was generated and exported to the SQL Database table by using the following commands:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="d2410-243">Na `1>` výzva, zadejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-243">At the `1>` prompt, enter the following query:</span></span>

    ```
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="d2410-244">Vrácené informace je podobná následující text:</span><span class="sxs-lookup"><span data-stu-id="d2410-244">The information returned is similar to the following text:</span></span>

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

<span data-ttu-id="d2410-245">Další informace o příkazu Oozie najdete v tématu [nástroj příkazového řádku Oozie](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span><span class="sxs-lookup"><span data-stu-id="d2410-245">For more information on the Oozie command, see [Oozie command-line tool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span></span>

## <a name="oozie-rest-api"></a><span data-ttu-id="d2410-246">Oozie REST API</span><span class="sxs-lookup"><span data-stu-id="d2410-246">Oozie REST API</span></span>

<span data-ttu-id="d2410-247">Rozhraní API REST Oozie umožňuje vytvářet vlastní nástroje, které pracují s Oozie.</span><span class="sxs-lookup"><span data-stu-id="d2410-247">The Oozie REST API allows you to build your own tools that work with Oozie.</span></span> <span data-ttu-id="d2410-248">Tady jsou HDInsight konkrétní informace o použití rozhraní REST API Oozie:</span><span class="sxs-lookup"><span data-stu-id="d2410-248">The following are HDInsight specific information about using the Oozie REST API:</span></span>

* <span data-ttu-id="d2410-249">**Identifikátor URI**: rozhraní API REST je přístupná z mimo cluster v`https://CLUSTERNAME.azurehdinsight.net/oozie`</span><span class="sxs-lookup"><span data-stu-id="d2410-249">**URI**: The REST API can be accessed from outside the cluster at `https://CLUSTERNAME.azurehdinsight.net/oozie`</span></span>

* <span data-ttu-id="d2410-250">**Ověřování**: ověření na rozhraní API pomocí účet clusteru HTTP (správce) a heslo.</span><span class="sxs-lookup"><span data-stu-id="d2410-250">**Authentication**: Authenticate to the API using the cluster HTTP account (admin) and password.</span></span> <span data-ttu-id="d2410-251">Například:</span><span class="sxs-lookup"><span data-stu-id="d2410-251">For example:</span></span>

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

<span data-ttu-id="d2410-252">Další informace o používání rozhraní API REST Oozie najdete v tématu [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="d2410-252">For more information on using the Oozie REST API, see [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

## <a name="oozie-web-ui"></a><span data-ttu-id="d2410-253">Oozie webového uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="d2410-253">Oozie Web UI</span></span>

<span data-ttu-id="d2410-254">Webové uživatelské rozhraní Oozie poskytuje webové pohled na stav Oozie úloh v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d2410-254">The Oozie Web UI provides a web-based view into the status of Oozie jobs on the cluster.</span></span> <span data-ttu-id="d2410-255">Webového uživatelského rozhraní vám umožní zobrazit následující informace:</span><span class="sxs-lookup"><span data-stu-id="d2410-255">The web UI allows you to view the following information:</span></span>

* <span data-ttu-id="d2410-256">Stav úlohy</span><span class="sxs-lookup"><span data-stu-id="d2410-256">Job status</span></span>
* <span data-ttu-id="d2410-257">Definice úlohy</span><span class="sxs-lookup"><span data-stu-id="d2410-257">Job definition</span></span>
* <span data-ttu-id="d2410-258">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="d2410-258">Configuration</span></span>
* <span data-ttu-id="d2410-259">Graf akce pro úlohu</span><span class="sxs-lookup"><span data-stu-id="d2410-259">A graph of the actions in the job</span></span>
* <span data-ttu-id="d2410-260">Protokoly pro úlohu</span><span class="sxs-lookup"><span data-stu-id="d2410-260">Logs for the job</span></span>

<span data-ttu-id="d2410-261">Můžete také zobrazit podrobnosti pro akce v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-261">You can also view details for actions within a job.</span></span>

<span data-ttu-id="d2410-262">Pro přístup k Oozie webového uživatelského rozhraní, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="d2410-262">To access the Oozie Web UI, use the following steps:</span></span>

1. <span data-ttu-id="d2410-263">Vytvoření tunelu SSH do clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d2410-263">Create an SSH tunnel to the HDInsight cluster.</span></span> <span data-ttu-id="d2410-264">Informace najdete v tématu [používání tunelového propojení SSH s HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d2410-264">For information, see the [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="d2410-265">Po vytvoření tunelu, otevřete webovému uživatelskému rozhraní Ambari ve webovém prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="d2410-265">Once a tunnel has been created, open the Ambari web UI in your web browser.</span></span> <span data-ttu-id="d2410-266">Je identifikátor URI webu Ambari **https://CLUSTERNAME.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="d2410-266">The URI for the Ambari site is **https://CLUSTERNAME.azurehdinsight.net**.</span></span> <span data-ttu-id="d2410-267">Nahraďte **CLUSTERNAME** s názvem vašeho clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="d2410-267">Replace **CLUSTERNAME** with the name of your Linux-based HDInsight cluster.</span></span>

3. <span data-ttu-id="d2410-268">Na levé straně stránky vyberte **Oozie**, pak **rychlé odkazy**a v neposlední řadě **Oozie webového uživatelského rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="d2410-268">From the left side of the page, select **Oozie**, then **Quick Links**, and finally **Oozie Web UI**.</span></span>

    ![obrázek v nabídkách](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. <span data-ttu-id="d2410-270">Webové uživatelské rozhraní Oozie výchozí zobrazení spuštěné úlohy pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="d2410-270">The Oozie Web UI defaults to displaying running Workflow Jobs.</span></span> <span data-ttu-id="d2410-271">Pokud chcete zobrazit všechny úlohy pracovního postupu, vyberte **všechny úlohy**.</span><span class="sxs-lookup"><span data-stu-id="d2410-271">To see all workflow jobs, select **All Jobs**.</span></span>

    ![Zobrazí všechny úlohy](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. <span data-ttu-id="d2410-273">Vyberte úlohy zobrazíte další informace o úloze.</span><span class="sxs-lookup"><span data-stu-id="d2410-273">Select a job to view more information about the job.</span></span>

    ![Informace o úloze](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. <span data-ttu-id="d2410-275">Na kartě informace o úloze můžete zobrazit informace o základní úlohy a jednotlivé akce v rámci úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-275">From the Job Info tab, you can see basic job information and the individual actions within the job.</span></span> <span data-ttu-id="d2410-276">Pomocí karet v horní části můžete zobrazit definici úlohy, úlohy konfigurace přístup protokolu úlohy nebo zobrazení směrované Acyklické grafu (DAG) úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-276">Using the tabs at the top you can view the Job Definition, Job Configuration, access the Job Log or view a Directed Acyclic Graph (DAG) of the job.</span></span>

   * <span data-ttu-id="d2410-277">**Protokol úlohy**: vyberte **GetLogs** tlačítko získat všechny protokoly pro úlohu, nebo použijte **zadejte vyhledávací filtr** pole pro filtrování protokolů</span><span class="sxs-lookup"><span data-stu-id="d2410-277">**Job Log**: Select the **GetLogs** button to get all logs for the job, or use the **Enter Search Filter** field to filter logs</span></span>

       ![Protokol úlohy](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * <span data-ttu-id="d2410-279">**JobDAG**: DAG je grafické přehled cest k datům prováděné v pracovním postupu</span><span class="sxs-lookup"><span data-stu-id="d2410-279">**JobDAG**: The DAG is a graphical overview of the data paths taken through the workflow</span></span>

       ![Úloha DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. <span data-ttu-id="d2410-281">Vyberte jednu z akcí z **informace o úloze** karta zobrazí informace o akci.</span><span class="sxs-lookup"><span data-stu-id="d2410-281">Selecting one of the actions from the **Job Info** tab brings up information for the action.</span></span> <span data-ttu-id="d2410-282">Vyberte například **RunHiveScript** akce.</span><span class="sxs-lookup"><span data-stu-id="d2410-282">For example, select the **RunHiveScript** action.</span></span>

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/action.png)

8. <span data-ttu-id="d2410-284">Zobrazí podrobnosti pro akce, například odkaz **adresa URL konzoly**.</span><span class="sxs-lookup"><span data-stu-id="d2410-284">You can see details for the action, such as a link to the **Console URL**.</span></span> <span data-ttu-id="d2410-285">Tento odkaz slouží k zobrazení informací o JobTracker pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="d2410-285">This link can be used to view JobTracker information for the job.</span></span>

## <a name="scheduling-jobs"></a><span data-ttu-id="d2410-286">Plánování úloh</span><span class="sxs-lookup"><span data-stu-id="d2410-286">Scheduling jobs</span></span>

<span data-ttu-id="d2410-287">Koordinátor umožňuje zadat začátek, konec a četnost výskytu pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-287">The coordinator allows you to specify a start, end, and occurrence frequency for jobs.</span></span> <span data-ttu-id="d2410-288">Chcete-li definovat plán pro pracovní postup, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d2410-288">To define a schedule for the workflow, use the following steps:</span></span>

1. <span data-ttu-id="d2410-289">Následující informace vám pomůžou vytvořit soubor s názvem **coordinator.xml**:</span><span class="sxs-lookup"><span data-stu-id="d2410-289">Use the following to create a file named **coordinator.xml**:</span></span>

    ```
    nano coordinator.xml
    ```

    <span data-ttu-id="d2410-290">Použijte následující kód XML jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-290">Use the following XML as the contents of the file:</span></span>

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
    > <span data-ttu-id="d2410-291">`${...}` Proměnné jsou nahrazena hodnotami v definici úlohy při spuštění.</span><span class="sxs-lookup"><span data-stu-id="d2410-291">The `${...}` variables are replaced by values in the job definition at run-time.</span></span> <span data-ttu-id="d2410-292">Proměnné jsou:</span><span class="sxs-lookup"><span data-stu-id="d2410-292">The variables are:</span></span>
    >
    > * <span data-ttu-id="d2410-293">`${coordFrequency}`: Doba mezi spuštěné instance úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-293">`${coordFrequency}`: Time between running instances of the job.</span></span>
    > <span data-ttu-id="d2410-294">** `${coordStart}`: Úloha Počáteční čas.</span><span class="sxs-lookup"><span data-stu-id="d2410-294">** `${coordStart}`: The job start time.</span></span>
    > * <span data-ttu-id="d2410-295">`${coordEnd}`: Času ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="d2410-295">`${coordEnd}`: The job end time.</span></span>
    > * <span data-ttu-id="d2410-296">`${coordTimezone}`: Koordinátor úlohy jsou v pevné časové pásmo s žádné letní čas (obvykle vyjádřený pomocí UTC).</span><span class="sxs-lookup"><span data-stu-id="d2410-296">`${coordTimezone}`: Coordinator jobs are in a fixed time zone with no daylight savings time (typically represented by using UTC).</span></span> <span data-ttu-id="d2410-297">Toto časové pásmo se označuje jako "Oozie zpracování časové pásmo."</span><span class="sxs-lookup"><span data-stu-id="d2410-297">This time zone is referred as the "Oozie processing timezone."</span></span>
    > * <span data-ttu-id="d2410-298">`${wfPath}`: Cesta k workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="d2410-298">`${wfPath}`: The path to the workflow.xml.</span></span>

2. <span data-ttu-id="d2410-299">Chcete uložit soubor, použijte kombinaci kláves Ctrl-X **Y**, a **Enter**.</span><span class="sxs-lookup"><span data-stu-id="d2410-299">To save the file, use Ctrl-X, **Y**, and **Enter**.</span></span>

3. <span data-ttu-id="d2410-300">Zkopírujte soubor do pracovní adresář pro tuto úlohu použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-300">Use the following command to copy the file to the working directory for this job:</span></span>

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. <span data-ttu-id="d2410-301">Použijte tento příkaz Upravit **job.xml** souboru:</span><span class="sxs-lookup"><span data-stu-id="d2410-301">Use the following to modify the **job.xml** file:</span></span>

    ```
    nano job.xml
    ```

    <span data-ttu-id="d2410-302">Proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="d2410-302">Make the following changes:</span></span>

   * <span data-ttu-id="d2410-303">Dáte pokyn, aby oozie coordinator soubor namísto pracovního postupu spustíte, změnit `<name>oozie.wf.application.path</name>` k `<name>oozie.coord.application.path</name>`.</span><span class="sxs-lookup"><span data-stu-id="d2410-303">To instruct oozie to run the coordinator file instead of the workflow, change `<name>oozie.wf.application.path</name>` to `<name>oozie.coord.application.path</name>`.</span></span>

   * <span data-ttu-id="d2410-304">Chcete-li nastavit `workflowPath` proměnné používané koordinátorem, přidejte následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="d2410-304">To set the `workflowPath` variable used by the coordinator, add the following XML:</span></span>

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       <span data-ttu-id="d2410-305">Nahraďte `wasb://mycontainer@mystorageaccount.blob.core.windows` text hodnota použitá v jiných položek v souboru job.xml.</span><span class="sxs-lookup"><span data-stu-id="d2410-305">Replace the `wasb://mycontainer@mystorageaccount.blob.core.windows` text with the value used in other entries in the job.xml file.</span></span>

   * <span data-ttu-id="d2410-306">Chcete-li definovat začátek, konec a četnost pro koordinátorem, přidejte následující kód XML:</span><span class="sxs-lookup"><span data-stu-id="d2410-306">To define the start, end, and frequency for the coordinator, add the following XML:</span></span>

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

       <span data-ttu-id="d2410-307">Tyto hodnoty na 10 může 2017, koncový čas na 12 může 2017 nastavit výchozí čas do 12:00 PM.</span><span class="sxs-lookup"><span data-stu-id="d2410-307">These values set the start time to 12:00PM on May 10, 2017, the end time to May 12, 2017.</span></span> <span data-ttu-id="d2410-308">Interval pro spuštění tuto úlohu denně.</span><span class="sxs-lookup"><span data-stu-id="d2410-308">The interval for running this job daily.</span></span> <span data-ttu-id="d2410-309">Frekvence se v minutách, takže 24 hodin x 60 minut = 1 440 minut.</span><span class="sxs-lookup"><span data-stu-id="d2410-309">The frequency is in minutes, so 24 hours x 60 minutes = 1440 minutes.</span></span> <span data-ttu-id="d2410-310">Nakonec se nastaví časové pásmo UTC.</span><span class="sxs-lookup"><span data-stu-id="d2410-310">Finally, the timezone is set to UTC.</span></span>

5. <span data-ttu-id="d2410-311">Použijte Ctrl-X, pak **Y** a **Enter** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="d2410-311">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

6. <span data-ttu-id="d2410-312">Pokud chcete spustit úlohu, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="d2410-312">To run the job, use the following command:</span></span>

    ```
    oozie job -config job.xml -run
    ```

    <span data-ttu-id="d2410-313">Tento příkaz odešle a spustí úlohu.</span><span class="sxs-lookup"><span data-stu-id="d2410-313">This command submits and starts the job.</span></span>

7. <span data-ttu-id="d2410-314">Pokud navštívíte Oozie webového uživatelského rozhraní a vyberte **koordinátor úlohy** kartě uvidíte informace podobně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d2410-314">If you visit the Oozie Web UI and select the **Coordinator Jobs** tab, you see information similar to the following image:</span></span>

    ![Karta úlohy Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    <span data-ttu-id="d2410-316">**Další Materialization** položka obsahuje další dobu, která má být úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d2410-316">The **Next Materialization** entry contains the next time that the job runs.</span></span>

8. <span data-ttu-id="d2410-317">Podobně jako u starších úlohy pracovního postupu, seznam výběr položek úlohy v webového uživatelského rozhraní zobrazí informace v úloze:</span><span class="sxs-lookup"><span data-stu-id="d2410-317">Similar to the earlier workflow job, selecting the job entry in the web UI displays information on the job:</span></span>

    ![Informace o úloze Coordinator](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > <span data-ttu-id="d2410-319">Tento image se zobrazí pouze úspěšně spustí úlohy, ne z individuálních akce v rámci naplánované pracovní postup.</span><span class="sxs-lookup"><span data-stu-id="d2410-319">This image only shows successful runs of the job, not individual actions within the scheduled workflow.</span></span> <span data-ttu-id="d2410-320">Chcete-li vidět, vyberte jednu z **akce** položky.</span><span class="sxs-lookup"><span data-stu-id="d2410-320">To see that, select one of the **Action** entries.</span></span>

    ![Informace o akci](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a><span data-ttu-id="d2410-322">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="d2410-322">Troubleshooting</span></span>

<span data-ttu-id="d2410-323">Rozhraní Oozie umožňuje zobrazit protokoly Oozie.</span><span class="sxs-lookup"><span data-stu-id="d2410-323">The Oozie UI allows you to view Oozie logs.</span></span> <span data-ttu-id="d2410-324">Obsahuje taky odkazy na JobTracker protokoly pro úlohy MapReduce spuštěna v tomto pracovním postupu.</span><span class="sxs-lookup"><span data-stu-id="d2410-324">It also contains links to JobTracker logs for MapReduce tasks started by the workflow.</span></span> <span data-ttu-id="d2410-325">Vzor pro řešení potíží s by měla být:</span><span class="sxs-lookup"><span data-stu-id="d2410-325">The pattern for troubleshooting should be:</span></span>

1. <span data-ttu-id="d2410-326">Zobrazte úlohy v Oozie webového uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d2410-326">View the job in Oozie Web UI.</span></span>

2. <span data-ttu-id="d2410-327">Pokud dojde k chybě nebo pro určité akce se nezdařilo, vyberte akci, která zjistí, zda **chybová zpráva** pole poskytuje další informace o selhání.</span><span class="sxs-lookup"><span data-stu-id="d2410-327">If there is an error or failure for a specific action, select the action to see if the **Error Message** field provides more information on the failure.</span></span>

3. <span data-ttu-id="d2410-328">Pokud je k dispozici, použijte adresu URL akce zobrazíte další podrobnosti (třeba protokoly JobTracker) pro akci.</span><span class="sxs-lookup"><span data-stu-id="d2410-328">If available, use the URL from the action to view more details (such as JobTracker logs) for the action.</span></span>

<span data-ttu-id="d2410-329">Dále jsou uvedeny konkrétní chyby, ke kterým může dojít a způsob jejich řešení.</span><span class="sxs-lookup"><span data-stu-id="d2410-329">The following are specific errors you may encounter, and how to resolve them.</span></span>

### <a name="ja009-cannot-initialize-cluster"></a><span data-ttu-id="d2410-330">JA009: Nelze inicializovat clusteru</span><span class="sxs-lookup"><span data-stu-id="d2410-330">JA009: Cannot initialize cluster</span></span>

<span data-ttu-id="d2410-331">**Příznaky**: stav úlohy změní na **POZASTAVENO**.</span><span class="sxs-lookup"><span data-stu-id="d2410-331">**Symptoms**: The job status changes to **SUSPENDED**.</span></span> <span data-ttu-id="d2410-332">Zobrazí podrobnosti pro úlohy, stav RunHiveScript jako **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="d2410-332">Details for the job show the RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="d2410-333">Výběr akce, zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d2410-333">Selecting the action displays the following error message:</span></span>

    JA009: Cannot initialize Cluster. Please check your configuration for map

<span data-ttu-id="d2410-334">**Příčina**: The WASB adresy použité v **job.xml** soubor neobsahuje kontejner úložiště nebo název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d2410-334">**Cause**: The WASB addresses used in the **job.xml** file do not contain the storage container or storage account name.</span></span> <span data-ttu-id="d2410-335">Musí být ve formátu adresa WASB `wasb://containername@storageaccountname.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="d2410-335">The WASB address format must be `wasb://containername@storageaccountname.blob.core.windows.net`.</span></span>

<span data-ttu-id="d2410-336">**Řešení**: Změna adresy WASB používá daná úloha.</span><span class="sxs-lookup"><span data-stu-id="d2410-336">**Resolution**: Change the WASB addresses used by the job.</span></span>

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a><span data-ttu-id="d2410-337">JA002: Oozie není povoleno zosobnění &lt;uživatele ></span><span class="sxs-lookup"><span data-stu-id="d2410-337">JA002: Oozie is not allowed to impersonate &lt;USER></span></span>

<span data-ttu-id="d2410-338">**Příznaky**: stav úlohy změní na **POZASTAVENO**.</span><span class="sxs-lookup"><span data-stu-id="d2410-338">**Symptoms**: The job status changes to **SUSPENDED**.</span></span> <span data-ttu-id="d2410-339">Zobrazí podrobnosti pro úlohy, stav RunHiveScript jako **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="d2410-339">Details for the job show the RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="d2410-340">Výběr akce zobrazí následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d2410-340">Selecting the action shows the following error message:</span></span>

    JA002: User: oozie is not allowed to impersonate <USER>

<span data-ttu-id="d2410-341">**Příčina**: nastavení aktuální oprávnění neumožňují Oozie zosobnit zadaný uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="d2410-341">**Cause**: Current permission settings do not allow Oozie to impersonate the specified user account.</span></span>

<span data-ttu-id="d2410-342">**Řešení**: Oozie je povoleno zosobnit uživatele v **uživatelé** skupiny.</span><span class="sxs-lookup"><span data-stu-id="d2410-342">**Resolution**: Oozie is allowed to impersonate users in the **users** group.</span></span> <span data-ttu-id="d2410-343">Použití `groups USERNAME` zobrazení skupin, které uživatelský účet je členem skupiny.</span><span class="sxs-lookup"><span data-stu-id="d2410-343">Use the `groups USERNAME` to see the groups that the user account is a member of.</span></span> <span data-ttu-id="d2410-344">Pokud uživatel není členem **uživatelé** skupiny, použijte následující příkaz a přidejte uživatele do skupiny:</span><span class="sxs-lookup"><span data-stu-id="d2410-344">If the user is not a member of the **users** group, use the following command to add the user to the group:</span></span>

    sudo adduser USERNAME users

> [!NOTE]
> <span data-ttu-id="d2410-345">To může trvat několik minut, než HDInsight rozpozná, že uživatel byl přidán do skupiny.</span><span class="sxs-lookup"><span data-stu-id="d2410-345">It may take several minutes before HDInsight recognizes that the user has been added to the group.</span></span>

### <a name="launcher-error-sqoop"></a><span data-ttu-id="d2410-346">Spouštěč chyby (Sqoop)</span><span class="sxs-lookup"><span data-stu-id="d2410-346">Launcher ERROR (Sqoop)</span></span>

<span data-ttu-id="d2410-347">**Příznaky**: stav úlohy změní na **KILLED**.</span><span class="sxs-lookup"><span data-stu-id="d2410-347">**Symptoms**: The job status changes to **KILLED**.</span></span> <span data-ttu-id="d2410-348">Zobrazí podrobnosti pro úlohy, stav RunSqoopExport jako **chyba**.</span><span class="sxs-lookup"><span data-stu-id="d2410-348">Details for the job show the RunSqoopExport status as **ERROR**.</span></span> <span data-ttu-id="d2410-349">Výběr akce zobrazí následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d2410-349">Selecting the action shows the following error message:</span></span>

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

<span data-ttu-id="d2410-350">**Příčina**: Sqoop se nepodařilo načíst ovladač databáze, které jsou nutné pro přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="d2410-350">**Cause**: Sqoop is unable to load the database driver required to access the database.</span></span>

<span data-ttu-id="d2410-351">**Řešení**: při použití Sqoop z úlohu Oozie, je nutné zahrnout databázi ovladačů s prostředky (například workflow.xml) používá daná úloha.</span><span class="sxs-lookup"><span data-stu-id="d2410-351">**Resolution**: When using Sqoop from an Oozie job, you must include the database driver with the other resources (such as the workflow.xml) used by the job.</span></span> <span data-ttu-id="d2410-352">Také odkazovat obsahující ovladač databáze z archivu `<sqoop>...</sqoop>` části workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="d2410-352">Also Reference the archive containing the database driver from the `<sqoop>...</sqoop>` section of the workflow.xml.</span></span>

<span data-ttu-id="d2410-353">Například by pro úlohu v tomto dokumentu, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d2410-353">For example, for the job in this document, you would use the following steps:</span></span>

1. <span data-ttu-id="d2410-354">Zkopírujte soubor sqljdbc4.1.jar k adresáři /tutorials/useoozie:</span><span class="sxs-lookup"><span data-stu-id="d2410-354">Copy the sqljdbc4.1.jar file to the /tutorials/useoozie directory:</span></span>

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. <span data-ttu-id="d2410-355">Upravit workflow.xml a přidejte následující kód XML na nový řádek výše `</sqoop>`:</span><span class="sxs-lookup"><span data-stu-id="d2410-355">Modify the workflow.xml to add the following XML on a new line above `</sqoop>`:</span></span>

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a><span data-ttu-id="d2410-356">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2410-356">Next steps</span></span>

<span data-ttu-id="d2410-357">V tomto kurzu jste se dozvěděli, jak definovat pracovním postupu Oozie a jak spustit úlohu Oozie.</span><span class="sxs-lookup"><span data-stu-id="d2410-357">In this tutorial, you learned how to define an Oozie workflow and how to run an Oozie job.</span></span> <span data-ttu-id="d2410-358">Další informace o práci s HDInsight, naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="d2410-358">To learn more about working with HDInsight, see the following articles:</span></span>

* <span data-ttu-id="d2410-359">[Použijte založené na čase Oozie Coordinator s HDInsight][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="d2410-359">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="d2410-360">[Nahrání dat pro úlohy Hadoop v HDInsight][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="d2410-360">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="d2410-361">[Použití nástroje Sqoop se systémem Hadoop v HDInsight][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="d2410-361">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="d2410-362">[Použijte Hive s Hadoop v HDInsight][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="d2410-362">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="d2410-363">[Použijte Pig s Hadoop v HDInsight][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="d2410-363">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="d2410-364">[Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="d2410-364">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

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
