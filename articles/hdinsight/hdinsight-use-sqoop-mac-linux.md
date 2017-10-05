---
title: "Apache Sqoop se systémem Hadoop - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití Apache Sqoop k importu a exportu mezi systémem Hadoop v HDInsight a Azure SQL Database."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
keywords: hadoop sqoop, sqoop
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: 35dcbb91e6af1480685c9fd5b829c54277c1c605
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-sqoop-to-import-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a><span data-ttu-id="86d5a-104">Použití Apache Sqoop k importu a exportu dat mezi systémem Hadoop v HDInsight a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="86d5a-104">Use Apache Sqoop to import and export data between Hadoop on HDInsight and SQL Database</span></span>

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="86d5a-105">Další informace o použití Apache Sqoop k importu a exportu mezi clusteru Hadoop v prostředí Azure HDInsight a databáze Azure SQL Database nebo Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="86d5a-105">Learn how to use Apache Sqoop to import and export between a Hadoop cluster in Azure HDInsight and Azure SQL Database or Microsoft SQL Server database.</span></span> <span data-ttu-id="86d5a-106">Kroky v tomto dokumentu pomocí `sqoop` příkaz přímo z headnode clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="86d5a-106">The steps in this document use the `sqoop` command directly from the headnode of the Hadoop cluster.</span></span> <span data-ttu-id="86d5a-107">Použití SSH k připojení k hlavnímu uzlu a spusťte příkazy v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-107">You use SSH to connect to the head node and run the commands in this document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86d5a-108">Kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux.</span><span class="sxs-lookup"><span data-stu-id="86d5a-108">The steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="86d5a-109">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="86d5a-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="86d5a-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="86d5a-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="install-freetds"></a><span data-ttu-id="86d5a-111">Nainstalujte FreeTDS</span><span class="sxs-lookup"><span data-stu-id="86d5a-111">Install FreeTDS</span></span>

1. <span data-ttu-id="86d5a-112">Použití SSH se připojit ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="86d5a-112">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="86d5a-113">Například následující příkaz připojí k primární headnode clusteru s názvem `mycluster`:</span><span class="sxs-lookup"><span data-stu-id="86d5a-113">For example, the following command connects to the primary headnode of a cluster named `mycluster`:</span></span>

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="86d5a-114">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="86d5a-114">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="86d5a-115">Použijte následující příkaz k instalaci FreeTDS:</span><span class="sxs-lookup"><span data-stu-id="86d5a-115">Use the following command to install FreeTDS:</span></span>

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    <span data-ttu-id="86d5a-116">FreeTDS se používá v několika krocích pro připojení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="86d5a-116">FreeTDS is used in several steps to connect to SQL Database.</span></span>

## <a name="create-the-table-in-sql-database"></a><span data-ttu-id="86d5a-117">Vytvořit v tabulce databáze SQL</span><span class="sxs-lookup"><span data-stu-id="86d5a-117">Create the table in SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="86d5a-118">Pokud používáte HDInsight cluster a databázi SQL vytvořit v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md), přeskočte postup v této části.</span><span class="sxs-lookup"><span data-stu-id="86d5a-118">If you are using the HDInsight cluster and SQL Database created in [Create cluster and SQL database](hdinsight-use-sqoop.md), skip the steps in this section.</span></span> <span data-ttu-id="86d5a-119">Databáze a tabulky, které byly vytvořeny jako součást kroky v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-119">The database and table were created as part of the steps in the [Create cluster and SQL database](hdinsight-use-sqoop.md) document.</span></span>

1. <span data-ttu-id="86d5a-120">Z relace SSH použijte následující příkaz pro připojení k databázi SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="86d5a-120">From the SSH session, use the following command to connect to the SQL Database server.</span></span>

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    <span data-ttu-id="86d5a-121">Zobrazí se výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="86d5a-121">You receive output similar to the following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

