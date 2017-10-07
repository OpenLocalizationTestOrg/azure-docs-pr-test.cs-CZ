---
title: "aaaApache Sqoop se systémem Hadoop - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse Apache Sqoop tooimport a export mezi systémem Hadoop v HDInsight a Azure SQL Database."
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
ms.openlocfilehash: b256285659bbcf18ff05e220ccdf51c21eb8fbf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a><span data-ttu-id="0b359-104">Použití Apache Sqoop tooimport a exportu dat mezi systémem Hadoop v HDInsight a databáze SQL</span><span class="sxs-lookup"><span data-stu-id="0b359-104">Use Apache Sqoop tooimport and export data between Hadoop on HDInsight and SQL Database</span></span>

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

<span data-ttu-id="0b359-105">Zjistěte, jak toouse Apache Sqoop tooimport a export mezi Hadoop cluster v Azure HDInsight a databáze Azure SQL Database nebo Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b359-105">Learn how toouse Apache Sqoop tooimport and export between a Hadoop cluster in Azure HDInsight and Azure SQL Database or Microsoft SQL Server database.</span></span> <span data-ttu-id="0b359-106">Hello kroky hello použití tohoto dokumentu `sqoop` příkaz přímo z headnode hello clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="0b359-106">hello steps in this document use hello `sqoop` command directly from hello headnode of hello Hadoop cluster.</span></span> <span data-ttu-id="0b359-107">Použití SSH tooconnect toohello hlavního uzlu a spusťte příkazy hello v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-107">You use SSH tooconnect toohello head node and run hello commands in this document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b359-108">Hello kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux.</span><span class="sxs-lookup"><span data-stu-id="0b359-108">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="0b359-109">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="0b359-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0b359-110">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0b359-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="install-freetds"></a><span data-ttu-id="0b359-111">Nainstalujte FreeTDS</span><span class="sxs-lookup"><span data-stu-id="0b359-111">Install FreeTDS</span></span>

1. <span data-ttu-id="0b359-112">Použití clusteru HDInsight toohello tooconnect SSH.</span><span class="sxs-lookup"><span data-stu-id="0b359-112">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="0b359-113">Například následující příkaz hello připojuje toohello primární headnode clusteru s názvem `mycluster`:</span><span class="sxs-lookup"><span data-stu-id="0b359-113">For example, hello following command connects toohello primary headnode of a cluster named `mycluster`:</span></span>

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="0b359-114">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0b359-114">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="0b359-115">Použijte následující příkaz tooinstall FreeTDS hello:</span><span class="sxs-lookup"><span data-stu-id="0b359-115">Use hello following command tooinstall FreeTDS:</span></span>

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    <span data-ttu-id="0b359-116">FreeTDS se používá v několika kroky tooconnect tooSQL databáze.</span><span class="sxs-lookup"><span data-stu-id="0b359-116">FreeTDS is used in several steps tooconnect tooSQL Database.</span></span>

## <a name="create-hello-table-in-sql-database"></a><span data-ttu-id="0b359-117">Vytvoření hello tabulky v databázi SQL</span><span class="sxs-lookup"><span data-stu-id="0b359-117">Create hello table in SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b359-118">Pokud používáte hello HDInsight cluster a databázi SQL vytvořit v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md), přeskočte hello kroky v této části.</span><span class="sxs-lookup"><span data-stu-id="0b359-118">If you are using hello HDInsight cluster and SQL Database created in [Create cluster and SQL database](hdinsight-use-sqoop.md), skip hello steps in this section.</span></span> <span data-ttu-id="0b359-119">Hello databáze a tabulky byly vytvořeny jako součást hello kroky hello [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-119">hello database and table were created as part of hello steps in hello [Create cluster and SQL database](hdinsight-use-sqoop.md) document.</span></span>

