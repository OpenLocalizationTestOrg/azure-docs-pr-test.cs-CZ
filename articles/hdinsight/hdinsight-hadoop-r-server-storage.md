---
title: "aaaAzure řešení úložiště pro R serverem v HDInsight - Azure | Microsoft Docs"
description: "Další informace o dostupných toousers možnosti hello jiného úložiště s R serverem v HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: ff5e80fee14d5e74cbc22e873e6bc1439a3b6037
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Řešení úložiště Azure pro R serverem v HDInsight

Microsoft R serverem v HDInsight obsahuje řadu dat toopersist řešení úložiště, kódu nebo objektů, které obsahují výsledky z analýzy. Mezi ně patří hello následující možnosti:

- [Objekt Blob systému Azure](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/)
- [Úložiště Azure File](https://azure.microsoft.com/services/storage/files/)

Máte také možnost hello přístup k několika účtům Azure storage nebo kontejnerů k vašemu clusteru HDI. Úložiště Azure File je možnost vhodná datová úložiště pro použití v hello hraniční uzel, který vám umožní toomount Azure Storage sdílenou složku, například hello systém souborů Linux. Ale sdílené složky Azure File můžete připojit a používat libovolný systém, který nemá podporovaný operační systém, například Windows nebo Linux. 

Při vytváření clusteru Hadoop v HDInsight, můžete zadat buď **úložiště Azure** účet nebo **úložiště Data Lake store**. Kontejner konkrétní úložiště z tohoto účtu obsahuje hello systému souborů pro hello cluster, který můžete vytvořit (například hello Hadoop Distributed File System). Další informace a pokyny najdete v tématu:

- [Používání Azure storage s HDInsight](hdinsight-hadoop-use-blob-storage.md)
- [Použití Data Lake Store s Azure HDInsight clustery](hdinsight-hadoop-use-data-lake-store.md). 

Další informace o řešení hello úložiště Azure najdete v tématu [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md). 

Informace o výběru hello nejvhodnější úložiště možnost toouse pro váš scénář, najdete v části [rozhodnutí při objektů BLOB Azure toouse, soubory Azure nebo Azure datových disků](../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Účty úložiště objektů Blob v Azure pomocí R Server

V případě potřeby můžete přistupovat k vašemu clusteru HDI více účtů úložiště Azure nebo kontejnerů. toodo, takže byste měli toospecify hello další účty úložiště v hello uživatelského rozhraní při vytváření clusteru hello a potom postupujte podle těchto kroků toouse je s R Server.

> [!WARNING]
> Z důvodů výkonu hello se HDInsight cluster vytvoří v hello stejné datovém centru jako účet hello primárního úložiště, který určíte. Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno.

1. Vytvoření clusteru HDInsight s názvem účtu úložiště **storage1** výchozí kontejner s názvem **container1**.
2. Zadejte účet úložiště, názvem **storage2**.  
3. Zkopírujte adresář/share hello mycsv.csv souboru toohello a provádět analýzy na daný soubor.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. V kódu jazyka R, nastavte název uzlu hello příliš**výchozím** a nastavte tooprocess vašeho adresáře a souboru.  

        myNameNode <- "default"
        myPort <- 0

        #Location of hello data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define hello Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify hello input file tooanalyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Všechny adresáře a souboru účet úložiště toohello bodu odkazy hello wasb://container1@storage1.blob.core.windows.net. Toto je hello **výchozí účet úložiště** který je spojen s hello clusteru HDInsight.

Nyní předpokládejme, že chcete tooprocess soubor nazývá mySpecial.csv, který je umístěný v hello /private adresář **container2** v **storage2**.

Ve vašem kódu R bodu hello název uzlu odkazu toohello **storage2** účet úložiště.


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of hello data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify hello input file tooanalyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Všechny hello adresáře a souboru odkazy nyní bodu toohello účtu úložiště wasb://container2@storage2.blob.core.windows.net. Toto je hello **název uzlu** který jste zadali.

Máte tooconfigure hello User/RevoShare/<SSH username> v **storage2** následujícím způsobem:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Použití Azure Data Lake store s R Server

ukládá toouse Data Lake s vaším účtem HDInsight, musíte toogive vašeho clusteru přístup k tooeach Azure Data Lake store, které chcete toouse. Pokyny, jak toouse hello portálu toocreate Azure HDInsight clusteru pomocí účtu Azure Data Lake Store jako hello výchozí úložiště nebo jako další úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Pak použijete hello úložiště ve vašem skriptu R mnohem stejně, jako jste účet sekundární úložiště Azure, jak je popsáno v předchozím postupu hello.

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a>Přidat přístup tooyour clusteru, které ukládá Azure Data Lake
Máte přístup k úložiště Data Lake store pomocí objektu služby Azure Active Directory (Azure AD), který je spojen s clusteru HDInsight.

tooadd objektu služby Azure AD:

1. Při vytváření clusteru HDInsight, vyberte **identita AAD clusteru** z hello **zdroj dat** kartě.

2. V hello **identita AAD clusteru** dialogovém **vyberte objekt služby AD**, vyberte **vytvořit nový**.

Po pojmenujte hello instanční objekt a vytvořte heslo pro něj, klikněte na tlačítko **Správa přístupu ADLS** tooassociate hello ukládá hlavní název služby s Data Lake.

Je také možné tooadd clusteru přístup tooone nebo další Data Lake ukládá po vytvoření clusteru. Otevřete hello Azure portálu položka pro úložiště Data Lake store a přejděte příliš**Průzkumníku dat > přístup > Přidat**. 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a>Jak tooaccess hello úložiště Data Lake store ze serveru R

Jakmile jste dali přístup k tooa Data Lake store, můžete použít hello úložiště v R Server v HDInsight hello stejným způsobem, jako účet sekundární úložiště Azure. Hello jen rozdílem je, že předpona hello **wasb: / /** změní příliš**adl: / /** následujícím způsobem:


    # Point toohello ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of hello data (assumes a /share directory on hello ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify hello input file in HDFS tooanalyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of hello week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define hello data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


Hello následující příkazy jsou použité tooconfigure hello účet úložiště Data Lake hello RevoShare adresáře a přidejte hello, ukázkový soubor .csv z předchozího příkladu hello:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Používání Azure File storage s R Server

Je také možnost vhodná datová úložiště pro použití v uzlu edge hello volat – Azure Files ((https://azure.microsoft.com/services/storage/files/). Umožní vám toomount toohello sdílené složky souboru Azure Storage souboru systému Linux. Tato možnost může být užitečný pro ukládání datové soubory, skripty R a objektů výsledků, které může být potřeba později, zejména v případě, že má smysl toouse hello nativní systému souborů hello hraniční uzel, místo HDFS. 

Hlavní výhodou soubory Azure je tento soubor hello sdílených složek můžete připojit a používat systémem, který má podporovaný operační systém, například Windows nebo Linux. Například můžete použít jiný cluster HDInsight s někým ve vašem týmu, virtuální počítač Azure nebo i v místním systému. Další informace naleznete v tématu:

- [Jak toouse Azure File storage s Linuxem](../storage/files/storage-how-to-use-files-linux.md)
- [Jak toouse Azure File storage ve Windows](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Další kroky

Teď, když znáte hello možnosti úložiště Azure, použijte následující hello propojí toodiscover způsob přenosu dat vědecké účely úlohy provádějí s R serverem v HDInsight.

* [Přehled R serverem v HDInsight](hdinsight-hadoop-r-server-overview.md)
* [Začínáme s serveru R na Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Přidat Server Rstudia tooHDInsight (Pokud není přidaný při vytváření clusteru)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Možnosti výpočetního kontextu pro R Server ve službě HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)

