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
# <a name="copy-data-between-data-lake-store-and-azure-sql-database-using-sqoop"></a>Kopírovat data mezi Data Lake Store a Azure SQL database pomocí Sqoop
Zjistěte, jak dat tooimport a export Apache Sqoop toouse mezi Azure SQL Database a Data Lake Store.

## <a name="what-is-sqoop"></a>Co je Sqoop?
Velké objemy dat aplikací jsou přirozené volbou pro zpracování nestrukturovaných a částečně strukturovaných dat, jako jsou protokoly a soubory. Však může také existovat nutnost tooprocess strukturovaná data uložená v relačních databází.

[Apache Sqoop](https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html) je tootransfer nástroj navržený dat mezi relačních databází a velké objemy dat úložiště, jako je například Data Lake Store. Můžete ho tooimport data ze systému správy relačních databází (RDBMS), jako je například databáze SQL Azure do Data Lake Store. Můžete pak transformuje a analyzovat data hello pomocí úloh velkých objemů dat a poté exportovat hello data zpět do relační. V tomto kurzu použijete jako tooimport/export relační databáze z databáze SQL Azure.

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto článku, musíte mít následující hello:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store. V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Tento článek předpokládá, že máte cluster HDInsight Linux s přístupem k Data Lake Store.
* **Azure SQL Database**. Návod, jak jeden, viz toocreate [vytvoření databáze Azure SQL](../sql-database/sql-database-get-started.md)

## <a name="do-you-learn-fast-with-videos"></a>Pomáhají vám při učení videa?
[Přehrát toto video](https://mix.office.com/watch/1butcdjxmu114) o toocopy dat mezi objektů BLOB služby Azure Storage a Data Lake Store pomocí DistCp.

## <a name="create-sample-tables-in-hello-azure-sql-database"></a>Vytvoření ukázkové tabulky v hello Azure SQL Database
1. toostart, vytvořte dva ukázkové tabulky v hello Azure SQL Database. Použití [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio tooconnect toohello databáze SQL Azure a pak spusťte hello následující dotazy.

    **Vytvoření tabulky1**

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

    **Vytvoření tabulky2**

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
2. V **tabulky1**, přidejte ukázková data. Nechte **tabulky2** prázdný. Budeme importovat data z **tabulky1** do Data Lake Store. Potom bude exportu dat z Data Lake Store do **tabulky2**. Spusťte hello následující fragment kódu.

        INSERT INTO [dbo].[Table1] VALUES (1,'Neal','Kell'), (2,'Lila','Fulton'), (3, 'Erna','Myers'), (4,'Annette','Simpson');


## <a name="use-sqoop-from-an-hdinsight-cluster-with-access-toodata-lake-store"></a>Použití Sqoop z clusteru služby HDInsight s přístup tooData Lake Store
Cluster služby HDInsight je již hello Sqoop balíčky k dispozici. Pokud jste nakonfigurovali hello HDInsight clusteru toouse Data Lake Store jako další úložiště, můžete použít Sqoop (bez změny konfigurace) tooimport a exportu dat mezi relační databáze (v tomto příkladu Azure SQL Database) a Data Lake Store účet.

1. V tomto kurzu předpokládáme, že jste vytvořili Linux cluster, měli byste použít SSH tooconnect toohello clusteru. V tématu [clusteru HDInsight se systémem Linux tooa Connect](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).
2. Ověřte, zda máte přístup z clusteru hello hello účtu Data Lake Store. Spusťte následující příkaz z řádku SSH hello hello:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net/

    To by mělo poskytovat seznam souborů a složek v účtu Data Lake Store hello.

### <a name="import-data-from-azure-sql-database-into-data-lake-store"></a>Importovat data z databáze SQL Azure do Data Lake Store
1. Přejděte toohello adresáře, kde jsou Sqoop balíčky k dispozici. Obvykle to bude v `/usr/hdp/<version>/sqoop/bin`.
2. Umožňuje importovat data hello z **tabulky1** do hello účtu Data Lake Store. Hello použijte následující syntaxi:

        sqoop-import --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table1 --target-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1

    Všimněte si, že **-název databáze sql-server** zástupný symbol představuje název hello hello serveru se spuštěným systémem hello Azure SQL database. **Název databáze SQL** zástupný symbol představuje název skutečné databáze hello.

    Například:


        sqoop-import --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table1 --target-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1

1. Ověřte, že hello data byla přenesena toohello účtu Data Lake Store. Spusťte následující příkaz hello:

        hdfs dfs -ls adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/

    Měli byste vidět následující výstup hello.


        -rwxrwxrwx   0 sshuser hdfs          0 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/_SUCCESS
        -rwxrwxrwx   0 sshuser hdfs         12 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00000
        -rwxrwxrwx   0 sshuser hdfs         14 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00001
        -rwxrwxrwx   0 sshuser hdfs         13 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00002
        -rwxrwxrwx   0 sshuser hdfs         18 2016-02-26 21:09 adl://hdiadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1/part-m-00003

    Každý **část-m -*** soubor odpovídá tooa řádků ve zdrojové tabulce hello, **tabulky1**. Můžete zobrazit obsah hello části hello - m-* soubory tooverify.


### <a name="export-data-from-data-lake-store-into-azure-sql-database"></a>Export dat z Data Lake Store do Azure SQL Database
1. Export hello dat z Data Lake Store účtu toohello prázdná tabulka, **tabulky2**, v hello Azure SQL Database. Pomocí následující syntaxe hello.

        sqoop-export --connect "jdbc:sqlserver://<sql-database-server-name>.database.windows.net:1433;username=<username>@<sql-database-server-name>;password=<password>;database=<sql-database-name>" --table Table2 --export-dir adl://<data-lake-store-name>.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

    Například:


        sqoop-export --connect "jdbc:sqlserver://mysqoopserver.database.windows.net:1433;username=nitinme@mysqoopserver;password=<password>;database=mysqoopdatabase" --table Table2 --export-dir adl://myadlstore.azuredatalakestore.net/Sqoop/SqoopImportTable1 --input-fields-terminated-by ","

1. Ověřte, že tento hello data byla načtený toohello tabulka databáze SQL. Použití [SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md) nebo Visual Studio tooconnect toohello databáze SQL Azure a pak spusťte hello následující dotaz.

        SELECT * FROM TABLE2

    To by měl mít následující výstup hello.

         ID  FName   LName
        ------------------
        1    Neal    Kell
        2    Lila    Fulton
        3    Erna    Myers
        4    Annette    Simpson

## <a name="performance-considerations-while-using-sqoop"></a>Faktory ovlivňující výkon při použití Sqoop

Ladění vaší Sqoop výkonu úlohy toocopy data tooData Lake Store, najdete v části [Sqoop výkonu dokumentu](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/).

## <a name="see-also"></a>Viz také
* [Kopírování dat z Azure úložiště objektů BLOB tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md)
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
