---
title: "Zpracování rozsáhlých datových sad, které pomocí služby Data Factory a Batch | Microsoft Docs"
description: "Popisuje, jak zpracovávat obrovské objemy dat v kanál služby Azure Data Factory pomocí paralelní zpracování funkce Azure Batch."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 688b964b-51d0-4faa-91a7-26c7e3150868
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: 9defbf7a6a515740fa3b3cb1c67a2f5f9d9baa01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a><span data-ttu-id="d4fb6-103">Zpracování rozsáhlých datových sad pomocí služeb Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-103">Process large-scale datasets using Data Factory and Batch</span></span>
<span data-ttu-id="d4fb6-104">Tento článek popisuje architekturu ukázkové řešení, které přesune a zpracuje rozsáhlých datových sad automatické a naplánované způsobem.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-104">This article describes an architecture of a sample solution that moves and processes large-scale datasets in an automatic and scheduled manner.</span></span> <span data-ttu-id="d4fb6-105">Je také začátku do konce návod k implementaci řešení pomocí Azure Data Factory a Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-105">It also provides an end-to-end walkthrough to implement the solution using Azure Data Factory and Azure Batch.</span></span>

<span data-ttu-id="d4fb6-106">Tento článek je delší než náš typické článek, protože obsahuje návod celé ukázkové řešení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-106">This article is longer than our typical article because it contains a walkthrough of an entire sample solution.</span></span> <span data-ttu-id="d4fb6-107">Pokud jste novým uživatelem Batch a služby Data Factory, můžete další informace o těchto službách a jak pracují společně.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-107">If you are new to Batch and Data Factory, you can learn about these services and how they work together.</span></span> <span data-ttu-id="d4fb6-108">Pokud znáte něco o službách a jsou návrhu nebo architektury řešení, se může zaměřit pouze na [části architektura](#architecture-of-sample-solution) článku a pokud vyvíjíte prototypu nebo řešení, můžete také můžete vyzkoušet na podrobné pokyny v [návod](#implementation-of-sample-solution).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-108">If you know something about the services and are designing/architecting a solution, you may focus just on the [architecture section](#architecture-of-sample-solution) of the article and if you are developing a prototype or a solution, you may also want to try out step-by-step instructions in the [walkthrough](#implementation-of-sample-solution).</span></span> <span data-ttu-id="d4fb6-109">Doporučujeme komentář k tomuto obsahu a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-109">We invite your comments about this content and how you use it.</span></span>

<span data-ttu-id="d4fb6-110">Nejprve podíváme, jak služby Data Factory a Batch vám mohou pomoci s zpracování rozsáhlých datových sad v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-110">First, let's look at how Data Factory and Batch services can help with processing large datasets in the cloud.</span></span>     

## <a name="why-azure-batch"></a><span data-ttu-id="d4fb6-111">Proč Azure Batch?</span><span class="sxs-lookup"><span data-stu-id="d4fb6-111">Why Azure Batch?</span></span>
<span data-ttu-id="d4fb6-112">Služba Azure Batch umožňuje efektivně spouštět rozsáhlé paralelní aplikace a aplikace vysokovýkonného výpočetního prostředí (HPC) v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-112">Azure Batch enables you to run large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="d4fb6-113">Je to služba platformy, která plánuje spouštění výpočetně náročných úloh ve spravované kolekci virtuálních počítačů a která dokáže automaticky škálovat výpočetní prostředky tak, aby splňovaly potřeby vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-113">It's a platform service that schedules compute-intensive work to run on a managed collection of virtual machines, and can automatically scale compute resources to meet the needs of your jobs.</span></span>

<span data-ttu-id="d4fb6-114">Pomocí služby Batch definujete výpočetní prostředky, které vaše aplikace spustí paralelně a škálovaně.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-114">With the Batch service, you define Azure compute resources to execute your applications in parallel, and at scale.</span></span> <span data-ttu-id="d4fb6-115">Můžete spouštět úlohy na vyžádání nebo naplánované úlohy a není nutné ručně vytvářet, konfigurovat a spravovat cluster prostředí HPC, jednotlivé virtuální počítače, virtuální sítě ani infrastrukturu komplexních úloh nebo plánování úkolů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-115">You can run on-demand or scheduled jobs, and you don't need to manually create, configure, and manage an HPC cluster, individual virtual machines, virtual networks, or a complex job and task scheduling infrastructure.</span></span>

<span data-ttu-id="d4fb6-116">Pokud nejste obeznámeni s Azure Batch, protože pomáhá při pochopení, architekturu nebo implementace řešení popsaných v tomto článku najdete v následujících článcích.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-116">See the following articles if you are not familiar with Azure Batch as it helps with understanding the architecture/implementation of the solution described in this article.</span></span>   

* [<span data-ttu-id="d4fb6-117">Základy služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-117">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
* [<span data-ttu-id="d4fb6-118">Přehled funkcí Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-118">Batch feature overview</span></span>](../batch/batch-api-basics.md)

<span data-ttu-id="d4fb6-119">(volitelné) Další informace o Azure Batch, najdete v článku [studijní Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-119">(optional) To learn more about Azure Batch, see the [Learning path for Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span></span>

## <a name="why-azure-data-factory"></a><span data-ttu-id="d4fb6-120">Proč pro vytváření dat Azure?</span><span class="sxs-lookup"><span data-stu-id="d4fb6-120">Why Azure Data Factory?</span></span>
<span data-ttu-id="d4fb6-121">Data Factory je cloudová služba pro integraci dat, která orchestruje a automatizuje přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-121">Data Factory is a cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="d4fb6-122">Pomocí služby Data Factory, můžete vytvořit spravované datové kanály, které přesun dat z místní a cloudové úložiště dat do úložiště dat centralizované (například: Azure Blob Storage) a proces nebo transformace dat pomocí služeb, jako je například Azure HDInsight a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-122">Using the Data Factory service, you can create managed data pipelines that move data from on-premises and cloud data stores to a centralized data store (for example: Azure Blob Storage), and process/transform data using services such as Azure HDInsight and Azure Machine Learning.</span></span> <span data-ttu-id="d4fb6-123">Můžete také naplánovat datových kanálů pro spouštění v naplánované způsobem (hodinové, denní, týdenní, atd.) a monitorování a spravovat na první pohled identifikovat problémy a provést akci.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-123">You can also schedule data pipelines to run in a scheduled manner (hourly, daily, weekly, etc.) and monitor and manage them at a glance to identify issues and take action.</span></span>

<span data-ttu-id="d4fb6-124">Pokud nejste obeznámeni s Azure Data Factory, protože pomáhá při pochopení, architekturu nebo implementace řešení popsaných v tomto článku najdete v následujících článcích.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-124">See the following articles if you are not familiar with Azure Data Factory as it helps with understanding the architecture/implementation of the solution described in this article.</span></span>  

* [<span data-ttu-id="d4fb6-125">Úvod objektu pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="d4fb6-125">Introduction of Azure Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="d4fb6-126">Sestavit svůj první kanál dat</span><span class="sxs-lookup"><span data-stu-id="d4fb6-126">Build your first data pipeline</span></span>](data-factory-build-your-first-pipeline.md)   

<span data-ttu-id="d4fb6-127">(volitelné) Další informace o Azure Data Factory najdete v tématu [studijní postup Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-127">(optional) To learn more about Azure Data Factory, see the [Learning path for Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span></span>

## <a name="data-factory-and-batch-together"></a><span data-ttu-id="d4fb6-128">Objekt pro vytváření dat a společně Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-128">Data Factory and Batch together</span></span>
<span data-ttu-id="d4fb6-129">Objekt pro vytváření dat obsahuje zabudované aktivity například aktivity kopírování zkopírovat nebo přesunout data ze zdrojového úložiště dat do cílového úložiště dat a Hive aktivity zpracování dat pomocí clusterů systému Hadoop (HDInsight) v Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-129">Data Factory includes built-in activities such as Copy Activity to copy/move data from a source data store to a destination data store and Hive Activity to process data using Hadoop clusters (HDInsight) on Azure.</span></span> <span data-ttu-id="d4fb6-130">V tématu [aktivit transformace dat](data-factory-data-transformation-activities.md) seznam aktivit transformace podporované.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-130">See [Data Transformation Activities](data-factory-data-transformation-activities.md) for a list of supported transformation activities.</span></span>

<span data-ttu-id="d4fb6-131">Také vám umožňuje vytvářet vlastní aktivity .NET k přesunutí nebo zpracování dat s vlastní logikou a spusťte tyto aktivity v clusteru Azure HDInsight nebo u fondu Azure Batch virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-131">It also allows you to create custom .NET activities to move or process data with your own logic and run these activities on an Azure HDInsight cluster or on an Azure Batch pool of VMs.</span></span> <span data-ttu-id="d4fb6-132">Při použití služby Azure Batch, můžete nakonfigurovat k automatickému škálování fondu (Přidat nebo odebrat na základě příslušného zatížení virtuálních počítačů) podle vzorce zadáte.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-132">When you use Azure Batch, you can configure the pool to auto-scale (add or remove VMs based on the workload) based on a formula you provide.</span></span>     

## <a name="architecture-of-sample-solution"></a><span data-ttu-id="d4fb6-133">Architektura ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="d4fb6-133">Architecture of sample solution</span></span>
<span data-ttu-id="d4fb6-134">I když architektury popsané v tomto článku je pro jednoduchým řešením, je relevantní pro komplexní scénáře, jako je riziko modelování finančních služeb, zpracování obrázků a vykreslování a Genomické analýzy.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-134">Even though the architecture described in this article is for a simple solution, it is relevant to complex scenarios such as risk modeling by financial services, image processing and rendering, and genomic analysis.</span></span>

<span data-ttu-id="d4fb6-135">Diagram znázorňuje 1) jak Data Factory orchestruje přesuny dat a zpracování a 2) jak Azure Batch zpracovává data paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-135">The diagram illustrates 1) how Data Factory orchestrates data movement and processing and 2) how Azure Batch processes the data in a parallel manner.</span></span> <span data-ttu-id="d4fb6-136">Stáhnout a vytisknout diagram referenční (11 × 17 palců.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-136">Download and print the diagram for easy reference (11 x 17 in.</span></span> <span data-ttu-id="d4fb6-137">nebo velikost A3): [orchestration HPC a data pomocí Azure Batch a objektu pro vytváření dat](http://go.microsoft.com/fwlink/?LinkId=717686).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-137">or A3 size): [HPC and data orchestration using Azure Batch and Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span></span>

<span data-ttu-id="d4fb6-138">[![Diagram zpracování velkých dat](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span><span class="sxs-lookup"><span data-stu-id="d4fb6-138">[![Large-scale data processing diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span></span>

<span data-ttu-id="d4fb6-139">Následující seznam uvádí základní kroky procesu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-139">The following list provides the basic steps of the process.</span></span> <span data-ttu-id="d4fb6-140">Řešení obsahuje kód a vysvětlení, k vytvoření řešení začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-140">The solution includes code and explanations to build the end-to-end solution.</span></span>

1. <span data-ttu-id="d4fb6-141">**Nakonfigurovat fond výpočetních uzlů (VM) Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-141">**Configure Azure Batch with a pool of compute nodes (VMs)**.</span></span> <span data-ttu-id="d4fb6-142">Můžete zadat počet uzlů a velikost každého uzlu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-142">You can specify the number of nodes and size of each node.</span></span>
2. <span data-ttu-id="d4fb6-143">**Vytvoření instance služby Azure Data Factory** nakonfigurovaný s entitami, které představují Azure blob storage, výpočetní služby Azure Batch, vstupní a výstupní data a pracovní postup nebo kanálu s aktivitami, které přesunout a transformovat data.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-143">**Create an Azure Data Factory instance** that is configured with entities that represent Azure blob storage, Azure Batch compute service, input/output data, and a workflow/pipeline with activities that move and transform data.</span></span>
3. <span data-ttu-id="d4fb6-144">**Vytvořit vlastní .NET aktivitu v kanálu pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-144">**Create a custom .NET activity in the Data Factory pipeline**.</span></span> <span data-ttu-id="d4fb6-145">Aktivita je váš kód uživatele, který běží ve fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-145">The activity is your user code that runs on the Azure Batch pool.</span></span>
4. <span data-ttu-id="d4fb6-146">**Ukládat velké množství vstupních dat jako objekty BLOB v úložišti Azure**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-146">**Store large amounts of input data as blobs in Azure storage**.</span></span> <span data-ttu-id="d4fb6-147">Data je rozdělena do logické řezy (obvykle čas).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-147">Data is divided into logical slices (usually by time).</span></span>
5. <span data-ttu-id="d4fb6-148">**Objekt pro vytváření dat zkopíruje data, která zpracovává paralelně** do sekundárního umístění.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-148">**Data Factory copies data that is processed in parallel** to the secondary location.</span></span>
6. <span data-ttu-id="d4fb6-149">**Objekt pro vytváření dat běží vlastní aktivity pomocí fondu přidělené dávkou**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-149">**Data Factory runs the custom activity using the pool allocated by Batch**.</span></span> <span data-ttu-id="d4fb6-150">Objekt pro vytváření dat mohou být aktivity současně.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-150">Data Factory can run activities concurrently.</span></span> <span data-ttu-id="d4fb6-151">Každá aktivita zpracovává řez data.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-151">Each activity processes a slice of data.</span></span> <span data-ttu-id="d4fb6-152">Výsledky jsou uloženy v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-152">The results are stored in Azure storage.</span></span>
7. <span data-ttu-id="d4fb6-153">**Objekt pro vytváření dat přesune konečných výsledků do třetí umístění**, k distribuci prostřednictvím aplikace nebo pro další zpracování jiných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-153">**Data Factory moves the final results to a third location**, either for distribution via an app, or for further processing by other tools.</span></span>

## <a name="implementation-of-sample-solution"></a><span data-ttu-id="d4fb6-154">Implementace ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="d4fb6-154">Implementation of sample solution</span></span>
<span data-ttu-id="d4fb6-155">Ukázkové řešení jsou záměrně jednoduchá a ukazují, jak používat společně objekt pro vytváření dat a Batch ke zpracování datové sady.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-155">The sample solution is intentionally simple and is to show you how to use Data Factory and Batch together to process datasets.</span></span> <span data-ttu-id="d4fb6-156">Řešení jednoduše spočítá počet výskytů hledaný termín ("Microsoft") v vstupní soubory, které jsou uspořádány do časové řady.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-156">The solution simply counts the number of occurrences of a search term (“Microsoft”) in input files organized in a time series.</span></span> <span data-ttu-id="d4fb6-157">Vyprodukuje počet, který má výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-157">It outputs the count to output files.</span></span>

<span data-ttu-id="d4fb6-158">**Čas**: Pokud se seznámíte se základy používání služby Azure Data Factory a Batch a dokončili požadavky uvedené níže, jsme odhadnout toto řešení trvá 1 – 2 hodiny.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-158">**Time**: If you are familiar with basics of Azure, Data Factory, and Batch, and have completed the prerequisites listed below, we estimate this solution takes 1-2 hours to complete.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d4fb6-159">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d4fb6-159">Prerequisites</span></span>
#### <a name="azure-subscription"></a><span data-ttu-id="d4fb6-160">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="d4fb6-160">Azure subscription</span></span>
<span data-ttu-id="d4fb6-161">Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-161">If you don't have an Azure subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d4fb6-162">V tématu [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-162">See [Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

#### <a name="azure-storage-account"></a><span data-ttu-id="d4fb6-163">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d4fb6-163">Azure storage account</span></span>
<span data-ttu-id="d4fb6-164">Používáte účet úložiště Azure pro ukládání dat v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-164">You use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="d4fb6-165">Pokud nemáte účet úložiště Azure, najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-165">If you don't have an Azure storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="d4fb6-166">Ukázkové řešení používá úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-166">The sample solution uses blob storage.</span></span>

#### <a name="azure-batch-account"></a><span data-ttu-id="d4fb6-167">Účet Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-167">Azure Batch account</span></span>
<span data-ttu-id="d4fb6-168">Vytvoření účtu Azure Batch pomocí [portál Azure](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-168">Create an Azure Batch account using the [Azure portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="d4fb6-169">V tématu [vytvoření a Správa účtu Azure Batch](../batch/batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-169">See [Create and manage an Azure Batch account](../batch/batch-account-create-portal.md).</span></span> <span data-ttu-id="d4fb6-170">Všimněte si klíč účet a název účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-170">Note the Azure Batch account name and account key.</span></span> <span data-ttu-id="d4fb6-171">Můžete také použít [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) rutiny k vytvoření účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-171">You can also use [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet to create an Azure Batch account.</span></span> <span data-ttu-id="d4fb6-172">V tématu [Začínáme pomocí rutin prostředí PowerShell Azure Batch](../batch/batch-powershell-cmdlets-get-started.md) pro podrobné pokyny k použití této rutiny.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-172">See [Get started with Azure Batch PowerShell cmdlets](../batch/batch-powershell-cmdlets-get-started.md) for detailed instructions on using this cmdlet.</span></span>

<span data-ttu-id="d4fb6-173">Ukázkové řešení používá Azure Batch (nepřímo přes kanál služby Azure Data Factory) pro zpracování dat paralelní způsobem ve fondu výpočetních uzlů (spravované kolekce virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-173">The sample solution uses Azure Batch (indirectly via an Azure Data Factory pipeline) to process data in a parallel manner on a pool of compute nodes (a managed collection of virtual machines).</span></span>

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a><span data-ttu-id="d4fb6-174">Azure Batch fondu virtuálních počítačů (VM)</span><span class="sxs-lookup"><span data-stu-id="d4fb6-174">Azure Batch pool of virtual machines (VMs)</span></span>
<span data-ttu-id="d4fb6-175">Vytvoření **fondu Azure Batch** s minimálně 2 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-175">Create an **Azure Batch pool** with at least 2 compute nodes.</span></span>

1. <span data-ttu-id="d4fb6-176">V [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce a klikněte na **účty Batch**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-176">In the [Azure portal](https://portal.azure.com), click **Browse** in the left menu, and click **Batch Accounts**.</span></span>
2. <span data-ttu-id="d4fb6-177">Vyberte svůj účet Azure Batch **účtu Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-177">Select your Azure Batch account to open the **Batch Account** blade.</span></span>
3. <span data-ttu-id="d4fb6-178">Klikněte na tlačítko **fondy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-178">Click **Pools** tile.</span></span>
4. <span data-ttu-id="d4fb6-179">V **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů na Přidat fond.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-179">In the **Pools** blade, click Add button on the toolbar to add a pool.</span></span>
   1. <span data-ttu-id="d4fb6-180">Zadejte ID fondu (**ID fondu**).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-180">Enter an ID for the pool (**Pool ID**).</span></span> <span data-ttu-id="d4fb6-181">Poznámka: **ID fondu**; je nutné při vytváření řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-181">Note the **ID of the pool**; you need it when creating the Data Factory solution.</span></span>
   2. <span data-ttu-id="d4fb6-182">Zadejte **Windows Server 2012 R2** pro nastavení řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-182">Specify **Windows Server 2012 R2** for the Operating System Family setting.</span></span>
   3. <span data-ttu-id="d4fb6-183">Vyberte **cenovou úroveň uzlu**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-183">Select a **node pricing tier**.</span></span>
   4. <span data-ttu-id="d4fb6-184">Zadejte **2** jako hodnota **cíl vyhrazené** nastavení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-184">Enter **2** as value for the **Target Dedicated** setting.</span></span>
   5. <span data-ttu-id="d4fb6-185">Zadejte **2** jako hodnota **maximální počet úloh na uzel** nastavení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-185">Enter **2** as value for the **Max tasks per node** setting.</span></span>
   6. <span data-ttu-id="d4fb6-186">Kliknutím na tlačítko **OK** vytvořte fond.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-186">Click **OK** to create the pool.</span></span>

#### <a name="azure-storage-explorer"></a><span data-ttu-id="d4fb6-187">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="d4fb6-187">Azure Storage Explorer</span></span>
<span data-ttu-id="d4fb6-188">[Azure Storage Explorer 6 (nástroj)](https://azurestorageexplorer.codeplex.com/) nebo [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (z ClumsyLeaf softwaru).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-188">[Azure Storage Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) or [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (from ClumsyLeaf Software).</span></span> <span data-ttu-id="d4fb6-189">Pomocí těchto nástrojů pro kontroly a změna dat v Azure Storage projekty, včetně protokolů aplikací hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-189">You use these tools for inspecting and altering the data in your Azure Storage projects including the logs of your cloud-hosted applications.</span></span>

1. <span data-ttu-id="d4fb6-190">Vytvořit kontejner s názvem **můj_kontejner** s přístupem k privátní (žádný anonymní přístup)</span><span class="sxs-lookup"><span data-stu-id="d4fb6-190">Create a container named **mycontainer** with private access (no anonymous access)</span></span>
2. <span data-ttu-id="d4fb6-191">Pokud používáte **CloudXplorer**, vytvořit složky a podsložky s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-191">If you are using **CloudXplorer**, create folders and subfolders with the following structure:</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   <span data-ttu-id="d4fb6-192">`Inputfolder`a `outputfolder` jsou nejvyšší úrovně složky v `mycontainer`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-192">`Inputfolder` and `outputfolder` are top-level folders in `mycontainer`.</span></span> <span data-ttu-id="d4fb6-193">`inputfolder` Má podsložky razítka data a času (rrrr-MM-DD-HH).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-193">The `inputfolder` has subfolders with date-time stamps (YYYY-MM-DD-HH).</span></span>

   <span data-ttu-id="d4fb6-194">Pokud používáte **Azure Storage Explorer**, v dalším kroku, budete muset odeslat soubory s názvy: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-194">If you are using **Azure Storage Explorer**, in the next step, you need to upload files with names: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` and so on.</span></span> <span data-ttu-id="d4fb6-195">Tento krok automaticky vytvoří složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-195">This step automatically creates the folders.</span></span>
3. <span data-ttu-id="d4fb6-196">Vytvořte textový soubor **soubor.txt** na počítači s obsahem, který obsahuje klíčové slovo **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-196">Create a text file **file.txt** on your machine with content that has the keyword **Microsoft**.</span></span> <span data-ttu-id="d4fb6-197">Například: "testovací vlastní aktivity Microsoft test vlastní aktivity Microsoft".</span><span class="sxs-lookup"><span data-stu-id="d4fb6-197">For example: “test custom activity Microsoft test custom activity Microsoft”.</span></span>
4. <span data-ttu-id="d4fb6-198">Nahrajte soubor do následující složky vstupní v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-198">Upload the file to the following input folders in Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   <span data-ttu-id="d4fb6-199">Pokud používáte **Azure Storage Explorer**, nahrajte soubor **soubor.txt** k **můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-199">If you are using **Azure Storage Explorer**, upload the file **file.txt** to **mycontainer**.</span></span> <span data-ttu-id="d4fb6-200">Klikněte na tlačítko **kopie** na panelu nástrojů vytvořit kopii objektu blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-200">Click **Copy** on the toolbar to create a copy of the blob.</span></span> <span data-ttu-id="d4fb6-201">V **kopírovat objekt Blob** dialogové okno, změny **název cílového objektu blob** k `inputfolder/2015-11-16-00/file.txt`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-201">In the **Copy Blob** dialog box, change the **destination blob name** to `inputfolder/2015-11-16-00/file.txt`.</span></span> <span data-ttu-id="d4fb6-202">Opakujte tento krok k vytvoření `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` a tak dále.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-202">Repeat this step to create `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` and so on.</span></span> <span data-ttu-id="d4fb6-203">Tato akce automaticky vytvoří složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-203">This action automatically creates the folders.</span></span>
5. <span data-ttu-id="d4fb6-204">Vytvořte jiný kontejner s názvem: `customactivitycontainer`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-204">Create another container named: `customactivitycontainer`.</span></span> <span data-ttu-id="d4fb6-205">Můžete nahrát soubor zip vlastní aktivitu do tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-205">You upload the custom activity zip file to this container.</span></span>

#### <a name="visual-studio"></a><span data-ttu-id="d4fb6-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d4fb6-206">Visual Studio</span></span>
<span data-ttu-id="d4fb6-207">Nainstalujte Microsoft Visual Studio 2012 nebo novějším k vytvoření vlastních dávkové aktivity pro použití v řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-207">Install Microsoft Visual Studio 2012 or later to create the custom Batch activity to be used in the Data Factory solution.</span></span>

### <a name="high-level-steps-to-create-the-solution"></a><span data-ttu-id="d4fb6-208">Základní kroky pro vytvoření řešení</span><span class="sxs-lookup"><span data-stu-id="d4fb6-208">High-level steps to create the solution</span></span>
1. <span data-ttu-id="d4fb6-209">Vytvořte vlastní aktivity, která obsahuje logiku zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-209">Create a custom activity that contains the data processing logic.</span></span>
2. <span data-ttu-id="d4fb6-210">Vytvoření služby Azure data factory, která používá vlastní aktivity:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-210">Create an Azure data factory that uses the custom activity:</span></span>

### <a name="create-the-custom-activity"></a><span data-ttu-id="d4fb6-211">Vytvořit vlastní aktivitu</span><span class="sxs-lookup"><span data-stu-id="d4fb6-211">Create the custom activity</span></span>
<span data-ttu-id="d4fb6-212">Vlastní aktivita služby Data Factory je jádrem této ukázkové řešení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-212">The Data Factory custom activity is the heart of this sample solution.</span></span> <span data-ttu-id="d4fb6-213">Ukázkové řešení používá Azure Batch pro spuštění vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-213">The sample solution uses Azure Batch to run the custom activity.</span></span> <span data-ttu-id="d4fb6-214">V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) pro základní informace o vývoji vlastních aktivit a použít je v kanálů služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-214">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) for the basic information to develop custom activities and use them in Azure Data Factory pipelines.</span></span>

<span data-ttu-id="d4fb6-215">Chcete-li vytvořit vlastní aktivity rozhraní .NET, který můžete použít v kanál služby Azure Data Factory, je potřeba vytvořit **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-215">To create a .NET custom activity that you can use in an Azure Data Factory pipeline, you need to create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="d4fb6-216">Toto rozhraní obsahuje pouze jednu metodu: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-216">This interface has only one method: **Execute**.</span></span> <span data-ttu-id="d4fb6-217">Tady je podpis metody:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-217">Here is the signature of the method:</span></span>

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

<span data-ttu-id="d4fb6-218">Metoda má několik klíčové komponenty, které je třeba pochopit.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-218">The method has a few key components that you need to understand.</span></span>

* <span data-ttu-id="d4fb6-219">Tato metoda přebírá čtyř parametrů:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-219">The method takes four parameters:</span></span>

  1. <span data-ttu-id="d4fb6-220">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-220">**linkedServices**.</span></span> <span data-ttu-id="d4fb6-221">Vyčíslitelná seznam propojené služby, které odkazují vstupní a výstupní datové zdroje (například: Azure Blob Storage) k objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-221">An enumerable list of linked services that link input/output data sources (for example: Azure Blob Storage) to the data factory.</span></span> <span data-ttu-id="d4fb6-222">V této ukázce je pouze jeden propojené služby typu úložiště Azure pro vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-222">In this sample, there is only one linked service of type Azure Storage used for both input and output.</span></span>
  2. <span data-ttu-id="d4fb6-223">**datové sady**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-223">**datasets**.</span></span> <span data-ttu-id="d4fb6-224">Toto je vyčíslitelná seznam datových sad.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-224">This is an enumerable list of datasets.</span></span> <span data-ttu-id="d4fb6-225">Tento parametr můžete získat umístění a schémata definované vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-225">You can use this parameter to get the locations and schemas defined by input and output datasets.</span></span>
  3. <span data-ttu-id="d4fb6-226">**aktivita**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-226">**activity**.</span></span> <span data-ttu-id="d4fb6-227">Tento parametr představuje aktuální výpočetní entitu – v takovém případě služby Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-227">This parameter represents the current compute entity - in this case, an Azure Batch service.</span></span>
  4. <span data-ttu-id="d4fb6-228">**protokolovač**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-228">**logger**.</span></span> <span data-ttu-id="d4fb6-229">Protokolovač umožňuje psát komentáře ladění tento prostor jako protokol "User" pro kanál.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-229">The logger lets you write debug comments that surface as the “User” log for the pipeline.</span></span>
* <span data-ttu-id="d4fb6-230">Metoda vrátí slovník, který slouží k řetězu vlastní aktivity společně v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-230">The method returns a dictionary that can be used to chain custom activities together in the future.</span></span> <span data-ttu-id="d4fb6-231">Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-231">This feature is not implemented yet, so return an empty dictionary from the method.</span></span>

#### <a name="procedure-create-the-custom-activity"></a><span data-ttu-id="d4fb6-232">Postup: Vytvoření vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="d4fb6-232">Procedure: Create the custom activity</span></span>
1. <span data-ttu-id="d4fb6-233">Vytvoření projektu knihovny tříd rozhraní .NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-233">Create a .NET Class Library project in Visual Studio.</span></span>

   1. <span data-ttu-id="d4fb6-234">Spusťte **sady Visual Studio 2012**/**2013 nebo 2015**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-234">Launch **Visual Studio 2012**/**2013/2015**.</span></span>
   2. <span data-ttu-id="d4fb6-235">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-235">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="d4fb6-236">Rozbalte položku **šablony**a vyberte **Visual C\#**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-236">Expand **Templates**, and select **Visual C\#**.</span></span> <span data-ttu-id="d4fb6-237">V tomto návodu můžete použít C\#, ale můžete použít kterémkoli jazyce platformy .NET pro vývoj vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-237">In this walkthrough, you use C\#, but you can use any .NET language to develop the custom activity.</span></span>
   4. <span data-ttu-id="d4fb6-238">Vyberte **knihovny tříd** ze seznamu typů projektu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-238">Select **Class Library** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="d4fb6-239">Zadejte **MyDotNetActivity** pro **název**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-239">Enter **MyDotNetActivity** for the **Name**.</span></span>
   6. <span data-ttu-id="d4fb6-240">Vyberte **C:\\ADF** pro **umístění**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-240">Select **C:\\ADF** for the **Location**.</span></span> <span data-ttu-id="d4fb6-241">Vytvořit složku **ADF** Pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-241">Create the folder **ADF** if it does not exist.</span></span>
   7. <span data-ttu-id="d4fb6-242">Kliknutím na tlačítko **OK** vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-242">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="d4fb6-243">Klikněte na **Nástroje**, přejděte na **Správce balíčků NuGet** a klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-243">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="d4fb6-244">V **Konzola správce balíčků**, spusťte následující příkaz pro import **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-244">In the **Package Manager Console**, execute the following command to import **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="d4fb6-245">Import **Azure Storage** balíčku NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-245">Import the **Azure Storage** NuGet package in to the project.</span></span> <span data-ttu-id="d4fb6-246">Tento balíček je nutné, protože můžete používat úložiště objektů Blob rozhraní API v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-246">You need this package because you use the Blob storage API in this sample.</span></span>

    ```powershell
    Install-Package Azure.Storage
    ```
5. <span data-ttu-id="d4fb6-247">Přidejte následující **pomocí** direktivy ke zdrojovému souboru v projektu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-247">Add the following **using** directives to the source file in the project.</span></span>

    ```csharp
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;
    
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="d4fb6-248">Změňte název **obor názvů** k **MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-248">Change the name of the **namespace** to **MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="d4fb6-249">Změnit název třídy pro **MyDotNetActivity** a odvodí z **IDotNetActivity** rozhraní, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-249">Change the name of the class to **MyDotNetActivity** and derive it from the **IDotNetActivity** interface as shown below.</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="d4fb6-250">Implementace (Přidat) **Execute** metodu **IDotNetActivity** rozhraní k **MyDotNetActivity** třídy a zkopírujte následující vzorový kód do metody.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-250">Implement (Add) the **Execute** method of the **IDotNetActivity** interface to the **MyDotNetActivity** class and copy the following sample code to the method.</span></span> <span data-ttu-id="d4fb6-251">Najdete v článku [spustit metodu](#execute-method) části vysvětlení pro logikou používanou v této metodě.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-251">See the [Execute Method](#execute-method) section for explanation for the logic used in this method.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is the only method of IDotNetActivity interface you must implement.
    /// In this sample, the method invokes the Calculate method to perform the core logic.  
    /// </summary>
    public IDictionary<string, string> Execute(
       IEnumerable<LinkedService> linkedServices,
       IEnumerable<Dataset> datasets,
       Activity activity,
       IActivityLogger logger)
    {
    
       // declare types for input and output data stores
       AzureStorageLinkedService inputLinkedService;
    
       Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
       foreach (LinkedService ls in linkedServices)
           logger.Write("linkedService.Name {0}", ls.Name);
    
       // using First method instead of Single since we are using the same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // To create an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass the connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize the continuation token before using it in the do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get the list of input blobs from the input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns the number of occurrences of
           // the search term (“Microsoft”) in each blob associated
           // with the data slice.
           //
           // definition of the method is shown in the next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob to the folder: {0}", folderPath);
    
       // create a storage object for the output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write the name of the file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload the output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} to the output blob", output);
       outputBlob.UploadText(output);
    
       // The dictionary can be used to chain custom activities together in the future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="d4fb6-252">Přidejte následující pomocné metody pro třídu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-252">Add the following helper methods to the class.</span></span> <span data-ttu-id="d4fb6-253">Tyto metody jsou vyvolány **Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-253">These methods are invoked by the **Execute** method.</span></span> <span data-ttu-id="d4fb6-254">Co je nejdůležitější **Calculate** metoda izoluje kód, který iteruje v rámci jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-254">Most importantly, the **Calculate** method isolates the code that iterates through each blob.</span></span>

    ```csharp
    /// <summary>
    /// Gets the folderPath value from the input/output dataset.
    /// </summary>
    private static string GetFolderPath(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FolderPath;
    }
    
    /// <summary>
    /// Gets the fileName value from the input/output dataset.
    /// </summary>
    
    private static string GetFileName(Dataset dataArtifact)
    {
       if (dataArtifact == null || dataArtifact.Properties == null)
       {
           return null;
       }
    
       AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
       if (blobDataset == null)
       {
           return null;
       }
    
       return blobDataset.FileName;
    }
    
    /// <summary>
    /// Iterates through each blob (file) in the folder, counts the number of instances of search term in the file,
    /// and prepares the output text that is written to the output blob.
    /// </summary>
    
    public static string Calculate(BlobResultSegment Bresult, IActivityLogger logger, string folderPath, ref BlobContinuationToken token, string searchTerm)
    {
       string output = string.Empty;
       logger.Write("number of blobs found: {0}", Bresult.Results.Count<IListBlobItem>());
       foreach (IListBlobItem listBlobItem in Bresult.Results)
       {
           CloudBlockBlob inputBlob = listBlobItem as CloudBlockBlob;
           if ((inputBlob != null) && (inputBlob.Name.IndexOf("$$$.$$$") == -1))
           {
               string blobText = inputBlob.DownloadText(Encoding.ASCII, null, null, null);
               logger.Write("input blob text: {0}", blobText);
               string[] source = blobText.Split(new char[] { '.', '?', '!', ' ', ';', ':', ',' }, StringSplitOptions.RemoveEmptyEntries);
               var matchQuery = from word in source
                                where word.ToLowerInvariant() == searchTerm.ToLowerInvariant()
                                select word;
               int wordCount = matchQuery.Count();
               output += string.Format("{0} occurrences(s) of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    <span data-ttu-id="d4fb6-255">**GetFolderPath** metoda vrací cestu ke složce, která odkazuje datovou sadu na a **GetFileName** metoda vrátí název objektu blob nebo soubor, který datová sada odkazuje na.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-255">The **GetFolderPath** method returns the path to the folder that the dataset points to and the **GetFileName** method returns the name of the blob/file that the dataset points to.</span></span>

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    <span data-ttu-id="d4fb6-256">**Calculate** metoda vypočítá počet instancí – klíčové slovo **Microsoft** ve vstupní soubory (objekty BLOB ve složce).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-256">The **Calculate** method calculates the number of instances of keyword **Microsoft** in the input files (blobs in the folder).</span></span> <span data-ttu-id="d4fb6-257">Hledaný termín ("Microsoft") je pevně zakódovaná v kódu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-257">The search term (“Microsoft”) is hard-coded in the code.</span></span>

1. <span data-ttu-id="d4fb6-258">Kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-258">Compile the project.</span></span> <span data-ttu-id="d4fb6-259">Klikněte na tlačítko **sestavení** z nabídky a klikněte na tlačítko **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-259">Click **Build** from the menu and click **Build Solution**.</span></span>
2. <span data-ttu-id="d4fb6-260">Spusťte **Průzkumníka Windows**a přejděte do **bin\\ladění** nebo **bin\\verze** složku v závislosti na typu sestavení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-260">Launch **Windows Explorer**, and navigate to **bin\\debug** or **bin\\release** folder depending on the type of build.</span></span>
3. <span data-ttu-id="d4fb6-261">Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory v  **\\bin\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-261">Create a zip file **MyDotNetActivity.zip** that contains all the binaries in the **\\bin\\Debug** folder.</span></span> <span data-ttu-id="d4fb6-262">Můžete zahrnout MyDotNetActivity. **pdb** tak, aby získat další podrobnosti, jako je například číslo řádku ve zdrojovém kódu, která způsobila problém, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-262">You may want to include the MyDotNetActivity.**pdb** file so that you get additional details such as line number in the source code that caused the issue when a failure occurs.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. <span data-ttu-id="d4fb6-263">Nahrát **MyDotNetActivity.zip** jako objekt blob do kontejneru objektů blob: `customactivitycontainer` ve službě Azure blob storage, **StorageLinkedService** propojená služba v  **ADFTutorialDataFactory** používá.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-263">Upload **MyDotNetActivity.zip** as a blob to the blob container: `customactivitycontainer` in the Azure blob storage that the **StorageLinkedService** linked service in the **ADFTutorialDataFactory** uses.</span></span> <span data-ttu-id="d4fb6-264">Vytvoření kontejneru objektů blob `customactivitycontainer` Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-264">Create the blob container `customactivitycontainer` if it does not already exist.</span></span>

#### <a name="execute-method"></a><span data-ttu-id="d4fb6-265">Execute – metoda</span><span class="sxs-lookup"><span data-stu-id="d4fb6-265">Execute method</span></span>
<span data-ttu-id="d4fb6-266">Tato část obsahuje další podrobnosti a poznámky o kód v Metoda Execute.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-266">This section provides more details and notes about the code in the Execute method.</span></span>

1. <span data-ttu-id="d4fb6-267">Členy pro iterace v rámci vstupní kolekce jsou součástí [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-267">The members for iterating through the input collection are found in the [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namespace.</span></span> <span data-ttu-id="d4fb6-268">Iterace v rámci kolekce objektů blob vyžaduje použití **BlobContinuationToken** třídy.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-268">Iterating through the blob collection requires using the **BlobContinuationToken** class.</span></span> <span data-ttu-id="d4fb6-269">V podstatě, je nutné použít DNT-při smyčky pomocí tokenu jako mechanismus pro ukončení opakování.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-269">In essence, you must use a do-while loop with the token as the mechanism for exiting the loop.</span></span> <span data-ttu-id="d4fb6-270">Další informace najdete v tématu [postup používání úložiště Blob z .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-270">For more information, see [How to use Blob storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="d4fb6-271">Zobrazí se zde základní smyčka:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-271">A basic loop is shown here:</span></span>

    ```csharp
    // Initialize the continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get the list of input blobs from the input storage client object.
    BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
    
                         true,
                                   BlobListingDetails.Metadata,
                                   null,
                                   continuationToken,
                                   null,
                                   null);
    // Return a string derived from parsing each blob.
    
     output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
    } while (continuationToken != null);

    ```
   <span data-ttu-id="d4fb6-272">Najdete v dokumentaci k [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metoda podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-272">See the documentation for the [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) method for details.</span></span>
2. <span data-ttu-id="d4fb6-273">Kód pro práci prostřednictvím sady objektů BLOB logicky přejde v rámci aplikace-při smyčky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-273">The code for working through the set of blobs logically goes within the do-while loop.</span></span> <span data-ttu-id="d4fb6-274">V **Execute** metoda, do-při smyčky předá seznam objektů BLOB metodu s názvem **Calculate**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-274">In the **Execute** method, the do-while loop passes the list of blobs to a method named **Calculate**.</span></span> <span data-ttu-id="d4fb6-275">Metoda vrátí řetězec proměnné s názvem **výstup** tedy výsledek s vstupní prostřednictvím všech objektů BLOB v segmentu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-275">The method returns a string variable named **output** that is the result of having iterated through all the blobs in the segment.</span></span>

   <span data-ttu-id="d4fb6-276">Vrátí počet výskytů hledaný termín (**Microsoft**) v objektu blob předaný **Calculate** metoda.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-276">It returns the number of occurrences of the search term (**Microsoft**) in the blob passed to the **Calculate** method.</span></span>

    ```csharp
    output += string.Format("{0} occurrences of the search term \"{1}\" were found in the file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. <span data-ttu-id="d4fb6-277">Jednou **Calculate** Metoda dokončení práce, musí být zapsané do nového objektu blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-277">Once the **Calculate** method has done the work, it must be written to a new blob.</span></span> <span data-ttu-id="d4fb6-278">Aby pro každou sadu objektů BLOB zpracovat nový objekt blob může být napsán s výsledky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-278">So for every set of blobs processed, a new blob can be written with the results.</span></span> <span data-ttu-id="d4fb6-279">Zápis do nového objektu blob, nejdřív najdete výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-279">To write to a new blob, first find the output dataset.</span></span>

    ```csharp
    // Get the output dataset using the name of the dataset matched to a name in the Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. <span data-ttu-id="d4fb6-280">Kód také volá metodu helper: **GetFolderPath** načíst cesta ke složce (název kontejneru úložiště).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-280">The code also calls a helper method: **GetFolderPath** to retrieve the folder path (the storage container name).</span></span>

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   <span data-ttu-id="d4fb6-281">**GetFolderPath** vrhá objekt datové sady, který má AzureBlobDataSet, který má vlastnost s názvem FolderPath.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-281">The **GetFolderPath** casts the DataSet object to an AzureBlobDataSet, which has a property named FolderPath.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. <span data-ttu-id="d4fb6-282">Volání kódu **GetFileName** metoda pro načtení názvu souboru (název objektu blob).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-282">The code calls the **GetFileName** method to retrieve the file name (blob name).</span></span> <span data-ttu-id="d4fb6-283">Kód je podobná výše uvedený kód slouží k získání cesty ke složce.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-283">The code is similar to the above code to get the folder path.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. <span data-ttu-id="d4fb6-284">Název souboru je zapsán vytvořením objektu URI.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-284">The name of the file is written by creating a URI object.</span></span> <span data-ttu-id="d4fb6-285">Konstruktor identifikátoru URI používá **BlobEndpoint** vlastnost vrátit název kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-285">The URI constructor uses the **BlobEndpoint** property to return the container name.</span></span> <span data-ttu-id="d4fb6-286">Název složky a cesta k souboru se přidají k vytvoření identifikátor URI objektu blob výstup.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-286">The folder path and file name are added to construct the output blob URI.</span></span>  

    ```csharp
    // Write the name of the file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. <span data-ttu-id="d4fb6-287">Název souboru byla zapsána a nyní můžete vytvořit řetězec výstup z **Calculate** metoda do nového objektu blob:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-287">The name of the file has been written and now you can write the output string from the **Calculate** method to a new blob:</span></span>

    ```csharp
    // Create a blob and upload the output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} to the output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-the-data-factory"></a><span data-ttu-id="d4fb6-288">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="d4fb6-288">Create the data factory</span></span>
<span data-ttu-id="d4fb6-289">V [vytvořit vlastní aktivitu](#create-the-custom-activity) části vytvořen vlastní aktivity a odesláno soubor zip binární soubory služby a souboru PDB na kontejner objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-289">In the [Create the custom activity](#create-the-custom-activity) section, you created a custom activity and uploaded the zip file with binaries and the PDB file to an Azure blob container.</span></span> <span data-ttu-id="d4fb6-290">V této části vytvoříte Azure **objekt pro vytváření dat** s **kanálu** používající **vlastní aktivity**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-290">In this section, you create an Azure **data factory** with a **pipeline** that uses the **custom activity**.</span></span>

<span data-ttu-id="d4fb6-291">Vstupní datové sady pro vlastní aktivita představuje objekty BLOB (soubory) ve vstupní složky (`mycontainer\\inputfolder`) v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-291">The input dataset for the custom activity represents the blobs (files) in the input folder (`mycontainer\\inputfolder`) in blob storage.</span></span> <span data-ttu-id="d4fb6-292">Představuje výstupní datovou sadu aktivity výstup objektů BLOB v zadané výstupní složce (`mycontainer\\outputfolder`) v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-292">The output dataset for the activity represents the output blobs in the output folder (`mycontainer\\outputfolder`) in blob storage.</span></span>

<span data-ttu-id="d4fb6-293">Vyřaďte jeden nebo více souborů ve složkách vstupní:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-293">Drop one or more files in the input folders:</span></span>

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

<span data-ttu-id="d4fb6-294">Například vyřaďte jeden soubor (soubor.txt) s následujícím obsahem do každé ze složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-294">For example, drop one file (file.txt) with the following content into each of the folders.</span></span>

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="d4fb6-295">Každé vstupní složky odpovídá řez v Azure Data Factory i v případě, že složka má 2 nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-295">Each input folder corresponds to a slice in Azure Data Factory even if the folder has 2 or more files.</span></span> <span data-ttu-id="d4fb6-296">Při zpracování každý řez v kanálu vlastní aktivita iteruje všechny objekty BLOB ve složce vstupní pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-296">When each slice is processed by the pipeline, the custom activity iterates through all the blobs in the input folder for that slice.</span></span>

<span data-ttu-id="d4fb6-297">Uvidíte pět výstupní soubory se stejným obsahem.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-297">You see five output files with the same content.</span></span> <span data-ttu-id="d4fb6-298">Například výstupní soubor z zpracování souboru ve složce 2015 11 16 00 má následující obsah:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-298">For example, the output file from processing the file in the 2015-11-16-00 folder has the following content:</span></span>

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
```

<span data-ttu-id="d4fb6-299">Pokud je do vstupní složky vyřadit více souborů (soubor.txt, Soubor2.txt, file3.txt) se stejným obsahem, zobrazí se následující obsah ve výstupním souboru.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-299">If you drop multiple files (file.txt, file2.txt, file3.txt) with the same content to the input folder, you see the following content in the output file.</span></span> <span data-ttu-id="d4fb6-300">Každé složky (2015-11-16-00 atd.) odpovídá řez v této ukázce to i v případě, že složka má více vstupních souborů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-300">Each folder (2015-11-16-00, etc.) corresponds to a slice in this sample even though the folder has multiple input files.</span></span>

```csharp
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file3.txt.
```

<span data-ttu-id="d4fb6-301">Výstupní soubor má tři řádky, jeden pro každý vstupní soubor (binární rozsáhlý objekt) ve složce přidružené řezu (2015-11-16-00).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-301">The output file has three lines now, one for each input file (blob) in the folder associated with the slice (2015-11-16-00).</span></span>

<span data-ttu-id="d4fb6-302">Úloha se vytvoří pro každou aktivitu spustit.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-302">A task is created for each activity run.</span></span> <span data-ttu-id="d4fb6-303">V této ukázce je jenom jedna aktivita v kanálu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-303">In this sample, there is only one activity in the pipeline.</span></span> <span data-ttu-id="d4fb6-304">Při zpracování řezu v kanálu vlastní aktivita běží na Azure Batch ke zpracování řezu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-304">When a slice is processed by the pipeline, the custom activity runs on Azure Batch to process the slice.</span></span> <span data-ttu-id="d4fb6-305">Vzhledem k tomu, že existují pět řezy (každý řez může mít více objektů BLOB nebo soubor), nejsou pět úkoly vytvořené v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-305">Since there are five slices (each slice can have multiple blobs or file), there are five tasks created in Azure Batch.</span></span> <span data-ttu-id="d4fb6-306">Když úloha běží na Batch, je ve skutečnosti vlastní aktivity, která je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-306">When a task runs on Batch, it is actually the custom activity that is running.</span></span>

<span data-ttu-id="d4fb6-307">Následující názorný postup obsahuje další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-307">The following walkthrough provides additional details.</span></span>

#### <a name="step-1-create-the-data-factory"></a><span data-ttu-id="d4fb6-308">Krok 1: Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="d4fb6-308">Step 1: Create the data factory</span></span>
1. <span data-ttu-id="d4fb6-309">Po přihlášení na [portál Azure](https://portal.azure.com/), proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-309">After logging in to the [Azure portal](https://portal.azure.com/), do the following steps:</span></span>

   1. <span data-ttu-id="d4fb6-310">V nabídce vlevo klikněte na **NOVÝ**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-310">Click **NEW** on the left menu.</span></span>
   2. <span data-ttu-id="d4fb6-311">Klikněte na tlačítko **Data + analýzy** v **nový** okno.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-311">Click **Data + Analytics** in the **New** blade.</span></span>
   3. <span data-ttu-id="d4fb6-312">V okně **Analýza dat** klikněte na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-312">Click **Data Factory** on the **Data analytics** blade.</span></span>
2. <span data-ttu-id="d4fb6-313">V **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro název.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-313">In the **New data factory** blade, enter **CustomActivityFactory** for the Name.</span></span> <span data-ttu-id="d4fb6-314">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-314">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="d4fb6-315">Pokud se zobrazí chyba: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte název objektu pro vytváření dat (například **yournameCustomActivityFactory**) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-315">If you receive the error: **Data factory name “CustomActivityFactory” is not available**, change the name of the data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>
3. <span data-ttu-id="d4fb6-316">Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-316">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="d4fb6-317">Ověřte, že používáte správné předplatné a oblasti, kde chcete objekt pro vytváření dat vytvořit.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-317">Verify that you are using the correct subscription and region where you want the data factory to be created.</span></span>
5. <span data-ttu-id="d4fb6-318">V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-318">Click **Create** on the **New data factory** blade.</span></span>
6. <span data-ttu-id="d4fb6-319">Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-319">You see the data factory being created in the **Dashboard** of the Azure portal.</span></span>
7. <span data-ttu-id="d4fb6-320">Po úspěšném vytvoření objektu pro vytváření dat se zobrazí stránka s obsahem objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-320">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a><span data-ttu-id="d4fb6-321">Krok 2: Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="d4fb6-321">Step 2: Create linked services</span></span>
<span data-ttu-id="d4fb6-322">Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-322">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="d4fb6-323">V tomto kroku propojíte vaše **Azure Storage** účtu a **Azure Batch** účet do data factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-323">In this step, you link your **Azure Storage** account and **Azure Batch** account to your data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="d4fb6-324">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d4fb6-324">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="d4fb6-325">Klikněte na tlačítko **vytvořit a nasadit** na dlaždici **objekt pro vytváření dat** okno pro **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-325">Click the **Author and deploy** tile on the **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="d4fb6-326">Zobrazí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-326">You see the Data Factory Editor.</span></span>
2. <span data-ttu-id="d4fb6-327">Klikněte na tlačítko **nové datové úložiště** na příkaz panelu a vyberte **úložiště Azure.**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-327">Click **New data store** on the command bar and choose **Azure storage.**</span></span> <span data-ttu-id="d4fb6-328">V editoru by se měl zobrazit skript JSON pro vytvoření propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-328">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. <span data-ttu-id="d4fb6-329">Nahraďte **název účtu** názvem účtu služby Azure Storage a **klíč účtu** přístupovým klíčem k účtu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-329">Replace **account name** with the name of your Azure storage account and **account key** with the access key of the Azure storage account.</span></span> <span data-ttu-id="d4fb6-330">Informace o tom, jak získat přístupový klíč k úložišti, najdete v článku o [zobrazení, kopírování a opětovném vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-330">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

4. <span data-ttu-id="d4fb6-331">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-331">Click **Deploy** on the command bar to deploy the linked service.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="d4fb6-332">Vytvoření služby Azure Batch propojené</span><span class="sxs-lookup"><span data-stu-id="d4fb6-332">Create Azure Batch linked service</span></span>
<span data-ttu-id="d4fb6-333">V tomto kroku vytvoříte propojené služby pro vaše **Azure Batch** účet, který se používá ke spuštění aktivity vlastní Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-333">In this step, you create a linked service for your **Azure Batch** account that is used to run the Data Factory custom activity.</span></span>

1. <span data-ttu-id="d4fb6-334">Klikněte na tlačítko **nový výpočet** na příkaz panelu a vyberte **Azure Batch.**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-334">Click **New compute** on the command bar and choose **Azure Batch.**</span></span> <span data-ttu-id="d4fb6-335">Měli byste vidět skript JSON pro vytvoření služby Azure Batch propojené v editoru.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-335">You should see the JSON script for creating an Azure Batch linked service in the editor.</span></span>
2. <span data-ttu-id="d4fb6-336">Ve skriptu JSON:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-336">In the JSON script:</span></span>

   1. <span data-ttu-id="d4fb6-337">Nahraďte **název účtu** s názvem vašeho účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-337">Replace **account name** with the name of your Azure Batch account.</span></span>
   2. <span data-ttu-id="d4fb6-338">Nahraďte **přístupový klíč** přístupovým klíčem k účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-338">Replace **access key** with the access key of the Azure Batch account.</span></span>
   3. <span data-ttu-id="d4fb6-339">Zadejte ID fondu **poolName** vlastnost**.**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-339">Enter the ID of the pool for the **poolName** property**.**</span></span> <span data-ttu-id="d4fb6-340">Pro tuto vlastnost lze zadat buď název fondu nebo fondu ID.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-340">For this property, you can specify either pool name or pool ID.</span></span>
   4. <span data-ttu-id="d4fb6-341">Zadejte batch identifikátor URI pro **batchUri** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-341">Enter the batch URI for the **batchUri** JSON property.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="d4fb6-342">**URL** z **okně účtu Azure Batch** je v následujícím formátu: \<accountname\>.\< oblast\>. batch.azure.com. Pro **batchUri** vlastností v kódu JSON, budete muset **odebrat "název účtu."**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-342">The **URL** from the **Azure Batch account blade** is in the following format: \<accountname\>.\<region\>.batch.azure.com. For the **batchUri** property in the JSON, you need to **remove "accountname."**</span></span> <span data-ttu-id="d4fb6-343">z adresy URL.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-343">from the URL.</span></span> <span data-ttu-id="d4fb6-344">Příklad: `"batchUri": "https://eastus.batch.azure.com"`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-344">Example: `"batchUri": "https://eastus.batch.azure.com"`.</span></span>
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      <span data-ttu-id="d4fb6-345">Pro **poolName** vlastnost, můžete také zadat ID fondu namísto názvu fondu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-345">For the **poolName** property, you can also specify the ID of the pool instead of the name of the pool.</span></span>

      > [!NOTE]
      > <span data-ttu-id="d4fb6-346">Služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-346">The Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="d4fb6-347">Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-347">You can only use your own Azure Batch pool in an Azure data factory.</span></span>
      >
      >
   5. <span data-ttu-id="d4fb6-348">Zadejte **StorageLinkedService** pro **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-348">Specify **StorageLinkedService** for the **linkedServiceName** property.</span></span> <span data-ttu-id="d4fb6-349">Tuto propojenou službu jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-349">You created this linked service in the previous step.</span></span> <span data-ttu-id="d4fb6-350">Toto úložiště se používá jako pracovní oblast pro soubory a protokoly.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-350">This storage is used as a staging area for files and logs.</span></span>
3. <span data-ttu-id="d4fb6-351">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-351">Click **Deploy** on the command bar to deploy the linked service.</span></span>

#### <a name="step-3-create-datasets"></a><span data-ttu-id="d4fb6-352">Krok 3: Vytvoření datové sady</span><span class="sxs-lookup"><span data-stu-id="d4fb6-352">Step 3: Create datasets</span></span>
<span data-ttu-id="d4fb6-353">V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-353">In this step, you create datasets to represent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="d4fb6-354">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="d4fb6-354">Create input dataset</span></span>
1. <span data-ttu-id="d4fb6-355">V **Editor** služby Data Factory klikněte na tlačítko **nová datová sada** na panelu nástrojů a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-355">In the **Editor** for the Data Factory, click **New dataset** button on the toolbar and click **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="d4fb6-356">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-356">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
       "name": "InputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
               "format": {
                   "type": "TextFormat"
               },
               "partitionedBy": [
                   {
                       "name": "Year",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy"
                       }
                   },
                   {
                       "name": "Month",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "MM"
                       }
                   },
                   {
                       "name": "Day",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "dd"
                       }
                   },
                   {
                       "name": "Hour",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           },
           "external": true,
           "policy": {}
       }
    }
    ```

    <span data-ttu-id="d4fb6-357">Vytvoření kanálu dále v tomto návodu se časem spuštění: 2015-11-16T00:00:00Z a koncový čas: 2015-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-357">You create a pipeline later in this walkthrough with start time: 2015-11-16T00:00:00Z and end time: 2015-11-16T05:00:00Z.</span></span> <span data-ttu-id="d4fb6-358">Je naplánována nevytvořila data **každou hodinu**, takže se 5 řezy vstupní a výstupní (mezi **00**: 00:00 -\> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-358">It is scheduled to produce data **hourly**, so there are 5 input/output slices (between **00**:00:00 -\> **05**:00:00).</span></span>

    <span data-ttu-id="d4fb6-359">**Frekvence** a **interval** vstupní datové sady je nastavený na **hodinu** a **1**, což znamená, že vstupní řez je k dispozici každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-359">The **frequency** and **interval** for the input dataset is set to **Hour** and **1**, which means that the input slice is available hourly.</span></span>

    <span data-ttu-id="d4fb6-360">Tady jsou časy zahájení pro každý řez, která je reprezentována **SliceStart** systémové proměnné ve výše uvedeném fragmentu JSON.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-360">Here are the start times for each slice, which is represented by **SliceStart** system variable in the above JSON snippet.</span></span>

    | <span data-ttu-id="d4fb6-361">**Řez**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-361">**Slice**</span></span> | <span data-ttu-id="d4fb6-362">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-362">**Start time**</span></span>          |
    |-----------|-------------------------|
    | <span data-ttu-id="d4fb6-363">1</span><span class="sxs-lookup"><span data-stu-id="d4fb6-363">1</span></span>         | <span data-ttu-id="d4fb6-364">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-364">2015-11-16T**00**:00:00</span></span> |
    | <span data-ttu-id="d4fb6-365">2</span><span class="sxs-lookup"><span data-stu-id="d4fb6-365">2</span></span>         | <span data-ttu-id="d4fb6-366">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-366">2015-11-16T**01**:00:00</span></span> |
    | <span data-ttu-id="d4fb6-367">3</span><span class="sxs-lookup"><span data-stu-id="d4fb6-367">3</span></span>         | <span data-ttu-id="d4fb6-368">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-368">2015-11-16T**02**:00:00</span></span> |
    | <span data-ttu-id="d4fb6-369">4</span><span class="sxs-lookup"><span data-stu-id="d4fb6-369">4</span></span>         | <span data-ttu-id="d4fb6-370">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-370">2015-11-16T**03**:00:00</span></span> |
    | <span data-ttu-id="d4fb6-371">5</span><span class="sxs-lookup"><span data-stu-id="d4fb6-371">5</span></span>         | <span data-ttu-id="d4fb6-372">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-372">2015-11-16T**04**:00:00</span></span> |

    <span data-ttu-id="d4fb6-373">**FolderPath** se vypočítává pomocí rok, měsíc, den a hodina součástí řez čas spuštění (**SliceStart**).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-373">The **folderPath** is calculated by using the year, month, day, and hour part of the slice start time (**SliceStart**).</span></span> <span data-ttu-id="d4fb6-374">Zde je proto jak vstupní složky je namapovaný na řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-374">Therefore, here is how an input folder is mapped to a slice.</span></span>

    | <span data-ttu-id="d4fb6-375">**Řez**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-375">**Slice**</span></span> | <span data-ttu-id="d4fb6-376">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-376">**Start time**</span></span>          | <span data-ttu-id="d4fb6-377">**Vstupní složky**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-377">**Input folder**</span></span>  |
    |-----------|-------------------------|-------------------|
    | <span data-ttu-id="d4fb6-378">1</span><span class="sxs-lookup"><span data-stu-id="d4fb6-378">1</span></span>         | <span data-ttu-id="d4fb6-379">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-379">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="d4fb6-380">2015-11-16-**00**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-380">2015-11-16-**00**</span></span> |
    | <span data-ttu-id="d4fb6-381">2</span><span class="sxs-lookup"><span data-stu-id="d4fb6-381">2</span></span>         | <span data-ttu-id="d4fb6-382">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-382">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="d4fb6-383">2015-11-16-**01**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-383">2015-11-16-**01**</span></span> |
    | <span data-ttu-id="d4fb6-384">3</span><span class="sxs-lookup"><span data-stu-id="d4fb6-384">3</span></span>         | <span data-ttu-id="d4fb6-385">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-385">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="d4fb6-386">2015-11-16-**02**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-386">2015-11-16-**02**</span></span> |
    | <span data-ttu-id="d4fb6-387">4</span><span class="sxs-lookup"><span data-stu-id="d4fb6-387">4</span></span>         | <span data-ttu-id="d4fb6-388">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-388">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="d4fb6-389">2015-11-16-**03**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-389">2015-11-16-**03**</span></span> |
    | <span data-ttu-id="d4fb6-390">5</span><span class="sxs-lookup"><span data-stu-id="d4fb6-390">5</span></span>         | <span data-ttu-id="d4fb6-391">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-391">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="d4fb6-392">2015-11-16-**04**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-392">2015-11-16-**04**</span></span> |

1. <span data-ttu-id="d4fb6-393">Klikněte na tlačítko **nasadit** na panelu nástrojů vytvořit a nasadit **InputDataset** tabulky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-393">Click **Deploy** on the toolbar to create and deploy the **InputDataset** table.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="d4fb6-394">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="d4fb6-394">Create output dataset</span></span>
<span data-ttu-id="d4fb6-395">V tomto kroku vytvoříte jinou datovou sadu typu AzureBlob, která bude představovat výstupní data.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-395">In this step, you create another dataset of type AzureBlob to represent the output data.</span></span>

1. <span data-ttu-id="d4fb6-396">V **Editor** služby Data Factory klikněte na tlačítko **nová datová sada** na panelu nástrojů a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-396">In the **Editor** for the Data Factory, click **New dataset** button on the toolbar and click **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="d4fb6-397">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-397">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
       "name": "OutputDataset",
       "properties": {
           "type": "AzureBlob",
           "linkedServiceName": "AzureStorageLinkedService",
           "typeProperties": {
               "fileName": "{slice}.txt",
               "folderPath": "mycontainer/outputfolder",
               "partitionedBy": [
                   {
                       "name": "slice",
                       "value": {
                           "type": "DateTime",
                           "date": "SliceStart",
                           "format": "yyyy-MM-dd-HH"
                       }
                   }
               ]
           },
           "availability": {
               "frequency": "Hour",
               "interval": 1
           }
       }
    }
    ```

    <span data-ttu-id="d4fb6-398">Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-398">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="d4fb6-399">Zde je, jak je výstupní soubor s názvem pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-399">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="d4fb6-400">Výstupní soubory jsou generovány v jednu výstupní složky: `mycontainer\\outputfolder`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-400">All the output files are generated in one output folder: `mycontainer\\outputfolder`.</span></span>

    | <span data-ttu-id="d4fb6-401">**Řez**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-401">**Slice**</span></span> | <span data-ttu-id="d4fb6-402">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-402">**Start time**</span></span>          | <span data-ttu-id="d4fb6-403">**Výstupní soubor**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-403">**Output file**</span></span>       |
    |-----------|-------------------------|-----------------------|
    | <span data-ttu-id="d4fb6-404">1</span><span class="sxs-lookup"><span data-stu-id="d4fb6-404">1</span></span>         | <span data-ttu-id="d4fb6-405">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-405">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="d4fb6-406">2015-11-16 -**00. txt**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-406">2015-11-16-**00.txt**</span></span> |
    | <span data-ttu-id="d4fb6-407">2</span><span class="sxs-lookup"><span data-stu-id="d4fb6-407">2</span></span>         | <span data-ttu-id="d4fb6-408">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-408">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="d4fb6-409">2015-11-16 -**01. txt**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-409">2015-11-16-**01.txt**</span></span> |
    | <span data-ttu-id="d4fb6-410">3</span><span class="sxs-lookup"><span data-stu-id="d4fb6-410">3</span></span>         | <span data-ttu-id="d4fb6-411">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-411">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="d4fb6-412">2015-11-16 -**02. txt**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-412">2015-11-16-**02.txt**</span></span> |
    | <span data-ttu-id="d4fb6-413">4</span><span class="sxs-lookup"><span data-stu-id="d4fb6-413">4</span></span>         | <span data-ttu-id="d4fb6-414">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-414">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="d4fb6-415">2015-11-16 -**03. txt**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-415">2015-11-16-**03.txt**</span></span> |
    | <span data-ttu-id="d4fb6-416">5</span><span class="sxs-lookup"><span data-stu-id="d4fb6-416">5</span></span>         | <span data-ttu-id="d4fb6-417">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="d4fb6-417">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="d4fb6-418">2015-11-16 -**04. txt**</span><span class="sxs-lookup"><span data-stu-id="d4fb6-418">2015-11-16-**04.txt**</span></span> |

    <span data-ttu-id="d4fb6-419">Nezapomeňte, že všechny soubory ve vstupní složky (například: 2015-11-16-00) jsou součástí řez se časem spuštění: 2015-11-16-00.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-419">Remember that all the files in an input folder (for example: 2015-11-16-00) are part of a slice with the start time: 2015-11-16-00.</span></span> <span data-ttu-id="d4fb6-420">Při zpracování této řezu se vlastní aktivita prohledává každý soubor a vytvoří řádek ve výstupním souboru s počtem výskytů hledaný termín ("Microsoft").</span><span class="sxs-lookup"><span data-stu-id="d4fb6-420">When this slice is processed, the custom activity scans through each file and produces a line in the output file with the number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="d4fb6-421">Pokud existují tři soubory ve složce 2015 11 16 00, se ve výstupním souboru tři řádky: 2015-11-16-00.txt.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-421">If there are three files in the folder 2015-11-16-00, there are three lines in the output file: 2015-11-16-00.txt.</span></span>

1. <span data-ttu-id="d4fb6-422">Klikněte na tlačítko **nasadit** na panelu nástrojů vytvořit a nasadit **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-422">Click **Deploy** on the toolbar to create and deploy the **OutputDataset**.</span></span>

#### <a name="step-4-create-and-run-the-pipeline-with-custom-activity"></a><span data-ttu-id="d4fb6-423">Krok 4: Vytvoření a spuštění kanálu s aktivitou vlastní</span><span class="sxs-lookup"><span data-stu-id="d4fb6-423">Step 4: Create and run the pipeline with custom activity</span></span>
<span data-ttu-id="d4fb6-424">V tomto kroku vytvoříte kanál s aktivitou jeden, vlastní aktivitu, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-424">In this step, you create a pipeline with one activity, the custom activity you created earlier.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d4fb6-425">Pokud ještě jste neodeslali **soubor.txt** jako vstup složek v kontejneru objektů blob, tak učinit před vytvořením kanálu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-425">If you haven't uploaded the **file.txt** to input folders in the blob container, do so before creating the pipeline.</span></span> <span data-ttu-id="d4fb6-426">**IsPaused** vlastnost nastavena na hodnotu false v kanálu formát JSON, takže se kanál okamžitě spustí jako **spustit** je datum v minulosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-426">The **isPaused** property is set to false in the pipeline JSON, so the pipeline runs immediately as the **start** date is in the past.</span></span>
>
>

1. <span data-ttu-id="d4fb6-427">V editoru služby Data Factory, klikněte na tlačítko **nový kanál** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-427">In the Data Factory Editor, click **New pipeline** on the command bar.</span></span> <span data-ttu-id="d4fb6-428">Pokud se příkaz nezobrazí, klikněte na tlačítko **... (Tři tečky)**  k jeho zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-428">If you do not see the command, click **... (Ellipsis)** to see it.</span></span>
2. <span data-ttu-id="d4fb6-429">Nahraďte kód JSON v pravém podokně se následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-429">Replace the JSON in the right pane with the following JSON script:</span></span>

    ```json
    {
       "name": "PipelineCustom",
       "properties": {
           "description": "Use custom activity",
           "activities": [
               {
                   "type": "DotNetActivity",
                   "typeProperties": {
                       "assemblyName": "MyDotNetActivity.dll",
                       "entryPoint": "MyDotNetActivityNS.MyDotNetActivity",
                       "packageLinkedService": "AzureStorageLinkedService",
                       "packageFile": "customactivitycontainer/MyDotNetActivity.zip"
                   },
                   "inputs": [
                       {
                           "name": "InputDataset"
                       }
                   ],
                   "outputs": [
                       {
                           "name": "OutputDataset"
                       }
                   ],
                   "policy": {
                       "timeout": "00:30:00",
                       "concurrency": 5,
                       "retry": 3
                   },
                   "scheduler": {
                       "frequency": "Hour",
                       "interval": 1
                   },
                   "name": "MyDotNetActivity",
                   "linkedServiceName": "AzureBatchLinkedService"
               }
           ],
           "start": "2015-11-16T00:00:00Z",
           "end": "2015-11-16T05:00:00Z",
           "isPaused": false
      }
    }
    ```
   <span data-ttu-id="d4fb6-430">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-430">Note the following points:</span></span>

   * <span data-ttu-id="d4fb6-431">Je jenom jedna aktivita v kanálu a který je typu: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-431">There is only one activity in the pipeline and that is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="d4fb6-432">**AssemblyName** nastavena na název knihovny DLL: **MyDotNetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-432">**AssemblyName** is set to the name of the DLL: **MyDotNetActivity.dll**.</span></span>
   * <span data-ttu-id="d4fb6-433">**EntryPoint** je nastaven na **MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-433">**EntryPoint** is set to **MyDotNetActivityNS.MyDotNetActivity**.</span></span> <span data-ttu-id="d4fb6-434">Je v podstatě \<obor názvů\>.\< Název třídy\> ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-434">It is basically \<namespace\>.\<classname\> in your code.</span></span>
   * <span data-ttu-id="d4fb6-435">**PackageLinkedService** je nastaven na **StorageLinkedService** který odkazuje na úložiště objektů blob, který obsahuje soubor zip vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-435">**PackageLinkedService** is set to **StorageLinkedService** that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="d4fb6-436">Pokud používáte jiné účty Azure Storage pro vstupní a výstupní soubory a soubor zip vlastní aktivity, musíte vytvořit jiný propojenou službu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-436">If you are using different Azure Storage accounts for input/output files and the custom activity zip file, you have to create another Azure Storage linked service.</span></span> <span data-ttu-id="d4fb6-437">Tento článek předpokládá, že používáte stejný účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-437">This article assumes that you are using the same Azure Storage account.</span></span>
   * <span data-ttu-id="d4fb6-438">**PackageFile** je nastaven na **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-438">**PackageFile** is set to **customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="d4fb6-439">Je ve formátu: \<containerforthezip\>/\<nameofthezip.zip\>.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-439">It is in the format: \<containerforthezip\>/\<nameofthezip.zip\>.</span></span>
   * <span data-ttu-id="d4fb6-440">Vlastní aktivita přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-440">The custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="d4fb6-441">**LinkedServiceName** vlastnost vlastní aktivity odkazuje na **AzureBatchLinkedService**, která říká službě Azure Data Factory, který vlastní aktivita je potřeba spustit v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-441">The **linkedServiceName** property of the custom activity points to the **AzureBatchLinkedService**, which tells Azure Data Factory that the custom activity needs to run on Azure Batch.</span></span>
   * <span data-ttu-id="d4fb6-442">**Souběžnosti** nastavení je důležité.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-442">The **concurrency** setting is important.</span></span> <span data-ttu-id="d4fb6-443">Pokud použijete výchozí hodnotu, která je 1, i v případě, že máte 2 nebo více výpočetních uzlů ve fondu Azure Batch, řezy jsou zpracovávány jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-443">If you use the default value, which is 1, even if you have 2 or more compute nodes in the Azure Batch pool, the slices are processed one after another.</span></span> <span data-ttu-id="d4fb6-444">Proto nejsou využívat výhod paralelní zpracování funkce Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-444">Therefore, you are not taking advantage of the parallel processing capability of Azure Batch.</span></span> <span data-ttu-id="d4fb6-445">Pokud nastavíte **souběžnosti** na vyšší hodnotu, například 2, znamená to, že dva řezy (odpovídá dvě úlohy v Azure Batch) může být zpracována ve stejnou dobu, v takovém případě oba virtuální počítače v Azure Batch jsou využité fondu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-445">If you set **concurrency** to a higher value, say 2, it means that two slices (corresponds to two tasks in Azure Batch) can be processed at the same time, in which case, both the VMs in the Azure Batch pool are utilized.</span></span> <span data-ttu-id="d4fb6-446">Proto správně nastavte vlastnost souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-446">Therefore, set the concurrency property appropriately.</span></span>
   * <span data-ttu-id="d4fb6-447">Pouze jednu úlohu (řez) je spustit na virtuálním počítači v libovolném bodě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-447">Only one task (slice) is executed on a VM at any point by default.</span></span> <span data-ttu-id="d4fb6-448">Důvodem je, že ve výchozím nastavení, **maximální počet úloh na virtuální počítač** je nastaven na hodnotu 1 pro fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-448">The reason is that, by default, the **Maximum tasks per VM** is set to 1 for an Azure Batch pool.</span></span> <span data-ttu-id="d4fb6-449">V rámci požadavků vytvořit fond se tato vlastnost nastavena na hodnotu 2, takže dva řezy objekt pro vytváření dat může být spuštěná na virtuálním počítači ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-449">As part of prerequisites, you created a pool with this property set to 2, so two Data Factory slices can be running on a VM at the same time.</span></span>

    -   <span data-ttu-id="d4fb6-450">**isPaused** je nastavena na hodnotu false ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-450">**isPaused** property is set to false by default.</span></span> <span data-ttu-id="d4fb6-451">Kanál se spustí okamžitě v tomto příkladu protože řezy spustit v minulosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-451">The pipeline runs immediately in this example because the slices start in the past.</span></span> <span data-ttu-id="d4fb6-452">Tuto vlastnost lze nastavit na hodnotu true pozastavení kanálu a nastavte ji zpět na hodnotu false restartovat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-452">You can set this property to true to pause the pipeline and set it back to false to restart.</span></span>

    -   <span data-ttu-id="d4fb6-453">**Spustit** čas a **end** časy jsou od sebe pět hodin a řezy vytváří každou hodinu, takže pět řezy vytváří v kanálu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-453">The **start** time and **end** times are five hours apart and slices are produced hourly, so five slices are produced by the pipeline.</span></span>

1. <span data-ttu-id="d4fb6-454">Kanál nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-454">Click **Deploy** on the command bar to deploy the pipeline.</span></span>

#### <a name="step-5-test-the-pipeline"></a><span data-ttu-id="d4fb6-455">Krok 5: Testování kanálu</span><span class="sxs-lookup"><span data-stu-id="d4fb6-455">Step 5: Test the pipeline</span></span>
<span data-ttu-id="d4fb6-456">V tomto kroku otestovat kanálu přetažením souborů do vstupní složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-456">In this step, you test the pipeline by dropping files into the input folders.</span></span> <span data-ttu-id="d4fb6-457">Začněme testování kanálu pomocí jeden soubor pro jeden vstupní složky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-457">Let’s start with testing the pipeline with one file per one input folder.</span></span>

1. <span data-ttu-id="d4fb6-458">V okně Data Factory na webu Azure portal, klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-458">In the Data Factory blade in the Azure portal, click **Diagram**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. <span data-ttu-id="d4fb6-459">V zobrazení diagramu, klikněte dvakrát na vstupní datové sady: **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-459">In the diagram view, double-click input dataset: **InputDataset**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. <span data-ttu-id="d4fb6-460">Měli byste vidět **InputDataset** okno s pěti všechny řezy připraven.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-460">You should see the **InputDataset** blade with all five slices ready.</span></span> <span data-ttu-id="d4fb6-461">Upozornění **čas spuštění ŘEZU** a **ŘEZ KONCOVÝ čas** pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-461">Notice the **SLICE START TIME** and **SLICE END TIME** for each slice.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. <span data-ttu-id="d4fb6-462">V **zobrazení diagramu**, klikněte na tlačítko **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-462">In the **Diagram View**, now click **OutputDataset**.</span></span>
5. <span data-ttu-id="d4fb6-463">Měli byste vidět, že pět Výstup řezy jsou ve stavu Připraveno, pokud již byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-463">You should see that the five output slices are in the Ready state if they have already been produced.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. <span data-ttu-id="d4fb6-464">Použití portálu Azure k zobrazení **úlohy** přidružené **řezy** a zjistit, jaké virtuálních počítačů, každý řez byla spuštěna na.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-464">Use Azure portal to view the **tasks** associated with the **slices** and see what VM each slice ran on.</span></span> <span data-ttu-id="d4fb6-465">V tématu [integraci služby Data Factory a Batch](#data-factory-and-batch-integration) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-465">See [Data Factory and Batch integration](#data-factory-and-batch-integration) section for details.</span></span>
7. <span data-ttu-id="d4fb6-466">Měli byste vidět výstupní soubory v `outputfolder` z `mycontainer` ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-466">You should see the output files in the `outputfolder` of `mycontainer` in your Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   <span data-ttu-id="d4fb6-467">Měli byste vidět pět výstupní soubory, jeden pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-467">You should see five output files, one for each input slice.</span></span> <span data-ttu-id="d4fb6-468">Každý výstupního souboru, musí mít obsah podobná následující výstup:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-468">Each of the output file should have content similar to the following output:</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-00/file.txt.
    ```
   <span data-ttu-id="d4fb6-469">Následující diagram znázorňuje, jak řezy Data Factory mapování na úkoly ve službě Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-469">The following diagram illustrates how the Data Factory slices map to tasks in Azure Batch.</span></span> <span data-ttu-id="d4fb6-470">V tomto příkladu má řez spustit pouze jeden.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-470">In this example, a slice has only one run.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. <span data-ttu-id="d4fb6-471">Nyní zkuste to prosím ještě s více soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-471">Now, let’s try with multiple files in a folder.</span></span> <span data-ttu-id="d4fb6-472">Vytvoření souborů: **Soubor2.txt**, **file3.txt**, **file4.txt**, a **file5.txt** se stejným obsahem jako soubor.txt ve složce:  **2015-11-06-01**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-472">Create files: **file2.txt**, **file3.txt**, **file4.txt**, and **file5.txt** with the same content as in file.txt in the folder: **2015-11-06-01**.</span></span>
9. <span data-ttu-id="d4fb6-473">V zadané výstupní složce **odstranit** výstupního souboru: **2015 11 16 01.txt**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-473">In the output folder, **delete** the output file: **2015-11-16-01.txt**.</span></span>
10. <span data-ttu-id="d4fb6-474">Nyní v **OutputDataset** okno, klikněte pravým tlačítkem na řez s **čas spuštění ŘEZU** nastavena na **11/16/2015 01:00:00 AM**a klikněte na tlačítko **spustit** na znovu spustit nebo zpětný-process řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-474">Now, in the **OutputDataset** blade, right-click the slice with **SLICE START TIME** set to **11/16/2015 01:00:00 AM**, and click **Run** to rerun/re-process the slice.</span></span> <span data-ttu-id="d4fb6-475">Řez teď má pět souborů místo jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-475">Now, the slice has five files instead of one file.</span></span>

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. <span data-ttu-id="d4fb6-476">Po spuštění řezu a její stav je **připraven**, ověřte obsah ve výstupním souboru pro tento řez (**2015 11 16 01.txt**) v `outputfolder` z `mycontainer` ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-476">After the slice runs and its status is **Ready**, verify the content in the output file for this slice (**2015-11-16-01.txt**) in the `outputfolder` of `mycontainer` in your blob storage.</span></span> <span data-ttu-id="d4fb6-477">Měla by existovat řádek pro každý soubor řezu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-477">There should be a line for each file of the slice.</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> <span data-ttu-id="d4fb6-478">Pokud výstupní soubor 2015-11-16-01.txt se neodstranila než to zkusíte s pěti vstupní soubory, zobrazí jeden řádek z předchozího spuštění řezu a pěti řádcích z aktuální spustit řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-478">If you did not delete the output file 2015-11-16-01.txt before trying with five input files, you see one line from the previous slice run and five lines from the current slice run.</span></span> <span data-ttu-id="d4fb6-479">Ve výchozím nastavení je obsah připojena do výstupního souboru, pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-479">By default, the content is appended to output file if it already exists.</span></span>
>
>

#### <a name="data-factory-and-batch-integration"></a><span data-ttu-id="d4fb6-480">Integrace služby Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-480">Data Factory and Batch integration</span></span>
<span data-ttu-id="d4fb6-481">Služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem: `adf-poolname:job-xxx`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-481">The Data Factory service creates a job in Azure Batch with the name: `adf-poolname:job-xxx`.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

<span data-ttu-id="d4fb6-483">Úloha v dané úloze se vytvoří při každém spuštění aktivity řezu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-483">A task in the job is created for each activity run of a slice.</span></span> <span data-ttu-id="d4fb6-484">Pokud je připravena k provedení 10 řezů, vytvoření 10 úkolů do úlohy.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-484">If there are 10 slices ready to be processed, 10 tasks are created in the job.</span></span> <span data-ttu-id="d4fb6-485">Můžete mít více než jeden řez spouštět současně, pokud máte několika výpočetních uzlech ve fondu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-485">You can have more than one slice running in parallel if you have multiple compute nodes in the pool.</span></span> <span data-ttu-id="d4fb6-486">Pokud > 1 je nastavena maximální úkolů na výpočetním uzlu, může být více než jeden řez na stejném výpočetním spuštěna.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-486">If the maximum tasks per compute node is set to > 1, there can be more than one slice running on the same compute.</span></span>

<span data-ttu-id="d4fb6-487">V tomto příkladu jsou pět řezů, takže pět úlohy v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-487">In this example, there are five slices, so five tasks in Azure Batch.</span></span> <span data-ttu-id="d4fb6-488">Pomocí **souběžnosti** nastavena na **5** v kanálu JSON v Azure Data Factory a **maximální počet úloh na virtuální počítač** nastavena na **2** ve fondu Azure Batch s **2** virtuálních počítačů, Rychlé úlohy běží (zkontrolujte, zda počáteční a koncový čas pro úlohy).</span><span class="sxs-lookup"><span data-stu-id="d4fb6-488">With the **concurrency** set to **5** in the pipeline JSON in Azure Data Factory and **Maximum tasks per VM** set to **2** in Azure Batch pool with **2** VMs, the tasks runs fast (check start and end times for tasks).</span></span>

<span data-ttu-id="d4fb6-489">Použití portálu k zobrazení dávkové úlohy a její úkoly, které jsou přidružené **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-489">Use the portal to view the Batch job and its tasks that are associated with the **slices** and see what VM each slice ran on.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-the-pipeline"></a><span data-ttu-id="d4fb6-491">Ladění kanálu</span><span class="sxs-lookup"><span data-stu-id="d4fb6-491">Debug the pipeline</span></span>
<span data-ttu-id="d4fb6-492">Ladění zahrnuje několik základních technik:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-492">Debugging consists of a few basic techniques:</span></span>

1. <span data-ttu-id="d4fb6-493">Pokud vstupní řez není nastaven na **připraven**, potvrďte, že vstupní složky struktura je správná a soubor.txt existuje ve vstupní složkách.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-493">If the input slice is not set to **Ready**, confirm that the input folder structure is correct and file.txt exists in the input folders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. <span data-ttu-id="d4fb6-494">V **Execute** metoda vlastní aktivity, použijte **IActivityLogger** objektu k protokolování informací, které vám pomůže vyřešit problémy.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-494">In the **Execute** method of your custom activity, use the **IActivityLogger** object to log information that helps you troubleshoot issues.</span></span> <span data-ttu-id="d4fb6-495">Zprávy zaznamenané v uživatele zobrazí\_souboru protokolu 0.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-495">The logged messages show up in the user\_0.log file.</span></span>

   <span data-ttu-id="d4fb6-496">V **OutputDataset** okně klikněte na tlačítko řez zobrazíte **datový ŘEZ** okno pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-496">In the **OutputDataset** blade, click the slice to see the **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="d4fb6-497">Zobrazí **běh aktivit** pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-497">You see **activity runs** for that slice.</span></span> <span data-ttu-id="d4fb6-498">Měli byste vidět jednu aktivitu spustit řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-498">You should see one activity run for the slice.</span></span> <span data-ttu-id="d4fb6-499">Pokud kliknete na tlačítko **spustit** na panelu příkazů můžete spustit jiné aktivity při spuštění stejné řez.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-499">If you click **Run** in the command bar, you can start another activity run for the same slice.</span></span>

   <span data-ttu-id="d4fb6-500">Když kliknete na aktivity při spuštění, uvidíte **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-500">When you click the activity run, you see the **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="d4fb6-501">Zobrazí zprávy zaznamenané v **uživatele\_0 protokolu** souboru.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-501">You see logged messages in the **user\_0.log** file.</span></span> <span data-ttu-id="d4fb6-502">Když dojde k chybě, zobrazí tři běh aktivit, protože počet opakování je nastavena na hodnotu 3 v kódu JSON kanálu nebo aktivity.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-502">When an error occurs, you see three activity runs because the retry count is set to 3 in the pipeline/activity JSON.</span></span> <span data-ttu-id="d4fb6-503">Po kliknutí na tlačítko spustit aktivitu, zobrazí se soubory protokolů, které můžete zkontrolovat, chcete-li vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-503">When you click the activity run, you see the log files that you can review to troubleshoot the error.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   <span data-ttu-id="d4fb6-504">V seznamu souborů protokolu, klikněte na **uživatele 0.log**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-504">In the list of log files, click the **user-0.log**.</span></span> <span data-ttu-id="d4fb6-505">V pravém panelu jsou výsledky pomocí **IActivityLogger.Write** metoda.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-505">In the right panel are the results of using the **IActivityLogger.Write** method.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   <span data-ttu-id="d4fb6-506">Kontrola systému 0.log pro všechny chybové zprávy systému a výjimky.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-506">Check system-0.log for any system error messages and exceptions.</span></span>

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. <span data-ttu-id="d4fb6-507">Zahrnout **PDB** souborů v souboru zip tak, aby podrobnosti o chybě informace, jako **zásobník volání** když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-507">Include the **PDB** file in the zip file so that the error details have information such as **call stack** when an error occurs.</span></span>
4. <span data-ttu-id="d4fb6-508">Všechny soubory v souboru zip vlastní aktivity musí být na **top úroveň** s ne v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-508">All the files in the zip file for the custom activity must be at the **top level** with no subfolders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. <span data-ttu-id="d4fb6-509">Ujistěte se, že **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (by měla odkazovat na Azure blob storage, který obsahuje soubor zip) jsou nastaveny na správný hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-509">Ensure that the **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point to the Azure blob storage that contains the zip file) are set to correct values.</span></span>
6. <span data-ttu-id="d4fb6-510">Pokud jste opravili chybu a chcete řez zpracovat znovu, klikněte na něj v okně **OutputDataset** pravým tlačítkem myši a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-510">If you fixed an error and want to reprocess the slice, right-click the slice in the **OutputDataset** blade and click **Run**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > <span data-ttu-id="d4fb6-511">Zobrazí **kontejneru** ve službě Azure Blob storage s názvem: `adfjobs`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-511">You see a **container** in your Azure Blob storage named: `adfjobs`.</span></span> <span data-ttu-id="d4fb6-512">Tento kontejner není automaticky odstraněn, ale můžete bezpečně odstranit po skončení testování řešení.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-512">This container is not automatically deleted, but you can safely delete it after you are done testing the solution.</span></span> <span data-ttu-id="d4fb6-513">Podobně řešení Data Factory vytvoří Azure Batch **úlohy** s názvem: `adf-\<pool ID/name\>:job-0000000001`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-513">Similarly, the Data Factory solution creates an Azure Batch **job** named: `adf-\<pool ID/name\>:job-0000000001`.</span></span> <span data-ttu-id="d4fb6-514">Po dokončení testu řešení Pokud chcete, můžete odstranit tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-514">You can delete this job after you test the solution if you like.</span></span>
   >
   >
7. <span data-ttu-id="d4fb6-515">Vlastní aktivity nepoužívá **app.config** soubor ze svého balíčku.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-515">The custom activity does not use the **app.config** file from your package.</span></span> <span data-ttu-id="d4fb6-516">Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru, ale nefunguje za běhu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-516">Therefore, if your code reads any connection strings from the configuration file, it does not work at runtime.</span></span> <span data-ttu-id="d4fb6-517">Osvědčeným postupem při použití Azure Batch je pro uložení všech tajných klíčů v **Azure KeyVault**, použijte objekt služby založené na certifikátech k ochraně keyvault a distribuovat certifikát do fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-517">The best practice when using Azure Batch is to hold any secrets in an **Azure KeyVault**, use a certificate-based service principal to protect the keyvault, and distribute the certificate to Azure Batch pool.</span></span> <span data-ttu-id="d4fb6-518">Vlastní aktivita .NET potom má přístup k tajným klíčům v trezoru za běhu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-518">The .NET custom activity then can access secrets from the KeyVault at runtime.</span></span> <span data-ttu-id="d4fb6-519">Toto řešení je obecný a můžete škálovat k libovolnému typu tajný klíč, ne jenom připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-519">This solution is a generic one and can scale to any type of secret, not just connection string.</span></span>

    <span data-ttu-id="d4fb6-520">Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** pomocí nastavení připojovacího řetězce, vytvořit datovou sadu, která používá propojené služby a řetězu datovou sadu jako fiktivní vstupní datové sady do vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-520">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses the linked service, and chain the dataset as a dummy input dataset to the custom .NET activity.</span></span> <span data-ttu-id="d4fb6-521">Máte přístup připojovací řetězec propojené služby v kódu vlastní aktivity a by bez problémů fungují za běhu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-521">You can then access the linked service's connection string in the custom activity code and it should work fine at runtime.</span></span>  

#### <a name="extend-the-sample"></a><span data-ttu-id="d4fb6-522">Ukázka rozšíření</span><span class="sxs-lookup"><span data-stu-id="d4fb6-522">Extend the sample</span></span>
<span data-ttu-id="d4fb6-523">Tato ukázka se dozvíte více o Azure Data Factory a Azure Batch funkce můžete rozšířit.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-523">You can extend this sample to learn more about Azure Data Factory and Azure Batch features.</span></span> <span data-ttu-id="d4fb6-524">Například ke zpracování řezů v jiné časové rozmezí, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-524">For example, to process slices in a different time range, do the following steps:</span></span>

1. <span data-ttu-id="d4fb6-525">Přidejte následující podsložky v `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 a umístěte vstupní soubory v těchto složkách.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-525">Add the following subfolders in the `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 and place input files in those folders.</span></span> <span data-ttu-id="d4fb6-526">Změnit koncový čas pro kanál z `2015-11-16T05:00:00Z` k `2015-11-16T10:00:00Z`.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-526">Change the end time for the pipeline from `2015-11-16T05:00:00Z` to `2015-11-16T10:00:00Z`.</span></span> <span data-ttu-id="d4fb6-527">V **zobrazení diagramu**, dvakrát klikněte **InputDataset**a potvrďte, že vstupní řezy jsou připraveny.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-527">In the **Diagram View**, double-click the **InputDataset**, and confirm that the input slices are ready.</span></span> <span data-ttu-id="d4fb6-528">Klikněte dvakrát na **OuptutDataset** zobrazíte stav výstup řezy.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-528">Double-click **OuptutDataset** to see the state of output slices.</span></span> <span data-ttu-id="d4fb6-529">Pokud jsou ve stavu Připraveno, zkontrolujte výstupní složky pro výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-529">If they are in Ready state, check the output folder for the output files.</span></span>
2. <span data-ttu-id="d4fb6-530">Zvětšit nebo zmenšit **souběžnosti** nastavení pochopit, jak ovlivňuje výkon vašeho řešení, zejména zpracování, k níž dojde v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-530">Increase or decrease the **concurrency** setting to understand how it affects the performance of your solution, especially the processing that occurs on Azure Batch.</span></span> <span data-ttu-id="d4fb6-531">(Viz krok 4: vytvoření a spuštění kanálu Další informace **souběžnosti** nastavení.)</span><span class="sxs-lookup"><span data-stu-id="d4fb6-531">(See Step 4: Create and run the pipeline for more on the **concurrency** setting.)</span></span>
3. <span data-ttu-id="d4fb6-532">Vytvoření fondu s vyšší nebo nižší **maximální počet úloh na virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-532">Create a pool with higher/lower **Maximum tasks per VM**.</span></span> <span data-ttu-id="d4fb6-533">Pokud chcete používat nový fond, kterou jste vytvořili, aktualizujte službu Azure Batch propojené v řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-533">To use the new pool you created, update the Azure Batch linked service in the Data Factory solution.</span></span> <span data-ttu-id="d4fb6-534">(Viz krok 4: vytvoření a spuštění kanálu Další informace **maximální počet úloh na virtuální počítač** nastavení.)</span><span class="sxs-lookup"><span data-stu-id="d4fb6-534">(See Step 4: Create and run the pipeline for more on the **Maximum tasks per VM** setting.)</span></span>
4. <span data-ttu-id="d4fb6-535">Vytvoření fondu Azure Batch s **škálování** funkce.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-535">Create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="d4fb6-536">Automatické škálování výpočetních uzlů ve fondu Azure Batch je dynamické přizpůsobení výpočetní výkon, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-536">Automatically scaling compute nodes in an Azure Batch pool is the dynamic adjustment of processing power used by your application.</span></span> 

    <span data-ttu-id="d4fb6-537">Vzorec ukázka tady dosahuje následující chování: při vytvoření fondu, začne s virtuálním Počítačem 1.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-537">The sample formula here achieves the following behavior: When the pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="d4fb6-538">Metrika $PendingTasks definuje počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-538">$PendingTasks metric defines the number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="d4fb6-539">Vzorec najde průměrný počet úkolů čekajících na zpracování v posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-539">The formula finds the average number of pending tasks in the last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="d4fb6-540">Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-540">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="d4fb6-541">Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování zmenšuje těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-541">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and the autoscaling shrinks those VMs.</span></span> <span data-ttu-id="d4fb6-542">startingNumberOfVMs a maxNumberofVMs lze upravit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-542">startingNumberOfVMs and maxNumberofVMs can be adjusted to your needs.</span></span>
 
    <span data-ttu-id="d4fb6-543">Vzorec škálování:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-543">Autoscale formula:</span></span>

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   <span data-ttu-id="d4fb6-544">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../batch/batch-automatic-scaling.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-544">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

   <span data-ttu-id="d4fb6-545">Pokud fondu používá výchozí [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), služba Batch může trvat 15 až 30 minut Příprava virtuálního počítače před spuštěním vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-545">If the pool is using the default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), the Batch service could take 15-30 minutes to prepare the VM before running the custom activity.</span></span>  <span data-ttu-id="d4fb6-546">Pokud fondu používá jiný autoScaleEvaluationInterval, služba Batch může trvat autoScaleEvaluationInterval + 10 minut.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-546">If the pool is using a different autoScaleEvaluationInterval, the Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>
5. <span data-ttu-id="d4fb6-547">V ukázkové řešení **Execute** metoda vyvolá **Calculate** metoda, která zpracuje řez vstupní data k vytvoření řez výstupních dat.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-547">In the sample solution, the **Execute** method invokes the **Calculate** method that processes an input data slice to produce an output data slice.</span></span> <span data-ttu-id="d4fb6-548">Můžete napsat vlastní metodu ke zpracování vstupních dat a nahraďte volání metody Calculate v Metoda Execute volat metodu.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-548">You can write your own method to process input data and replace the Calculate method call in the Execute method with a call to your method.</span></span>

### <a name="next-steps-consume-the-data"></a><span data-ttu-id="d4fb6-549">Další kroky: využívat data</span><span class="sxs-lookup"><span data-stu-id="d4fb6-549">Next steps: Consume the data</span></span>
<span data-ttu-id="d4fb6-550">Po při zpracování dat, budete moct pracovat s online nástroje, například **Microsoft Power BI**.</span><span class="sxs-lookup"><span data-stu-id="d4fb6-550">After you process data, you can consume it with online tools like **Microsoft Power BI**.</span></span> <span data-ttu-id="d4fb6-551">Tady jsou odkazy, které vám pomohou pochopit Power BI a způsobu jeho použití v Azure:</span><span class="sxs-lookup"><span data-stu-id="d4fb6-551">Here are links to help you understand Power BI and how to use it in Azure:</span></span>

* [<span data-ttu-id="d4fb6-552">Prozkoumejte datovou sadu v Power BI</span><span class="sxs-lookup"><span data-stu-id="d4fb6-552">Explore a dataset in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [<span data-ttu-id="d4fb6-553">Začínáme s Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="d4fb6-553">Getting started with the Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [<span data-ttu-id="d4fb6-554">Aktualizovat data v Power BI</span><span class="sxs-lookup"><span data-stu-id="d4fb6-554">Refresh data in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [<span data-ttu-id="d4fb6-555">Azure a Power BI – základní – přehled</span><span class="sxs-lookup"><span data-stu-id="d4fb6-555">Azure and Power BI - basic overview</span></span>](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a><span data-ttu-id="d4fb6-556">Odkazy</span><span class="sxs-lookup"><span data-stu-id="d4fb6-556">References</span></span>
* [<span data-ttu-id="d4fb6-557">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="d4fb6-557">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

  * [<span data-ttu-id="d4fb6-558">Úvod do služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="d4fb6-558">Introduction to Azure Data Factory service</span></span>](data-factory-introduction.md)
  * [<span data-ttu-id="d4fb6-559">Začínáme s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="d4fb6-559">Get started with Azure Data Factory</span></span>](data-factory-build-your-first-pipeline.md)
  * [<span data-ttu-id="d4fb6-560">Použití vlastních aktivit v kanálu Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="d4fb6-560">Use custom activities in an Azure Data Factory pipeline</span></span>](data-factory-use-custom-activities.md)
* [<span data-ttu-id="d4fb6-561">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-561">Azure Batch</span></span>](https://azure.microsoft.com/documentation/services/batch/)

  * [<span data-ttu-id="d4fb6-562">Základy služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-562">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
  * [<span data-ttu-id="d4fb6-563">Přehled funkcí Azure Batch</span><span class="sxs-lookup"><span data-stu-id="d4fb6-563">Overview of Azure Batch features</span></span>](../batch/batch-api-basics.md)
  * [<span data-ttu-id="d4fb6-564">Vytvoření a Správa účtu Azure Batch na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d4fb6-564">Create and manage Azure Batch account in the Azure portal</span></span>](../batch/batch-account-create-portal.md)
  * [<span data-ttu-id="d4fb6-565">Začínáme s Azure Batch Library pro .NET</span><span class="sxs-lookup"><span data-stu-id="d4fb6-565">Get started with Azure Batch Library .NET</span></span>](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
