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
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a><span data-ttu-id="81cd4-103">Řešení úložiště Azure pro R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="81cd4-103">Azure Storage solutions for R Server on HDInsight</span></span>

<span data-ttu-id="81cd4-104">Microsoft R serverem v HDInsight obsahuje řadu dat toopersist řešení úložiště, kódu nebo objektů, které obsahují výsledky z analýzy.</span><span class="sxs-lookup"><span data-stu-id="81cd4-104">Microsoft R Server on HDInsight has a variety of storage solutions toopersist data, code, or objects that contain results from analysis.</span></span> <span data-ttu-id="81cd4-105">Mezi ně patří hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="81cd4-105">These include hello following options:</span></span>

- [<span data-ttu-id="81cd4-106">Objekt Blob systému Azure</span><span class="sxs-lookup"><span data-stu-id="81cd4-106">Azure Blob</span></span>](https://azure.microsoft.com/services/storage/blobs/)
- [<span data-ttu-id="81cd4-107">Azure Data Lake Storage</span><span class="sxs-lookup"><span data-stu-id="81cd4-107">Azure Data Lake Storage</span></span>](https://azure.microsoft.com/services/data-lake-store/)
- [<span data-ttu-id="81cd4-108">Úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="81cd4-108">Azure File storage</span></span>](https://azure.microsoft.com/services/storage/files/)

<span data-ttu-id="81cd4-109">Máte také možnost hello přístup k několika účtům Azure storage nebo kontejnerů k vašemu clusteru HDI.</span><span class="sxs-lookup"><span data-stu-id="81cd4-109">You also have hello option of accessing multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="81cd4-110">Úložiště Azure File je možnost vhodná datová úložiště pro použití v hello hraniční uzel, který vám umožní toomount Azure Storage sdílenou složku, například hello systém souborů Linux.</span><span class="sxs-lookup"><span data-stu-id="81cd4-110">Azure File storage is a convenient data storage option for use on hello edge node that enables you toomount an Azure Storage file share to, for example, hello Linux file system.</span></span> <span data-ttu-id="81cd4-111">Ale sdílené složky Azure File můžete připojit a používat libovolný systém, který nemá podporovaný operační systém, například Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="81cd4-111">But Azure File shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> 

<span data-ttu-id="81cd4-112">Při vytváření clusteru Hadoop v HDInsight, můžete zadat buď **úložiště Azure** účet nebo **úložiště Data Lake store**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-112">When you create an Hadoop cluster in HDInsight, you specify either an **Azure storage** account or a **Data Lake store**.</span></span> <span data-ttu-id="81cd4-113">Kontejner konkrétní úložiště z tohoto účtu obsahuje hello systému souborů pro hello cluster, který můžete vytvořit (například hello Hadoop Distributed File System).</span><span class="sxs-lookup"><span data-stu-id="81cd4-113">A specific storage container from that account holds hello file system for hello cluster that you create (for example, hello Hadoop Distributed File System).</span></span> <span data-ttu-id="81cd4-114">Další informace a pokyny najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="81cd4-114">For more information and guidance, see:</span></span>

- [<span data-ttu-id="81cd4-115">Používání Azure storage s HDInsight</span><span class="sxs-lookup"><span data-stu-id="81cd4-115">Use Azure storage with HDInsight</span></span>](hdinsight-hadoop-use-blob-storage.md)
- <span data-ttu-id="81cd4-116">[Použití Data Lake Store s Azure HDInsight clustery](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="81cd4-116">[Use Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> 

<span data-ttu-id="81cd4-117">Další informace o řešení hello úložiště Azure najdete v tématu [tooMicrosoft Úvod Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="81cd4-117">For more information on hello Azure storage solutions, see [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span> 

<span data-ttu-id="81cd4-118">Informace o výběru hello nejvhodnější úložiště možnost toouse pro váš scénář, najdete v části [rozhodnutí při objektů BLOB Azure toouse, soubory Azure nebo Azure datových disků](../storage/common/storage-decide-blobs-files-disks.md)</span><span class="sxs-lookup"><span data-stu-id="81cd4-118">For guidance on selecting hello most appropriate storage option toouse for your scenario, see [Deciding when toouse Azure Blobs, Azure Files, or Azure Data Disks](../storage/common/storage-decide-blobs-files-disks.md)</span></span> 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a><span data-ttu-id="81cd4-119">Účty úložiště objektů Blob v Azure pomocí R Server</span><span class="sxs-lookup"><span data-stu-id="81cd4-119">Use Azure Blob storage accounts with R Server</span></span>

<span data-ttu-id="81cd4-120">V případě potřeby můžete přistupovat k vašemu clusteru HDI více účtů úložiště Azure nebo kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="81cd4-120">If necessary, you can access multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="81cd4-121">toodo, takže byste měli toospecify hello další účty úložiště v hello uživatelského rozhraní při vytváření clusteru hello a potom postupujte podle těchto kroků toouse je s R Server.</span><span class="sxs-lookup"><span data-stu-id="81cd4-121">toodo so, you need toospecify hello additional storage accounts in hello UI when you create hello cluster, and then follow these steps toouse them with R Server.</span></span>

> [!WARNING]
> <span data-ttu-id="81cd4-122">Z důvodů výkonu hello se HDInsight cluster vytvoří v hello stejné datovém centru jako účet hello primárního úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="81cd4-122">For performance purposes, hello HDInsight cluster is created in hello same data center as hello primary storage account that you specify.</span></span> <span data-ttu-id="81cd4-123">Použití účtu úložiště v jiném umístění než hello HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="81cd4-123">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

1. <span data-ttu-id="81cd4-124">Vytvoření clusteru HDInsight s názvem účtu úložiště **storage1** výchozí kontejner s názvem **container1**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-124">Create an HDInsight cluster with a storage account name of **storage1** and a default container called **container1**.</span></span>
2. <span data-ttu-id="81cd4-125">Zadejte účet úložiště, názvem **storage2**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-125">Specify an additional storage account called **storage2**.</span></span>  
3. <span data-ttu-id="81cd4-126">Zkopírujte adresář/share hello mycsv.csv souboru toohello a provádět analýzy na daný soubor.</span><span class="sxs-lookup"><span data-stu-id="81cd4-126">Copy hello mycsv.csv file toohello /share directory, and perform analysis on that file.</span></span>  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. <span data-ttu-id="81cd4-127">V kódu jazyka R, nastavte název uzlu hello příliš**výchozím** a nastavte tooprocess vašeho adresáře a souboru.</span><span class="sxs-lookup"><span data-stu-id="81cd4-127">In R code, set hello name node too**default,** and set your directory and file tooprocess.</span></span>  

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

<span data-ttu-id="81cd4-128">Všechny adresáře a souboru účet úložiště toohello bodu odkazy hello wasb://container1@storage1.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="81cd4-128">All hello directory and file references point toohello storage account wasb://container1@storage1.blob.core.windows.net.</span></span> <span data-ttu-id="81cd4-129">Toto je hello **výchozí účet úložiště** který je spojen s hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81cd4-129">This is hello **default storage account** that's associated with hello HDInsight cluster.</span></span>

<span data-ttu-id="81cd4-130">Nyní předpokládejme, že chcete tooprocess soubor nazývá mySpecial.csv, který je umístěný v hello /private adresář **container2** v **storage2**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-130">Now, suppose you want tooprocess a file called mySpecial.csv that's located in hello  /private directory of **container2** in **storage2**.</span></span>

<span data-ttu-id="81cd4-131">Ve vašem kódu R bodu hello název uzlu odkazu toohello **storage2** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="81cd4-131">In your R code, point hello name node reference toohello **storage2** storage account.</span></span>


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

<span data-ttu-id="81cd4-132">Všechny hello adresáře a souboru odkazy nyní bodu toohello účtu úložiště wasb://container2@storage2.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="81cd4-132">All of hello directory and file references now point toohello storage account wasb://container2@storage2.blob.core.windows.net.</span></span> <span data-ttu-id="81cd4-133">Toto je hello **název uzlu** který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="81cd4-133">This is hello **Name Node** that you’ve specified.</span></span>

<span data-ttu-id="81cd4-134">Máte tooconfigure hello User/RevoShare/<SSH username> v **storage2** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="81cd4-134">You have tooconfigure hello /user/RevoShare/<SSH username> directory on **storage2** as follows:</span></span>


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a><span data-ttu-id="81cd4-135">Použití Azure Data Lake store s R Server</span><span class="sxs-lookup"><span data-stu-id="81cd4-135">Use an Azure Data Lake store with R Server</span></span>

<span data-ttu-id="81cd4-136">ukládá toouse Data Lake s vaším účtem HDInsight, musíte toogive vašeho clusteru přístup k tooeach Azure Data Lake store, které chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="81cd4-136">toouse Data Lake stores with your HDInsight account, you need toogive your cluster access tooeach Azure Data Lake store that you want toouse.</span></span> <span data-ttu-id="81cd4-137">Pokyny, jak toouse hello portálu toocreate Azure HDInsight clusteru pomocí účtu Azure Data Lake Store jako hello výchozí úložiště nebo jako další úložiště, najdete v části [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="81cd4-137">For instructions on how toouse hello Azure portal toocreate a HDInsight cluster with an Azure Data Lake Store account as hello default storage or as an additional store, see [Create an HDInsight cluster with Data Lake Store using Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

<span data-ttu-id="81cd4-138">Pak použijete hello úložiště ve vašem skriptu R mnohem stejně, jako jste účet sekundární úložiště Azure, jak je popsáno v předchozím postupu hello.</span><span class="sxs-lookup"><span data-stu-id="81cd4-138">You then use hello store in your R script much like you did a secondary Azure storage account as described in hello previous procedure.</span></span>

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a><span data-ttu-id="81cd4-139">Přidat přístup tooyour clusteru, které ukládá Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="81cd4-139">Add cluster access tooyour Azure Data Lake stores</span></span>
<span data-ttu-id="81cd4-140">Máte přístup k úložiště Data Lake store pomocí objektu služby Azure Active Directory (Azure AD), který je spojen s clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81cd4-140">You access a Data Lake store by using an Azure Active Directory (Azure AD) Service Principal that's associated with your HDInsight cluster.</span></span>

<span data-ttu-id="81cd4-141">tooadd objektu služby Azure AD:</span><span class="sxs-lookup"><span data-stu-id="81cd4-141">tooadd an Azure AD Service Principal:</span></span>

1. <span data-ttu-id="81cd4-142">Při vytváření clusteru HDInsight, vyberte **identita AAD clusteru** z hello **zdroj dat** kartě.</span><span class="sxs-lookup"><span data-stu-id="81cd4-142">When you create your HDInsight cluster, select **Cluster AAD Identity** from hello **Data Source** tab.</span></span>

2. <span data-ttu-id="81cd4-143">V hello **identita AAD clusteru** dialogovém **vyberte objekt služby AD**, vyberte **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-143">In hello **Cluster AAD Identity** dialog box, under **Select AD Service Principal**, select **Create new**.</span></span>

<span data-ttu-id="81cd4-144">Po pojmenujte hello instanční objekt a vytvořte heslo pro něj, klikněte na tlačítko **Správa přístupu ADLS** tooassociate hello ukládá hlavní název služby s Data Lake.</span><span class="sxs-lookup"><span data-stu-id="81cd4-144">After you give hello Service Principal a name and create a password for it, click **Manage ADLS Access** tooassociate hello Service Principal with your Data Lake stores.</span></span>

<span data-ttu-id="81cd4-145">Je také možné tooadd clusteru přístup tooone nebo další Data Lake ukládá po vytvoření clusteru.</span><span class="sxs-lookup"><span data-stu-id="81cd4-145">It’s also possible tooadd cluster access tooone or more Data Lake stores following cluster creation.</span></span> <span data-ttu-id="81cd4-146">Otevřete hello Azure portálu položka pro úložiště Data Lake store a přejděte příliš**Průzkumníku dat > přístup > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="81cd4-146">Open hello Azure portal entry for a Data Lake store and go too**Data Explorer > Access > Add**.</span></span> 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a><span data-ttu-id="81cd4-147">Jak tooaccess hello úložiště Data Lake store ze serveru R</span><span class="sxs-lookup"><span data-stu-id="81cd4-147">How tooaccess hello Data Lake store from R Server</span></span>

<span data-ttu-id="81cd4-148">Jakmile jste dali přístup k tooa Data Lake store, můžete použít hello úložiště v R Server v HDInsight hello stejným způsobem, jako účet sekundární úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="81cd4-148">Once you’ve given access tooa Data Lake store, you can use hello store in R Server on HDInsight hello way you would a secondary Azure storage account.</span></span> <span data-ttu-id="81cd4-149">Hello jen rozdílem je, že předpona hello **wasb: / /** změní příliš**adl: / /** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="81cd4-149">hello only difference is that hello prefix **wasb://** changes too**adl://** as follows:</span></span>


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


<span data-ttu-id="81cd4-150">Hello následující příkazy jsou použité tooconfigure hello účet úložiště Data Lake hello RevoShare adresáře a přidejte hello, ukázkový soubor .csv z předchozího příkladu hello:</span><span class="sxs-lookup"><span data-stu-id="81cd4-150">hello following commands are used tooconfigure hello Data Lake storage account with hello RevoShare directory and add hello sample .csv file from hello previous example:</span></span>


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a><span data-ttu-id="81cd4-151">Používání Azure File storage s R Server</span><span class="sxs-lookup"><span data-stu-id="81cd4-151">Use Azure File storage with R Server</span></span>

<span data-ttu-id="81cd4-152">Je také možnost vhodná datová úložiště pro použití v uzlu edge hello volat – Azure Files ((https://azure.microsoft.com/services/storage/files/).</span><span class="sxs-lookup"><span data-stu-id="81cd4-152">There is also a convenient data storage option for use on hello edge node called [Azure Files]((https://azure.microsoft.com/services/storage/files/).</span></span> <span data-ttu-id="81cd4-153">Umožní vám toomount toohello sdílené složky souboru Azure Storage souboru systému Linux.</span><span class="sxs-lookup"><span data-stu-id="81cd4-153">It enables you toomount an Azure Storage file share toohello Linux file system.</span></span> <span data-ttu-id="81cd4-154">Tato možnost může být užitečný pro ukládání datové soubory, skripty R a objektů výsledků, které může být potřeba později, zejména v případě, že má smysl toouse hello nativní systému souborů hello hraniční uzel, místo HDFS.</span><span class="sxs-lookup"><span data-stu-id="81cd4-154">This option can be handy for storing data files, R scripts, and result objects that might be needed later, especially when it makes sense toouse hello native file system on hello edge node rather than HDFS.</span></span> 

<span data-ttu-id="81cd4-155">Hlavní výhodou soubory Azure je tento soubor hello sdílených složek můžete připojit a používat systémem, který má podporovaný operační systém, například Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="81cd4-155">A major benefit of Azure Files is that hello file shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> <span data-ttu-id="81cd4-156">Například můžete použít jiný cluster HDInsight s někým ve vašem týmu, virtuální počítač Azure nebo i v místním systému.</span><span class="sxs-lookup"><span data-stu-id="81cd4-156">For example, it can be used by another HDInsight cluster that you or someone on your team has, by an Azure VM, or even by an on-premises system.</span></span> <span data-ttu-id="81cd4-157">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="81cd4-157">For more information, see:</span></span>

- [<span data-ttu-id="81cd4-158">Jak toouse Azure File storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="81cd4-158">How toouse Azure File storage with Linux</span></span>](../storage/files/storage-how-to-use-files-linux.md)
- [<span data-ttu-id="81cd4-159">Jak toouse Azure File storage ve Windows</span><span class="sxs-lookup"><span data-stu-id="81cd4-159">How toouse Azure File storage on Windows</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a><span data-ttu-id="81cd4-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="81cd4-160">Next steps</span></span>

<span data-ttu-id="81cd4-161">Teď, když znáte hello možnosti úložiště Azure, použijte následující hello propojí toodiscover způsob přenosu dat vědecké účely úlohy provádějí s R serverem v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81cd4-161">Now that you understand hello Azure storage options, use hello following links toodiscover ways of getting data science tasks done with R Server on HDInsight.</span></span>

* [<span data-ttu-id="81cd4-162">Přehled R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="81cd4-162">Overview of R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-overview.md)
* [<span data-ttu-id="81cd4-163">Začínáme s serveru R na Hadoop</span><span class="sxs-lookup"><span data-stu-id="81cd4-163">Get started with R server on Hadoop</span></span>](hdinsight-hadoop-r-server-get-started.md)
* [<span data-ttu-id="81cd4-164">Přidat Server Rstudia tooHDInsight (Pokud není přidaný při vytváření clusteru)</span><span class="sxs-lookup"><span data-stu-id="81cd4-164">Add RStudio Server tooHDInsight (if not added during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="81cd4-165">Možnosti výpočetního kontextu pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="81cd4-165">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)

