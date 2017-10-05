---
title: "Přesun dat do nebo z Azure Blob Storage pomocí služby SSIS konektory | Microsoft Docs"
description: "Přesun dat do nebo z Azure Blob Storage pomocí služby SSIS konektory."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 575beaea5443919bd9728016bf100b43de8e4aab
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a><span data-ttu-id="2e95c-103">Přesun dat do nebo z Azure Blob Storage pomocí služby SSIS konektory</span><span class="sxs-lookup"><span data-stu-id="2e95c-103">Move data to or from Azure Blob Storage using SSIS connectors</span></span>
<span data-ttu-id="2e95c-104">[SQL Server Integration Services Feature Pack pro Azure](https://msdn.microsoft.com/library/mt146770.aspx) poskytuje součásti pro připojení k Azure, přenos dat mezi Azure a místní zdroje dat a zpracování dat uložených v Azure.</span><span class="sxs-lookup"><span data-stu-id="2e95c-104">The [SQL Server Integration Services Feature Pack for Azure](https://msdn.microsoft.com/library/mt146770.aspx) provides components to connect to Azure, transfer data between Azure and on-premises data sources, and process data stored in Azure.</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

<span data-ttu-id="2e95c-105">Jakmile zákazníci byl přesunut místní data do cloudu, můžete přístup z libovolnou službu Azure využít potenciál sada Azure technologií.</span><span class="sxs-lookup"><span data-stu-id="2e95c-105">Once customers have moved on-premises data into the cloud, they can access it from any Azure service to leverage the full power of the suite of Azure technologies.</span></span> <span data-ttu-id="2e95c-106">Se může použít, například v Azure Machine Learning, nebo v clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2e95c-106">It may be used, for example, in Azure Machine Learning or on an HDInsight cluster.</span></span>

<span data-ttu-id="2e95c-107">To je obvykle být prvním krokem pro [SQL](machine-learning-data-science-process-sql-walkthrough.md) a [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) návody.</span><span class="sxs-lookup"><span data-stu-id="2e95c-107">This is typically be the first step for the [SQL](machine-learning-data-science-process-sql-walkthrough.md) and [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) walkthroughs.</span></span>

<span data-ttu-id="2e95c-108">Informace kanonický scénářů, které využívají služby SSIS k dosažení obchodních potřeb, které jsou běžné při hybridní scénáře integrace dat, naleznete v [to více s SQL Server Integration Services Feature Pack pro Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogu.</span><span class="sxs-lookup"><span data-stu-id="2e95c-108">For a discussion of canonical scenarios that use SSIS to accomplish business needs common in hybrid data integration scenarios, see [Doing more with SQL Server Integration Services Feature Pack for Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blog.</span></span>

> [!NOTE]
> <span data-ttu-id="2e95c-109">Dokončení Úvod do Azure blob storage, najdete v části [základy objektu Blob Azure](../storage/blobs/storage-dotnet-how-to-use-blobs.md) a [služby objektů Blob Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e95c-109">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="2e95c-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="2e95c-110">Prerequisites</span></span>
<span data-ttu-id="2e95c-111">K provedení úlohy popsané v tomto článku, musíte mít předplatné Azure a účet úložiště Azure nastavit.</span><span class="sxs-lookup"><span data-stu-id="2e95c-111">To perform the tasks described in this article, you must have an Azure subscription and an Azure storage account set up.</span></span> <span data-ttu-id="2e95c-112">Je nutné znát váš klíč účtu úložiště Azure název a účet k odeslání nebo stažení data.</span><span class="sxs-lookup"><span data-stu-id="2e95c-112">You must know your Azure storage account name and account key to upload or download data.</span></span>

* <span data-ttu-id="2e95c-113">Nastavit **předplatného Azure**, najdete v části [bezplatnou zkušební verzi jeden měsíc](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2e95c-113">To set up an **Azure subscription**, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="2e95c-114">Pokyny k vytváření **účet úložiště** a získání účtu a klíč, najdete v části [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="2e95c-114">For instructions on creating a **storage account** and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

<span data-ttu-id="2e95c-115">Použít **SSIS konektory**, je nutné stáhnout:</span><span class="sxs-lookup"><span data-stu-id="2e95c-115">To use the **SSIS connectors**, you must download:</span></span>

* <span data-ttu-id="2e95c-116">**SQL Server 2014 nebo 2016 Standard (nebo novější)**: instalace zahrnuje SQL Server Integration Services.</span><span class="sxs-lookup"><span data-stu-id="2e95c-116">**SQL Server 2014 or 2016 Standard (or above)**: Install includes SQL Server Integration Services.</span></span>
* <span data-ttu-id="2e95c-117">**Microsoft SQL Server 2014 nebo 2016 Integration Services Feature Pack pro Azure**: tyto soubory můžete stáhnout, v uvedeném pořadí, z [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) a [SQL serveru 2016 integrace Služby](https://www.microsoft.com/download/details.aspx?id=49492) stránky.</span><span class="sxs-lookup"><span data-stu-id="2e95c-117">**Microsoft SQL Server 2014 or 2016 Integration Services Feature Pack for Azure**: These can be downloaded, respectively, from the [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) and [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) pages.</span></span>

> [!NOTE]
> <span data-ttu-id="2e95c-118">Služby SSIS je nainstalovaná se systémem SQL Server, ale není zahrnut ve verzi Express.</span><span class="sxs-lookup"><span data-stu-id="2e95c-118">SSIS is installed with SQL Server, but is not included in the Express version.</span></span> <span data-ttu-id="2e95c-119">Informace, na které aplikace jsou zahrnuty v různých edicích systému SQL Server najdete v tématu [edice serveru SQL](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)</span><span class="sxs-lookup"><span data-stu-id="2e95c-119">For information on what applications are included in various editions of SQL Server, see [SQL Server Editions](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)</span></span>
> 
> 

<span data-ttu-id="2e95c-120">Výukové materiály v SSIS, najdete v části [rukou na školení pro SSIS](http://www.microsoft.com/download/details.aspx?id=20766)</span><span class="sxs-lookup"><span data-stu-id="2e95c-120">For training materials on SSIS, see [Hands On Training for SSIS](http://www.microsoft.com/download/details.aspx?id=20766)</span></span>

<span data-ttu-id="2e95c-121">Informace o tom, jak získat nahoru a spuštění použití SISS k sestavení jednoduchá extrakce, transformace a načítání (ETL) balíčky, viz [SSIS kurz: vytvoření jednoduchého balíčku ETL](https://msdn.microsoft.com/library/ms169917.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e95c-121">For information on how to get up-and-running using SISS to build simple extraction, transformation, and load (ETL) packages, see [SSIS Tutorial: Creating a Simple ETL Package](https://msdn.microsoft.com/library/ms169917.aspx).</span></span>

## <a name="download-nyc-taxi-dataset"></a><span data-ttu-id="2e95c-122">Stáhnout NYC taxíkem datové sady</span><span class="sxs-lookup"><span data-stu-id="2e95c-122">Download NYC Taxi dataset</span></span>
<span data-ttu-id="2e95c-123">Příklad popsané tady použít veřejně dostupné datovou sadu – [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="2e95c-123">The example described here use a publicly available dataset -- the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="2e95c-124">Datová sada se skládá z o 173 milionů taxíkem lyžovačku v NYC roku 2013.</span><span class="sxs-lookup"><span data-stu-id="2e95c-124">The dataset consists of about 173 million taxi rides in NYC in the year 2013.</span></span> <span data-ttu-id="2e95c-125">Existují dva typy dat: cestě podrobnosti dat a tarif data.</span><span class="sxs-lookup"><span data-stu-id="2e95c-125">There are two types of data: trip details data and fare data.</span></span> <span data-ttu-id="2e95c-126">Protože soubor pro každý měsíc, máme 24 soubory ve všech, z nichž každý je přibližně 2GB nekomprimovaným.</span><span class="sxs-lookup"><span data-stu-id="2e95c-126">As there is a file for each month, we have 24 files in all, each of which is approximately 2GB uncompressed.</span></span>

## <a name="upload-data-to-azure-blob-storage"></a><span data-ttu-id="2e95c-127">Nahrání dat do Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="2e95c-127">Upload data to Azure blob storage</span></span>
<span data-ttu-id="2e95c-128">Pro přesun dat pomocí služby SSIS balíček z místního do Azure blob storage funkcí, můžeme použít instanci systému [ **úloh nahrát objekt Blob Azure**](https://msdn.microsoft.com/library/mt146776.aspx), znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="2e95c-128">To move data using the SSIS feature pack from on-premises to Azure blob storage, we use an instance of the [**Azure Blob Upload Task**](https://msdn.microsoft.com/library/mt146776.aspx), shown here:</span></span>

![Konfigurace data-vědecké účely vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

<span data-ttu-id="2e95c-130">Parametry, které používá úlohy jsou popsány zde:</span><span class="sxs-lookup"><span data-stu-id="2e95c-130">The parameters that the task uses are described here:</span></span>

| <span data-ttu-id="2e95c-131">Pole</span><span class="sxs-lookup"><span data-stu-id="2e95c-131">Field</span></span> | <span data-ttu-id="2e95c-132">Popis</span><span class="sxs-lookup"><span data-stu-id="2e95c-132">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2e95c-133">**AzureStorageConnection**</span><span class="sxs-lookup"><span data-stu-id="2e95c-133">**AzureStorageConnection**</span></span> |<span data-ttu-id="2e95c-134">Určuje existující Správce připojení úložiště Azure nebo vytvoří novou, která odkazuje na účet úložiště Azure, který odkazuje na hostitelem souborů objektů blob.</span><span class="sxs-lookup"><span data-stu-id="2e95c-134">Specifies an existing Azure Storage Connection Manager or creates a new one that refers to an Azure storage account that points to where the blob files are hosted.</span></span> |
| <span data-ttu-id="2e95c-135">**BlobContainer**</span><span class="sxs-lookup"><span data-stu-id="2e95c-135">**BlobContainer**</span></span> |<span data-ttu-id="2e95c-136">Určuje název kontejneru objektů blob, který obsahovat odeslané soubory jako objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="2e95c-136">Specifies the name of the blob container that hold the uploaded files as blobs.</span></span> |
| <span data-ttu-id="2e95c-137">**BlobDirectory**</span><span class="sxs-lookup"><span data-stu-id="2e95c-137">**BlobDirectory**</span></span> |<span data-ttu-id="2e95c-138">Určuje adresář objektu blob se uloží nahrávaný soubor jako objekt blob bloku.</span><span class="sxs-lookup"><span data-stu-id="2e95c-138">Specifies the blob directory where the uploaded file is stored as a block blob.</span></span> <span data-ttu-id="2e95c-139">Objekt blob adresář je hierarchická struktura virtuální.</span><span class="sxs-lookup"><span data-stu-id="2e95c-139">The blob directory is a virtual hierarchical structure.</span></span> <span data-ttu-id="2e95c-140">Pokud již existuje objekt blob, it ia nahradit.</span><span class="sxs-lookup"><span data-stu-id="2e95c-140">If the blob already exists, it ia replaced.</span></span> |
| <span data-ttu-id="2e95c-141">**LocalDirectory**</span><span class="sxs-lookup"><span data-stu-id="2e95c-141">**LocalDirectory**</span></span> |<span data-ttu-id="2e95c-142">Určuje místní adresář, který obsahuje soubory k odeslání.</span><span class="sxs-lookup"><span data-stu-id="2e95c-142">Specifies the local directory that contains the files to be uploaded.</span></span> |
| <span data-ttu-id="2e95c-143">**Název souboru**</span><span class="sxs-lookup"><span data-stu-id="2e95c-143">**FileName**</span></span> |<span data-ttu-id="2e95c-144">Určuje název filtr pro výběr souborů pomocí vzoru zadaný název.</span><span class="sxs-lookup"><span data-stu-id="2e95c-144">Specifies a name filter to select files with the specified name pattern.</span></span> <span data-ttu-id="2e95c-145">Například MySheet\*.xls\* obsahuje soubory, jako jsou MySheet001.xls a MySheetABC.xlsx</span><span class="sxs-lookup"><span data-stu-id="2e95c-145">For example, MySheet\*.xls\* includes files such as MySheet001.xls and MySheetABC.xlsx</span></span> |
| <span data-ttu-id="2e95c-146">**TimeRangeFrom/TimeRangeTo**</span><span class="sxs-lookup"><span data-stu-id="2e95c-146">**TimeRangeFrom/TimeRangeTo**</span></span> |<span data-ttu-id="2e95c-147">Určuje časový rozsah filtr.</span><span class="sxs-lookup"><span data-stu-id="2e95c-147">Specifies a time range filter.</span></span> <span data-ttu-id="2e95c-148">Soubory upravené po *TimeRangeFrom* a před *TimeRangeTo* jsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="2e95c-148">Files modified after *TimeRangeFrom* and before *TimeRangeTo* are included.</span></span> |

> [!NOTE]
> <span data-ttu-id="2e95c-149">**AzureStorageConnection** přihlašovací údaje musí být správné a **BlobContainer** musí existovat před pokusem o přenos.</span><span class="sxs-lookup"><span data-stu-id="2e95c-149">The **AzureStorageConnection** credentials need to be correct and the **BlobContainer** must exist before the transfer is attempted.</span></span>
> 
> 

## <a name="download-data-from-azure-blob-storage"></a><span data-ttu-id="2e95c-150">Stahování dat z Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="2e95c-150">Download data from Azure blob storage</span></span>
<span data-ttu-id="2e95c-151">Stahování dat z Azure blob storage do místního úložiště s SSIS použít instanci systému [úloh nahrát objekt Blob Azure](https://msdn.microsoft.com/library/mt146779.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e95c-151">To download data from Azure blob storage to on-premises storage with SSIS, use an instance of the [Azure Blob Upload Task](https://msdn.microsoft.com/library/mt146779.aspx).</span></span>

## <a name="more-advanced-ssis-azure-scenarios"></a><span data-ttu-id="2e95c-152">Pokročilejší scénáře SSIS Azure</span><span class="sxs-lookup"><span data-stu-id="2e95c-152">More advanced SSIS-Azure scenarios</span></span>
<span data-ttu-id="2e95c-153">Balíček funkcí služby SSIS umožňuje složitější toky zpracovávat úlohy balení společně.</span><span class="sxs-lookup"><span data-stu-id="2e95c-153">The SSIS feature pack allows for more complex flows to be handled by packaging tasks together.</span></span> <span data-ttu-id="2e95c-154">Data objektu blob může například kanálu přímo do clusteru HDInsight, jejíž výstup může stáhnout zpět do objektu blob a potom do místního úložiště.</span><span class="sxs-lookup"><span data-stu-id="2e95c-154">For example, the blob data could feed directly into an HDInsight cluster, whose output could be downloaded back to a blob and then to on-premises storage.</span></span> <span data-ttu-id="2e95c-155">SSIS můžete spustit úlohy Hive a Pig v clusteru HDInsight pomocí dalších SSIS konektory:</span><span class="sxs-lookup"><span data-stu-id="2e95c-155">SSIS can run Hive and Pig jobs on an HDInsight cluster using additional SSIS connectors:</span></span>

* <span data-ttu-id="2e95c-156">Chcete-li spustit skript Hive v clusteru Azure HDInsight s SSIS, použijte [Azure HDInsight Hive úloh](https://msdn.microsoft.com/library/mt146771.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e95c-156">To run a Hive script on an Azure HDInsight cluster with SSIS, use [Azure HDInsight Hive Task](https://msdn.microsoft.com/library/mt146771.aspx).</span></span>
* <span data-ttu-id="2e95c-157">Chcete-li spustit skript Pig v clusteru Azure HDInsight s SSIS, použijte [Azure HDInsight Pig úloh](https://msdn.microsoft.com/library/mt146781.aspx).</span><span class="sxs-lookup"><span data-stu-id="2e95c-157">To run a Pig script on an Azure HDInsight cluster with SSIS, use [Azure HDInsight Pig Task](https://msdn.microsoft.com/library/mt146781.aspx).</span></span>