1. <span data-ttu-id="0b359-120">Z relace SSH hello použijte následující příkaz tooconnect toohello databáze SQL server hello.</span><span class="sxs-lookup"><span data-stu-id="0b359-120">From hello SSH session, use hello following command tooconnect toohello SQL Database server.</span></span>

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    <span data-ttu-id="0b359-121">Zobrazí se výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="0b359-121">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. <span data-ttu-id="0b359-122">V hello `1>` výzva, zadejte hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="0b359-122">At hello `1>` prompt, enter hello following query:</span></span>

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

    <span data-ttu-id="0b359-123">Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy.</span><span class="sxs-lookup"><span data-stu-id="0b359-123">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="0b359-124">Nejprve hello **mobiledata** tabulka bude vytvořena a potom clusterovaný index je přidána tooit (vyžadována databáze SQL.)</span><span class="sxs-lookup"><span data-stu-id="0b359-124">First, hello **mobiledata** table is created, then a clustered index is added tooit (required by SQL Database.)</span></span>

    <span data-ttu-id="0b359-125">Použití hello následující tooverify dotazu, který hello tabulka byla vytvořena:</span><span class="sxs-lookup"><span data-stu-id="0b359-125">Use hello following query tooverify that hello table has been created:</span></span>

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="0b359-126">Zobrazí výstup podobný toohello následující text:</span><span class="sxs-lookup"><span data-stu-id="0b359-126">You see output similar toohello following text:</span></span>

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. <span data-ttu-id="0b359-127">Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.</span><span class="sxs-lookup"><span data-stu-id="0b359-127">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="sqoop-export"></a><span data-ttu-id="0b359-128">Sqoop exportu</span><span class="sxs-lookup"><span data-stu-id="0b359-128">Sqoop export</span></span>

1. <span data-ttu-id="0b359-129">Z clusteru toohello připojení hello SSH použijte následující příkaz tooverify Sqoop můžete najdete v části SQL Database hello:</span><span class="sxs-lookup"><span data-stu-id="0b359-129">From hello SSH connection toohello cluster, use hello following command tooverify that Sqoop can see your SQL Database:</span></span>

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    <span data-ttu-id="0b359-130">Po zobrazení výzvy zadejte heslo hello přihlášení hello databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="0b359-130">When prompted, enter hello password for hello SQL Database login.</span></span>

    <span data-ttu-id="0b359-131">Tento příkaz vrátí seznam databází, včetně hello **sqooptest** databáze, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="0b359-131">This command returns a list of databases, including hello **sqooptest** database that you created earlier.</span></span>

2. <span data-ttu-id="0b359-132">tooexport data z **hivesampletable** toohello **mobiledata** tabulky, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="0b359-132">tooexport data from **hivesampletable** toohello **mobiledata** table, use hello following command:</span></span>

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    <span data-ttu-id="0b359-133">Tento příkaz nastaví Sqoop tooconnect toohello **sqooptest** databáze.</span><span class="sxs-lookup"><span data-stu-id="0b359-133">This command instructs Sqoop tooconnect toohello **sqooptest** database.</span></span> <span data-ttu-id="0b359-134">Sqoop exportuje data z **wasb: / / / hive nebo skladu/hivesampletable** toohello **mobiledata** tabulky.</span><span class="sxs-lookup"><span data-stu-id="0b359-134">Sqoop then exports data from **wasb:///hive/warehouse/hivesampletable** toohello **mobiledata** table.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0b359-135">Použití `wasb:///` Pokud je účet služby Azure Storage hello výchozí úložiště pro cluster.</span><span class="sxs-lookup"><span data-stu-id="0b359-135">Use `wasb:///` if hello default storage for your cluster is an Azure Storage account.</span></span> <span data-ttu-id="0b359-136">Použití `adl:///` Pokud je Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0b359-136">Use `adl:///` if it is an Azure Data Lake Store.</span></span>

