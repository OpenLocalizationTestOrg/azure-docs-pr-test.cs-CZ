---
title: "aaaProcess rozsáhlých datových sad, pomocí služby Data Factory a Batch | Microsoft Docs"
description: "Popisuje, jak kanálu tooprocess obrovské objemy dat v objektu pro vytváření dat Azure pomocí paralelní zpracování funkce Azure Batch."
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
ms.openlocfilehash: 6788f02de555d2e9d6588cc990a39043866d7e97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-large-scale-datasets-using-data-factory-and-batch"></a><span data-ttu-id="5f92b-103">Zpracování rozsáhlých datových sad pomocí služeb Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-103">Process large-scale datasets using Data Factory and Batch</span></span>
<span data-ttu-id="5f92b-104">Tento článek popisuje architekturu ukázkové řešení, které přesune a zpracuje rozsáhlých datových sad automatické a naplánované způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f92b-104">This article describes an architecture of a sample solution that moves and processes large-scale datasets in an automatic and scheduled manner.</span></span> <span data-ttu-id="5f92b-105">Také poskytuje návod začátku do konce tooimplement hello řešení pomocí Azure Data Factory a Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-105">It also provides an end-to-end walkthrough tooimplement hello solution using Azure Data Factory and Azure Batch.</span></span>

<span data-ttu-id="5f92b-106">Tento článek je delší než náš typické článek, protože obsahuje návod celé ukázkové řešení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-106">This article is longer than our typical article because it contains a walkthrough of an entire sample solution.</span></span> <span data-ttu-id="5f92b-107">Pokud jste nový tooBatch a objekt pro vytváření dat, můžete další informace o těchto službách a jak pracují společně.</span><span class="sxs-lookup"><span data-stu-id="5f92b-107">If you are new tooBatch and Data Factory, you can learn about these services and how they work together.</span></span> <span data-ttu-id="5f92b-108">Pokud znáte něco o službách hello a jsou návrhu nebo architektury řešení, může soustředit jenom na hello [části architektura](#architecture-of-sample-solution) hello článku a pokud vyvíjíte prototypu nebo řešení, můžete také tootry out podrobné pokyny v hello [návod](#implementation-of-sample-solution).</span><span class="sxs-lookup"><span data-stu-id="5f92b-108">If you know something about hello services and are designing/architecting a solution, you may focus just on hello [architecture section](#architecture-of-sample-solution) of hello article and if you are developing a prototype or a solution, you may also want tootry out step-by-step instructions in hello [walkthrough](#implementation-of-sample-solution).</span></span> <span data-ttu-id="5f92b-109">Doporučujeme komentář k tomuto obsahu a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="5f92b-109">We invite your comments about this content and how you use it.</span></span>

<span data-ttu-id="5f92b-110">Nejprve podíváme, jak služby Data Factory a Batch vám mohou pomoci s zpracování rozsáhlých datových sad v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-110">First, let's look at how Data Factory and Batch services can help with processing large datasets in hello cloud.</span></span>     

## <a name="why-azure-batch"></a><span data-ttu-id="5f92b-111">Proč Azure Batch?</span><span class="sxs-lookup"><span data-stu-id="5f92b-111">Why Azure Batch?</span></span>
<span data-ttu-id="5f92b-112">Azure Batch umožňuje efektivně v cloudu hello aplikace toorun rozsáhlé paralelní a vysoce výkonné výpočty (HPC).</span><span class="sxs-lookup"><span data-stu-id="5f92b-112">Azure Batch enables you toorun large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="5f92b-113">Je to služba platformy, která plánuje výpočetně náročných toorun ve spravované kolekci virtuálních počítačů, a dokáže automaticky škálovat výpočetní prostředky toomeet hello potřeby vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="5f92b-113">It's a platform service that schedules compute-intensive work toorun on a managed collection of virtual machines, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span>

<span data-ttu-id="5f92b-114">S hello služby Batch definujete výpočetní prostředky tooexecute aplikace paralelně a škálovaně.</span><span class="sxs-lookup"><span data-stu-id="5f92b-114">With hello Batch service, you define Azure compute resources tooexecute your applications in parallel, and at scale.</span></span> <span data-ttu-id="5f92b-115">Můžete spustit na vyžádání nebo naplánované úlohy a není nutné toomanually vytvářet, konfigurovat a spravovat HPC cluster, jednotlivé virtuální počítače, virtuální sítě nebo komplexních úloh a úloh plánování infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="5f92b-115">You can run on-demand or scheduled jobs, and you don't need toomanually create, configure, and manage an HPC cluster, individual virtual machines, virtual networks, or a complex job and task scheduling infrastructure.</span></span>

<span data-ttu-id="5f92b-116">V tématu hello následující články, pokud nejste obeznámeni s Azure Batch, protože pomáhá při pochopení hello architektura a implementace řešení hello popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5f92b-116">See hello following articles if you are not familiar with Azure Batch as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>   