2. <span data-ttu-id="86d5a-122">Na `1>` výzva, zadejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="86d5a-122">At the `1>` prompt, enter the following query:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
    GO
    ```

    <span data-ttu-id="86d5a-123">Když `GO` příkaz je zadán, jsou vyhodnocovány předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="86d5a-123">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="86d5a-124">Nejdřív **mobiledata** tabulka bude vytvořena a potom clusterovaný index se přidá k němu (vyžadována databáze SQL.)</span><span class="sxs-lookup"><span data-stu-id="86d5a-124">First, the **mobiledata** table is created, then a clustered index is added to it (required by SQL Database.)</span></span>

    <span data-ttu-id="86d5a-125">Chcete-li ověřit, zda byl vytvořen v tabulce použijte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="86d5a-125">Use the following query to verify that the table has been created:</span></span>

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="86d5a-126">Zobrazí výstup podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="86d5a-126">You see output similar to the following text:</span></span>

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. <span data-ttu-id="86d5a-127">Zadejte `exit` na `1>` výzvy ukončete nástroj tsql.</span><span class="sxs-lookup"><span data-stu-id="86d5a-127">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="sqoop-export"></a><span data-ttu-id="86d5a-128">Sqoop exportu</span><span class="sxs-lookup"><span data-stu-id="86d5a-128">Sqoop export</span></span>

1. <span data-ttu-id="86d5a-129">Z připojení SSH do clusteru použijte následující příkaz k ověření, že Sqoop vidí vaší databázi SQL:</span><span class="sxs-lookup"><span data-stu-id="86d5a-129">From the SSH connection to the cluster, use the following command to verify that Sqoop can see your SQL Database:</span></span>

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    <span data-ttu-id="86d5a-130">Po zobrazení výzvy zadejte heslo pro přihlášení k databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="86d5a-130">When prompted, enter the password for the SQL Database login.</span></span>

    <span data-ttu-id="86d5a-131">Tento příkaz vrátí seznam databází, včetně **sqooptest** databáze, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="86d5a-131">This command returns a list of databases, including the **sqooptest** database that you created earlier.</span></span>

2. <span data-ttu-id="86d5a-132">Export dat z **hivesampletable** k **mobiledata** tabulky, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="86d5a-132">To export data from **hivesampletable** to the **mobiledata** table, use the following command:</span></span>

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="86d5a-133">Tento příkaz nastaví Sqoop se připojit k **sqooptest** databáze.</span><span class="sxs-lookup"><span data-stu-id="86d5a-133">This command instructs Sqoop to connect to the **sqooptest** database.</span></span> <span data-ttu-id="86d5a-134">Sqoop exportuje data z **wasb: / / / hive nebo skladu/hivesampletable** k **mobiledata** tabulky.</span><span class="sxs-lookup"><span data-stu-id="86d5a-134">Sqoop then exports data from **wasb:///hive/warehouse/hivesampletable** to the **mobiledata** table.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="86d5a-135">Použití `wasb:///` Pokud výchozí úložiště pro cluster se účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="86d5a-135">Use `wasb:///` if the default storage for your cluster is an Azure Storage account.</span></span> <span data-ttu-id="86d5a-136">Použití `adl:///` Pokud je Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="86d5a-136">Use `adl:///` if it is an Azure Data Lake Store.</span></span>

3. <span data-ttu-id="86d5a-137">Po dokončení příkazu pro připojení k databázi pomocí TSQL použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="86d5a-137">After the command completes, use the following command to connect to the database using TSQL:</span></span>

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    <span data-ttu-id="86d5a-138">Po připojení, použijte následující příkazy k ověření, že se data se exportují do **mobiledata** tabulky:</span><span class="sxs-lookup"><span data-stu-id="86d5a-138">Once connected, use the following statements to verify that the data was exported to the **mobiledata** table:</span></span>

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="86d5a-139">Měli byste vidět seznam dat v tabulce.</span><span class="sxs-lookup"><span data-stu-id="86d5a-139">You should see a listing of data in the table.</span></span> <span data-ttu-id="86d5a-140">Typ `exit` ukončete nástroj tsql.</span><span class="sxs-lookup"><span data-stu-id="86d5a-140">Type `exit` to exit the tsql utility.</span></span>

## <a name="sqoop-import"></a><span data-ttu-id="86d5a-141">Sqoop import</span><span class="sxs-lookup"><span data-stu-id="86d5a-141">Sqoop import</span></span>

1. <span data-ttu-id="86d5a-142">Použijte následující příkaz pro import dat z **mobiledata** do tabulky v databázi SQL, **wasb: / / / kurzy/usesqoop/importeddata** v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="86d5a-142">Use the following command to import data from the **mobiledata** table in SQL Database, to the **wasb:///tutorials/usesqoop/importeddata** directory on HDInsight:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    <span data-ttu-id="86d5a-143">Pole v datech jsou oddělených tabulátorem a řádky se ukončila příkazem znak nového řádku.</span><span class="sxs-lookup"><span data-stu-id="86d5a-143">The fields in the data are separated by a tab character, and the lines are terminated by a new-line character.</span></span>

2. <span data-ttu-id="86d5a-144">Po dokončení importu použijte následující příkaz, který seznamu se data v adresáři nové:</span><span class="sxs-lookup"><span data-stu-id="86d5a-144">Once the import has completed, use the following command to list out the data in the new directory:</span></span>

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a><span data-ttu-id="86d5a-145">Pomocí SQL serveru</span><span class="sxs-lookup"><span data-stu-id="86d5a-145">Using SQL Server</span></span>

<span data-ttu-id="86d5a-146">Můžete taky Sqoop k importu a exportu dat z SQL serveru, buď ve vašem datovém centru, nebo na virtuální počítač hostovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="86d5a-146">You can also use Sqoop to import and export data from SQL Server, either in your data center or on a Virtual Machine hosted in Azure.</span></span> <span data-ttu-id="86d5a-147">Rozdíly mezi použitím SQL Database a SQL Server jsou:</span><span class="sxs-lookup"><span data-stu-id="86d5a-147">The differences between using SQL Database and SQL Server are:</span></span>