3. <span data-ttu-id="0b359-137">Po dokončení příkazu hello použijte následující příkaz tooconnect toohello databázi pomocí TSQL hello:</span><span class="sxs-lookup"><span data-stu-id="0b359-137">After hello command completes, use hello following command tooconnect toohello database using TSQL:</span></span>

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    <span data-ttu-id="0b359-138">Po připojení hello použijte následující příkazy tooverify, který hello data byla exportovaný toohello **mobiledata** tabulky:</span><span class="sxs-lookup"><span data-stu-id="0b359-138">Once connected, use hello following statements tooverify that hello data was exported toohello **mobiledata** table:</span></span>

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="0b359-139">Měli byste vidět seznam data v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="0b359-139">You should see a listing of data in hello table.</span></span> <span data-ttu-id="0b359-140">Typ `exit` tooexit hello tsql nástroj.</span><span class="sxs-lookup"><span data-stu-id="0b359-140">Type `exit` tooexit hello tsql utility.</span></span>

## <a name="sqoop-import"></a><span data-ttu-id="0b359-141">Sqoop import</span><span class="sxs-lookup"><span data-stu-id="0b359-141">Sqoop import</span></span>

1. <span data-ttu-id="0b359-142">Použití hello následující příkaz tooimport data z hello **mobiledata** tabulky v databázi SQL, toohello **wasb: / / / kurzy/usesqoop/importeddata** v HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0b359-142">Use hello following command tooimport data from hello **mobiledata** table in SQL Database, toohello **wasb:///tutorials/usesqoop/importeddata** directory on HDInsight:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    <span data-ttu-id="0b359-143">Hello pole v datech hello jsou oddělených tabulátorem a hello řádky se ukončila příkazem znak nového řádku.</span><span class="sxs-lookup"><span data-stu-id="0b359-143">hello fields in hello data are separated by a tab character, and hello lines are terminated by a new-line character.</span></span>

2. <span data-ttu-id="0b359-144">Po dokončení importu hello použijte následující příkaz toolist hello data v adresáři nové hello hello:</span><span class="sxs-lookup"><span data-stu-id="0b359-144">Once hello import has completed, use hello following command toolist out hello data in hello new directory:</span></span>

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a><span data-ttu-id="0b359-145">Pomocí SQL serveru</span><span class="sxs-lookup"><span data-stu-id="0b359-145">Using SQL Server</span></span>

<span data-ttu-id="0b359-146">Můžete také použít Sqoop tooimport a exportu dat z SQL serveru, buď ve vašem datovém centru, nebo na virtuální počítač hostovaný v Azure.</span><span class="sxs-lookup"><span data-stu-id="0b359-146">You can also use Sqoop tooimport and export data from SQL Server, either in your data center or on a Virtual Machine hosted in Azure.</span></span> <span data-ttu-id="0b359-147">Hello rozdíly mezi použitím SQL Database a SQL Server jsou následující:</span><span class="sxs-lookup"><span data-stu-id="0b359-147">hello differences between using SQL Database and SQL Server are:</span></span>

