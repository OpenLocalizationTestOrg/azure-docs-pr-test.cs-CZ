---
title: "aaaCopy dat mezi Data Lake Store a Azure SQL database pomocí Sqoop | Microsoft Docs"
description: "Použít Sqoop toocopy dat mezi Azure SQL Database a Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 3f914b2a-83cc-4950-b3f7-69c921851683
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: f58483455f0ebe9544673a1d5c5884f2721c800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a><span data-ttu-id="81893-103">Kopírovat data mezi Data Lake Store a Azure SQL database pomocí Sqoop</span><span class="sxs-lookup"><span data-stu-id="81893-103">Copy data between Data Lake Store and Azure SQL database using Sqoop</span></span>
<span data-ttu-id="81893-104">Zjistěte, jak dat tooimport a export Apache Sqoop toouse mezi Azure SQL Database a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-104">Learn how toouse Apache Sqoop tooimport and export data between Azure SQL Database and Data Lake Store.</span></span>

## <a name="what-is-sqoop"></a><span data-ttu-id="81893-105">Co je Sqoop?</span><span class="sxs-lookup"><span data-stu-id="81893-105">What is Sqoop?</span></span>
<span data-ttu-id="81893-106">Velké objemy dat aplikací jsou přirozené volbou pro zpracování nestrukturovaných a částečně strukturovaných dat, jako jsou protokoly a soubory.</span><span class="sxs-lookup"><span data-stu-id="81893-106">Big data applications are a natural choice for processing unstructured and semi-structured data, such as logs and files.</span></span> <span data-ttu-id="81893-107">Však může také existovat nutnost tooprocess strukturovaná data uložená v relačních databází.</span><span class="sxs-lookup"><span data-stu-id="81893-107">However, there may also be a need tooprocess structured data that is stored in relational databases.</span></span>

