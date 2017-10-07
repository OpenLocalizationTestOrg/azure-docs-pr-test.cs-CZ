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
# <a name="use-apache-sqoop-tooimport-and-export-data-between-hadoop-on-hdinsight-and-sql-database"></a>Použití Apache Sqoop tooimport a exportu dat mezi systémem Hadoop v HDInsight a databáze SQL

[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Zjistěte, jak toouse Apache Sqoop tooimport a export mezi Hadoop cluster v Azure HDInsight a databáze Azure SQL Database nebo Microsoft SQL Server. Hello kroky hello použití tohoto dokumentu `sqoop` příkaz přímo z headnode hello clusteru Hadoop hello. Použití SSH tooconnect toohello hlavního uzlu a spusťte příkazy hello v tomto dokumentu.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu fungovat jenom s clustery HDInsight, které používají systém Linux. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="install-freetds"></a>Nainstalujte FreeTDS

1. Použití clusteru HDInsight toohello tooconnect SSH. Například následující příkaz hello připojuje toohello primární headnode clusteru s názvem `mycluster`:

    ```bash
    ssh CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Použijte následující příkaz tooinstall FreeTDS hello:

    ```bash
    sudo apt --assume-yes install freetds-dev freetds-bin
    ```

    FreeTDS se používá v několika kroky tooconnect tooSQL databáze.

## <a name="create-hello-table-in-sql-database"></a>Vytvoření hello tabulky v databázi SQL

> [!IMPORTANT]
> Pokud používáte hello HDInsight cluster a databázi SQL vytvořit v [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md), přeskočte hello kroky v této části. Hello databáze a tabulky byly vytvořeny jako součást hello kroky hello [vytvoření clusteru a databáze SQL](hdinsight-use-sqoop.md) dokumentu.

1. Z relace SSH hello použijte následující příkaz tooconnect toohello databáze SQL server hello.

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Zobrazí se výstup podobný toohello následující text:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toosqooptest
        1>

2. V hello `1>` výzva, zadejte hello následující dotaz:

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

    Když hello `GO` příkaz je zadán, jsou vyhodnocovány hello předchozí příkazy. Nejprve hello **mobiledata** tabulka bude vytvořena a potom clusterovaný index je přidána tooit (vyžadována databáze SQL.)

    Použití hello následující tooverify dotazu, který hello tabulka byla vytvořena:

    ```sql
    SELECT * FROM information_schema.tables
    GO
    ```

    Zobrazí výstup podobný toohello následující text:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE

3. Zadejte `exit` v hello `1>` výzvu tooexit hello tsql nástroj.

## <a name="sqoop-export"></a>Sqoop exportu

1. Z clusteru toohello připojení hello SSH použijte následující příkaz tooverify Sqoop můžete najdete v části SQL Database hello:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> -P
    ```
    Po zobrazení výzvy zadejte heslo hello přihlášení hello databázi SQL.

    Tento příkaz vrátí seznam databází, včetně hello **sqooptest** databáze, kterou jste vytvořili dříve.

2. tooexport data z **hivesampletable** toohello **mobiledata** tabulky, použijte následující příkaz hello:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> -P --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
    ```

    Tento příkaz nastaví Sqoop tooconnect toohello **sqooptest** databáze. Sqoop exportuje data z **wasb: / / / hive nebo skladu/hivesampletable** toohello **mobiledata** tabulky.

    > [!IMPORTANT]
    > Použití `wasb:///` Pokud je účet služby Azure Storage hello výchozí úložiště pro cluster. Použití `adl:///` Pokud je Azure Data Lake Store.

3. Po dokončení příkazu hello použijte následující příkaz tooconnect toohello databázi pomocí TSQL hello:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P -p 1433 -D sqooptest
    ```

    Po připojení hello použijte následující příkazy tooverify, který hello data byla exportovaný toohello **mobiledata** tabulky:

    ```sql
    SET ROWCOUNT 50;
    SELECT * FROM mobiledata
    GO
    ```

    Měli byste vidět seznam data v tabulce hello. Typ `exit` tooexit hello tsql nástroj.

## <a name="sqoop-import"></a>Sqoop import

1. Použití hello následující příkaz tooimport data z hello **mobiledata** tabulky v databázi SQL, toohello **wasb: / / / kurzy/usesqoop/importeddata** v HDInsight:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

    Hello pole v datech hello jsou oddělených tabulátorem a hello řádky se ukončila příkazem znak nového řádku.

2. Po dokončení importu hello použijte následující příkaz toolist hello data v adresáři nové hello hello:

    ```bash
    hdfs dfs -text /tutorials/usesqoop/importeddata/part-m-00000
    ```

## <a name="using-sql-server"></a>Pomocí SQL serveru

Můžete také použít Sqoop tooimport a exportu dat z SQL serveru, buď ve vašem datovém centru, nebo na virtuální počítač hostovaný v Azure. Hello rozdíly mezi použitím SQL Database a SQL Server jsou následující:

* HDInsight a SQL Server musí být na hello stejné virtuální síti Azure.

    Příklad najdete v tématu hello [připojit HDInsight tooyour do místní sítě](./connect-on-premises-network.md) dokumentu.

    Další informace o používání HDInsight s virtuální síť Azure, najdete v části hello [rozšíření prostředí HDInsight pomocí Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentu. Další informace o Azure Virtual Network, najdete v části hello [Přehled virtuálních sítí](../virtual-network/virtual-networks-overview.md) dokumentu.

* SQL Server musí být nakonfigurované tooallow ověřování SQL. Další informace najdete v tématu hello [volba režimu ověřování](https://msdn.microsoft.com/ms144284.aspx) dokumentu.

* Můžete mít tooconfigure systému SQL Server tooaccept vzdálená připojení. Další informace najdete v tématu hello [jak tootroubleshoot připojování toohello systému SQL Server databáze modul](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) dokumentu.

* Vytvoření hello **sqooptest** databáze serveru SQL Server pomocí nástroj, jako třeba **SQL Server Management Studio** nebo **tsql**. Postup Hello použití hello rozhraní příkazového řádku Azure fungovat pouze pro databázi SQL Azure.

    Použití hello následující hello toocreate příkazy jazyka Transact-SQL **mobiledata** tabulky:

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

* Při připojování toohello systému SQL Server z prostředí HDInsight, můžete mít toouse hello IP adresu hello systému SQL Server. Například:

    ```bash
    sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
    ```

## <a name="limitations"></a>Omezení

* Hromadně export - s Linuxovým systémem HDInsight, hello Sqoop konektor používaný tooexport data tooMicrosoft systému SQL Server nebo Azure SQL Database v současné době nepodporuje hromadné vložení.

* Dávkování - s HDInsight se systémem Linux, při použití hello `-batch` přepnout při vložení, Sqoop umožňuje více vloží místo dávkování operace insert hello.

## <a name="next-steps"></a>Další kroky

Nyní jste se naučili, jak toouse Sqoop. toolearn více, najdete v části:

* [Použijte Oozie s HDInsight][hdinsight-use-oozie]: použití Sqoop akce v pracovním postupu Oozie.
* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-data]: použití Hive letu tooanalyze zpoždění data a pak použijte Sqoop tooexport data tooan Azure SQL database.
* [Nahrání dat tooHDInsight][hdinsight-upload-data]: Najít další metody pro odesílání dat tooHDInsight/Azure Blob storage.

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
