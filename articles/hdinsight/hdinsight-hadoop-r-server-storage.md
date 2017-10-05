---
title: "Řešení úložiště Azure pro R serverem v HDInsight - Azure | Microsoft Docs"
description: "Další informace o možnostech jiného úložiště k dispozici uživatelům s R serverem v HDInsight"
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
ms.openlocfilehash: aef9c15636ccaecce07d4fa218a40ed26ebad9df
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a><span data-ttu-id="55146-103">Řešení úložiště Azure pro R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="55146-103">Azure Storage solutions for R Server on HDInsight</span></span>

<span data-ttu-id="55146-104">Microsoft R serverem v HDInsight obsahuje celou řadu řešení úložiště k uchování dat, kódu nebo objektů, které obsahují výsledky z analýzy.</span><span class="sxs-lookup"><span data-stu-id="55146-104">Microsoft R Server on HDInsight has a variety of storage solutions to persist data, code, or objects that contain results from analysis.</span></span> <span data-ttu-id="55146-105">Mezi ně patří následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="55146-105">These include the following options:</span></span>

- [<span data-ttu-id="55146-106">Objekt Blob systému Azure</span><span class="sxs-lookup"><span data-stu-id="55146-106">Azure Blob</span></span>](https://azure.microsoft.com/services/storage/blobs/)
- [<span data-ttu-id="55146-107">Azure Data Lake Storage</span><span class="sxs-lookup"><span data-stu-id="55146-107">Azure Data Lake Storage</span></span>](https://azure.microsoft.com/services/data-lake-store/)
- [<span data-ttu-id="55146-108">Úložiště Azure File</span><span class="sxs-lookup"><span data-stu-id="55146-108">Azure File storage</span></span>](https://azure.microsoft.com/services/storage/files/)

<span data-ttu-id="55146-109">Máte také možnost přístupu k několika účtům Azure storage nebo kontejnerů k vašemu clusteru HDI.</span><span class="sxs-lookup"><span data-stu-id="55146-109">You also have the option of accessing multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="55146-110">Úložiště Azure File je možnost vhodná datová úložiště pro použití na hraniční uzel, který umožňuje připojení Azure úložné sdílené složky, například Linux systému souborů.</span><span class="sxs-lookup"><span data-stu-id="55146-110">Azure File storage is a convenient data storage option for use on the edge node that enables you to mount an Azure Storage file share to, for example, the Linux file system.</span></span> <span data-ttu-id="55146-111">Ale sdílené složky Azure File můžete připojit a používat libovolný systém, který nemá podporovaný operační systém, například Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="55146-111">But Azure File shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> 

<span data-ttu-id="55146-112">Při vytváření clusteru Hadoop v HDInsight, můžete zadat buď **úložiště Azure** účet nebo **úložiště Data Lake store**.</span><span class="sxs-lookup"><span data-stu-id="55146-112">When you create an Hadoop cluster in HDInsight, you specify either an **Azure storage** account or a **Data Lake store**.</span></span> <span data-ttu-id="55146-113">Kontejner konkrétní úložiště z tohoto účtu obsahuje systému souborů pro cluster, který vytvoříte (například Hadoop Distributed File System).</span><span class="sxs-lookup"><span data-stu-id="55146-113">A specific storage container from that account holds the file system for the cluster that you create (for example, the Hadoop Distributed File System).</span></span> <span data-ttu-id="55146-114">Další informace a pokyny najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="55146-114">For more information and guidance, see:</span></span>

- [<span data-ttu-id="55146-115">Používání Azure storage s HDInsight</span><span class="sxs-lookup"><span data-stu-id="55146-115">Use Azure storage with HDInsight</span></span>](hdinsight-hadoop-use-blob-storage.md)
- <span data-ttu-id="55146-116">[Použití Data Lake Store s Azure HDInsight clustery](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="55146-116">[Use Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> 

<span data-ttu-id="55146-117">Další informace o řešení úložiště Azure najdete v tématu [Úvod do Microsoft Azure Storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="55146-117">For more information on the Azure storage solutions, see [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md).</span></span> 

<span data-ttu-id="55146-118">Informace o výběru nejvhodnější možnosti úložiště pro váš scénář, najdete v části [rozhodování o použití objektů BLOB služby Azure, Azure soubory nebo datové disky Azure](../storage/common/storage-decide-blobs-files-disks.md)</span><span class="sxs-lookup"><span data-stu-id="55146-118">For guidance on selecting the most appropriate storage option to use for your scenario, see [Deciding when to use Azure Blobs, Azure Files, or Azure Data Disks](../storage/common/storage-decide-blobs-files-disks.md)</span></span> 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a><span data-ttu-id="55146-119">Účty úložiště objektů Blob v Azure pomocí R Server</span><span class="sxs-lookup"><span data-stu-id="55146-119">Use Azure Blob storage accounts with R Server</span></span>

<span data-ttu-id="55146-120">V případě potřeby můžete přistupovat k vašemu clusteru HDI více účtů úložiště Azure nebo kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="55146-120">If necessary, you can access multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="55146-121">Uděláte to tak, je třeba zadat další úložiště účtů v uživatelském rozhraní, při vytváření clusteru a potom postupujte podle těchto kroků používat s R Server.</span><span class="sxs-lookup"><span data-stu-id="55146-121">To do so, you need to specify the additional storage accounts in the UI when you create the cluster, and then follow these steps to use them with R Server.</span></span>

> [!WARNING]
> <span data-ttu-id="55146-122">Z důvodů výkonu se HDInsight cluster vytvoří ve stejném datovém centru jako účet primárního úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="55146-122">For performance purposes, the HDInsight cluster is created in the same data center as the primary storage account that you specify.</span></span> <span data-ttu-id="55146-123">Použití účtu úložiště v jiném umístění než HDInsight cluster není podporováno.</span><span class="sxs-lookup"><span data-stu-id="55146-123">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span>

1. <span data-ttu-id="55146-124">Vytvoření clusteru HDInsight s názvem účtu úložiště **storage1** výchozí kontejner s názvem **container1**.</span><span class="sxs-lookup"><span data-stu-id="55146-124">Create an HDInsight cluster with a storage account name of **storage1** and a default container called **container1**.</span></span>
2. <span data-ttu-id="55146-125">Zadejte účet úložiště, názvem **storage2**.</span><span class="sxs-lookup"><span data-stu-id="55146-125">Specify an additional storage account called **storage2**.</span></span>  
3. <span data-ttu-id="55146-126">Zkopírujte soubor mycsv.csv do adresáře/Share a provádět analýzy na daný soubor.</span><span class="sxs-lookup"><span data-stu-id="55146-126">Copy the mycsv.csv file to the /share directory, and perform analysis on that file.</span></span>  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. <span data-ttu-id="55146-127">V kódu jazyka R, nastavte na název uzlu **výchozím** a nastavte adresáře a souboru ke zpracování.</span><span class="sxs-lookup"><span data-stu-id="55146-127">In R code, set the name node to **default,** and set your directory and file to process.</span></span>  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

<span data-ttu-id="55146-128">Všechny adresáře a souboru odkazy přejděte na účet úložiště wasb://container1@storage1.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="55146-128">All the directory and file references point to the storage account wasb://container1@storage1.blob.core.windows.net.</span></span> <span data-ttu-id="55146-129">Toto je **výchozí účet úložiště** který je spojen s clusterem HDInsight.</span><span class="sxs-lookup"><span data-stu-id="55146-129">This is the **default storage account** that's associated with the HDInsight cluster.</span></span>

<span data-ttu-id="55146-130">Nyní předpokládejme, že chcete zpracovat soubor s názvem mySpecial.csv, který je umístěný v /private adresář **container2** v **storage2**.</span><span class="sxs-lookup"><span data-stu-id="55146-130">Now, suppose you want to process a file called mySpecial.csv that's located in the  /private directory of **container2** in **storage2**.</span></span>

<span data-ttu-id="55146-131">Ve vašem kódu R bodu název uzlu odkazu na **storage2** účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="55146-131">In your R code, point the name node reference to the **storage2** storage account.</span></span>


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

<span data-ttu-id="55146-132">Všechny adresáře a souboru odkazy nyní přejděte na účet úložiště wasb://container2@storage2.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="55146-132">All of the directory and file references now point to the storage account wasb://container2@storage2.blob.core.windows.net.</span></span> <span data-ttu-id="55146-133">Toto je **název uzlu** který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="55146-133">This is the **Name Node** that you’ve specified.</span></span>

<span data-ttu-id="55146-134">Budete muset nakonfigurovat User/RevoShare/<SSH username> v **storage2** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="55146-134">You have to configure the /user/RevoShare/<SSH username> directory on **storage2** as follows:</span></span>


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a><span data-ttu-id="55146-135">Použití Azure Data Lake store s R Server</span><span class="sxs-lookup"><span data-stu-id="55146-135">Use an Azure Data Lake store with R Server</span></span>

<span data-ttu-id="55146-136">Pokud chcete používat s vaším účtem HDInsight ukládá Data Lake, budete muset poskytnout vašeho clusteru přístup k každý Azure Data Lake store, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="55146-136">To use Data Lake stores with your HDInsight account, you need to give your cluster access to each Azure Data Lake store that you want to use.</span></span> <span data-ttu-id="55146-137">Pokyny o tom, jak pomocí portálu Azure k vytvoření clusteru HDInsight pomocí účtu Azure Data Lake Store jako výchozí úložiště nebo jako další úložiště najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="55146-137">For instructions on how to use the Azure portal to create a HDInsight cluster with an Azure Data Lake Store account as the default storage or as an additional store, see [Create an HDInsight cluster with Data Lake Store using Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

<span data-ttu-id="55146-138">Pak použijete úložišti ve vašem skriptu R mnohem stejně, jako jste účet sekundární úložiště Azure, jak je popsáno v předchozím postupu.</span><span class="sxs-lookup"><span data-stu-id="55146-138">You then use the store in your R script much like you did a secondary Azure storage account as described in the previous procedure.</span></span>

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a><span data-ttu-id="55146-139">Přístup ke clusteru přidat do vašeho úložiště Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="55146-139">Add cluster access to your Azure Data Lake stores</span></span>
<span data-ttu-id="55146-140">Máte přístup k úložiště Data Lake store pomocí objektu služby Azure Active Directory (Azure AD), který je spojen s clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="55146-140">You access a Data Lake store by using an Azure Active Directory (Azure AD) Service Principal that's associated with your HDInsight cluster.</span></span>

<span data-ttu-id="55146-141">Chcete-li přidat objektu služby Azure AD:</span><span class="sxs-lookup"><span data-stu-id="55146-141">To add an Azure AD Service Principal:</span></span>

1. <span data-ttu-id="55146-142">Při vytváření clusteru HDInsight, vyberte **identita AAD clusteru** z **zdroj dat** kartě.</span><span class="sxs-lookup"><span data-stu-id="55146-142">When you create your HDInsight cluster, select **Cluster AAD Identity** from the **Data Source** tab.</span></span>

2. <span data-ttu-id="55146-143">V **identita AAD clusteru** dialogovém **vyberte objekt služby AD**, vyberte **vytvořit nový**.</span><span class="sxs-lookup"><span data-stu-id="55146-143">In the **Cluster AAD Identity** dialog box, under **Select AD Service Principal**, select **Create new**.</span></span>

<span data-ttu-id="55146-144">Po zadejte název objektu služby a vytvořit heslo pro něj, klikněte na tlačítko **Správa přístupu ADLS** instanční objekt přidružit vašeho úložiště Data Lake.</span><span class="sxs-lookup"><span data-stu-id="55146-144">After you give the Service Principal a name and create a password for it, click **Manage ADLS Access** to associate the Service Principal with your Data Lake stores.</span></span>

<span data-ttu-id="55146-145">Je také možné přidat do úložiště Data Lake pro jeden nebo více po vytvoření clusteru přístup ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="55146-145">It’s also possible to add cluster access to one or more Data Lake stores following cluster creation.</span></span> <span data-ttu-id="55146-146">Otevřete položku portál Azure pro Data Lake store a přejděte na **Průzkumníku dat > přístup > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="55146-146">Open the Azure portal entry for a Data Lake store and go to **Data Explorer > Access > Add**.</span></span> 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a><span data-ttu-id="55146-147">Jak získat přístup ze serveru R úložiště Data Lake store</span><span class="sxs-lookup"><span data-stu-id="55146-147">How to access the Data Lake store from R Server</span></span>

<span data-ttu-id="55146-148">Jakmile jste dali přístup do úložiště Data Lake store, můžete použít úložiště v R Server v HDInsight způsob, jakým byste účet sekundární úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="55146-148">Once you’ve given access to a Data Lake store, you can use the store in R Server on HDInsight the way you would a secondary Azure storage account.</span></span> <span data-ttu-id="55146-149">Jediným rozdílem je, že předpona **wasb: / /** změny **adl: / /** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="55146-149">The only difference is that the prefix **wasb://** changes to **adl://** as follows:</span></span>


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of the week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define the data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


<span data-ttu-id="55146-150">Konfigurace účtu úložiště Data Lake s RevoShare adresáře a přidejte ukázkový soubor .csv z předchozího příkladu se používají následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="55146-150">The following commands are used to configure the Data Lake storage account with the RevoShare directory and add the sample .csv file from the previous example:</span></span>


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a><span data-ttu-id="55146-151">Používání Azure File storage s R Server</span><span class="sxs-lookup"><span data-stu-id="55146-151">Use Azure File storage with R Server</span></span>

<span data-ttu-id="55146-152">Je také možnost vhodná datová úložiště pro použití v uzlu edge volat – Azure Files ((https://azure.microsoft.com/services/storage/files/).</span><span class="sxs-lookup"><span data-stu-id="55146-152">There is also a convenient data storage option for use on the edge node called [Azure Files]((https://azure.microsoft.com/services/storage/files/).</span></span> <span data-ttu-id="55146-153">Umožňuje připojit Azure úložné sdílené složky systému souborů Linux.</span><span class="sxs-lookup"><span data-stu-id="55146-153">It enables you to mount an Azure Storage file share to the Linux file system.</span></span> <span data-ttu-id="55146-154">Tato možnost může být užitečný pro ukládání datové soubory, skripty R a objektů výsledků, které může být potřeba později, zejména v případě, že má smysl pro systém nativní souborů na uzlu edge namísto HDFS.</span><span class="sxs-lookup"><span data-stu-id="55146-154">This option can be handy for storing data files, R scripts, and result objects that might be needed later, especially when it makes sense to use the native file system on the edge node rather than HDFS.</span></span> 

<span data-ttu-id="55146-155">Hlavní výhodou soubory Azure je, sdílené složky můžete připojit a používat systémem, který má podporovaný operační systém, například Windows nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="55146-155">A major benefit of Azure Files is that the file shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> <span data-ttu-id="55146-156">Například můžete použít jiný cluster HDInsight s někým ve vašem týmu, virtuální počítač Azure nebo i v místním systému.</span><span class="sxs-lookup"><span data-stu-id="55146-156">For example, it can be used by another HDInsight cluster that you or someone on your team has, by an Azure VM, or even by an on-premises system.</span></span> <span data-ttu-id="55146-157">Další informace naleznete v tématu:</span><span class="sxs-lookup"><span data-stu-id="55146-157">For more information, see:</span></span>

- [<span data-ttu-id="55146-158">Jak používat Azure File Storage s Linuxem</span><span class="sxs-lookup"><span data-stu-id="55146-158">How to use Azure File storage with Linux</span></span>](../storage/files/storage-how-to-use-files-linux.md)
- [<span data-ttu-id="55146-159">Jak používat Azure File storage ve Windows</span><span class="sxs-lookup"><span data-stu-id="55146-159">How to use Azure File storage on Windows</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a><span data-ttu-id="55146-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55146-160">Next steps</span></span>

<span data-ttu-id="55146-161">Teď, když znáte možnosti úložiště Azure, pomocí následujících odkazů ke zjišťování způsob přenosu dat vědecké účely úlohy provádějí s R serverem v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="55146-161">Now that you understand the Azure storage options, use the following links to discover ways of getting data science tasks done with R Server on HDInsight.</span></span>

* [<span data-ttu-id="55146-162">Přehled R serverem v HDInsight</span><span class="sxs-lookup"><span data-stu-id="55146-162">Overview of R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-overview.md)
* [<span data-ttu-id="55146-163">Začínáme s serveru R na Hadoop</span><span class="sxs-lookup"><span data-stu-id="55146-163">Get started with R server on Hadoop</span></span>](hdinsight-hadoop-r-server-get-started.md)
* [<span data-ttu-id="55146-164">Přidání serveru Rstudia do HDInsight (Pokud není při vytváření clusteru přidat)</span><span class="sxs-lookup"><span data-stu-id="55146-164">Add RStudio Server to HDInsight (if not added during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="55146-165">Možnosti výpočetního kontextu pro R Server ve službě HDInsight</span><span class="sxs-lookup"><span data-stu-id="55146-165">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)