<span data-ttu-id="81893-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) je tootransfer nástroj navržený dat mezi relačních databází a velké objemy dat úložiště, jako je například Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-108">[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) is a tool designed tootransfer data between  relational databases and a big data repository, such as Data Lake Store.</span></span> <span data-ttu-id="81893-109">Můžete ho tooimport data ze systému správy relačních databází (RDBMS), jako je například databáze SQL Azure do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-109">You can use it tooimport data from a relational database management system (RDBMS) such as Azure SQL Database into Data Lake Store.</span></span> <span data-ttu-id="81893-110">Můžete pak transformuje a analyzovat data hello pomocí úloh velkých objemů dat a poté exportovat hello data zpět do relační.</span><span class="sxs-lookup"><span data-stu-id="81893-110">You can then transform and analyze hello data using big data workloads and then export hello data back into an RDBMS.</span></span> <span data-ttu-id="81893-111">V tomto kurzu použijete jako tooimport/export relační databáze z databáze SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="81893-111">In this tutorial, you use an Azure SQL Database as your relational database tooimport/export from.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81893-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="81893-112">Prerequisites</span></span>
<span data-ttu-id="81893-113">Před zahájením tohoto článku, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="81893-113">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="81893-114">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="81893-114">**An Azure subscription**.</span></span> <span data-ttu-id="81893-115">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="81893-115">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="81893-116">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="81893-116">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="81893-117">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="81893-117">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="81893-118">**Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-118">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="81893-119">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="81893-119">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="81893-120">Tento článek předpokládá, že máte cluster HDInsight Linux s přístupem k Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-120">This article assumes you have an HDInsight Linux cluster with Data Lake Store access.</span></span>
* <span data-ttu-id="81893-121">**Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="81893-121">**Azure SQL Database**.</span></span> <span data-ttu-id="81893-122">Návod, jak jeden, viz toocreate [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="81893-122">For instructions on how toocreate one, see [Create an Azure SQL database](../sql-database/sql-database-get-started.md)</span></span>

## <a name="do-you-learn-fast-with-videos"></a><span data-ttu-id="81893-123">Pomáhají vám při učení videa?</span><span class="sxs-lookup"><span data-stu-id="81893-123">Do you learn fast with videos?</span></span>
<span data-ttu-id="81893-124">[Přehrát toto video](https://mix.office.com/watch/1butcdjxmu114) o toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store pomocí DistCp.</span><span class="sxs-lookup"><span data-stu-id="81893-124">[Watch this video](https://mix.office.com/watch/1butcdjxmu114) on how toocopy data between Azure Storage Blobs and Data Lake Store using DistCp.</span></span>

## <a name="create-sample-tables-in-hello-azure-sql-database"></a><span data-ttu-id="81893-125">Vytvoření ukázkové tabulky v hello Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="81893-125">Create sample tables in hello Azure SQL Database</span></span>
1. <span data-ttu-id="81893-126">toostart, vytvořte dva ukázkové tabulky v hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="81893-126">toostart with, create two sample tables in hello Azure SQL Database.</span></span> <span data-ttu-id="81893-127">Použití [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio tooconnect toohello databáze SQL Azure a pak spusťte hello následující dotazy.</span><span class="sxs-lookup"><span data-stu-id="81893-127">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following queries.</span></span>

    <span data-ttu-id="81893-128">**Vytvoření tabulky1**</span><span class="sxs-lookup"><span data-stu-id="81893-128">**Create Table1**</span></span>

        CREATE TABLE [dbo].[Table1](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO

    <span data-ttu-id="81893-129">**Vytvoření tabulky2**</span><span class="sxs-lookup"><span data-stu-id="81893-129">**Create Table2**</span></span>

        CREATE TABLE [dbo].[Table2](
        [ID] [int] NOT NULL,
        [FName] [nvarchar](50) NOT NULL,
        [LName] [nvarchar](50) NOT NULL,
         CONSTRAINT [PK_Table_4] PRIMARY KEY CLUSTERED
            (
                   [ID] ASC
            )
        ) ON [PRIMARY]
        GO
2. <span data-ttu-id="81893-130">V **tabulky1**, přidejte ukázková data.</span><span class="sxs-lookup"><span data-stu-id="81893-130">In **Table1**, add some sample data.</span></span> <span data-ttu-id="81893-131">Nechte **tabulky2** prázdný.</span><span class="sxs-lookup"><span data-stu-id="81893-131">Leave **Table2** empty.</span></span> <span data-ttu-id="81893-132">Budeme importovat data z **tabulky1** do Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-132">We will import data from **Table1** into Data Lake Store.</span></span> <span data-ttu-id="81893-133">Potom bude exportu dat z Data Lake Store do **tabulky2**.</span><span class="sxs-lookup"><span data-stu-id="81893-133">Then, we will export data from Data Lake Store into **Table2**.</span></span> <span data-ttu-id="81893-134">Spusťte hello následující fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="81893-134">Run hello following snippet.</span></span>

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a><span data-ttu-id="81893-135">Použití Sqoop z clusteru služby HDInsight s přístup tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-135">Use Sqoop from an HDInsight cluster with access tooData Lake Store</span></span>
<span data-ttu-id="81893-136">Cluster služby HDInsight je již hello Sqoop balíčky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81893-136">An HDInsight cluster already has hello Sqoop packages available.</span></span> <span data-ttu-id="81893-137">Pokud jste nakonfigurovali hello HDInsight clusteru toouse Data Lake Store jako další úložiště, můžete použít Sqoop (bez změny konfigurace) tooimport a exportu dat mezi relační databáze (v tomto příkladu Azure SQL Database) a Data Lake Store účet.</span><span class="sxs-lookup"><span data-stu-id="81893-137">If you have configured hello HDInsight cluster toouse Data Lake Store as an additional storage, you can use Sqoop (without any configuration changes) tooimport/export data between a relational database (in this example, Azure SQL Database) and a Data Lake Store account.</span></span>

1. <span data-ttu-id="81893-138">V tomto kurzu předpokládáme, že jste vytvořili Linux cluster, měli byste použít SSH tooconnect toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="81893-138">For this tutorial, we assume you created a Linux cluster so you should use SSH tooconnect toohello cluster.</span></span> <span data-ttu-id="81893-139">V tématu [clusteru HDInsight se systémem Linux tooa Connect](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="81893-139">See [Connect tooa Linux-based HDInsight cluster](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
2. <span data-ttu-id="81893-140">Ověřte, zda máte přístup z clusteru hello hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-140">Verify whether you can access hello Data Lake Store account from hello cluster.</span></span> <span data-ttu-id="81893-141">Spusťte následující příkaz z řádku SSH hello hello:</span><span class="sxs-lookup"><span data-stu-id="81893-141">Run hello following command from hello SSH prompt:</span></span>

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    <span data-ttu-id="81893-142">To by mělo poskytovat seznam souborů a složek v účtu Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="81893-142">This should provide a list of files/folders in hello Data Lake Store account.</span></span>

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a><span data-ttu-id="81893-143">Importovat data z databáze SQL Azure do Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-143">Import data from Azure SQL Database into Data Lake Store</span></span>
1. <span data-ttu-id="81893-144">Přejděte toohello adresáře, kde jsou Sqoop balíčky k dispozici.</span><span class="sxs-lookup"><span data-stu-id="81893-144">Navigate toohello directory where Sqoop packages are available.</span></span> <span data-ttu-id="81893-145">Obvykle to bude v `/usr/hdp/<version>/sqoop/bin`.</span><span class="sxs-lookup"><span data-stu-id="81893-145">Typically, this will be at `/usr/hdp/<version>/sqoop/bin`.</span></span>
2. <span data-ttu-id="81893-146">Umožňuje importovat data hello z **tabulky1** do hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-146">Import hello data from **Table1** into hello Data Lake Store account.</span></span> <span data-ttu-id="81893-147">Hello použijte následující syntaxi:</span><span class="sxs-lookup"><span data-stu-id="81893-147">Use hello following syntax:</span></span>

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    <span data-ttu-id="81893-148">Všimněte si, že **-název databáze sql-server** zástupný symbol představuje název hello hello serveru se spuštěným systémem hello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="81893-148">Note that **sql-database-server-name** placeholder represents hello name of hello server where hello Azure SQL database is running.</span></span> <span data-ttu-id="81893-149">**Název databáze SQL** zástupný symbol představuje název skutečné databáze hello.</span><span class="sxs-lookup"><span data-stu-id="81893-149">**sql-database-name** placeholder represents hello actual database name.</span></span>

    <span data-ttu-id="81893-150">Například:</span><span class="sxs-lookup"><span data-stu-id="81893-150">For example,</span></span>


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. <span data-ttu-id="81893-151">Ověřte, že hello data byla přenesena toohello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="81893-151">Verify that hello data has been transferred toohello Data Lake Store account.</span></span> <span data-ttu-id="81893-152">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="81893-152">Run hello following command:</span></span>

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    <span data-ttu-id="81893-153">Měli byste vidět následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="81893-153">You should see hello following output.</span></span>


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    <span data-ttu-id="81893-154">Každý **část-m -*** soubor odpovídá tooa řádků ve zdrojové tabulce hello, **tabulky1**.</span><span class="sxs-lookup"><span data-stu-id="81893-154">Each **part-m-*** file corresponds tooa row in hello source table, **Table1**.</span></span> <span data-ttu-id="81893-155">Můžete zobrazit obsah hello části hello - m-* soubory tooverify.</span><span class="sxs-lookup"><span data-stu-id="81893-155">You can view hello contents of hello part-m-* files tooverify.</span></span>


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a><span data-ttu-id="81893-156">Export dat z Data Lake Store do Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="81893-156">Export data from Data Lake Store into Azure SQL Database</span></span>
1. <span data-ttu-id="81893-157">Export hello dat z Data Lake Store účtu toohello prázdná tabulka, **tabulky2**, v hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="81893-157">Export hello data from Data Lake Store account toohello empty table, **Table2**, in hello Azure SQL Database.</span></span> <span data-ttu-id="81893-158">Pomocí následující syntaxe hello.</span><span class="sxs-lookup"><span data-stu-id="81893-158">Use hello following syntax.</span></span>

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    <span data-ttu-id="81893-159">Například:</span><span class="sxs-lookup"><span data-stu-id="81893-159">For example,</span></span>


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. <span data-ttu-id="81893-160">Ověřte, že tento hello data byla načtený toohello tabulka databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="81893-160">Verify that hello data was uploaded toohello SQL Database table.</span></span> <span data-ttu-id="81893-161">Použití [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio tooconnect toohello databáze SQL Azure a pak spusťte hello následující dotaz.</span><span class="sxs-lookup"><span data-stu-id="81893-161">Use [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) or Visual Studio tooconnect toohello Azure SQL Database and then run hello following query.</span></span>

        SELECT * FROM TABLE2

    <span data-ttu-id="81893-162">To by měl mít následující výstup hello.</span><span class="sxs-lookup"><span data-stu-id="81893-162">This should have hello following output.</span></span>

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a><span data-ttu-id="81893-163">Faktory ovlivňující výkon při použití Sqoop</span><span class="sxs-lookup"><span data-stu-id="81893-163">Performance considerations while using Sqoop</span></span>

<span data-ttu-id="81893-164">Ladění vaší Sqoop výkonu úlohy toocopy data tooData Lake Store, najdete v části [Sqoop výkonu dokumentu](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span><span class="sxs-lookup"><span data-stu-id="81893-164">For performance tuning your Sqoop job toocopy data tooData Lake Store, see [Sqoop performance document](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).</span></span>

## <a name="see-also"></a><span data-ttu-id="81893-165">Viz také</span><span class="sxs-lookup"><span data-stu-id="81893-165">See also</span></span>
* [<span data-ttu-id="81893-166">Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-166">Copy data from Azure Storage Blobs tooData Lake Store</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
* [<span data-ttu-id="81893-167">Zabezpečení dat ve službě Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-167">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="81893-168">Použití Azure Data Lake Analytics se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-168">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="81893-169">Použití Azure HDInsight se službou Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="81893-169">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