* <span data-ttu-id="86d5a-148">HDInsight a SQL Server musí být ve stejné virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="86d5a-148">Both HDInsight and SQL Server must be on the same Azure Virtual Network.</span></span>

    <span data-ttu-id="86d5a-149">Příklad, naleznete v části [HDInsight připojit k místní síti](./connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-149">For an example, see the [Connect HDInsight to your on-premises network](./connect-on-premises-network.md) document.</span></span>

    <span data-ttu-id="86d5a-150">Další informace o používání HDInsight s virtuální síť Azure, najdete v článku [rozšíření prostředí HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-150">For more information on using HDInsight with an Azure Virtual Network, see the [Extend HDInsight with Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span> <span data-ttu-id="86d5a-151">Další informace o Azure Virtual Network, najdete v článku [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-151">For more information on Azure Virtual Network, see the [Virtual Network Overview](../virtual-network/virtual-networks-overview.md) document.</span></span>

* <span data-ttu-id="86d5a-152">SQL Server musí nakonfigurovat pro povolení ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="86d5a-152">SQL Server must be configured to allow SQL authentication.</span></span> <span data-ttu-id="86d5a-153">Další informace najdete v tématu [volba režimu ověřování](https://msdn.microsoft.com/ms144284.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-153">For more information, see the [Choose an Authentication Mode](https://msdn.microsoft.com/ms144284.aspx) document.</span></span>

* <span data-ttu-id="86d5a-154">Budete muset nakonfigurovat SQL Server tak, aby přijímal vzdálená připojení.</span><span class="sxs-lookup"><span data-stu-id="86d5a-154">You may have to configure SQL Server to accept remote connections.</span></span> <span data-ttu-id="86d5a-155">Další informace najdete v tématu [řešení potíží s připojením k databázovému stroji systému SQL Server](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="86d5a-155">For more information, see the [How to troubleshoot connecting to the SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.</span></span>

* <span data-ttu-id="86d5a-156">Vytvořte **sqooptest** databáze serveru SQL Server pomocí nástroj, jako třeba **SQL Server Management Studio** nebo **tsql**.</span><span class="sxs-lookup"><span data-stu-id="86d5a-156">Create the **sqooptest** database in SQL Server using a utility such as **SQL Server Management Studio** or **tsql**.</span></span> <span data-ttu-id="86d5a-157">Kroky pro použití Azure CLI fungovat pouze pro databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="86d5a-157">The steps for using the Azure CLI only work for Azure SQL Database.</span></span>

    <span data-ttu-id="86d5a-158">Pomocí následujících příkazů Transact-SQL můžete vytvořit **mobiledata** tabulky:</span><span class="sxs-lookup"><span data-stu-id="86d5a-158">Use the following Transact-SQL statements to create the **mobiledata** table:</span></span>

    ```sql
    CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder] [bigint])
    ```

* <span data-ttu-id="86d5a-159">Při připojování k systému SQL Server z prostředí HDInsight, budete muset použít IP adresu serveru SQL Server.</span><span class="sxs-lookup"><span data-stu-id="86d5a-159">When connecting to the SQL Server from HDInsight, you may have to use the IP address of the SQL Server.</span></span> <span data-ttu-id="86d5a-160">Například:</span><span class="sxs-lookup"><span data-stu-id="86d5a-160">For example:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a><span data-ttu-id="86d5a-161">Omezení</span><span class="sxs-lookup"><span data-stu-id="86d5a-161">Limitations</span></span>

* <span data-ttu-id="86d5a-162">Hromadné export - s Linuxovým systémem HDInsight, Sqoop konektor umožňuje exportovat data do systému Microsoft SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="86d5a-162">Bulk export - With Linux-based HDInsight, the Sqoop connector used to export data to Microsoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>

* <span data-ttu-id="86d5a-163">Dávkování - s HDInsight se systémem Linux, při použití `-batch` přepnout při vložení, Sqoop umožňuje více vloží místo dávkování operace insert.</span><span class="sxs-lookup"><span data-stu-id="86d5a-163">Batching - With Linux-based HDInsight, When using the `-batch` switch when performing inserts, Sqoop makes multiple inserts instead of batching the insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="86d5a-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="86d5a-164">Next steps</span></span>

<span data-ttu-id="86d5a-165">Nyní jste se naučili postup použití nástroje Sqoop.</span><span class="sxs-lookup"><span data-stu-id="86d5a-165">Now you have learned how to use Sqoop.</span></span> <span data-ttu-id="86d5a-166">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="86d5a-166">To learn more, see:</span></span>

* <span data-ttu-id="86d5a-167">[Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.</span><span class="sxs-lookup"><span data-stu-id="86d5a-167">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="86d5a-168">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive k analýze letu zpoždění dat a pak pomocí Sqoop exportovat data do Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="86d5a-168">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive to analyze flight delay data, and then use Sqoop to export data to an Azure SQL database.</span></span>
* <span data-ttu-id="86d5a-169">[Nahrání dat do HDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat do HDInsight nebo Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="86d5a-169">[Upload data to HDInsight][hdinsight-upload-data]: Find other methods for uploading data to HDInsight/Azure Blob storage.</span></span>

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