* [<span data-ttu-id="5f92b-117">Základy služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-117">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
* [<span data-ttu-id="5f92b-118">Přehled funkcí Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-118">Batch feature overview</span></span>](../batch/batch-api-basics.md)

<span data-ttu-id="5f92b-119">(volitelné) toolearn Další informace o Azure Batch, najdete v části hello [studijní Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span><span class="sxs-lookup"><span data-stu-id="5f92b-119">(optional) toolearn more about Azure Batch, see hello [Learning path for Azure Batch](https://azure.microsoft.com/documentation/learning-paths/batch/).</span></span>

## <a name="why-azure-data-factory"></a><span data-ttu-id="5f92b-120">Proč pro vytváření dat Azure?</span><span class="sxs-lookup"><span data-stu-id="5f92b-120">Why Azure Data Factory?</span></span>
<span data-ttu-id="5f92b-121">Objekt pro vytváření dat je služba pro integraci dat založené na cloudu, která orchestruje a automatizuje hello přesouvání a transformaci dat.</span><span class="sxs-lookup"><span data-stu-id="5f92b-121">Data Factory is a cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="5f92b-122">Pomocí služby Data Factory hello, můžete vytvořit spravované datové kanály, které přesun dat z místní a cloudové úložiště dat úložiště tooa centralizované data (například: Azure Blob Storage) a proces nebo transformace dat pomocí služeb, jako je například Azure HDInsight a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5f92b-122">Using hello Data Factory service, you can create managed data pipelines that move data from on-premises and cloud data stores tooa centralized data store (for example: Azure Blob Storage), and process/transform data using services such as Azure HDInsight and Azure Machine Learning.</span></span> <span data-ttu-id="5f92b-123">Můžete také plánovat datové kanály toorun naplánované způsobem (hodinové, denní, týdenní, atd.) a monitorování a spravovat na problémy první pohled tooidentify a provést akci.</span><span class="sxs-lookup"><span data-stu-id="5f92b-123">You can also schedule data pipelines toorun in a scheduled manner (hourly, daily, weekly, etc.) and monitor and manage them at a glance tooidentify issues and take action.</span></span>

<span data-ttu-id="5f92b-124">V tématu hello následující články, pokud nejste obeznámeni s Azure Data Factory, protože pomáhá při pochopení hello architektura a implementace řešení hello popsané v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="5f92b-124">See hello following articles if you are not familiar with Azure Data Factory as it helps with understanding hello architecture/implementation of hello solution described in this article.</span></span>  

* [<span data-ttu-id="5f92b-125">Úvod objektu pro vytváření dat Azure</span><span class="sxs-lookup"><span data-stu-id="5f92b-125">Introduction of Azure Data Factory</span></span>](data-factory-introduction.md)
* [<span data-ttu-id="5f92b-126">Sestavit svůj první kanál dat</span><span class="sxs-lookup"><span data-stu-id="5f92b-126">Build your first data pipeline</span></span>](data-factory-build-your-first-pipeline.md)   

<span data-ttu-id="5f92b-127">(volitelné) toolearn Další informace o Azure Data Factory najdete v části hello [studijní postup Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="5f92b-127">(optional) toolearn more about Azure Data Factory, see hello [Learning path for Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/).</span></span>

## <a name="data-factory-and-batch-together"></a><span data-ttu-id="5f92b-128">Objekt pro vytváření dat a společně Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-128">Data Factory and Batch together</span></span>
<span data-ttu-id="5f92b-129">Objekt pro vytváření dat obsahuje zabudované aktivity, jako např. aktivity kopírování toocopy nebo přesunout data ze zdrojových dat úložiště tooa cílového úložiště dat a dat tooprocess Hive aktivity pomocí clusterů systému Hadoop (HDInsight) v Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-129">Data Factory includes built-in activities such as Copy Activity toocopy/move data from a source data store tooa destination data store and Hive Activity tooprocess data using Hadoop clusters (HDInsight) on Azure.</span></span> <span data-ttu-id="5f92b-130">V tématu [aktivit transformace dat](data-factory-data-transformation-activities.md) seznam aktivit transformace podporované.</span><span class="sxs-lookup"><span data-stu-id="5f92b-130">See [Data Transformation Activities](data-factory-data-transformation-activities.md) for a list of supported transformation activities.</span></span>

<span data-ttu-id="5f92b-131">Také umožňuje toomove nebo proces data s vlastní logikou jste toocreate vlastní aktivity rozhraní .NET a spusťte tyto aktivity v clusteru Azure HDInsight nebo na fondu Azure Batch virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-131">It also allows you toocreate custom .NET activities toomove or process data with your own logic and run these activities on an Azure HDInsight cluster or on an Azure Batch pool of VMs.</span></span> <span data-ttu-id="5f92b-132">Při použití služby Azure Batch, můžete nakonfigurovat fond hello tooauto škálování (Přidat nebo odebrat virtuální počítače založené na pracovním vytížení hello) podle vzorce zadáte.</span><span class="sxs-lookup"><span data-stu-id="5f92b-132">When you use Azure Batch, you can configure hello pool tooauto-scale (add or remove VMs based on hello workload) based on a formula you provide.</span></span>     

## <a name="architecture-of-sample-solution"></a><span data-ttu-id="5f92b-133">Architektura ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="5f92b-133">Architecture of sample solution</span></span>
<span data-ttu-id="5f92b-134">I když hello architektury popsané v tomto článku je pro jednoduchým řešením, je důležité toocomplex scénáře, jako je riziko modelování finančních služeb, zpracování obrázků a vykreslování a Genomické analysis.</span><span class="sxs-lookup"><span data-stu-id="5f92b-134">Even though hello architecture described in this article is for a simple solution, it is relevant toocomplex scenarios such as risk modeling by financial services, image processing and rendering, and genomic analysis.</span></span>

<span data-ttu-id="5f92b-135">Hello diagram znázorňuje, 1) jak Data Factory orchestruje přesun dat a zpracování a 2) jak Azure Batch zpracovává hello data paralelní způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f92b-135">hello diagram illustrates 1) how Data Factory orchestrates data movement and processing and 2) how Azure Batch processes hello data in a parallel manner.</span></span> <span data-ttu-id="5f92b-136">Stažení a tisku hello diagram referenční (11 × 17 palců.</span><span class="sxs-lookup"><span data-stu-id="5f92b-136">Download and print hello diagram for easy reference (11 x 17 in.</span></span> <span data-ttu-id="5f92b-137">nebo velikost A3): [orchestration HPC a data pomocí Azure Batch a objektu pro vytváření dat](http://go.microsoft.com/fwlink/?LinkId=717686).</span><span class="sxs-lookup"><span data-stu-id="5f92b-137">or A3 size): [HPC and data orchestration using Azure Batch and Data Factory](http://go.microsoft.com/fwlink/?LinkId=717686).</span></span>

<span data-ttu-id="5f92b-138">[![Diagram zpracování velkých dat](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span><span class="sxs-lookup"><span data-stu-id="5f92b-138">[![Large-scale data processing diagram](./media/data-factory-data-processing-using-batch/image1.png)](http://go.microsoft.com/fwlink/?LinkId=717686)</span></span>

<span data-ttu-id="5f92b-139">Hello následující seznam uvádí základní kroky procesu hello hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-139">hello following list provides hello basic steps of hello process.</span></span> <span data-ttu-id="5f92b-140">Hello řešení zahrnuje kód a vysvětlení toobuild hello začátku do konce řešení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-140">hello solution includes code and explanations toobuild hello end-to-end solution.</span></span>

1. <span data-ttu-id="5f92b-141">**Nakonfigurovat fond výpočetních uzlů (VM) Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-141">**Configure Azure Batch with a pool of compute nodes (VMs)**.</span></span> <span data-ttu-id="5f92b-142">Můžete zadat hello počet uzlů a velikost každého uzlu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-142">You can specify hello number of nodes and size of each node.</span></span>
2. <span data-ttu-id="5f92b-143">**Vytvoření instance služby Azure Data Factory** nakonfigurovaný s entitami, které představují Azure blob storage, výpočetní služby Azure Batch, vstupní a výstupní data a pracovní postup nebo kanálu s aktivitami, které přesunout a transformovat data.</span><span class="sxs-lookup"><span data-stu-id="5f92b-143">**Create an Azure Data Factory instance** that is configured with entities that represent Azure blob storage, Azure Batch compute service, input/output data, and a workflow/pipeline with activities that move and transform data.</span></span>
3. <span data-ttu-id="5f92b-144">**Vytvořit vlastní .NET aktivitu v kanálu pro vytváření dat hello**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-144">**Create a custom .NET activity in hello Data Factory pipeline**.</span></span> <span data-ttu-id="5f92b-145">Aktivita Hello je uživatelského kódu, který běží na hello fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-145">hello activity is your user code that runs on hello Azure Batch pool.</span></span>
4. <span data-ttu-id="5f92b-146">**Ukládat velké množství vstupních dat jako objekty BLOB v úložišti Azure**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-146">**Store large amounts of input data as blobs in Azure storage**.</span></span> <span data-ttu-id="5f92b-147">Data je rozdělena do logické řezy (obvykle čas).</span><span class="sxs-lookup"><span data-stu-id="5f92b-147">Data is divided into logical slices (usually by time).</span></span>
5. <span data-ttu-id="5f92b-148">**Objekt pro vytváření dat zkopíruje data, která zpracovává paralelně** toohello sekundárního umístění.</span><span class="sxs-lookup"><span data-stu-id="5f92b-148">**Data Factory copies data that is processed in parallel** toohello secondary location.</span></span>
6. <span data-ttu-id="5f92b-149">**Objekt pro vytváření dat běží hello vlastní aktivitu pomocí hello fondu přidělené dávkou**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-149">**Data Factory runs hello custom activity using hello pool allocated by Batch**.</span></span> <span data-ttu-id="5f92b-150">Objekt pro vytváření dat mohou být aktivity současně.</span><span class="sxs-lookup"><span data-stu-id="5f92b-150">Data Factory can run activities concurrently.</span></span> <span data-ttu-id="5f92b-151">Každá aktivita zpracovává řez data.</span><span class="sxs-lookup"><span data-stu-id="5f92b-151">Each activity processes a slice of data.</span></span> <span data-ttu-id="5f92b-152">výsledky Hello jsou uloženy v úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-152">hello results are stored in Azure storage.</span></span>
7. <span data-ttu-id="5f92b-153">**Objekt pro vytváření dat přesune hello konečných výsledků tooa třetí umístění**, k distribuci prostřednictvím aplikace nebo pro další zpracování jiných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-153">**Data Factory moves hello final results tooa third location**, either for distribution via an app, or for further processing by other tools.</span></span>

## <a name="implementation-of-sample-solution"></a><span data-ttu-id="5f92b-154">Implementace ukázkové řešení</span><span class="sxs-lookup"><span data-stu-id="5f92b-154">Implementation of sample solution</span></span>
<span data-ttu-id="5f92b-155">Hello ukázkové řešení je záměrně jednoduchá a tooshow můžete jak toouse objekt pro vytváření dat a Batch společně tooprocess datové sady.</span><span class="sxs-lookup"><span data-stu-id="5f92b-155">hello sample solution is intentionally simple and is tooshow you how toouse Data Factory and Batch together tooprocess datasets.</span></span> <span data-ttu-id="5f92b-156">řešení Hello jednoduše vrátí hello počet výskytů hledaný termín ("Microsoft") v vstupní soubory, které jsou uspořádány do časové řady.</span><span class="sxs-lookup"><span data-stu-id="5f92b-156">hello solution simply counts hello number of occurrences of a search term (“Microsoft”) in input files organized in a time series.</span></span> <span data-ttu-id="5f92b-157">Vyprodukuje hello počet toooutput soubory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-157">It outputs hello count toooutput files.</span></span>

<span data-ttu-id="5f92b-158">**Čas**: Pokud jste obeznámení se základy používání služby Azure Data Factory a Batch, a mít dokončené hello požadavky uvedené níže, jsme odhadnout toto řešení trvá toocomplete 1 – 2 hodiny.</span><span class="sxs-lookup"><span data-stu-id="5f92b-158">**Time**: If you are familiar with basics of Azure, Data Factory, and Batch, and have completed hello prerequisites listed below, we estimate this solution takes 1-2 hours toocomplete.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="5f92b-159">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5f92b-159">Prerequisites</span></span>
#### <a name="azure-subscription"></a><span data-ttu-id="5f92b-160">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="5f92b-160">Azure subscription</span></span>
<span data-ttu-id="5f92b-161">Pokud nemáte předplatné Azure, můžete si během několika minut vytvořit Bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="5f92b-161">If you don't have an Azure subscription, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="5f92b-162">V tématu [bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5f92b-162">See [Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

#### <a name="azure-storage-account"></a><span data-ttu-id="5f92b-163">Účet služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5f92b-163">Azure storage account</span></span>
<span data-ttu-id="5f92b-164">Používáte účet úložiště Azure pro ukládání dat hello v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-164">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="5f92b-165">Pokud nemáte účet úložiště Azure, najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5f92b-165">If you don't have an Azure storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="5f92b-166">Hello ukázkové řešení používá úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-166">hello sample solution uses blob storage.</span></span>

#### <a name="azure-batch-account"></a><span data-ttu-id="5f92b-167">Účet Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-167">Azure Batch account</span></span>
<span data-ttu-id="5f92b-168">Vytvoření účtu Azure Batch pomocí hello [portál Azure](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="5f92b-168">Create an Azure Batch account using hello [Azure portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="5f92b-169">V tématu [vytvoření a Správa účtu Azure Batch](../batch/batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5f92b-169">See [Create and manage an Azure Batch account](../batch/batch-account-create-portal.md).</span></span> <span data-ttu-id="5f92b-170">Všimněte si hello Azure Batch účet názvu a klíče účtu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-170">Note hello Azure Batch account name and account key.</span></span> <span data-ttu-id="5f92b-171">Můžete také použít [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) rutiny toocreate účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-171">You can also use [New-AzureRmBatchAccount](https://msdn.microsoft.com/library/mt603749.aspx) cmdlet toocreate an Azure Batch account.</span></span> <span data-ttu-id="5f92b-172">V tématu [Začínáme pomocí rutin prostředí PowerShell Azure Batch](../batch/batch-powershell-cmdlets-get-started.md) pro podrobné pokyny k použití této rutiny.</span><span class="sxs-lookup"><span data-stu-id="5f92b-172">See [Get started with Azure Batch PowerShell cmdlets](../batch/batch-powershell-cmdlets-get-started.md) for detailed instructions on using this cmdlet.</span></span>

<span data-ttu-id="5f92b-173">Hello ukázkové řešení používá Azure Batch (nepřímo přes kanál služby Azure Data Factory) tooprocess data paralelní způsobem ve fondu výpočetních uzlů (spravované kolekce virtuálních počítačů).</span><span class="sxs-lookup"><span data-stu-id="5f92b-173">hello sample solution uses Azure Batch (indirectly via an Azure Data Factory pipeline) tooprocess data in a parallel manner on a pool of compute nodes (a managed collection of virtual machines).</span></span>

#### <a name="azure-batch-pool-of-virtual-machines-vms"></a><span data-ttu-id="5f92b-174">Azure Batch fondu virtuálních počítačů (VM)</span><span class="sxs-lookup"><span data-stu-id="5f92b-174">Azure Batch pool of virtual machines (VMs)</span></span>
<span data-ttu-id="5f92b-175">Vytvoření **fondu Azure Batch** s minimálně 2 výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-175">Create an **Azure Batch pool** with at least 2 compute nodes.</span></span>

1. <span data-ttu-id="5f92b-176">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce text hello a klikněte na tlačítko **účty Batch**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-176">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
2. <span data-ttu-id="5f92b-177">Vyberte vaše hello tooopen účet Azure Batch **účtu Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="5f92b-177">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
3. <span data-ttu-id="5f92b-178">Klikněte na tlačítko **fondy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="5f92b-178">Click **Pools** tile.</span></span>
4. <span data-ttu-id="5f92b-179">V hello **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů tooadd hello fondu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-179">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
   1. <span data-ttu-id="5f92b-180">Zadejte ID fondu hello (**ID fondu**).</span><span class="sxs-lookup"><span data-stu-id="5f92b-180">Enter an ID for hello pool (**Pool ID**).</span></span> <span data-ttu-id="5f92b-181">Poznámka: hello **ID fondu hello**; je nutné při vytváření řešení Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-181">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
   2. <span data-ttu-id="5f92b-182">Zadejte **Windows Server 2012 R2** pro nastavení hello řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5f92b-182">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
   3. <span data-ttu-id="5f92b-183">Vyberte **cenovou úroveň uzlu**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-183">Select a **node pricing tier**.</span></span>
   4. <span data-ttu-id="5f92b-184">Zadejte **2** jako hodnota pro hello **cíl vyhrazené** nastavení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-184">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
   5. <span data-ttu-id="5f92b-185">Zadejte **2** jako hodnota pro hello **maximální počet úloh na uzel** nastavení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-185">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   6. <span data-ttu-id="5f92b-186">Klikněte na tlačítko **OK** toocreate hello fondu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-186">Click **OK** toocreate hello pool.</span></span>

#### <a name="azure-storage-explorer"></a><span data-ttu-id="5f92b-187">Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="5f92b-187">Azure Storage Explorer</span></span>
<span data-ttu-id="5f92b-188">[Azure Storage Explorer 6 (nástroj)](https://azurestorageexplorer.codeplex.com/) nebo [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (z ClumsyLeaf softwaru).</span><span class="sxs-lookup"><span data-stu-id="5f92b-188">[Azure Storage Explorer 6 (tool)](https://azurestorageexplorer.codeplex.com/) or [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) (from ClumsyLeaf Software).</span></span> <span data-ttu-id="5f92b-189">Pomocí těchto nástrojů pro kontroly a změna hello data ve vašich projektů Azure Storage, včetně protokolů hello aplikací hostovaných v cloudu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-189">You use these tools for inspecting and altering hello data in your Azure Storage projects including hello logs of your cloud-hosted applications.</span></span>

1. <span data-ttu-id="5f92b-190">Vytvořit kontejner s názvem **můj_kontejner** s přístupem k privátní (žádný anonymní přístup)</span><span class="sxs-lookup"><span data-stu-id="5f92b-190">Create a container named **mycontainer** with private access (no anonymous access)</span></span>
2. <span data-ttu-id="5f92b-191">Pokud používáte **CloudXplorer**, vytvořte složky a podsložky hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="5f92b-191">If you are using **CloudXplorer**, create folders and subfolders with hello following structure:</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)

   <span data-ttu-id="5f92b-192">`Inputfolder`a `outputfolder` jsou nejvyšší úrovně složky v `mycontainer`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-192">`Inputfolder` and `outputfolder` are top-level folders in `mycontainer`.</span></span> <span data-ttu-id="5f92b-193">Hello `inputfolder` má podsložky razítka data a času (rrrr-MM-DD-HH).</span><span class="sxs-lookup"><span data-stu-id="5f92b-193">hello `inputfolder` has subfolders with date-time stamps (YYYY-MM-DD-HH).</span></span>

   <span data-ttu-id="5f92b-194">Pokud používáte **Azure Storage Explorer**, musíte v dalším kroku hello tooupload soubory s názvy: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5f92b-194">If you are using **Azure Storage Explorer**, in hello next step, you need tooupload files with names: `inputfolder/2015-11-16-00/file.txt`, `inputfolder/2015-11-16-01/file.txt` and so on.</span></span> <span data-ttu-id="5f92b-195">Tento krok automaticky vytvoří hello složek.</span><span class="sxs-lookup"><span data-stu-id="5f92b-195">This step automatically creates hello folders.</span></span>
3. <span data-ttu-id="5f92b-196">Vytvořte textový soubor **soubor.txt** na počítači s obsahem, který má hello – klíčové slovo **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-196">Create a text file **file.txt** on your machine with content that has hello keyword **Microsoft**.</span></span> <span data-ttu-id="5f92b-197">Například: "testovací vlastní aktivity Microsoft test vlastní aktivity Microsoft".</span><span class="sxs-lookup"><span data-stu-id="5f92b-197">For example: “test custom activity Microsoft test custom activity Microsoft”.</span></span>
4. <span data-ttu-id="5f92b-198">Nahrajte toohello souboru hello následující vstupní složky v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5f92b-198">Upload hello file toohello following input folders in Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image4.png)

   <span data-ttu-id="5f92b-199">Pokud používáte **Azure Storage Explorer**, nahrajte soubor hello **soubor.txt** příliš**můj_kontejner**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-199">If you are using **Azure Storage Explorer**, upload hello file **file.txt** too**mycontainer**.</span></span> <span data-ttu-id="5f92b-200">Klikněte na tlačítko **kopie** na hello nástrojů toocreate kopii hello objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-200">Click **Copy** on hello toolbar toocreate a copy of hello blob.</span></span> <span data-ttu-id="5f92b-201">V hello **kopírovat objekt Blob** dialogové okno, změna hello **název cílového objektu blob** příliš`inputfolder/2015-11-16-00/file.txt`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-201">In hello **Copy Blob** dialog box, change hello **destination blob name** too`inputfolder/2015-11-16-00/file.txt`.</span></span> <span data-ttu-id="5f92b-202">Opakujte tento krok toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` a tak dále.</span><span class="sxs-lookup"><span data-stu-id="5f92b-202">Repeat this step toocreate `inputfolder/2015-11-16-01/file.txt`, `inputfolder/2015-11-16-02/file.txt`, `inputfolder/2015-11-16-03/file.txt`, `inputfolder/2015-11-16-04/file.txt` and so on.</span></span> <span data-ttu-id="5f92b-203">Tato akce automaticky vytvoří hello složek.</span><span class="sxs-lookup"><span data-stu-id="5f92b-203">This action automatically creates hello folders.</span></span>
5. <span data-ttu-id="5f92b-204">Vytvořte jiný kontejner s názvem: `customactivitycontainer`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-204">Create another container named: `customactivitycontainer`.</span></span> <span data-ttu-id="5f92b-205">Můžete nahrát hello vlastní aktivita zip souboru toothis kontejneru.</span><span class="sxs-lookup"><span data-stu-id="5f92b-205">You upload hello custom activity zip file toothis container.</span></span>

#### <a name="visual-studio"></a><span data-ttu-id="5f92b-206">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f92b-206">Visual Studio</span></span>
<span data-ttu-id="5f92b-207">Nainstalujte sadu Microsoft Visual Studio 2012 nebo novější toocreate hello vlastní Batch aktivity toobe použít v hello řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-207">Install Microsoft Visual Studio 2012 or later toocreate hello custom Batch activity toobe used in hello Data Factory solution.</span></span>

### <a name="high-level-steps-toocreate-hello-solution"></a><span data-ttu-id="5f92b-208">Postup vysoké úrovně toocreate hello řešení</span><span class="sxs-lookup"><span data-stu-id="5f92b-208">High-level steps toocreate hello solution</span></span>
1. <span data-ttu-id="5f92b-209">Vytvořte vlastní aktivity, která obsahuje logiku zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-209">Create a custom activity that contains hello data processing logic.</span></span>
2. <span data-ttu-id="5f92b-210">Vytvoření služby Azure data factory, která používá vlastní aktivity hello:</span><span class="sxs-lookup"><span data-stu-id="5f92b-210">Create an Azure data factory that uses hello custom activity:</span></span>

### <a name="create-hello-custom-activity"></a><span data-ttu-id="5f92b-211">Vytvořit vlastní aktivitu hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-211">Create hello custom activity</span></span>
<span data-ttu-id="5f92b-212">Hello vlastní aktivity služby Data Factory je hello srdcem této ukázkové řešení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-212">hello Data Factory custom activity is hello heart of this sample solution.</span></span> <span data-ttu-id="5f92b-213">Hello ukázkové řešení používá Azure Batch toorun hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5f92b-213">hello sample solution uses Azure Batch toorun hello custom activity.</span></span> <span data-ttu-id="5f92b-214">V tématu [použít vlastní aktivity v kanálu Azure Data Factory](data-factory-use-custom-activities.md) pro vlastní aktivity toodevelop hello základní informace a použít je v Azure Data Factory kanálů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-214">See [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) for hello basic information toodevelop custom activities and use them in Azure Data Factory pipelines.</span></span>

<span data-ttu-id="5f92b-215">toocreate .NET vlastní aktivity, které můžete použít v kanál služby Azure Data Factory, budete potřebovat toocreate **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5f92b-215">toocreate a .NET custom activity that you can use in an Azure Data Factory pipeline, you need toocreate a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="5f92b-216">Toto rozhraní obsahuje pouze jednu metodu: **Execute**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-216">This interface has only one method: **Execute**.</span></span> <span data-ttu-id="5f92b-217">Tady je hello podpis metody hello:</span><span class="sxs-lookup"><span data-stu-id="5f92b-217">Here is hello signature of hello method:</span></span>

```csharp
public IDictionary<string, string> Execute(
            IEnumerable<LinkedService> linkedServices,
            IEnumerable<Dataset> datasets,
            Activity activity,
            IActivityLogger logger)
```

<span data-ttu-id="5f92b-218">Hello metoda má několik klíčových součástí, je nutné, aby toounderstand.</span><span class="sxs-lookup"><span data-stu-id="5f92b-218">hello method has a few key components that you need toounderstand.</span></span>

* <span data-ttu-id="5f92b-219">Metoda Hello přijímá čtyř parametrů:</span><span class="sxs-lookup"><span data-stu-id="5f92b-219">hello method takes four parameters:</span></span>

  1. <span data-ttu-id="5f92b-220">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-220">**linkedServices**.</span></span> <span data-ttu-id="5f92b-221">Vyčíslitelná seznam propojené služby, které odkazují vstupní a výstupní datové zdroje (například: Azure Blob Storage) toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-221">An enumerable list of linked services that link input/output data sources (for example: Azure Blob Storage) toohello data factory.</span></span> <span data-ttu-id="5f92b-222">V této ukázce je pouze jeden propojené služby typu úložiště Azure pro vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="5f92b-222">In this sample, there is only one linked service of type Azure Storage used for both input and output.</span></span>
  2. <span data-ttu-id="5f92b-223">**datové sady**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-223">**datasets**.</span></span> <span data-ttu-id="5f92b-224">Toto je vyčíslitelná seznam datových sad.</span><span class="sxs-lookup"><span data-stu-id="5f92b-224">This is an enumerable list of datasets.</span></span> <span data-ttu-id="5f92b-225">Můžete použít tento parametr tooget hello umístění a schémata definované vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5f92b-225">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
  3. <span data-ttu-id="5f92b-226">**aktivita**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-226">**activity**.</span></span> <span data-ttu-id="5f92b-227">Tento parametr představuje hello aktuální výpočetní entitu – v takovém případě služby Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-227">This parameter represents hello current compute entity - in this case, an Azure Batch service.</span></span>
  4. <span data-ttu-id="5f92b-228">**protokolovač**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-228">**logger**.</span></span> <span data-ttu-id="5f92b-229">Umožňuje protokolovacího nástroje Hello tento prostor pro psaní komentářů ladění jako hello "User" protokolu hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-229">hello logger lets you write debug comments that surface as hello “User” log for hello pipeline.</span></span>
* <span data-ttu-id="5f92b-230">Hello metoda vrátí slovník, který může být vlastní aktivity používané toochain společně v budoucnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-230">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="5f92b-231">Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-231">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>

#### <a name="procedure-create-hello-custom-activity"></a><span data-ttu-id="5f92b-232">Postup: Vytvoření vlastní aktivity hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-232">Procedure: Create hello custom activity</span></span>
1. <span data-ttu-id="5f92b-233">Vytvoření projektu knihovny tříd rozhraní .NET v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5f92b-233">Create a .NET Class Library project in Visual Studio.</span></span>

   1. <span data-ttu-id="5f92b-234">Spusťte **sady Visual Studio 2012**/**2013 nebo 2015**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-234">Launch **Visual Studio 2012**/**2013/2015**.</span></span>
   2. <span data-ttu-id="5f92b-235">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-235">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="5f92b-236">Rozbalte položku **šablony**a vyberte **Visual C\#**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-236">Expand **Templates**, and select **Visual C\#**.</span></span> <span data-ttu-id="5f92b-237">V tomto návodu můžete použít C\#, ale můžete použít libovolné .NET jazyk toodevelop hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5f92b-237">In this walkthrough, you use C\#, but you can use any .NET language toodevelop hello custom activity.</span></span>
   4. <span data-ttu-id="5f92b-238">Vyberte **knihovny tříd** hello seznamu typů projektu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="5f92b-238">Select **Class Library** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="5f92b-239">Zadejte **MyDotNetActivity** pro hello **název**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-239">Enter **MyDotNetActivity** for hello **Name**.</span></span>
   6. <span data-ttu-id="5f92b-240">Vyberte **C:\\ADF** pro hello **umístění**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-240">Select **C:\\ADF** for hello **Location**.</span></span> <span data-ttu-id="5f92b-241">Vytvořte složku hello **ADF** Pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5f92b-241">Create hello folder **ADF** if it does not exist.</span></span>
   7. <span data-ttu-id="5f92b-242">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-242">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="5f92b-243">Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-243">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="5f92b-244">V hello **Konzola správce balíčků**, spustit následující příkaz tooimport hello **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-244">In hello **Package Manager Console**, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```powershell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="5f92b-245">Import hello **Azure Storage** balíček NuGet v toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-245">Import hello **Azure Storage** NuGet package in toohello project.</span></span> <span data-ttu-id="5f92b-246">Tento balíček je nutné, protože v této ukázce použijete rozhraní API úložiště objektů Blob hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-246">You need this package because you use hello Blob storage API in this sample.</span></span>

    ```powershell
    Install-Package Azure.Storage
    ```
5. <span data-ttu-id="5f92b-247">Přidejte následující hello **pomocí** direktivy toohello zdrojový soubor v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-247">Add hello following **using** directives toohello source file in hello project.</span></span>

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
6. <span data-ttu-id="5f92b-248">Změnit název hello hello **obor názvů** příliš**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-248">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="5f92b-249">Změňte název hello třídy hello příliš**MyDotNetActivity** a odvodí z hello **IDotNetActivity** rozhraní, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="5f92b-249">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown below.</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="5f92b-250">Implementace (Přidat) hello **Execute** metoda hello **IDotNetActivity** rozhraní toohello **MyDotNetActivity** hello třídy a zkopírujte následující ukázka kódu toohello metoda.</span><span class="sxs-lookup"><span data-stu-id="5f92b-250">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span> <span data-ttu-id="5f92b-251">V tématu hello [spustit metodu](#execute-method) části vysvětlení pro hello logikou používanou v této metodě.</span><span class="sxs-lookup"><span data-stu-id="5f92b-251">See hello [Execute Method](#execute-method) section for explanation for hello logic used in this method.</span></span>

    ```csharp
    /// <summary>
    /// Execute method is hello only method of IDotNetActivity interface you must implement.
    /// In this sample, hello method invokes hello Calculate method tooperform hello core logic.  
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
    
       // using First method instead of Single since we are using hello same
       // Azure Storage linked service for input and output.
       inputLinkedService = linkedServices.First(
           linkedService =>
           linkedService.Name ==
           inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
           as AzureStorageLinkedService;
    
       string connectionString = inputLinkedService.ConnectionString; // toocreate an input storage client.
       string folderPath = GetFolderPath(inputDataset);
       string output = string.Empty; // for use later.
    
       // create storage client for input. Pass hello connection string.
       CloudStorageAccount inputStorageAccount = CloudStorageAccount.Parse(connectionString);
       CloudBlobClient inputClient = inputStorageAccount.CreateCloudBlobClient();
    
       // initialize hello continuation token before using it in hello do-while loop.
       BlobContinuationToken continuationToken = null;
       do
       {   // get hello list of input blobs from hello input storage client object.
           BlobResultSegment blobList = inputClient.ListBlobsSegmented(folderPath,
                                    true,
                                    BlobListingDetails.Metadata,
                                    null,
                                    continuationToken,
                                    null,
                                    null);
    
           // Calculate method returns hello number of occurrences of
           // hello search term (“Microsoft”) in each blob associated
           // with hello data slice.
           //
           // definition of hello method is shown in hello next step.
           output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
       } while (continuationToken != null);
    
       // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
       Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    
       folderPath = GetFolderPath(outputDataset);
    
       logger.Write("Writing blob toohello folder: {0}", folderPath);
    
       // create a storage object for hello output blob.
       CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
       // write hello name of hello file.
       Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
       logger.Write("output blob URI: {0}", outputBlobUri.ToString());
       // create a blob and upload hello output text.
       CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
       logger.Write("Writing {0} toohello output blob", output);
       outputBlob.UploadText(output);
    
       // hello dictionary can be used toochain custom activities together in hello future.
       // This feature is not implemented yet, so just return an empty dictionary.
       return new Dictionary<string, string>();
    }
    ```
9. <span data-ttu-id="5f92b-252">Přidejte následující pomocná třída toohello metody hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-252">Add hello following helper methods toohello class.</span></span> <span data-ttu-id="5f92b-253">Tyto metody jsou vyvolány hello **Execute** metoda.</span><span class="sxs-lookup"><span data-stu-id="5f92b-253">These methods are invoked by hello **Execute** method.</span></span> <span data-ttu-id="5f92b-254">Co je nejdůležitější – hello **Calculate** metoda izoluje hello kód, který iteruje v rámci jednotlivých objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-254">Most importantly, hello **Calculate** method isolates hello code that iterates through each blob.</span></span>

    ```csharp
    /// <summary>
    /// Gets hello folderPath value from hello input/output dataset.
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
    /// Gets hello fileName value from hello input/output dataset.
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
    /// Iterates through each blob (file) in hello folder, counts hello number of instances of search term in hello file,
    /// and prepares hello output text that is written toohello output blob.
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
               output += string.Format("{0} occurrences(s) of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
           }
       }
       return output;
    }
    ```
    <span data-ttu-id="5f92b-255">Hello **GetFolderPath** metoda vrátí hello cesta toohello složky této hello datovou sadu body tooand hello **GetFileName** metoda vrátí hello název hello objektů blob nebo souboru, který hello body datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-255">hello **GetFolderPath** method returns hello path toohello folder that hello dataset points tooand hello **GetFileName** method returns hello name of hello blob/file that hello dataset points to.</span></span>

    ```csharp

    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "mycontainer/inputfolder/{Year}-{Month}-{Day}-{Hour}",
    ```

    <span data-ttu-id="5f92b-256">Hello **Calculate** metoda vypočítá hello počet instancí – klíčové slovo **Microsoft** v hello vstupní soubory (objekty BLOB ve složce hello).</span><span class="sxs-lookup"><span data-stu-id="5f92b-256">hello **Calculate** method calculates hello number of instances of keyword **Microsoft** in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="5f92b-257">hledaný termín Hello ("Microsoft") je pevně zakódovaná v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-257">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>

1. <span data-ttu-id="5f92b-258">Kompilace projektu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-258">Compile hello project.</span></span> <span data-ttu-id="5f92b-259">Klikněte na tlačítko **sestavení** hello nabídky a klikněte na **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-259">Click **Build** from hello menu and click **Build Solution**.</span></span>
2. <span data-ttu-id="5f92b-260">Spusťte **Průzkumníka Windows**a přejděte příliš**bin\\ladění** nebo **bin\\verze** složku v závislosti na typu hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-260">Launch **Windows Explorer**, and navigate too**bin\\debug** or **bin\\release** folder depending on hello type of build.</span></span>
3. <span data-ttu-id="5f92b-261">Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory hello v hello  **\\bin\\ladění** složky.</span><span class="sxs-lookup"><span data-stu-id="5f92b-261">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello **\\bin\\Debug** folder.</span></span> <span data-ttu-id="5f92b-262">Můžete chtít tooinclude hello MyDotNetActivity. **pdb** tak, aby získat další podrobnosti, jako je například číslo řádku v hello zdrojový kód, který způsobil hello problém, když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="5f92b-262">You may want tooinclude hello MyDotNetActivity.**pdb** file so that you get additional details such as line number in hello source code that caused hello issue when a failure occurs.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image5.png)
4. <span data-ttu-id="5f92b-263">Nahrát **MyDotNetActivity.zip** jako kontejner objektů blob toohello objektů blob: `customactivitycontainer` v hello Azure blob storage této hello **StorageLinkedService** propojená služba v hello  **ADFTutorialDataFactory** používá.</span><span class="sxs-lookup"><span data-stu-id="5f92b-263">Upload **MyDotNetActivity.zip** as a blob toohello blob container: `customactivitycontainer` in hello Azure blob storage that hello **StorageLinkedService** linked service in hello **ADFTutorialDataFactory** uses.</span></span> <span data-ttu-id="5f92b-264">Vytvoření kontejneru objektů blob hello `customactivitycontainer` Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="5f92b-264">Create hello blob container `customactivitycontainer` if it does not already exist.</span></span>

#### <a name="execute-method"></a><span data-ttu-id="5f92b-265">Execute – metoda</span><span class="sxs-lookup"><span data-stu-id="5f92b-265">Execute method</span></span>
<span data-ttu-id="5f92b-266">Tato část obsahuje další podrobnosti a poznámky o hello kódu v hello Metoda Execute.</span><span class="sxs-lookup"><span data-stu-id="5f92b-266">This section provides more details and notes about hello code in hello Execute method.</span></span>

1. <span data-ttu-id="5f92b-267">Členové Hello iterace v rámci hello vstupní kolekce se nacházejí v hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-267">hello members for iterating through hello input collection are found in hello [Microsoft.WindowsAzure.Storage.Blob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.aspx) namespace.</span></span> <span data-ttu-id="5f92b-268">Iterace v rámci kolekce objektů blob hello vyžaduje použití hello **BlobContinuationToken** třídy.</span><span class="sxs-lookup"><span data-stu-id="5f92b-268">Iterating through hello blob collection requires using hello **BlobContinuationToken** class.</span></span> <span data-ttu-id="5f92b-269">V podstatě, je nutné použít DNT-při smyčky pomocí tokenu hello hello mechanismus pro ukončení smyčky hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-269">In essence, you must use a do-while loop with hello token as hello mechanism for exiting hello loop.</span></span> <span data-ttu-id="5f92b-270">Další informace najdete v tématu [jak toouse úložiště Blob z .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="5f92b-270">For more information, see [How toouse Blob storage from .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="5f92b-271">Zobrazí se zde základní smyčka:</span><span class="sxs-lookup"><span data-stu-id="5f92b-271">A basic loop is shown here:</span></span>

    ```csharp
    // Initialize hello continuation token.
    BlobContinuationToken continuationToken = null;
    do
    {
    // Get hello list of input blobs from hello input storage client object.
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
   <span data-ttu-id="5f92b-272">Naleznete v dokumentaci k hello hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) metoda podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5f92b-272">See hello documentation for hello [ListBlobsSegmented](https://msdn.microsoft.com/library/jj717596.aspx) method for details.</span></span>
2. <span data-ttu-id="5f92b-273">Hello kód pro práci prostřednictvím hello sadu objektů BLOB logicky přejde v rámci hello udělat-při smyčky.</span><span class="sxs-lookup"><span data-stu-id="5f92b-273">hello code for working through hello set of blobs logically goes within hello do-while loop.</span></span> <span data-ttu-id="5f92b-274">V hello **Execute** metoda, proveďte hello-při smyčky předá hello seznam objektů BLOB s názvem metoda tooa **Calculate**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-274">In hello **Execute** method, hello do-while loop passes hello list of blobs tooa method named **Calculate**.</span></span> <span data-ttu-id="5f92b-275">Hello metoda vrátí řetězec proměnné s názvem **výstup** tedy hello výsledek s vstupní prostřednictvím všech objektů BLOB hello v segmentu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-275">hello method returns a string variable named **output** that is hello result of having iterated through all hello blobs in hello segment.</span></span>

   <span data-ttu-id="5f92b-276">Vrátí hello počtu výskytů hello hledaný termín (**Microsoft**) v objektu blob hello předán toohello **Calculate** metoda.</span><span class="sxs-lookup"><span data-stu-id="5f92b-276">It returns hello number of occurrences of hello search term (**Microsoft**) in hello blob passed toohello **Calculate** method.</span></span>

    ```csharp
    output += string.Format("{0} occurrences of hello search term \"{1}\" were found in hello file {2}.\r\n", wordCount, searchTerm, inputBlob.Name);
    ```
3. <span data-ttu-id="5f92b-277">Jednou hello **Calculate** metoda provedla pracovní hello, se musí být napsané tooa nový objekt blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-277">Once hello **Calculate** method has done hello work, it must be written tooa new blob.</span></span> <span data-ttu-id="5f92b-278">Aby pro každou sadu objektů BLOB zpracovat nový objekt blob může být napsán s výsledky hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-278">So for every set of blobs processed, a new blob can be written with hello results.</span></span> <span data-ttu-id="5f92b-279">Nový objekt blob tooa toowrite, první najít hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-279">toowrite tooa new blob, first find hello output dataset.</span></span>

    ```csharp
    // Get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
    Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);
    ```
4. <span data-ttu-id="5f92b-280">Hello kód také volá metodu helper: **GetFolderPath** tooretrieve cesta ke složce hello (název kontejneru úložiště hello).</span><span class="sxs-lookup"><span data-stu-id="5f92b-280">hello code also calls a helper method: **GetFolderPath** tooretrieve hello folder path (hello storage container name).</span></span>

    ```csharp
    folderPath = GetFolderPath(outputDataset);
    ```
   <span data-ttu-id="5f92b-281">Hello **GetFolderPath** přetypování hello datovou sadu objektu tooan AzureBlobDataSet, který má vlastnost s názvem FolderPath.</span><span class="sxs-lookup"><span data-stu-id="5f92b-281">hello **GetFolderPath** casts hello DataSet object tooan AzureBlobDataSet, which has a property named FolderPath.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FolderPath;
    ```
5. <span data-ttu-id="5f92b-282">Hello kód volání hello **GetFileName** metoda tooretrieve hello souboru name (název objektu blob).</span><span class="sxs-lookup"><span data-stu-id="5f92b-282">hello code calls hello **GetFileName** method tooretrieve hello file name (blob name).</span></span> <span data-ttu-id="5f92b-283">Kód Hello je podobné toohello výše cesta ke složce hello tooget kódu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-283">hello code is similar toohello above code tooget hello folder path.</span></span>

    ```csharp
    AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
    
    return blobDataset.FileName;
    ```
6. <span data-ttu-id="5f92b-284">Název Hello hello souboru je zapsán vytvořením objektu URI.</span><span class="sxs-lookup"><span data-stu-id="5f92b-284">hello name of hello file is written by creating a URI object.</span></span> <span data-ttu-id="5f92b-285">Konstruktor URI Hello používá hello **BlobEndpoint** název kontejneru hello tooreturn vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5f92b-285">hello URI constructor uses hello **BlobEndpoint** property tooreturn hello container name.</span></span> <span data-ttu-id="5f92b-286">název a cesta k souboru Hello složky se přidají identifikátor URI výstupního objektu blob tooconstruct hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-286">hello folder path and file name are added tooconstruct hello output blob URI.</span></span>  

    ```csharp
    // Write hello name of hello file.
    Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    ```
7. <span data-ttu-id="5f92b-287">byl proveden zápis Hello název souboru hello a nyní může zapisovat hello výstupní řetězec z hello **Calculate** metoda tooa nové blob:</span><span class="sxs-lookup"><span data-stu-id="5f92b-287">hello name of hello file has been written and now you can write hello output string from hello **Calculate** method tooa new blob:</span></span>

    ```csharp
    // Create a blob and upload hello output text.
    CloudBlockBlob outputBlob = new CloudBlockBlob(outputBlobUri, outputStorageAccount.Credentials);
    logger.Write("Writing {0} toohello output blob", output);
    outputBlob.UploadText(output);
    ```

### <a name="create-hello-data-factory"></a><span data-ttu-id="5f92b-288">Vytvoření objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-288">Create hello data factory</span></span>
<span data-ttu-id="5f92b-289">V hello [vytvořit vlastní aktivitu hello](#create-the-custom-activity) části jste vytvořili vlastní aktivity a soubor zip nahrané hello s binární soubory a hello PDB souboru tooan kontejner objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-289">In hello [Create hello custom activity](#create-the-custom-activity) section, you created a custom activity and uploaded hello zip file with binaries and hello PDB file tooan Azure blob container.</span></span> <span data-ttu-id="5f92b-290">V této části vytvoříte Azure **objekt pro vytváření dat** s **kanálu** používající hello **vlastní aktivity**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-290">In this section, you create an Azure **data factory** with a **pipeline** that uses hello **custom activity**.</span></span>

<span data-ttu-id="5f92b-291">pro vlastní aktivity hello představuje hello objekty BLOB (soubory) ve složce vstupní hello Hello vstupní datové sady (`mycontainer\\inputfolder`) v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-291">hello input dataset for hello custom activity represents hello blobs (files) in hello input folder (`mycontainer\\inputfolder`) in blob storage.</span></span> <span data-ttu-id="5f92b-292">Hello výstupní datovou sadu pro aktivity hello představuje objekty BLOB výstup hello v hello výstupní složce (`mycontainer\\outputfolder`) v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5f92b-292">hello output dataset for hello activity represents hello output blobs in hello output folder (`mycontainer\\outputfolder`) in blob storage.</span></span>

<span data-ttu-id="5f92b-293">Vyřaďte jeden nebo více souborů ve složkách vstupní hello:</span><span class="sxs-lookup"><span data-stu-id="5f92b-293">Drop one or more files in hello input folders:</span></span>

```
mycontainer -\> inputfolder
    2015-11-16-00
    2015-11-16-01
    2015-11-16-02
    2015-11-16-03
    2015-11-16-04
```

<span data-ttu-id="5f92b-294">Například můžete vyřaďte jeden soubor (soubor.txt) s hello následující obsah do jednotlivých složek hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-294">For example, drop one file (file.txt) with hello following content into each of hello folders.</span></span>

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="5f92b-295">Každé vstupní složky odpovídá tooa řez v Azure Data Factory i v případě, že složka hello obsahuje 2 nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-295">Each input folder corresponds tooa slice in Azure Data Factory even if hello folder has 2 or more files.</span></span> <span data-ttu-id="5f92b-296">Při zpracování kanálu hello se každý řez iteruje hello vlastní aktivity všech objektů BLOB hello v hello vstupní složky pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-296">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="5f92b-297">Uvidíte pět výstupní soubory s hello stejné obsahu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-297">You see five output files with hello same content.</span></span> <span data-ttu-id="5f92b-298">Například soubor výstup hello zpracování hello souboru ve složce hello 2015 11 16 00 má hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="5f92b-298">For example, hello output file from processing hello file in hello 2015-11-16-00 folder has hello following content:</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
```

<span data-ttu-id="5f92b-299">Pokud je vyřadit více souborů (soubor.txt, Soubor2.txt, file3.txt) s hello stejné toohello vstupní složky obsahu, najdete v části hello následující obsah ve výstupním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-299">If you drop multiple files (file.txt, file2.txt, file3.txt) with hello same content toohello input folder, you see hello following content in hello output file.</span></span> <span data-ttu-id="5f92b-300">Každé složky (2015-11-16-00 atd.) odpovídá tooa řez v této ukázce, i když hello složka obsahuje více vstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-300">Each folder (2015-11-16-00, etc.) corresponds tooa slice in this sample even though hello folder has multiple input files.</span></span>

```csharp
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file2.txt.
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file3.txt.
```

<span data-ttu-id="5f92b-301">Hello výstupní soubor má tři řádky, jeden pro každý vstupní soubor (binární rozsáhlý objekt) ve složce hello přidružené řez hello (2015-11-16-00).</span><span class="sxs-lookup"><span data-stu-id="5f92b-301">hello output file has three lines now, one for each input file (blob) in hello folder associated with hello slice (2015-11-16-00).</span></span>

<span data-ttu-id="5f92b-302">Úloha se vytvoří pro každou aktivitu spustit.</span><span class="sxs-lookup"><span data-stu-id="5f92b-302">A task is created for each activity run.</span></span> <span data-ttu-id="5f92b-303">V této ukázce je jenom jedna aktivita v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-303">In this sample, there is only one activity in hello pipeline.</span></span> <span data-ttu-id="5f92b-304">Při zpracování kanálu hello se řez hello vlastní aktivity spouští na Azure Batch tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-304">When a slice is processed by hello pipeline, hello custom activity runs on Azure Batch tooprocess hello slice.</span></span> <span data-ttu-id="5f92b-305">Vzhledem k tomu, že existují pět řezy (každý řez může mít více objektů BLOB nebo soubor), nejsou pět úkoly vytvořené v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-305">Since there are five slices (each slice can have multiple blobs or file), there are five tasks created in Azure Batch.</span></span> <span data-ttu-id="5f92b-306">Když úloha běží na Batch, je ve skutečnosti hello vlastní aktivity se systémem.</span><span class="sxs-lookup"><span data-stu-id="5f92b-306">When a task runs on Batch, it is actually hello custom activity that is running.</span></span>

<span data-ttu-id="5f92b-307">Následující postup Hello poskytuje další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5f92b-307">hello following walkthrough provides additional details.</span></span>

#### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="5f92b-308">Krok 1: Vytvoření objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-308">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="5f92b-309">Po přihlášení toohello [portál Azure](https://portal.azure.com/), hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f92b-309">After logging in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>

   1. <span data-ttu-id="5f92b-310">Klikněte na tlačítko **nový** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-310">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="5f92b-311">Klikněte na tlačítko **Data + analýzy** v hello **nový** okno.</span><span class="sxs-lookup"><span data-stu-id="5f92b-311">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="5f92b-312">Klikněte na tlačítko **Data Factory** na hello **analýzy dat** okno.</span><span class="sxs-lookup"><span data-stu-id="5f92b-312">Click **Data Factory** on hello **Data analytics** blade.</span></span>
2. <span data-ttu-id="5f92b-313">V hello **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="5f92b-313">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="5f92b-314">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5f92b-314">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="5f92b-315">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například **yournameCustomActivityFactory**) a zkuste vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-315">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>
3. <span data-ttu-id="5f92b-316">Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5f92b-316">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="5f92b-317">Ověřte, že používáte správné předplatné hello a oblast, kam chcete hello data factory toobe vytvořit.</span><span class="sxs-lookup"><span data-stu-id="5f92b-317">Verify that you are using hello correct subscription and region where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="5f92b-318">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="5f92b-318">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="5f92b-319">Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-319">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="5f92b-320">Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-320">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image6.png)

#### <a name="step-2-create-linked-services"></a><span data-ttu-id="5f92b-321">Krok 2: Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="5f92b-321">Step 2: Create linked services</span></span>
<span data-ttu-id="5f92b-322">Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-322">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="5f92b-323">V tomto kroku propojíte vaše **Azure Storage** účtu a **Azure Batch** objekt pro vytváření dat účet tooyour.</span><span class="sxs-lookup"><span data-stu-id="5f92b-323">In this step, you link your **Azure Storage** account and **Azure Batch** account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="5f92b-324">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="5f92b-324">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="5f92b-325">Klikněte na tlačítko hello **vytvořit a nasadit** na hello dlaždici **DATA FACTORY** okno pro **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-325">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="5f92b-326">Zobrazí hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-326">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="5f92b-327">Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **úložiště Azure.**</span><span class="sxs-lookup"><span data-stu-id="5f92b-327">Click **New data store** on hello command bar and choose **Azure storage.**</span></span> <span data-ttu-id="5f92b-328">Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-328">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image7.png)

3. <span data-ttu-id="5f92b-329">Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-329">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="5f92b-330">toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5f92b-330">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

4. <span data-ttu-id="5f92b-331">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5f92b-331">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image8.png)

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="5f92b-332">Vytvoření služby Azure Batch propojené</span><span class="sxs-lookup"><span data-stu-id="5f92b-332">Create Azure Batch linked service</span></span>
<span data-ttu-id="5f92b-333">V tomto kroku vytvoříte propojené služby pro vaše **Azure Batch** účtu, který je použité toorun hello objekt pro vytváření dat vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5f92b-333">In this step, you create a linked service for your **Azure Batch** account that is used toorun hello Data Factory custom activity.</span></span>

1. <span data-ttu-id="5f92b-334">Klikněte na tlačítko **nový výpočet** na panelu příkazů hello a zvolte **Azure Batch.**</span><span class="sxs-lookup"><span data-stu-id="5f92b-334">Click **New compute** on hello command bar and choose **Azure Batch.**</span></span> <span data-ttu-id="5f92b-335">Měli byste vidět hello skript JSON pro vytvoření Azure Batch propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-335">You should see hello JSON script for creating an Azure Batch linked service in hello editor.</span></span>
2. <span data-ttu-id="5f92b-336">V hello skript JSON:</span><span class="sxs-lookup"><span data-stu-id="5f92b-336">In hello JSON script:</span></span>

   1. <span data-ttu-id="5f92b-337">Nahraďte **název účtu** s názvem hello účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-337">Replace **account name** with hello name of your Azure Batch account.</span></span>
   2. <span data-ttu-id="5f92b-338">Nahraďte **přístupový klíč** s přístupovým klíčem hello hello účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-338">Replace **access key** with hello access key of hello Azure Batch account.</span></span>
   3. <span data-ttu-id="5f92b-339">Zadejte ID hello hello fondu pro hello **poolName** vlastnost**.**</span><span class="sxs-lookup"><span data-stu-id="5f92b-339">Enter hello ID of hello pool for hello **poolName** property**.**</span></span> <span data-ttu-id="5f92b-340">Pro tuto vlastnost lze zadat buď název fondu nebo fondu ID.</span><span class="sxs-lookup"><span data-stu-id="5f92b-340">For this property, you can specify either pool name or pool ID.</span></span>
   4. <span data-ttu-id="5f92b-341">Zadejte hello batch identifikátor URI pro hello **batchUri** vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="5f92b-341">Enter hello batch URI for hello **batchUri** JSON property.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="5f92b-342">Hello **URL** z hello **okně účtu Azure Batch** je ve formátu hello: \<accountname\>.\< oblast\>. batch.azure.com. Pro hello **batchUri** vlastnost hello JSON, budete potřebovat příliš**odebrat "název účtu."**</span><span class="sxs-lookup"><span data-stu-id="5f92b-342">hello **URL** from hello **Azure Batch account blade** is in hello following format: \<accountname\>.\<region\>.batch.azure.com. For hello **batchUri** property in hello JSON, you need too**remove "accountname."**</span></span> <span data-ttu-id="5f92b-343">z adresy URL hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-343">from hello URL.</span></span> <span data-ttu-id="5f92b-344">Příklad: `"batchUri": "https://eastus.batch.azure.com"`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-344">Example: `"batchUri": "https://eastus.batch.azure.com"`.</span></span>
      >
      >

      ![](./media/data-factory-data-processing-using-batch/image9.png)

      <span data-ttu-id="5f92b-345">Pro hello **poolName** vlastnost, můžete také zadat hello ID fondu hello místo názvu hello hello fondu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-345">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!NOTE]
      > <span data-ttu-id="5f92b-346">Hello služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="5f92b-346">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="5f92b-347">Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-347">You can only use your own Azure Batch pool in an Azure data factory.</span></span>
      >
      >
   5. <span data-ttu-id="5f92b-348">Zadejte **StorageLinkedService** pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5f92b-348">Specify **StorageLinkedService** for hello **linkedServiceName** property.</span></span> <span data-ttu-id="5f92b-349">Tuto propojenou službu jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-349">You created this linked service in hello previous step.</span></span> <span data-ttu-id="5f92b-350">Toto úložiště se používá jako pracovní oblast pro soubory a protokoly.</span><span class="sxs-lookup"><span data-stu-id="5f92b-350">This storage is used as a staging area for files and logs.</span></span>
3. <span data-ttu-id="5f92b-351">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5f92b-351">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="step-3-create-datasets"></a><span data-ttu-id="5f92b-352">Krok 3: Vytvoření datové sady</span><span class="sxs-lookup"><span data-stu-id="5f92b-352">Step 3: Create datasets</span></span>
<span data-ttu-id="5f92b-353">V tomto kroku vytvoříte datové sady toorepresent vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="5f92b-353">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="5f92b-354">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="5f92b-354">Create input dataset</span></span>
1. <span data-ttu-id="5f92b-355">V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **nová datová sada** na panelu nástrojů hello a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-355">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="5f92b-356">Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="5f92b-356">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

    <span data-ttu-id="5f92b-357">Vytvoření kanálu dále v tomto návodu se časem spuštění: 2015-11-16T00:00:00Z a koncový čas: 2015-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="5f92b-357">You create a pipeline later in this walkthrough with start time: 2015-11-16T00:00:00Z and end time: 2015-11-16T05:00:00Z.</span></span> <span data-ttu-id="5f92b-358">Je naplánované tooproduce data **každou hodinu**, takže se 5 řezy vstupní a výstupní (mezi **00**: 00:00 -\> **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="5f92b-358">It is scheduled tooproduce data **hourly**, so there are 5 input/output slices (between **00**:00:00 -\> **05**:00:00).</span></span>

    <span data-ttu-id="5f92b-359">Hello **frekvence** a **interval** hello vstupní datové sady je nastavený příliš**hodinu** a **1**, což znamená, že hello vstupní řez je k dispozici každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-359">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span>

    <span data-ttu-id="5f92b-360">Tady jsou hello času zahájení pro každý řez, která je reprezentována **SliceStart** systémové proměnné v hello výše fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="5f92b-360">Here are hello start times for each slice, which is represented by **SliceStart** system variable in hello above JSON snippet.</span></span>

    | <span data-ttu-id="5f92b-361">**Řez**</span><span class="sxs-lookup"><span data-stu-id="5f92b-361">**Slice**</span></span> | <span data-ttu-id="5f92b-362">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="5f92b-362">**Start time**</span></span>          |
    |-----------|-------------------------|
    | <span data-ttu-id="5f92b-363">1</span><span class="sxs-lookup"><span data-stu-id="5f92b-363">1</span></span>         | <span data-ttu-id="5f92b-364">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-364">2015-11-16T**00**:00:00</span></span> |
    | <span data-ttu-id="5f92b-365">2</span><span class="sxs-lookup"><span data-stu-id="5f92b-365">2</span></span>         | <span data-ttu-id="5f92b-366">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-366">2015-11-16T**01**:00:00</span></span> |
    | <span data-ttu-id="5f92b-367">3</span><span class="sxs-lookup"><span data-stu-id="5f92b-367">3</span></span>         | <span data-ttu-id="5f92b-368">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-368">2015-11-16T**02**:00:00</span></span> |
    | <span data-ttu-id="5f92b-369">4</span><span class="sxs-lookup"><span data-stu-id="5f92b-369">4</span></span>         | <span data-ttu-id="5f92b-370">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-370">2015-11-16T**03**:00:00</span></span> |
    | <span data-ttu-id="5f92b-371">5</span><span class="sxs-lookup"><span data-stu-id="5f92b-371">5</span></span>         | <span data-ttu-id="5f92b-372">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-372">2015-11-16T**04**:00:00</span></span> |

    <span data-ttu-id="5f92b-373">Hello **folderPath** se vypočítává pomocí hello rok, měsíc, den a hodina část času spuštění řezu hello (**SliceStart**).</span><span class="sxs-lookup"><span data-stu-id="5f92b-373">hello **folderPath** is calculated by using hello year, month, day, and hour part of hello slice start time (**SliceStart**).</span></span> <span data-ttu-id="5f92b-374">Zde je proto jak vstupní složky je namapované tooa řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-374">Therefore, here is how an input folder is mapped tooa slice.</span></span>

    | <span data-ttu-id="5f92b-375">**Řez**</span><span class="sxs-lookup"><span data-stu-id="5f92b-375">**Slice**</span></span> | <span data-ttu-id="5f92b-376">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="5f92b-376">**Start time**</span></span>          | <span data-ttu-id="5f92b-377">**Vstupní složky**</span><span class="sxs-lookup"><span data-stu-id="5f92b-377">**Input folder**</span></span>  |
    |-----------|-------------------------|-------------------|
    | <span data-ttu-id="5f92b-378">1</span><span class="sxs-lookup"><span data-stu-id="5f92b-378">1</span></span>         | <span data-ttu-id="5f92b-379">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-379">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="5f92b-380">2015-11-16-**00**</span><span class="sxs-lookup"><span data-stu-id="5f92b-380">2015-11-16-**00**</span></span> |
    | <span data-ttu-id="5f92b-381">2</span><span class="sxs-lookup"><span data-stu-id="5f92b-381">2</span></span>         | <span data-ttu-id="5f92b-382">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-382">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="5f92b-383">2015-11-16-**01**</span><span class="sxs-lookup"><span data-stu-id="5f92b-383">2015-11-16-**01**</span></span> |
    | <span data-ttu-id="5f92b-384">3</span><span class="sxs-lookup"><span data-stu-id="5f92b-384">3</span></span>         | <span data-ttu-id="5f92b-385">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-385">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="5f92b-386">2015-11-16-**02**</span><span class="sxs-lookup"><span data-stu-id="5f92b-386">2015-11-16-**02**</span></span> |
    | <span data-ttu-id="5f92b-387">4</span><span class="sxs-lookup"><span data-stu-id="5f92b-387">4</span></span>         | <span data-ttu-id="5f92b-388">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-388">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="5f92b-389">2015-11-16-**03**</span><span class="sxs-lookup"><span data-stu-id="5f92b-389">2015-11-16-**03**</span></span> |
    | <span data-ttu-id="5f92b-390">5</span><span class="sxs-lookup"><span data-stu-id="5f92b-390">5</span></span>         | <span data-ttu-id="5f92b-391">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-391">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="5f92b-392">2015-11-16-**04**</span><span class="sxs-lookup"><span data-stu-id="5f92b-392">2015-11-16-**04**</span></span> |

1. <span data-ttu-id="5f92b-393">Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **InputDataset** tabulky.</span><span class="sxs-lookup"><span data-stu-id="5f92b-393">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset** table.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="5f92b-394">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="5f92b-394">Create output dataset</span></span>
<span data-ttu-id="5f92b-395">V tomto kroku vytvoříte jinou datovou sadu typu AzureBlob toorepresent hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="5f92b-395">In this step, you create another dataset of type AzureBlob toorepresent hello output data.</span></span>

1. <span data-ttu-id="5f92b-396">V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **nová datová sada** na panelu nástrojů hello a klikněte na tlačítko **úložiště objektů Azure Blob** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-396">In hello **Editor** for hello Data Factory, click **New dataset** button on hello toolbar and click **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="5f92b-397">Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="5f92b-397">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

    <span data-ttu-id="5f92b-398">Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-398">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="5f92b-399">Zde je, jak je výstupní soubor s názvem pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-399">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="5f92b-400">Všechny soubory výstup hello vygenerují na jednu výstupní složky: `mycontainer\\outputfolder`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-400">All hello output files are generated in one output folder: `mycontainer\\outputfolder`.</span></span>

    | <span data-ttu-id="5f92b-401">**Řez**</span><span class="sxs-lookup"><span data-stu-id="5f92b-401">**Slice**</span></span> | <span data-ttu-id="5f92b-402">**Čas spuštění**</span><span class="sxs-lookup"><span data-stu-id="5f92b-402">**Start time**</span></span>          | <span data-ttu-id="5f92b-403">**Výstupní soubor**</span><span class="sxs-lookup"><span data-stu-id="5f92b-403">**Output file**</span></span>       |
    |-----------|-------------------------|-----------------------|
    | <span data-ttu-id="5f92b-404">1</span><span class="sxs-lookup"><span data-stu-id="5f92b-404">1</span></span>         | <span data-ttu-id="5f92b-405">2015. 11 16T**00**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-405">2015-11-16T**00**:00:00</span></span> | <span data-ttu-id="5f92b-406">2015-11-16 -**00. txt**</span><span class="sxs-lookup"><span data-stu-id="5f92b-406">2015-11-16-**00.txt**</span></span> |
    | <span data-ttu-id="5f92b-407">2</span><span class="sxs-lookup"><span data-stu-id="5f92b-407">2</span></span>         | <span data-ttu-id="5f92b-408">2015. 11 16T**01**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-408">2015-11-16T**01**:00:00</span></span> | <span data-ttu-id="5f92b-409">2015-11-16 -**01. txt**</span><span class="sxs-lookup"><span data-stu-id="5f92b-409">2015-11-16-**01.txt**</span></span> |
    | <span data-ttu-id="5f92b-410">3</span><span class="sxs-lookup"><span data-stu-id="5f92b-410">3</span></span>         | <span data-ttu-id="5f92b-411">2015. 11 16T**02**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-411">2015-11-16T**02**:00:00</span></span> | <span data-ttu-id="5f92b-412">2015-11-16 -**02. txt**</span><span class="sxs-lookup"><span data-stu-id="5f92b-412">2015-11-16-**02.txt**</span></span> |
    | <span data-ttu-id="5f92b-413">4</span><span class="sxs-lookup"><span data-stu-id="5f92b-413">4</span></span>         | <span data-ttu-id="5f92b-414">2015. 11 16T**03**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-414">2015-11-16T**03**:00:00</span></span> | <span data-ttu-id="5f92b-415">2015-11-16 -**03. txt**</span><span class="sxs-lookup"><span data-stu-id="5f92b-415">2015-11-16-**03.txt**</span></span> |
    | <span data-ttu-id="5f92b-416">5</span><span class="sxs-lookup"><span data-stu-id="5f92b-416">5</span></span>         | <span data-ttu-id="5f92b-417">2015. 11 16T**04**: 00:00</span><span class="sxs-lookup"><span data-stu-id="5f92b-417">2015-11-16T**04**:00:00</span></span> | <span data-ttu-id="5f92b-418">2015-11-16 -**04. txt**</span><span class="sxs-lookup"><span data-stu-id="5f92b-418">2015-11-16-**04.txt**</span></span> |

    <span data-ttu-id="5f92b-419">Mějte na paměti, že všechny soubory ve složce aplikace vstupní hello (například: 2015-11-16-00) jsou součástí řez se časem spuštění hello: 2015-11-16-00.</span><span class="sxs-lookup"><span data-stu-id="5f92b-419">Remember that all hello files in an input folder (for example: 2015-11-16-00) are part of a slice with hello start time: 2015-11-16-00.</span></span> <span data-ttu-id="5f92b-420">Při zpracování této řezu se vlastní aktivity hello prohledává každý soubor a vytvoří řádek v souboru výstup hello s hello počtu výskytů hledaný termín ("Microsoft").</span><span class="sxs-lookup"><span data-stu-id="5f92b-420">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="5f92b-421">Pokud existují tři soubory ve složce hello 2015 11 16 00, se ve výstupním souboru hello tři řádky: 2015-11-16-00.txt.</span><span class="sxs-lookup"><span data-stu-id="5f92b-421">If there are three files in hello folder 2015-11-16-00, there are three lines in hello output file: 2015-11-16-00.txt.</span></span>

1. <span data-ttu-id="5f92b-422">Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-422">Click **Deploy** on hello toolbar toocreate and deploy hello **OutputDataset**.</span></span>

#### <a name="step-4-create-and-run-hello-pipeline-with-custom-activity"></a><span data-ttu-id="5f92b-423">Krok 4: Vytvoření a spuštění kanálu hello s vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="5f92b-423">Step 4: Create and run hello pipeline with custom activity</span></span>
<span data-ttu-id="5f92b-424">V tomto kroku vytvoříte kanál s aktivitou jeden, hello vlastní aktivity, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="5f92b-424">In this step, you create a pipeline with one activity, hello custom activity you created earlier.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f92b-425">Pokud ještě jste neodeslali hello **soubor.txt** tooinput složek v kontejneru objektu blob hello tak učinit před vytvořením kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-425">If you haven't uploaded hello **file.txt** tooinput folders in hello blob container, do so before creating hello pipeline.</span></span> <span data-ttu-id="5f92b-426">Hello **isPaused** vlastnost nastavena toofalse v kanálu hello formát JSON, takže hello kanál okamžitě spustí jako hello **spustit** datum je v minulosti hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-426">hello **isPaused** property is set toofalse in hello pipeline JSON, so hello pipeline runs immediately as hello **start** date is in hello past.</span></span>
>
>

1. <span data-ttu-id="5f92b-427">V editoru služby Data Factory hello, klikněte na **nový kanál** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-427">In hello Data Factory Editor, click **New pipeline** on hello command bar.</span></span> <span data-ttu-id="5f92b-428">Pokud se příkaz hello nezobrazí, klikněte na tlačítko **... (Tři tečky)**  toosee ho.</span><span class="sxs-lookup"><span data-stu-id="5f92b-428">If you do not see hello command, click **... (Ellipsis)** toosee it.</span></span>
2. <span data-ttu-id="5f92b-429">Nahraďte hello JSON v pravém podokně hello hello následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="5f92b-429">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

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
   <span data-ttu-id="5f92b-430">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="5f92b-430">Note hello following points:</span></span>

   * <span data-ttu-id="5f92b-431">Je jenom jedna aktivita v kanálu hello a který je typu: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-431">There is only one activity in hello pipeline and that is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="5f92b-432">**AssemblyName** nastavena toohello název hello knihovny DLL: **MyDotNetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-432">**AssemblyName** is set toohello name of hello DLL: **MyDotNetActivity.dll**.</span></span>
   * <span data-ttu-id="5f92b-433">**EntryPoint** je nastaven příliš**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-433">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span> <span data-ttu-id="5f92b-434">Je v podstatě \<obor názvů\>.\< Název třídy\> ve vašem kódu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-434">It is basically \<namespace\>.\<classname\> in your code.</span></span>
   * <span data-ttu-id="5f92b-435">**PackageLinkedService** je nastaven příliš**StorageLinkedService** který odkazuje toohello úložiště objektů blob, který obsahuje soubor zip hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5f92b-435">**PackageLinkedService** is set too**StorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="5f92b-436">Pokud používáte jiné účty Azure Storage pro vstupní a výstupní soubory a soubor zip hello vlastní aktivitu, máte toocreate jiné Azure Storage propojené služby.</span><span class="sxs-lookup"><span data-stu-id="5f92b-436">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you have toocreate another Azure Storage linked service.</span></span> <span data-ttu-id="5f92b-437">Tento článek předpokládá, že používáte hello stejný účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5f92b-437">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="5f92b-438">**PackageFile** je nastaven příliš**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-438">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="5f92b-439">Je ve formátu hello: \<containerforthezip\>/\<nameofthezip.zip\>.</span><span class="sxs-lookup"><span data-stu-id="5f92b-439">It is in hello format: \<containerforthezip\>/\<nameofthezip.zip\>.</span></span>
   * <span data-ttu-id="5f92b-440">vlastní aktivita Hello přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="5f92b-440">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="5f92b-441">Hello **linkedServiceName** vlastnost vlastní aktivity hello ukazuje toohello **AzureBatchLinkedService**, která informuje Azure Data Factory hello vlastní aktivity musí toorun na Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-441">hello **linkedServiceName** property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch.</span></span>
   * <span data-ttu-id="5f92b-442">Hello **souběžnosti** nastavení je důležité.</span><span class="sxs-lookup"><span data-stu-id="5f92b-442">hello **concurrency** setting is important.</span></span> <span data-ttu-id="5f92b-443">Pokud používáte hello výchozí hodnotu, která je 1, i v případě, že máte 2 nebo více výpočetních uzlů ve fondu Azure Batch hello, hello řezy jsou zpracovávány jeden po druhém.</span><span class="sxs-lookup"><span data-stu-id="5f92b-443">If you use hello default value, which is 1, even if you have 2 or more compute nodes in hello Azure Batch pool, hello slices are processed one after another.</span></span> <span data-ttu-id="5f92b-444">Proto nejsou využívat výhod hello paralelní zpracování funkce Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-444">Therefore, you are not taking advantage of hello parallel processing capability of Azure Batch.</span></span> <span data-ttu-id="5f92b-445">Pokud nastavíte **souběžnosti** tooa vyšší hodnota, například 2, znamená to, že dva řezy (odpovídá tootwo úlohy v Azure Batch) může zpracovat hello stejný čas, v takovém případě oba hello virtuální počítače v Azure Batch jsou využité fondu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-445">If you set **concurrency** tooa higher value, say 2, it means that two slices (corresponds tootwo tasks in Azure Batch) can be processed at hello same time, in which case, both hello VMs in hello Azure Batch pool are utilized.</span></span> <span data-ttu-id="5f92b-446">Proto správně nastavte vlastnost souběžnosti hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-446">Therefore, set hello concurrency property appropriately.</span></span>
   * <span data-ttu-id="5f92b-447">Pouze jednu úlohu (řez) je spustit na virtuálním počítači v libovolném bodě ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5f92b-447">Only one task (slice) is executed on a VM at any point by default.</span></span> <span data-ttu-id="5f92b-448">Hello důvod předpokládají, že ve výchozím nastavení, hello **maximální počet úloh na virtuální počítač** nastavena too1 fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-448">hello reason is that, by default, hello **Maximum tasks per VM** is set too1 for an Azure Batch pool.</span></span> <span data-ttu-id="5f92b-449">V rámci požadavků, jste vytvořili fond s too2 sadu tuto vlastnost, tak dva řezy objekt pro vytváření dat může mít spuštěný na virtuálním počítači na hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-449">As part of prerequisites, you created a pool with this property set too2, so two Data Factory slices can be running on a VM at hello same time.</span></span>

    -   <span data-ttu-id="5f92b-450">**isPaused** vlastnost je ve výchozím nastavení toofalse.</span><span class="sxs-lookup"><span data-stu-id="5f92b-450">**isPaused** property is set toofalse by default.</span></span> <span data-ttu-id="5f92b-451">kanál Hello spustí hned v tomto příkladu, protože hello řezy spustit v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-451">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="5f92b-452">Můžete nastavit tuto vlastnost tootrue toopause hello kanálu a nastavte ji zpět toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="5f92b-452">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>

    -   <span data-ttu-id="5f92b-453">Hello **spustit** čas a **end** časy jsou od sebe pět hodin a řezy vytváří každou hodinu, takže pět řezy vytváří hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-453">hello **start** time and **end** times are five hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>

1. <span data-ttu-id="5f92b-454">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-454">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span>

#### <a name="step-5-test-hello-pipeline"></a><span data-ttu-id="5f92b-455">Krok 5: Testování hello kanálu</span><span class="sxs-lookup"><span data-stu-id="5f92b-455">Step 5: Test hello pipeline</span></span>
<span data-ttu-id="5f92b-456">V tomto kroku otestovat hello kanálu přetažením souborů do složky vstupní hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-456">In this step, you test hello pipeline by dropping files into hello input folders.</span></span> <span data-ttu-id="5f92b-457">Začneme s testování kanálu hello jeden soubor pro jeden vstupní složky.</span><span class="sxs-lookup"><span data-stu-id="5f92b-457">Let’s start with testing hello pipeline with one file per one input folder.</span></span>

1. <span data-ttu-id="5f92b-458">V okně Data Factory hello v hello portálu Azure, klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-458">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image10.png)
2. <span data-ttu-id="5f92b-459">V zobrazení diagramu hello, klikněte dvakrát na vstupní datové sady: **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-459">In hello diagram view, double-click input dataset: **InputDataset**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image11.png)
3. <span data-ttu-id="5f92b-460">Měli byste vidět hello **InputDataset** okno s pěti všechny řezy připraven.</span><span class="sxs-lookup"><span data-stu-id="5f92b-460">You should see hello **InputDataset** blade with all five slices ready.</span></span> <span data-ttu-id="5f92b-461">Všimněte si hello **čas spuštění ŘEZU** a **ŘEZ KONCOVÝ čas** pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-461">Notice hello **SLICE START TIME** and **SLICE END TIME** for each slice.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image12.png)
4. <span data-ttu-id="5f92b-462">V hello **zobrazení diagramu**, klikněte na tlačítko **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-462">In hello **Diagram View**, now click **OutputDataset**.</span></span>
5. <span data-ttu-id="5f92b-463">Měli byste vidět, že hello pět Výstup řezy jsou ve stavu Připraveno hello, pokud již byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="5f92b-463">You should see that hello five output slices are in hello Ready state if they have already been produced.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image13.png)
6. <span data-ttu-id="5f92b-464">Použití portálu Azure tooview hello **úlohy** přidružené hello **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-464">Use Azure portal tooview hello **tasks** associated with hello **slices** and see what VM each slice ran on.</span></span> <span data-ttu-id="5f92b-465">V tématu [integraci služby Data Factory a Batch](#data-factory-and-batch-integration) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5f92b-465">See [Data Factory and Batch integration](#data-factory-and-batch-integration) section for details.</span></span>
7. <span data-ttu-id="5f92b-466">Měli byste vidět hello výstupní soubory v hello `outputfolder` z `mycontainer` ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5f92b-466">You should see hello output files in hello `outputfolder` of `mycontainer` in your Azure blob storage.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image15.png)

   <span data-ttu-id="5f92b-467">Měli byste vidět pět výstupní soubory, jeden pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-467">You should see five output files, one for each input slice.</span></span> <span data-ttu-id="5f92b-468">Každý z hello výstupu soubor by měl mít obsahu podobné toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="5f92b-468">Each of hello output file should have content similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-00/file.txt.
    ```
   <span data-ttu-id="5f92b-469">Hello následující diagram znázorňuje, jak hello Data Factory řezy mapování tootasks ve službě Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-469">hello following diagram illustrates how hello Data Factory slices map tootasks in Azure Batch.</span></span> <span data-ttu-id="5f92b-470">V tomto příkladu má řez spustit pouze jeden.</span><span class="sxs-lookup"><span data-stu-id="5f92b-470">In this example, a slice has only one run.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image16.png)
8. <span data-ttu-id="5f92b-471">Nyní zkuste to prosím ještě s více soubory ve složce.</span><span class="sxs-lookup"><span data-stu-id="5f92b-471">Now, let’s try with multiple files in a folder.</span></span> <span data-ttu-id="5f92b-472">Vytvoření souborů: **Soubor2.txt**, **file3.txt**, **file4.txt**, a **file5.txt** s hello stejný obsah jako soubor.txt ve složce hello: **2015-11-06-01**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-472">Create files: **file2.txt**, **file3.txt**, **file4.txt**, and **file5.txt** with hello same content as in file.txt in hello folder: **2015-11-06-01**.</span></span>
9. <span data-ttu-id="5f92b-473">Ve složce výstup hello **odstranit** hello výstupního souboru: **2015 11 16 01.txt**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-473">In hello output folder, **delete** hello output file: **2015-11-16-01.txt**.</span></span>
10. <span data-ttu-id="5f92b-474">Nyní v hello **OutputDataset** okno, klikněte pravým tlačítkem na hello řez s **čas spuštění ŘEZU** nastavit příliš**11/16/2015 01:00:00 AM**a klikněte na tlačítko **spustit**hello toorerun nebo zpětný-process řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-474">Now, in hello **OutputDataset** blade, right-click hello slice with **SLICE START TIME** set too**11/16/2015 01:00:00 AM**, and click **Run** toorerun/re-process hello slice.</span></span> <span data-ttu-id="5f92b-475">Řez hello teď má pět souborů místo jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="5f92b-475">Now, hello slice has five files instead of one file.</span></span>

    ![](./media/data-factory-data-processing-using-batch/image17.png)
11. <span data-ttu-id="5f92b-476">Po spuštění řezu hello a její stav je **připraven**, ověřte obsah hello hello výstupního souboru pro tento řez (**2015 11 16 01.txt**) v hello `outputfolder` z `mycontainer` ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="5f92b-476">After hello slice runs and its status is **Ready**, verify hello content in hello output file for this slice (**2015-11-16-01.txt**) in hello `outputfolder` of `mycontainer` in your blob storage.</span></span> <span data-ttu-id="5f92b-477">Měla by existovat řádek pro každý soubor hello řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-477">There should be a line for each file of hello slice.</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file2.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file3.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file4.txt.
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2015-11-16-01/file5.txt.
    ```

> [!NOTE]
> <span data-ttu-id="5f92b-478">Pokud jste před dalším pokusem s pěti vstupní soubory neodstranila 2015 modulu hello výstupní soubor-11-16-01.txt, zobrazí jeden řádek z předchozí řez hello spustit a pěti řádcích z hello spuštění aktuálního řezu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-478">If you did not delete hello output file 2015-11-16-01.txt before trying with five input files, you see one line from hello previous slice run and five lines from hello current slice run.</span></span> <span data-ttu-id="5f92b-479">Ve výchozím nastavení je hello obsah připojením toooutput soubor, pokud již existuje.</span><span class="sxs-lookup"><span data-stu-id="5f92b-479">By default, hello content is appended toooutput file if it already exists.</span></span>
>
>

#### <a name="data-factory-and-batch-integration"></a><span data-ttu-id="5f92b-480">Integrace služby Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-480">Data Factory and Batch integration</span></span>
<span data-ttu-id="5f92b-481">Hello služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem hello: `adf-poolname:job-xxx`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-481">hello Data Factory service creates a job in Azure Batch with hello name: `adf-poolname:job-xxx`.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-jobs.png)

<span data-ttu-id="5f92b-483">Úloha v úloze hello se vytvoří při každém spuštění aktivity řezu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-483">A task in hello job is created for each activity run of a slice.</span></span> <span data-ttu-id="5f92b-484">Pokud existují 10 toobe připraven řezy, které se zpracovat, vytvoření 10 úkolů v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-484">If there are 10 slices ready toobe processed, 10 tasks are created in hello job.</span></span> <span data-ttu-id="5f92b-485">Můžete mít více než jeden řez spouštět současně, pokud máte několika výpočetních uzlech ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-485">You can have more than one slice running in parallel if you have multiple compute nodes in hello pool.</span></span> <span data-ttu-id="5f92b-486">Pokud hello maximální úkolů na výpočetní uzel je nastaven příliš > 1, může existovat více než jeden řezu systémem hello stejné výpočty.</span><span class="sxs-lookup"><span data-stu-id="5f92b-486">If hello maximum tasks per compute node is set too> 1, there can be more than one slice running on hello same compute.</span></span>

<span data-ttu-id="5f92b-487">V tomto příkladu jsou pět řezů, takže pět úlohy v Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-487">In this example, there are five slices, so five tasks in Azure Batch.</span></span> <span data-ttu-id="5f92b-488">S hello **souběžnosti** nastavit příliš**5** v hello kanálu JSON v Azure Data Factory a **maximální počet úloh na virtuální počítač** nastavit příliš**2** ve službě Azure Batch fond s **2** virtuálních počítačů, hello úlohy spustí rychlou (zkontrolujte, zda počáteční a koncový čas pro úlohy).</span><span class="sxs-lookup"><span data-stu-id="5f92b-488">With hello **concurrency** set too**5** in hello pipeline JSON in Azure Data Factory and **Maximum tasks per VM** set too**2** in Azure Batch pool with **2** VMs, hello tasks runs fast (check start and end times for tasks).</span></span>

<span data-ttu-id="5f92b-489">Použít hello portálu tooview hello dávkové úlohy a její úkoly, které jsou přidruženy hello **řezy** a zjistit, jaké virtuálního počítače byla spuštěna na každý řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-489">Use hello portal tooview hello Batch job and its tasks that are associated with hello **slices** and see what VM each slice ran on.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-data-processing-using-batch/data-factory-batch-job-tasks.png)

### <a name="debug-hello-pipeline"></a><span data-ttu-id="5f92b-491">Ladění hello kanálu</span><span class="sxs-lookup"><span data-stu-id="5f92b-491">Debug hello pipeline</span></span>
<span data-ttu-id="5f92b-492">Ladění zahrnuje několik základních technik:</span><span class="sxs-lookup"><span data-stu-id="5f92b-492">Debugging consists of a few basic techniques:</span></span>

1. <span data-ttu-id="5f92b-493">Pokud vstupní řez hello není nastaven příliš**připraven**, potvrďte, že vstupní složky struktura hello je správná a soubor.txt existuje ve vstupní složkách hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-493">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and file.txt exists in hello input folders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image3.png)
2. <span data-ttu-id="5f92b-494">V hello **Execute** metoda vlastní aktivity, použijte hello **IActivityLogger** toolog informace o objektu, který vám pomůže vyřešit problémy.</span><span class="sxs-lookup"><span data-stu-id="5f92b-494">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="5f92b-495">zprávy Hello přihlášení zobrazí v hello uživatele\_souboru protokolu 0.</span><span class="sxs-lookup"><span data-stu-id="5f92b-495">hello logged messages show up in hello user\_0.log file.</span></span>

   <span data-ttu-id="5f92b-496">V hello **OutputDataset** okně klikněte na tlačítko hello řez toosee hello **datový ŘEZ** okno pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-496">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="5f92b-497">Zobrazí **běh aktivit** pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-497">You see **activity runs** for that slice.</span></span> <span data-ttu-id="5f92b-498">Měli byste vidět jednu aktivitu spustit pro řez hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-498">You should see one activity run for hello slice.</span></span> <span data-ttu-id="5f92b-499">Pokud kliknete na tlačítko **spustit** v řádku nabídek hello můžete spustit jinou aktivitu spustit pro hello stejné řez.</span><span class="sxs-lookup"><span data-stu-id="5f92b-499">If you click **Run** in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="5f92b-500">Po kliknutí na tlačítko hello aktivity při spuštění, se zobrazí hello **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-500">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="5f92b-501">Zobrazí zprávy zaznamenané v hello **uživatele\_0 protokolu** souboru.</span><span class="sxs-lookup"><span data-stu-id="5f92b-501">You see logged messages in hello **user\_0.log** file.</span></span> <span data-ttu-id="5f92b-502">Pokud dojde k chybě, uvidíte tři běh aktivit protože hello počet opakování je nastavena too3 v kanálu nebo aktivity hello JSON.</span><span class="sxs-lookup"><span data-stu-id="5f92b-502">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="5f92b-503">Po kliknutí na tlačítko hello aktivity při spuštění, zobrazí hello soubory protokolů, abyste si prošli tootroubleshoot hello chyby.</span><span class="sxs-lookup"><span data-stu-id="5f92b-503">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image18.png)

   <span data-ttu-id="5f92b-504">V seznamu hello protokolových souborů, klikněte na tlačítko hello **uživatele 0.log**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-504">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="5f92b-505">V pravém panelu hello jsou výsledky hello pomocí hello **IActivityLogger.Write** metoda.</span><span class="sxs-lookup"><span data-stu-id="5f92b-505">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image19.png)

   <span data-ttu-id="5f92b-506">Kontrola systému 0.log pro všechny chybové zprávy systému a výjimky.</span><span class="sxs-lookup"><span data-stu-id="5f92b-506">Check system-0.log for any system error messages and exceptions.</span></span>

    ```
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Loading assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Creating an instance of MyDotNetActivityNS.MyDotNetActivity from assembly file MyDotNetActivity...
    
    Trace\_T\_D\_12/6/2015 1:43:35 AM\_T\_D\_\_T\_D\_Verbose\_T\_D\_0\_T\_D\_Executing Module
    
    Trace\_T\_D\_12/6/2015 1:43:38 AM\_T\_D\_\_T\_D\_Information\_T\_D\_0\_T\_D\_Activity e3817da0-d843-4c5c-85c6-40ba7424dce2 finished successfully
    ```
3. <span data-ttu-id="5f92b-507">Zahrnout hello **PDB** souborů v souboru zip hello tak, aby podrobnosti o chybě hello informace, jako **zásobník volání** když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="5f92b-507">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
4. <span data-ttu-id="5f92b-508">Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s ne v podsložkách.</span><span class="sxs-lookup"><span data-stu-id="5f92b-508">All hello files in hello zip file for hello custom activity must be at hello **top level** with no subfolders.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image20.png)
5. <span data-ttu-id="5f92b-509">Ujistěte se, že hello **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (měli toohello bodu Azure úložiště objektů blob obsahující soubor zip hello) jsou nastavené toocorrect hodnoty.</span><span class="sxs-lookup"><span data-stu-id="5f92b-509">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint** (MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
6. <span data-ttu-id="5f92b-510">Pokud můžete opravit chyby a chcete řez hello tooreprocess, klikněte pravým tlačítkem na hello řez ve hello **OutputDataset** a klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-510">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>

   ![](./media/data-factory-data-processing-using-batch/image21.png)

   > [!NOTE]
   > <span data-ttu-id="5f92b-511">Zobrazí **kontejneru** ve službě Azure Blob storage s názvem: `adfjobs`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-511">You see a **container** in your Azure Blob storage named: `adfjobs`.</span></span> <span data-ttu-id="5f92b-512">Tento kontejner není automaticky odstraněn, ale můžete ji po dokončení testování řešení hello bezpečně odstranit.</span><span class="sxs-lookup"><span data-stu-id="5f92b-512">This container is not automatically deleted, but you can safely delete it after you are done testing hello solution.</span></span> <span data-ttu-id="5f92b-513">Podobně hello řešení Data Factory vytvoří Azure Batch **úlohy** s názvem: `adf-\<pool ID/name\>:job-0000000001`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-513">Similarly, hello Data Factory solution creates an Azure Batch **job** named: `adf-\<pool ID/name\>:job-0000000001`.</span></span> <span data-ttu-id="5f92b-514">Po dokončení testu hello řešení Pokud chcete, můžete odstranit tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-514">You can delete this job after you test hello solution if you like.</span></span>
   >
   >
7. <span data-ttu-id="5f92b-515">vlastní aktivity Hello nepoužívá hello **app.config** soubor ze svého balíčku.</span><span class="sxs-lookup"><span data-stu-id="5f92b-515">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="5f92b-516">Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru hello, ale nefunguje za běhu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-516">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="5f92b-517">Hello osvědčený postup, při použití Azure Batch je toohold všech tajných klíčů v **Azure KeyVault**, použijte keyvault hello hlavní tooprotect služeb na základě certifikátů a distribuovat fondu Batch tooAzure hello certifikátu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-517">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello keyvault, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="5f92b-518">Hello vlastní aktivity .NET pak k dispozici tajné klíče z hello KeyVault za běhu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-518">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="5f92b-519">Toto řešení je obecný a můžete škálovat tooany typ tajný klíč, ne jenom připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5f92b-519">This solution is a generic one and can scale tooany type of secret, not just connection string.</span></span>

    <span data-ttu-id="5f92b-520">Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** s nastavení připojovacího řetězce, vytvořit datovou sadu, zda text hello používá propojené služby a řetězu hello datovou sadu jako fiktivní vstupní datové sady toohello vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="5f92b-520">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="5f92b-521">Můžete pak přístup hello propojené služby připojovací řetězec v kódu hello vlastní aktivity a by bez problémů fungují za běhu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-521">You can then access hello linked service's connection string in hello custom activity code and it should work fine at runtime.</span></span>  

#### <a name="extend-hello-sample"></a><span data-ttu-id="5f92b-522">Rozšíření ukázka hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-522">Extend hello sample</span></span>
<span data-ttu-id="5f92b-523">Tato ukázka toolearn můžete rozšířit informace o funkcích Azure Data Factory a Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-523">You can extend this sample toolearn more about Azure Data Factory and Azure Batch features.</span></span> <span data-ttu-id="5f92b-524">Řezy tooprocess v jinou dobu rozsahu, například hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5f92b-524">For example, tooprocess slices in a different time range, do hello following steps:</span></span>

1. <span data-ttu-id="5f92b-525">Přidejte následující podsložky v hello hello `inputfolder`: 2015-11-16-05 2015-11-16-06 201-11-16-07, 2011-11-16-08, 2015-11-16-09 a umístěte vstupní soubory v těchto složkách.</span><span class="sxs-lookup"><span data-stu-id="5f92b-525">Add hello following subfolders in hello `inputfolder`: 2015-11-16-05, 2015-11-16-06, 201-11-16-07, 2011-11-16-08, 2015-11-16-09 and place input files in those folders.</span></span> <span data-ttu-id="5f92b-526">Změnit hello koncový čas pro kanál hello z `2015-11-16T05:00:00Z` příliš`2015-11-16T10:00:00Z`.</span><span class="sxs-lookup"><span data-stu-id="5f92b-526">Change hello end time for hello pipeline from `2015-11-16T05:00:00Z` too`2015-11-16T10:00:00Z`.</span></span> <span data-ttu-id="5f92b-527">V hello **zobrazení diagramu**, dvakrát klikněte na hello **InputDataset**a ověřte, zda text hello vstupní řezy jsou připravené.</span><span class="sxs-lookup"><span data-stu-id="5f92b-527">In hello **Diagram View**, double-click hello **InputDataset**, and confirm that hello input slices are ready.</span></span> <span data-ttu-id="5f92b-528">Klikněte dvakrát na **OuptutDataset** toosee hello stav výstup řezy.</span><span class="sxs-lookup"><span data-stu-id="5f92b-528">Double-click **OuptutDataset** toosee hello state of output slices.</span></span> <span data-ttu-id="5f92b-529">Pokud jsou ve stavu Připraveno, zkontrolujte hello výstupní složky pro hello výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="5f92b-529">If they are in Ready state, check hello output folder for hello output files.</span></span>
2. <span data-ttu-id="5f92b-530">Zvýšení nebo snížení hello **souběžnosti** nastavení toounderstand jak ovlivňuje výkon hello řešení, zejména hello zpracování které proběhne Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="5f92b-530">Increase or decrease hello **concurrency** setting toounderstand how it affects hello performance of your solution, especially hello processing that occurs on Azure Batch.</span></span> <span data-ttu-id="5f92b-531">(Viz krok 4: vytvoření a spuštění kanálu hello Další informace o hello **souběžnosti** nastavení.)</span><span class="sxs-lookup"><span data-stu-id="5f92b-531">(See Step 4: Create and run hello pipeline for more on hello **concurrency** setting.)</span></span>
3. <span data-ttu-id="5f92b-532">Vytvoření fondu s vyšší nebo nižší **maximální počet úloh na virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-532">Create a pool with higher/lower **Maximum tasks per VM**.</span></span> <span data-ttu-id="5f92b-533">toouse hello nový fond, které jste vytvořili, hello aktualizace služby Azure Batch propojené v řešení Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="5f92b-533">toouse hello new pool you created, update hello Azure Batch linked service in hello Data Factory solution.</span></span> <span data-ttu-id="5f92b-534">(Viz krok 4: vytvoření a spuštění kanálu hello Další informace o hello **maximální počet úloh na virtuální počítač** nastavení.)</span><span class="sxs-lookup"><span data-stu-id="5f92b-534">(See Step 4: Create and run hello pipeline for more on hello **Maximum tasks per VM** setting.)</span></span>
4. <span data-ttu-id="5f92b-535">Vytvoření fondu Azure Batch s **škálování** funkce.</span><span class="sxs-lookup"><span data-stu-id="5f92b-535">Create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="5f92b-536">Automatické škálování výpočetních uzlů ve fondu Azure Batch je hello dynamické přizpůsobení výpočetní výkon, které používá vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="5f92b-536">Automatically scaling compute nodes in an Azure Batch pool is hello dynamic adjustment of processing power used by your application.</span></span> 

    <span data-ttu-id="5f92b-537">jednoduchý vzorec Hello dosahuje hello následující chování: při počátečním vytvoření fondu hello začíná 1 virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5f92b-537">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="5f92b-538">Metrika $PendingTasks definuje hello počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.</span><span class="sxs-lookup"><span data-stu-id="5f92b-538">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="5f92b-539">Vzorec Hello najde hello průměrný počet úkolů čekajících na zpracování v hello posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="5f92b-539">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="5f92b-540">Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-540">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="5f92b-541">Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování hello zmenšuje těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5f92b-541">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="5f92b-542">startingNumberOfVMs a maxNumberofVMs může být upravenou tooyour potřebám.</span><span class="sxs-lookup"><span data-stu-id="5f92b-542">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>
 
    <span data-ttu-id="5f92b-543">Vzorec škálování:</span><span class="sxs-lookup"><span data-stu-id="5f92b-543">Autoscale formula:</span></span>

    ``` 
    startingNumberOfVMs = 1;
    maxNumberofVMs = 25;
    pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
    pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
    $TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
    ```

   <span data-ttu-id="5f92b-544">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../batch/batch-automatic-scaling.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5f92b-544">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

   <span data-ttu-id="5f92b-545">Pokud fond hello používá výchozí hello [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello služba Batch může trvat 15 až 30 minut tooprepare hello virtuálních počítačů před spuštěním hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="5f92b-545">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="5f92b-546">Pokud fond hello používá jiný autoScaleEvaluationInterval, může trvat hello služba Batch autoScaleEvaluationInterval + 10 minut.</span><span class="sxs-lookup"><span data-stu-id="5f92b-546">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>
5. <span data-ttu-id="5f92b-547">V řešení ukázka hello hello **Execute** metoda vyvolá hello **Calculate** metoda, která zpracuje vstupní data řez tooproduce řez výstupních dat.</span><span class="sxs-lookup"><span data-stu-id="5f92b-547">In hello sample solution, hello **Execute** method invokes hello **Calculate** method that processes an input data slice tooproduce an output data slice.</span></span> <span data-ttu-id="5f92b-548">Můžete napsat vlastní metoda tooprocess vstupní data a nahraďte metodu tooyour volání volání metody Calculate hello v hello Metoda Execute.</span><span class="sxs-lookup"><span data-stu-id="5f92b-548">You can write your own method tooprocess input data and replace hello Calculate method call in hello Execute method with a call tooyour method.</span></span>

### <a name="next-steps-consume-hello-data"></a><span data-ttu-id="5f92b-549">Další kroky: využívají hello data</span><span class="sxs-lookup"><span data-stu-id="5f92b-549">Next steps: Consume hello data</span></span>
<span data-ttu-id="5f92b-550">Po při zpracování dat, budete moct pracovat s online nástroje, například **Microsoft Power BI**.</span><span class="sxs-lookup"><span data-stu-id="5f92b-550">After you process data, you can consume it with online tools like **Microsoft Power BI**.</span></span> <span data-ttu-id="5f92b-551">Tady jsou odkazy toohelp pochopit Power BI a jak toouse ho v Azure:</span><span class="sxs-lookup"><span data-stu-id="5f92b-551">Here are links toohelp you understand Power BI and how toouse it in Azure:</span></span>

* [<span data-ttu-id="5f92b-552">Prozkoumejte datovou sadu v Power BI</span><span class="sxs-lookup"><span data-stu-id="5f92b-552">Explore a dataset in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-service-get-data/)
* [<span data-ttu-id="5f92b-553">Začínáme s Power BI Desktop hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-553">Getting started with hello Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)
* [<span data-ttu-id="5f92b-554">Aktualizovat data v Power BI</span><span class="sxs-lookup"><span data-stu-id="5f92b-554">Refresh data in Power BI</span></span>](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/)
* [<span data-ttu-id="5f92b-555">Azure a Power BI – základní – přehled</span><span class="sxs-lookup"><span data-stu-id="5f92b-555">Azure and Power BI - basic overview</span></span>](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

## <a name="references"></a><span data-ttu-id="5f92b-556">Odkazy</span><span class="sxs-lookup"><span data-stu-id="5f92b-556">References</span></span>
* [<span data-ttu-id="5f92b-557">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5f92b-557">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/services/data-factory/)

  * [<span data-ttu-id="5f92b-558">Úvod tooAzure služba Data Factory</span><span class="sxs-lookup"><span data-stu-id="5f92b-558">Introduction tooAzure Data Factory service</span></span>](data-factory-introduction.md)
  * [<span data-ttu-id="5f92b-559">Začínáme s Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5f92b-559">Get started with Azure Data Factory</span></span>](data-factory-build-your-first-pipeline.md)
  * [<span data-ttu-id="5f92b-560">Použití vlastních aktivit v kanálu Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="5f92b-560">Use custom activities in an Azure Data Factory pipeline</span></span>](data-factory-use-custom-activities.md)
* [<span data-ttu-id="5f92b-561">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-561">Azure Batch</span></span>](https://azure.microsoft.com/documentation/services/batch/)

  * [<span data-ttu-id="5f92b-562">Základy služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-562">Basics of Azure Batch</span></span>](../batch/batch-technical-overview.md)
  * [<span data-ttu-id="5f92b-563">Přehled funkcí Azure Batch</span><span class="sxs-lookup"><span data-stu-id="5f92b-563">Overview of Azure Batch features</span></span>](../batch/batch-api-basics.md)
  * [<span data-ttu-id="5f92b-564">Vytvoření a Správa účtu Azure Batch na portálu Azure hello</span><span class="sxs-lookup"><span data-stu-id="5f92b-564">Create and manage Azure Batch account in hello Azure portal</span></span>](../batch/batch-account-create-portal.md)
  * [<span data-ttu-id="5f92b-565">Začínáme s Azure Batch Library pro .NET</span><span class="sxs-lookup"><span data-stu-id="5f92b-565">Get started with Azure Batch Library .NET</span></span>](../batch/batch-dotnet-get-started.md)

[batch-explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[batch-explorer-walkthrough]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