* <span data-ttu-id="0b359-148">HDInsight a SQL Server musí být na hello stejné virtuální síti Azure.</span><span class="sxs-lookup"><span data-stu-id="0b359-148">Both HDInsight and SQL Server must be on hello same Azure Virtual Network.</span></span>

    <span data-ttu-id="0b359-149">Příklad najdete v tématu hello [připojit HDInsight tooyour do místní sítě](./connect-on-premises-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-149">For an example, see hello [Connect HDInsight tooyour on-premises network](./connect-on-premises-network.md) document.</span></span>

    <span data-ttu-id="0b359-150">Další informace o používání HDInsight s virtuální síť Azure, najdete v části hello [rozšíření prostředí HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-150">For more information on using HDInsight with an Azure Virtual Network, see hello [Extend HDInsight with Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span> <span data-ttu-id="0b359-151">Další informace o Azure Virtual Network, najdete v části hello [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-151">For more information on Azure Virtual Network, see hello [Virtual Network Overview](../virtual-network/virtual-networks-overview.md) document.</span></span>

* <span data-ttu-id="0b359-152">SQL Server musí být nakonfigurované tooallow ověřování SQL.</span><span class="sxs-lookup"><span data-stu-id="0b359-152">SQL Server must be configured tooallow SQL authentication.</span></span> <span data-ttu-id="0b359-153">Další informace najdete v tématu hello [volba režimu ověřování](https://msdn.microsoft.com/ms144284.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-153">For more information, see hello [Choose an Authentication Mode](https://msdn.microsoft.com/ms144284.aspx) document.</span></span>

* <span data-ttu-id="0b359-154">Můžete mít tooconfigure systému SQL Server tooaccept vzdálená připojení.</span><span class="sxs-lookup"><span data-stu-id="0b359-154">You may have tooconfigure SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="0b359-155">Další informace najdete v tématu hello [jak tootroubleshoot připojování toohello systému SQL Server databáze modul](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="0b359-155">For more information, see hello [How tootroubleshoot connecting toohello SQL Server database engine](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) document.</span></span>

* <span data-ttu-id="0b359-156">Vytvoření hello **sqooptest** databáze serveru SQL Server pomocí nástroj, jako třeba **SQL Server Management Studio** nebo **tsql**.</span><span class="sxs-lookup"><span data-stu-id="0b359-156">Create hello **sqooptest** database in SQL Server using a utility such as **SQL Server Management Studio** or **tsql**.</span></span> <span data-ttu-id="0b359-157">Postup Hello použití hello rozhraní příkazového řádku Azure fungovat pouze pro databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="0b359-157">hello steps for using hello Azure CLI only work for Azure SQL Database.</span></span>

    <span data-ttu-id="0b359-158">Použití hello následující hello toocreate příkazy jazyka Transact-SQL **mobiledata** tabulky:</span><span class="sxs-lookup"><span data-stu-id="0b359-158">Use hello following Transact-SQL statements toocreate hello **mobiledata** table:</span></span>

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

* <span data-ttu-id="0b359-159">Při připojování toohello systému SQL Server z prostředí HDInsight, můžete mít toouse hello IP adresu hello systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b359-159">When connecting toohello SQL Server from HDInsight, you may have toouse hello IP address of hello SQL Server.</span></span> <span data-ttu-id="0b359-160">Například:</span><span class="sxs-lookup"><span data-stu-id="0b359-160">For example:</span></span>

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a><span data-ttu-id="0b359-161">Omezení</span><span class="sxs-lookup"><span data-stu-id="0b359-161">Limitations</span></span>

* <span data-ttu-id="0b359-162">Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.</span><span class="sxs-lookup"><span data-stu-id="0b359-162">Bulk export - With Linux-based HDInsight, hello Sqoop connector used tooexport data tooMicrosoft SQL Server or Azure SQL Database does not currently support bulk inserts.</span></span>

* <span data-ttu-id="0b359-163">Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop umožňuje více vloží místo dávkování operace insert hello.</span><span class="sxs-lookup"><span data-stu-id="0b359-163">Batching - With Linux-based HDInsight, When using hello `-batch` switch when performing inserts, Sqoop makes multiple inserts instead of batching hello insert operations.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b359-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0b359-164">Next steps</span></span>

<span data-ttu-id="0b359-165">Nyní jste se naučili, jak toouse Sqoop.</span><span class="sxs-lookup"><span data-stu-id="0b359-165">Now you have learned how toouse Sqoop.</span></span> <span data-ttu-id="0b359-166">toolearn více, najdete v části:</span><span class="sxs-lookup"><span data-stu-id="0b359-166">toolearn more, see:</span></span>

* <span data-ttu-id="0b359-167">[Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.</span><span class="sxs-lookup"><span data-stu-id="0b359-167">[Use Oozie with HDInsight][hdinsight-use-oozie]: Use Sqoop action in an Oozie workflow.</span></span>
* <span data-ttu-id="0b359-168">[Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive letu tooanalyze zpoždění data a pak použijte Sqoop tooexport data tooan Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="0b359-168">[Analyze flight delay data using HDInsight][hdinsight-analyze-flight-data]: Use Hive tooanalyze flight delay data, and then use Sqoop tooexport data tooan Azure SQL database.</span></span>
* <span data-ttu-id="0b359-169">[Nahrání dat tooHDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat tooHDInsight/Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0b359-169">[Upload data tooHDInsight][hdinsight-upload-data]: Find other methods for uploading data tooHDInsight/Azure Blob storage.</span></span>

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
