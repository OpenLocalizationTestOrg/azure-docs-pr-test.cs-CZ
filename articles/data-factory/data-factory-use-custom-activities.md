---
title: "Použití vlastních aktivit v kanálu Azure Data Factory"
description: "Zjistěte, jak vytvořit vlastní aktivity a použít je v kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8dd7ba14-15d2-4fd9-9ada-0b2c684327e9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f3d265f31cb653d32076747e586383d67bbccc41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="651a9-103">Použití vlastních aktivit v kanálu Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="651a9-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="651a9-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="651a9-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="651a9-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="651a9-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="651a9-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="651a9-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="651a9-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="651a9-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="651a9-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="651a9-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="651a9-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="651a9-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="651a9-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="651a9-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="651a9-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="651a9-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="651a9-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="651a9-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="651a9-114">Existují dva typy aktivit, které můžete použít v kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="651a9-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="651a9-115">[Aktivity přesunu dat](data-factory-data-movement-activities.md) pro přesun dat mezi [podporovanými úložišti dat zdroj a jímka](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="651a9-115">[Data Movement Activities](data-factory-data-movement-activities.md) to move data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="651a9-116">[Aktivity transformace dat](data-factory-data-transformation-activities.md) k transformaci dat pomocí výpočetních služeb, například Azure HDInsight, Azure Batch a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="651a9-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) to transform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="651a9-117">Pro přesun dat do nebo z úložiště dat, který nepodporuje objekt pro vytváření dat, vytvořit **vlastní aktivity** s vlastní logiky přesun dat a použití aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-117">To move data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use the activity in a pipeline.</span></span> <span data-ttu-id="651a9-118">Podobně transformace nebo zpracovat data způsobem, který není podporován službou Data Factory, vytvořte vlastní aktivity s vlastní logikou transformaci dat a použijte aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-118">Similarly, to transform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use the activity in a pipeline.</span></span> 

<span data-ttu-id="651a9-119">Můžete nakonfigurovat vlastní aktivity ke spuštění na **Azure Batch** fondu virtuálních počítačů nebo systémem Windows **Azure HDInsight** clusteru.</span><span class="sxs-lookup"><span data-stu-id="651a9-119">You can configure a custom activity to run on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="651a9-120">Pokud používáte Azure Batch, můžete použít pouze existujícího fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="651a9-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="651a9-121">Vzhledem k tomu, při použití HDInsight, můžete použít stávající cluster HDInsight nebo clusteru, který se automaticky vytvoří pro můžete na vyžádání za běhu.</span><span class="sxs-lookup"><span data-stu-id="651a9-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="651a9-122">Následující postup obsahuje podrobné pokyny pro vytváření vlastních aktivit rozhraní .NET a používání vlastní aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-122">The following walkthrough provides step-by-step instructions for creating a custom .NET activity and using the custom activity in a pipeline.</span></span> <span data-ttu-id="651a9-123">Průvodce používá **Azure Batch** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="651a9-123">The walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="651a9-124">Použití Azure HDInsight propojená služba místo, vytvoření propojené služby typu **HDInsight** (vlastní cluster HDInsight) nebo **HDInsightOnDemand** (Data Factory vytvoří cluster služby HDInsight na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="651a9-124">To use an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="651a9-125">Potom nakonfigurujte vlastní aktivity používat propojená služba HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-125">Then, configure custom activity to use the HDInsight linked service.</span></span> <span data-ttu-id="651a9-126">V tématu [použití Azure HDInsight propojené služby](#use-hdinsight-compute-service) část Podrobnosti o použití Azure HDInsight ke spuštění vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight to run the custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="651a9-127">Vlastní aktivity .NET spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="651a9-127">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="651a9-128">Alternativní řešení pro toto omezení je pomocí mapy snížit aktivity ke spuštění vlastního kódu Java na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="651a9-128">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="651a9-129">Další možností je použití fondu virtuálních počítačů služby Azure Batch ke spouštění vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-129">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="651a9-130">Není možné používat pro přístup k místním zdrojům dat Brána pro správu dat z vlastních aktivit.</span><span class="sxs-lookup"><span data-stu-id="651a9-130">It is not possible to use a Data Management Gateway from a custom activity to access on-premises data sources.</span></span> <span data-ttu-id="651a9-131">V současné době [Brána pro správu dat](data-factory-data-management-gateway.md) podporuje pouze aktivity kopírování a aktivity uložené procedury v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="651a9-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only the copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="651a9-132">Návod: vytvoření vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="651a9-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="651a9-133">Prerequisites</span></span>
* <span data-ttu-id="651a9-134">Visual Studio 2012/2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="651a9-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="651a9-135">Stáhněte sadu [Azure .NET SDK](https://azure.microsoft.com/downloads/) a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="651a9-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="651a9-136">Požadavky Azure Batch</span><span class="sxs-lookup"><span data-stu-id="651a9-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="651a9-137">V tomto návodu spusťte vlastní aktivitu .NET pomocí Azure Batch jako výpočetní prostředek.</span><span class="sxs-lookup"><span data-stu-id="651a9-137">In the walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="651a9-138">**Azure Batch** je platforma služby pro spuštění rozsáhlé paralelní a vysoce výkonné výpočty aplikace (HPC) efektivně v cloudu.</span><span class="sxs-lookup"><span data-stu-id="651a9-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in the cloud.</span></span> <span data-ttu-id="651a9-139">Azure Batch plány výpočetně náročných na spravované spouštět **kolekci virtuálních počítačů**, a dokáže automaticky škálovat výpočetní prostředky ke splnění potřeb vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="651a9-139">Azure Batch schedules compute-intensive work to run on a managed **collection of virtual machines**, and can automatically scale compute resources to meet the needs of your jobs.</span></span> <span data-ttu-id="651a9-140">V tématu [základy Azure Batch] [ batch-technical-overview] článku podrobnější přehled služby Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="651a9-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of the Azure Batch service.</span></span>

<span data-ttu-id="651a9-141">Pro tento kurz vytvoření účtu Azure Batch s fondem virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651a9-141">For the tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="651a9-142">Postup je následující:</span><span class="sxs-lookup"><span data-stu-id="651a9-142">Here are the steps:</span></span>

1. <span data-ttu-id="651a9-143">Vytvoření **účtu Azure Batch** pomocí [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="651a9-143">Create an **Azure Batch account** using the [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="651a9-144">V tématu [vytvoření a Správa účtu Azure Batch] [ batch-create-account] pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="651a9-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="651a9-145">Poznamenejte si název účtu Azure Batch, klíč účtu, identifikátor URI a název fondu.</span><span class="sxs-lookup"><span data-stu-id="651a9-145">Note down the Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="651a9-146">Je k vytvoření služby Azure Batch propojené potřebujete.</span><span class="sxs-lookup"><span data-stu-id="651a9-146">You need them to create an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="651a9-147">Na domovské stránce pro účet Azure Batch, uvidíte **URL** v následujícím formátu: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="651a9-147">On the home page for Azure Batch account, you see a **URL** in the following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="651a9-148">V tomto příkladu **stránku Můj účet** je název účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="651a9-148">In this example, **myaccount** is the name of the Azure Batch account.</span></span> <span data-ttu-id="651a9-149">Identifikátor URI, které můžete použít v definici propojené služby je adresa URL bez názvu účtu.</span><span class="sxs-lookup"><span data-stu-id="651a9-149">URI you use in the linked service definition is the URL without the name of the account.</span></span> <span data-ttu-id="651a9-150">Například: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="651a9-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="651a9-151">Klikněte na tlačítko **klíče** v levé nabídce a zkopírujte **primární přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="651a9-151">Click **Keys** on the left menu, and copy the **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="651a9-152">Chcete-li použít existující fond, klikněte na tlačítko **fondy** v nabídce a zapište **ID** fondu.</span><span class="sxs-lookup"><span data-stu-id="651a9-152">To use an existing pool, click **Pools** on the menu, and note down the **ID** of the pool.</span></span> <span data-ttu-id="651a9-153">Pokud nemáte existující fond, přejděte k dalšímu kroku.</span><span class="sxs-lookup"><span data-stu-id="651a9-153">If you don't have an existing pool, move to the next step.</span></span>     
2. <span data-ttu-id="651a9-154">Vytvoření **fondu Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="651a9-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="651a9-155">V [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce a klikněte na **účty Batch**.</span><span class="sxs-lookup"><span data-stu-id="651a9-155">In the [Azure portal](https://portal.azure.com), click **Browse** in the left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="651a9-156">Vyberte svůj účet Azure Batch **účtu Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="651a9-156">Select your Azure Batch account to open the **Batch Account** blade.</span></span>
   3. <span data-ttu-id="651a9-157">Klikněte na tlačítko **fondy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="651a9-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="651a9-158">V **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů na Přidat fond.</span><span class="sxs-lookup"><span data-stu-id="651a9-158">In the **Pools** blade, click Add button on the toolbar to add a pool.</span></span>
      1. <span data-ttu-id="651a9-159">Zadejte ID fondu (ID fondu).</span><span class="sxs-lookup"><span data-stu-id="651a9-159">Enter an ID for the pool (Pool ID).</span></span> <span data-ttu-id="651a9-160">Poznámka: **ID fondu**; je nutné při vytváření řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="651a9-160">Note the **ID of the pool**; you need it when creating the Data Factory solution.</span></span>
      2. <span data-ttu-id="651a9-161">Zadejte **Windows Server 2012 R2** pro nastavení řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="651a9-161">Specify **Windows Server 2012 R2** for the Operating System Family setting.</span></span>
      3. <span data-ttu-id="651a9-162">Vyberte **cenovou úroveň uzlu**.</span><span class="sxs-lookup"><span data-stu-id="651a9-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="651a9-163">Zadejte **2** jako hodnota **cíl vyhrazené** nastavení.</span><span class="sxs-lookup"><span data-stu-id="651a9-163">Enter **2** as value for the **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="651a9-164">Zadejte **2** jako hodnota **maximální počet úloh na uzel** nastavení.</span><span class="sxs-lookup"><span data-stu-id="651a9-164">Enter **2** as value for the **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="651a9-165">Kliknutím na tlačítko **OK** vytvořte fond.</span><span class="sxs-lookup"><span data-stu-id="651a9-165">Click **OK** to create the pool.</span></span>
   6. <span data-ttu-id="651a9-166">Poznamenejte si **ID** fondu.</span><span class="sxs-lookup"><span data-stu-id="651a9-166">Note down the **ID** of the pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="651a9-167">Postup vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="651a9-167">High-level steps</span></span>
<span data-ttu-id="651a9-168">Zde jsou dva hlavní kroky, které můžete provádět v rámci tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="651a9-168">Here are the two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="651a9-169">Vytvořte vlastní aktivitu, která obsahuje jednoduché datové transformaci/zpracování logiky.</span><span class="sxs-lookup"><span data-stu-id="651a9-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="651a9-170">Vytvořte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-170">Create an Azure data factory with a pipeline that uses the custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="651a9-171">Vytvoření vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-171">Create a custom activity</span></span>
<span data-ttu-id="651a9-172">Chcete-li vytvořit vlastní aktivity rozhraní .NET, vytvořte **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="651a9-172">To create a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="651a9-173">Toto rozhraní obsahuje pouze jednu metodu: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) a jeho podpis je:</span><span class="sxs-lookup"><span data-stu-id="651a9-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="651a9-174">Tato metoda přebírá čtyř parametrů:</span><span class="sxs-lookup"><span data-stu-id="651a9-174">The method takes four parameters:</span></span>

- <span data-ttu-id="651a9-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="651a9-175">**linkedServices**.</span></span> <span data-ttu-id="651a9-176">Tato vlastnost je vyčíslitelná seznam úložiště dat propojené služby odkazuje vstupní a výstupní datové sady pro aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for the activity.</span></span>   
- <span data-ttu-id="651a9-177">**datové sady**.</span><span class="sxs-lookup"><span data-stu-id="651a9-177">**datasets**.</span></span> <span data-ttu-id="651a9-178">Tato vlastnost je vyčíslitelná seznam vstupní a výstupní datové sady pro aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-178">This property is an enumerable list of input/output datasets for the activity.</span></span> <span data-ttu-id="651a9-179">Tento parametr můžete získat umístění a schémata definované vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="651a9-179">You can use this parameter to get the locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="651a9-180">**aktivita**.</span><span class="sxs-lookup"><span data-stu-id="651a9-180">**activity**.</span></span> <span data-ttu-id="651a9-181">Tato vlastnost představuje aktuální aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-181">This property represents the current activity.</span></span> <span data-ttu-id="651a9-182">Slouží pro přístup k rozšířené vlastnosti související s vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-182">It can be used to access extended properties associated with the custom activity.</span></span> <span data-ttu-id="651a9-183">V tématu [přístup rozšířené vlastnosti](#access-extended-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="651a9-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="651a9-184">**protokolovač**.</span><span class="sxs-lookup"><span data-stu-id="651a9-184">**logger**.</span></span> <span data-ttu-id="651a9-185">Tento objekt umožňuje psát komentáře ladění tento prostor v protokolu uživatele pro kanál.</span><span class="sxs-lookup"><span data-stu-id="651a9-185">This object lets you write debug comments that surface in the user log for the pipeline.</span></span>

<span data-ttu-id="651a9-186">Metoda vrátí slovník, který slouží k řetězu vlastní aktivity společně v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="651a9-186">The method returns a dictionary that can be used to chain custom activities together in the future.</span></span> <span data-ttu-id="651a9-187">Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody.</span><span class="sxs-lookup"><span data-stu-id="651a9-187">This feature is not implemented yet, so return an empty dictionary from the method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="651a9-188">Postup</span><span class="sxs-lookup"><span data-stu-id="651a9-188">Procedure</span></span>
1. <span data-ttu-id="651a9-189">Vytvoření **knihovna tříd rozhraní .NET** projektu.</span><span class="sxs-lookup"><span data-stu-id="651a9-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="651a9-190">Spusťte <b>Visual Studio 2017</b> nebo <b>sady Visual Studio 2015</b> nebo <b>Visual Studio 2013</b> nebo <b>sady Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="651a9-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="651a9-191">Klikněte na <b>Soubor</b>, přejděte na <b>Nový</b> a klikněte na <b>Projekt</b>.</span><span class="sxs-lookup"><span data-stu-id="651a9-191">Click <b>File</b>, point to <b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="651a9-192">Rozbalte <b>Šablony</b> a vyberte <b>Visual C#</b>.</span><span class="sxs-lookup"><span data-stu-id="651a9-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="651a9-193">V tomto návodu použijete C#, ale můžete v kterémkoli jazyce platformy .NET vyvíjet vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-193">In this walkthrough, you use C#, but you can use any .NET language to develop the custom activity.</span></span></li>
     <li><span data-ttu-id="651a9-194">Vyberte <b>knihovny tříd</b> ze seznamu typů projektu na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="651a9-194">Select <b>Class Library</b> from the list of project types on the right.</span></span> <span data-ttu-id="651a9-195">V VS 2017, zvolte <b>knihovny tříd (rozhraní .NET Framework)</b> </span><span class="sxs-lookup"><span data-stu-id="651a9-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="651a9-196">Zadejte <b>MyDotNetActivity</b> pro <b>název</b>.</span><span class="sxs-lookup"><span data-stu-id="651a9-196">Enter <b>MyDotNetActivity</b> for the <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="651a9-197">Vyberte <b>C:\ADFGetStarted</b> pro <b>umístění</b>.</span><span class="sxs-lookup"><span data-stu-id="651a9-197">Select <b>C:\ADFGetStarted</b> for the <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="651a9-198">Kliknutím na tlačítko <b>OK</b> vytvořte projekt.</span><span class="sxs-lookup"><span data-stu-id="651a9-198">Click <b>OK</b> to create the project.</span></span></li>
   </ol><span data-ttu-id="651a9-199">
2.Klikněte na tlačítko **nástroje**, přejděte na příkaz **Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="651a9-199">
2. Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="651a9-200">3.</span><span class="sxs-lookup"><span data-stu-id="651a9-200">3.</span></span> <span data-ttu-id="651a9-201">V konzole Správce balíčků spustíte následující příkaz pro import **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="651a9-201">In the Package Manager Console, execute the following command to import **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="651a9-202">Import **Azure Storage** balíčku NuGet do projektu.</span><span class="sxs-lookup"><span data-stu-id="651a9-202">Import the **Azure Storage** NuGet package in to the project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="651a9-203">Spouštěč služby Data Factory vyžaduje 4.3 verzi WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-203">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="651a9-204">Pokud přidáte odkaz na novější verzi sestavení Azure Storage ve vašem projektu vlastní aktivitu, zobrazí chybu, když aktivita provede.</span><span class="sxs-lookup"><span data-stu-id="651a9-204">If you add a reference to a later version of Azure Storage assembly in your custom activity project, you see an error when the activity executes.</span></span> <span data-ttu-id="651a9-205">Chcete-li vyřešit chyby, najdete v části [izolace domény aplikace](#appdomain-isolation) části.</span><span class="sxs-lookup"><span data-stu-id="651a9-205">To resolve the error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="651a9-206">Přidejte následující **pomocí** příkazy ke zdrojovému souboru v projektu.</span><span class="sxs-lookup"><span data-stu-id="651a9-206">Add the following **using** statements to the source file in the project.</span></span>

    ```csharp

    // Comment these lines if using VS 2017
    using System.IO;
    using System.Globalization;
    using System.Diagnostics;
    using System.Linq;
    // --------------------

    // Comment these lines if using <= VS 2015
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    // ---------------------

    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Runtime;

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```
6. <span data-ttu-id="651a9-207">Změňte název **obor názvů** k **MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="651a9-207">Change the name of the **namespace** to **MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="651a9-208">Změnit název třídy pro **MyDotNetActivity** a odvodí z **IDotNetActivity** rozhraní, jak je znázorněno v následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="651a9-208">Change the name of the class to **MyDotNetActivity** and derive it from the **IDotNetActivity** interface as shown in the following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="651a9-209">Implementace (Přidat) **Execute** metodu **IDotNetActivity** rozhraní k **MyDotNetActivity** třídy a zkopírujte následující vzorový kód do metody.</span><span class="sxs-lookup"><span data-stu-id="651a9-209">Implement (Add) the **Execute** method of the **IDotNetActivity** interface to the **MyDotNetActivity** class and copy the following sample code to the method.</span></span>

    <span data-ttu-id="651a9-210">Následující příklad vrátí počet výskytů hledaný termín ("Microsoft") v jednotlivých objektů blob přidružený datový řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-210">The following sample counts the number of occurrences of the search term (“Microsoft”) in each blob associated with a data slice.</span></span>

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
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // to log information, use the logger object
        // log all extended properties            
        IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
        logger.Write("Logging extended properties if any...");
        foreach (KeyValuePair<string, string> entry in extendedProperties)
        {
            logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
        }
    
        // linked service for input and output data stores
        // in this example, same storage is used for both input/output
        AzureStorageLinkedService inputLinkedService;

        // get the input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables to hold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from the dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and the other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get the first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using the same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get the connection string in the linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get the folder path from the input dataset definition
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
               // with the data slice. definition of the method is shown in the next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get the output dataset using the name of the dataset matched to a name in the Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for the output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get the folder path from the output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log the output folder path   
        logger.Write("Writing blob to the folder: {0}", folderPath);
    
        // create a storage object for the output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write the name of the file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log the output file name
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
9. <span data-ttu-id="651a9-211">Přidejte následující metody pomocné rutiny:</span><span class="sxs-lookup"><span data-stu-id="651a9-211">Add the following helper methods:</span></span> 

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

        // get type properties of the dataset   
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the folder path found in the type properties
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
    
        // get type properties of the dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return the blob/file name in the type properties
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

    <span data-ttu-id="651a9-212">Metoda GetFolderPath vrací cestu ke složce, která odkazuje datovou sadu na a vrátí název objektu blob nebo soubor, který datová sada odkazuje na metodu GetFileName.</span><span class="sxs-lookup"><span data-stu-id="651a9-212">The GetFolderPath method returns the path to the folder that the dataset points to and the GetFileName method returns the name of the blob/file that the dataset points to.</span></span> <span data-ttu-id="651a9-213">Pokud jste havefolderPath definuje pomocí proměnných, například {Year}, {Month}, {Day} atd., metoda vrátí řetězec, protože je bez jejich nahrazení s hodnotami runtime.</span><span class="sxs-lookup"><span data-stu-id="651a9-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., the method returns the string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="651a9-214">V tématu [přístup rozšířené vlastnosti](#access-extended-properties) část Podrobnosti o přístup k SliceStart, SliceEnd atd.</span><span class="sxs-lookup"><span data-stu-id="651a9-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="651a9-215">Metoda Calculate vypočítá počet instancí – klíčové slovo Microsoft ve vstupní soubory (objekty BLOB ve složce).</span><span class="sxs-lookup"><span data-stu-id="651a9-215">The Calculate method calculates the number of instances of keyword Microsoft in the input files (blobs in the folder).</span></span> <span data-ttu-id="651a9-216">Hledaný termín ("Microsoft") je pevně zakódovaná v kódu.</span><span class="sxs-lookup"><span data-stu-id="651a9-216">The search term (“Microsoft”) is hard-coded in the code.</span></span>
10. <span data-ttu-id="651a9-217">Kompilace projektu.</span><span class="sxs-lookup"><span data-stu-id="651a9-217">Compile the project.</span></span> <span data-ttu-id="651a9-218">Klikněte na tlačítko **sestavení** z nabídky a klikněte na tlačítko **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="651a9-218">Click **Build** from the menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="651a9-219">Nastavit 4.5.2 verzi rozhraní .NET Framework jako cílové rozhraní pro svůj projekt: klikněte pravým tlačítkem na projekt a klikněte na tlačítko **vlastnosti** nastavit cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="651a9-219">Set 4.5.2 version of .NET Framework as the target framework for your project: right-click the project, and click **Properties** to set the target framework.</span></span> <span data-ttu-id="651a9-220">Objekt pro vytváření dat nepodporuje vlastní aktivity, které jsou novější než 4.5.2 zkompilovat proti verze rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="651a9-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="651a9-221">Spusťte **Průzkumníka Windows**a přejděte do **bin\debug** nebo **bin\release** složku v závislosti na typu sestavení.</span><span class="sxs-lookup"><span data-stu-id="651a9-221">Launch **Windows Explorer**, and navigate to **bin\debug** or **bin\release** folder depending on the type of build.</span></span>
12. <span data-ttu-id="651a9-222">Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory v <project folder>\bin\Debug složka.</span><span class="sxs-lookup"><span data-stu-id="651a9-222">Create a zip file **MyDotNetActivity.zip** that contains all the binaries in the <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="651a9-223">Zahrnout **MyDotNetActivity.pdb** tak, aby získat další podrobnosti, jako je například číslo řádku ve zdrojovém kódu, která způsobila problém, pokud došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="651a9-223">Include the **MyDotNetActivity.pdb** file so that you get additional details such as line number in the source code that caused the issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="651a9-224">Všechny soubory v souboru .zip pro vlastní aktivitu musí být na **nejvyšší úrovni**, bez podsložek.</span><span class="sxs-lookup"><span data-stu-id="651a9-224">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>

    ![Binární výstupní soubory](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="651a9-226">Vytvořte kontejner objektů blob s názvem **customactivitycontainer** Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="651a9-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="651a9-227">Nahrát MyDotNetActivity.zip jako objekt blob customactivitycontainer v **pro obecné účely** úložiště objektů blob v Azure (úložiště objektů Blob není za provozu nebo nástrojů), který odkazuje AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="651a9-227">Upload MyDotNetActivity.zip as a blob to the customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="651a9-228">Pokud přidáte tento projekt aktivity .NET na řešení v sadě Visual Studio, který obsahuje projekt Data Factory a přidejte odkaz na rozhraní .NET projektu aktivity z projektu pro vytváření dat aplikace, není potřeba provádět poslední dva kroky ručně vytvoření souboru zip soubor a pak ho nahrát do úložiště objektů blob v Azure pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="651a9-228">If you add this .NET activity project to a solution in Visual Studio that contains a Data Factory project, and add a reference to .NET activity project from the Data Factory application project, you do not need to perform the last two steps of manually creating the zip file and uploading it to the general-purpose Azure blob storage.</span></span> <span data-ttu-id="651a9-229">Když publikujete entity služby Data Factory pomocí sady Visual Studio, tyto kroky automaticky provádí proces publikování.</span><span class="sxs-lookup"><span data-stu-id="651a9-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by the publishing process.</span></span> <span data-ttu-id="651a9-230">Další informace najdete v tématu [projekt Data Factory v sadě Visual Studio](#data-factory-project-in-visual-studio) části.</span><span class="sxs-lookup"><span data-stu-id="651a9-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="651a9-231">Vytvoření kanálu s vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="651a9-232">Vytvoření vlastní aktivity a nahrané do kontejneru objektů blob v souboru zip binární soubory služby **pro obecné účely** účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-232">You have created a custom activity and uploaded the zip file with binaries to a blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="651a9-233">V této části vytvoříte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-233">In this section, you create an Azure data factory with a pipeline that uses the custom activity.</span></span>

<span data-ttu-id="651a9-234">Vstupní datové sady pro vlastní aktivita představuje objekty BLOB (soubory) ve složce customactivityinput adftutorial kontejneru ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-234">The input dataset for the custom activity represents blobs (files) in the customactivityinput folder of adftutorial container in the blob storage.</span></span> <span data-ttu-id="651a9-235">Výstupní datovou sadu aktivity představuje výstupní objekty BLOB ve složce customactivityoutput adftutorial kontejneru ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-235">The output dataset for the activity represents output blobs in the customactivityoutput folder of adftutorial container in the blob storage.</span></span>

<span data-ttu-id="651a9-236">Vytvoření **soubor.txt** soubor s následujícím obsahem a nahrajte ho do **customactivityinput** složky **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="651a9-236">Create **file.txt** file with the following content and upload it to **customactivityinput** folder of the **adftutorial** container.</span></span> <span data-ttu-id="651a9-237">Vytvoření kontejneru adftutorial, pokud ho ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="651a9-237">Create the adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="651a9-238">Vstupní složky odpovídá řez v Azure Data Factory i v případě, že složka obsahuje dvě nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="651a9-238">The input folder corresponds to a slice in Azure Data Factory even if the folder has two or more files.</span></span> <span data-ttu-id="651a9-239">Při zpracování každý řez v kanálu vlastní aktivita iteruje všechny objekty BLOB ve složce vstupní pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-239">When each slice is processed by the pipeline, the custom activity iterates through all the blobs in the input folder for that slice.</span></span>

<span data-ttu-id="651a9-240">Uvidíte, že jednu výstupní soubor s ve složce adftutorial\customactivityoutput s jedním nebo více řádky (stejný jako počet objektů BLOB ve složce vstupní):</span><span class="sxs-lookup"><span data-stu-id="651a9-240">You see one output file with in the adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in the input folder):</span></span>

```
2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="651a9-241">Tady jsou kroky, které můžete provádět v této části:</span><span class="sxs-lookup"><span data-stu-id="651a9-241">Here are the steps you perform in this section:</span></span>

1. <span data-ttu-id="651a9-242">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="651a9-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="651a9-243">Vytvořit **propojené služby** fondu Azure Batch virtuálních počítačů, na který vlastní aktivita se spustí a úložišti Azure, který obsahuje objekty BLOB vstupu a výstupu.</span><span class="sxs-lookup"><span data-stu-id="651a9-243">Create **Linked services** for the Azure Batch pool of VMs on which the custom activity runs and the Azure Storage that holds the input/output blobs.</span></span>
3. <span data-ttu-id="651a9-244">Vytvoření vstupní a výstupní **datové sady** které představují vstupní a výstupní vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-244">Create input and output **datasets** that represent input and output of the custom activity.</span></span>
4. <span data-ttu-id="651a9-245">Vytvoření **kanálu** používající vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-245">Create a **pipeline** that uses the custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="651a9-246">Vytvořte **soubor.txt** a nahrajte ho do kontejneru objektů blob, pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="651a9-246">Create the **file.txt** and upload it to a blob container if you haven't already done so.</span></span> <span data-ttu-id="651a9-247">Pokyny naleznete v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="651a9-247">See instructions in the preceding section.</span></span>   

### <a name="step-1-create-the-data-factory"></a><span data-ttu-id="651a9-248">Krok 1: Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="651a9-248">Step 1: Create the data factory</span></span>
1. <span data-ttu-id="651a9-249">Po přihlášení k portálu Azure, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="651a9-249">After logging in to the Azure portal, do the following steps:</span></span>
   1. <span data-ttu-id="651a9-250">V nabídce vlevo klikněte na **NOVÝ**.</span><span class="sxs-lookup"><span data-stu-id="651a9-250">Click **NEW** on the left menu.</span></span>
   2. <span data-ttu-id="651a9-251">Klikněte na tlačítko **Data + analýzy** v **nový** okno.</span><span class="sxs-lookup"><span data-stu-id="651a9-251">Click **Data + Analytics** in the **New** blade.</span></span>
   3. <span data-ttu-id="651a9-252">V okně **Analýza dat** klikněte na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="651a9-252">Click **Data Factory** on the **Data analytics** blade.</span></span>
   
    ![Nové nabídky Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="651a9-254">V **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro název.</span><span class="sxs-lookup"><span data-stu-id="651a9-254">In the **New data factory** blade, enter **CustomActivityFactory** for the Name.</span></span> <span data-ttu-id="651a9-255">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="651a9-255">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="651a9-256">Pokud se zobrazí chyba: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte název objektu pro vytváření dat (například **yournameCustomActivityFactory**) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="651a9-256">If you receive the error: **Data factory name “CustomActivityFactory” is not available**, change the name of the data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Nové okno Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="651a9-258">Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="651a9-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="651a9-259">Ověřte, že používáte správný **předplatné** a **oblast** místo, kam chcete objekt pro vytváření dat vytvořit.</span><span class="sxs-lookup"><span data-stu-id="651a9-259">Verify that you are using the correct **subscription** and **region** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="651a9-260">V okně **Nový objekt pro vytváření dat** klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="651a9-260">Click **Create** on the **New data factory** blade.</span></span>
6. <span data-ttu-id="651a9-261">Zobrazí objektu pro vytváření dat vytváří ve **řídicí panel** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-261">You see the data factory being created in the **Dashboard** of the Azure portal.</span></span>
7. <span data-ttu-id="651a9-262">Po úspěšném vytvoření objektu pro vytváření dat, zobrazí se okno objekt pro vytváření dat, který zobrazuje obsah objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="651a9-262">After the data factory has been created successfully, you see the Data Factory blade, which shows you the contents of the data factory.</span></span>
    
    ![Okno Objekt pro vytváření dat](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="651a9-264">Krok 2: Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="651a9-264">Step 2: Create linked services</span></span>
<span data-ttu-id="651a9-265">Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-265">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="651a9-266">V tomto kroku propojíte svůj účet úložiště Azure a účet Azure Batch pro vytváření dat..</span><span class="sxs-lookup"><span data-stu-id="651a9-266">In this step, you link your Azure Storage account and Azure Batch account to your data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="651a9-267">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="651a9-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="651a9-268">Klikněte na tlačítko **vytvořit a nasadit** na dlaždici **objekt pro vytváření dat** okno pro **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="651a9-268">Click the **Author and deploy** tile on the **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="651a9-269">Zobrazí editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="651a9-269">You see the Data Factory Editor.</span></span>
2. <span data-ttu-id="651a9-270">Klikněte na tlačítko **nové datové úložiště** na příkaz panelu a vyberte **úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="651a9-270">Click **New data store** on the command bar and choose **Azure storage**.</span></span> <span data-ttu-id="651a9-271">V editoru by se měl zobrazit skript JSON pro vytvoření propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-271">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>
    
    ![Nové datové úložiště - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="651a9-273">Nahraďte `<accountname>` s názvem účtu úložiště Azure a `<accountkey>` s přístupovým klíčem k účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of the Azure storage account.</span></span> <span data-ttu-id="651a9-274">Informace o tom, jak získat přístupový klíč k úložišti, najdete v článku o [zobrazení, kopírování a opětovném vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="651a9-274">To learn how to get your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Úložiště Azure líbilo služby](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="651a9-276">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-276">Click **Deploy** on the command bar to deploy the linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="651a9-277">Vytvoření služby Azure Batch propojené</span><span class="sxs-lookup"><span data-stu-id="651a9-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="651a9-278">V editoru služby Data Factory, klikněte na tlačítko **... Další** na panelu příkazů klikněte na tlačítko **nový výpočet**a potom vyberte **Azure Batch** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="651a9-278">In the Data Factory Editor, click **... More** on the command bar, click **New compute**, and then select **Azure Batch** from the menu.</span></span>

    ![Nový výpočet - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="651a9-280">Skript JSON proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="651a9-280">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="651a9-281">Zadejte název účtu Azure Batch **accountName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="651a9-281">Specify Azure Batch account name for the **accountName** property.</span></span> <span data-ttu-id="651a9-282">**URL** z **okně účtu Azure Batch** je v následujícím formátu: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="651a9-282">The **URL** from the **Azure Batch account blade** is in the following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="651a9-283">Pro **batchUri** vlastností v kódu JSON, je třeba odebrat `accountname.` z adresy URL a použití `accountname` pro `accountName` vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="651a9-283">For the **batchUri** property in the JSON, you need to remove `accountname.` from the URL and use the `accountname` for the `accountName` JSON property.</span></span>
   2. <span data-ttu-id="651a9-284">Zadejte klíč účtu Azure Batch pro **accessKey** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="651a9-284">Specify the Azure Batch account key for the **accessKey** property.</span></span>
   3. <span data-ttu-id="651a9-285">Zadejte název fondu, který jste vytvořili v rámci požadavků pro **poolName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="651a9-285">Specify the name of the pool you created as part of prerequisites for the **poolName** property.</span></span> <span data-ttu-id="651a9-286">Můžete také zadat ID fondu namísto názvu fondu.</span><span class="sxs-lookup"><span data-stu-id="651a9-286">You can also specify the ID of the pool instead of the name of the pool.</span></span>
   4. <span data-ttu-id="651a9-287">Zadejte Azure Batch URI **batchUri** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="651a9-287">Specify Azure Batch URI for the **batchUri** property.</span></span> <span data-ttu-id="651a9-288">Příklad: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="651a9-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="651a9-289">Zadejte **AzureStorageLinkedService** pro **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="651a9-289">Specify the **AzureStorageLinkedService** for the **linkedServiceName** property.</span></span>

        ```json
        {
         "name": "AzureBatchLinkedService",
         "properties": {
           "type": "AzureBatch",
           "typeProperties": {
             "accountName": "myazurebatchaccount",
             "batchUri": "https://westus.batch.azure.com",
             "accessKey": "<yourbatchaccountkey>",
             "poolName": "myazurebatchpool",
             "linkedServiceName": "AzureStorageLinkedService"
           }
         }
        }
        ```

       <span data-ttu-id="651a9-290">Pro **poolName** vlastnost, můžete také zadat ID fondu namísto názvu fondu.</span><span class="sxs-lookup"><span data-stu-id="651a9-290">For the **poolName** property, you can also specify the ID of the pool instead of the name of the pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="651a9-291">Služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-291">The Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="651a9-292">Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="651a9-293">Krok 3: Vytvoření datové sady</span><span class="sxs-lookup"><span data-stu-id="651a9-293">Step 3: Create datasets</span></span>
<span data-ttu-id="651a9-294">V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="651a9-294">In this step, you create datasets to represent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="651a9-295">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="651a9-295">Create input dataset</span></span>
1. <span data-ttu-id="651a9-296">V **editoru** služby Data Factory klikněte na **... Další** na panelu příkazů klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="651a9-296">In the **Editor** for the Data Factory, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage** from the drop-down menu.</span></span>
2. <span data-ttu-id="651a9-297">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="651a9-297">Replace the JSON in the right pane with the following JSON snippet:</span></span>

    ```json
    {
     "name": "InputDataset",
     "properties": {
         "type": "AzureBlob",
         "linkedServiceName": "AzureStorageLinkedService",
         "typeProperties": {
             "folderPath": "adftutorial/customactivityinput/",
             "format": {
                 "type": "TextFormat"
             }
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

   <span data-ttu-id="651a9-298">Vytvoření kanálu dále v tomto návodu se časem spuštění: 2016-11-16T00:00:00Z a koncový čas: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="651a9-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="651a9-299">Je naplánována nevytvořila data každou hodinu, takže se pět řezy vstupní a výstupní (mezi **00**: -> 00:00 **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="651a9-299">It is scheduled to produce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="651a9-300">**Frekvence** a **interval** vstupní datové sady je nastavený na **hodinu** a **1**, což znamená, že vstupní řez je k dispozici každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="651a9-300">The **frequency** and **interval** for the input dataset is set to **Hour** and **1**, which means that the input slice is available hourly.</span></span> <span data-ttu-id="651a9-301">V této ukázce je stejný soubor (soubor.txt) v intputfolder.</span><span class="sxs-lookup"><span data-stu-id="651a9-301">In this sample, it is the same file (file.txt) in the intputfolder.</span></span>

   <span data-ttu-id="651a9-302">Tady jsou časy zahájení pro každý řez, která je reprezentována SliceStart systémové proměnné ve výše uvedeném fragmentu JSON.</span><span class="sxs-lookup"><span data-stu-id="651a9-302">Here are the start times for each slice, which is represented by SliceStart system variable in the above JSON snippet.</span></span>
3. <span data-ttu-id="651a9-303">Klikněte na tlačítko **nasadit** na panelu nástrojů vytvořit a nasadit **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="651a9-303">Click **Deploy** on the toolbar to create and deploy the **InputDataset**.</span></span> <span data-ttu-id="651a9-304">Potvrďte, že se v záhlaví okna editoru zobrazila zpráva **ÚSPĚŠNÉ VYTVOŘENÍ TABULKY**.</span><span class="sxs-lookup"><span data-stu-id="651a9-304">Confirm that you see the **TABLE CREATED SUCCESSFULLY** message on the title bar of the Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="651a9-305">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="651a9-305">Create an output dataset</span></span>
1. <span data-ttu-id="651a9-306">V **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="651a9-306">In the **Data Factory editor**, click **... More** on the command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="651a9-307">Skript JSON v pravém podokně nahraďte následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="651a9-307">Replace the JSON script in the right pane with the following JSON script:</span></span>

    ```JSON
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "{slice}.txt",
                "folderPath": "adftutorial/customactivityoutput/",
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

     <span data-ttu-id="651a9-308">Výstupní umístění je **adftutorial/customactivityoutput/** a název výstupního souboru je rrrr MM-dd-HH.txt, kde je rrrr MM-dd HH rok, měsíc, datum a hodinu řez se vytváří.</span><span class="sxs-lookup"><span data-stu-id="651a9-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is the year, month, date, and hour of the slice being produced.</span></span> <span data-ttu-id="651a9-309">V tématu [referenční informace pro vývojáře] [ adf-developer-reference] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="651a9-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="651a9-310">Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="651a9-311">Zde je, jak je výstupní soubor s názvem pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="651a9-312">Výstupní soubory jsou generovány v jednu výstupní složky: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="651a9-312">All the output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="651a9-313">Řez</span><span class="sxs-lookup"><span data-stu-id="651a9-313">Slice</span></span> | <span data-ttu-id="651a9-314">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="651a9-314">Start time</span></span> | <span data-ttu-id="651a9-315">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="651a9-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="651a9-316">1</span><span class="sxs-lookup"><span data-stu-id="651a9-316">1</span></span> |<span data-ttu-id="651a9-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="651a9-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="651a9-318">2016 11 16 00.txt</span><span class="sxs-lookup"><span data-stu-id="651a9-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="651a9-319">2</span><span class="sxs-lookup"><span data-stu-id="651a9-319">2</span></span> |<span data-ttu-id="651a9-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="651a9-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="651a9-321">2016 11 16 01.txt</span><span class="sxs-lookup"><span data-stu-id="651a9-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="651a9-322">3</span><span class="sxs-lookup"><span data-stu-id="651a9-322">3</span></span> |<span data-ttu-id="651a9-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="651a9-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="651a9-324">2016 11 16 02.txt</span><span class="sxs-lookup"><span data-stu-id="651a9-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="651a9-325">4</span><span class="sxs-lookup"><span data-stu-id="651a9-325">4</span></span> |<span data-ttu-id="651a9-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="651a9-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="651a9-327">2016 11 16 03.txt</span><span class="sxs-lookup"><span data-stu-id="651a9-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="651a9-328">5</span><span class="sxs-lookup"><span data-stu-id="651a9-328">5</span></span> |<span data-ttu-id="651a9-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="651a9-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="651a9-330">2016 11 16 04.txt</span><span class="sxs-lookup"><span data-stu-id="651a9-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="651a9-331">Mějte na paměti, že všechny soubory ve vstupní složky jsou součástí řez s časy zahájení uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="651a9-331">Remember that all the files in an input folder are part of a slice with the start times mentioned above.</span></span> <span data-ttu-id="651a9-332">Při zpracování této řezu se vlastní aktivita prohledává každý soubor a vytvoří řádek ve výstupním souboru s počtem výskytů hledaný termín ("Microsoft").</span><span class="sxs-lookup"><span data-stu-id="651a9-332">When this slice is processed, the custom activity scans through each file and produces a line in the output file with the number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="651a9-333">Pokud jsou inputfolder tři soubory, se ve výstupním souboru pro každý hodinové řez tři řádky: 2016-11-16-00.txt 2016-11-16:01:00:00.txt atd.</span><span class="sxs-lookup"><span data-stu-id="651a9-333">If there are three files in the inputfolder, there are three lines in the output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="651a9-334">K nasazení **OutputDataset**, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-334">To deploy the **OutputDataset**, click **Deploy** on the command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-the-custom-activity"></a><span data-ttu-id="651a9-335">Vytvoření a spuštění kanálu, který používá vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-335">Create and run a pipeline that uses the custom activity</span></span>
1. <span data-ttu-id="651a9-336">V editoru služby Data Factory, klikněte na tlačítko **... Další**a potom vyberte **nový kanál** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-336">In the Data Factory Editor, click **... More**, and then select **New pipeline** on the command bar.</span></span> 
2. <span data-ttu-id="651a9-337">Nahraďte kód JSON v pravém podokně se následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="651a9-337">Replace the JSON in the right pane with the following JSON script:</span></span>

    ```JSON
    {
      "name": "ADFTutorialPipelineCustom",
      "properties": {
        "description": "Use custom activity",
        "activities": [
          {
            "Name": "MyDotNetActivity",
            "Type": "DotNetActivity",
            "Inputs": [
              {
                "Name": "InputDataset"
              }
            ],
            "Outputs": [
              {
                "Name": "OutputDataset"
              }
            ],
            "LinkedServiceName": "AzureBatchLinkedService",
            "typeProperties": {
              "AssemblyName": "MyDotNetActivity.dll",
              "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
              "PackageLinkedService": "AzureStorageLinkedService",
              "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
              "extendedProperties": {
                "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
              }
            },
            "Policy": {
              "Concurrency": 2,
              "ExecutionPriorityOrder": "OldestFirst",
              "Retry": 3,
              "Timeout": "00:30:00",
              "Delay": "00:00:00"
            }
          }
        ],
        "start": "2016-11-16T00:00:00Z",
        "end": "2016-11-16T05:00:00Z",
        "isPaused": false
      }
    }
    ```

    <span data-ttu-id="651a9-338">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="651a9-338">Note the following points:</span></span>

   * <span data-ttu-id="651a9-339">**Concurrency** je nastaven na **2** tak, aby dva řezy jsou zpracovávány paralelně 2 virtuální počítače ve fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="651a9-339">**Concurrency** is set to **2** so that two slices are processed in parallel by 2 VMs in the Azure Batch pool.</span></span>
   * <span data-ttu-id="651a9-340">V části aktivit je jedna aktivita a je typu: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="651a9-340">There is one activity in the activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="651a9-341">**AssemblyName** nastavena na název knihovny DLL: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="651a9-341">**AssemblyName** is set to the name of the DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="651a9-342">**EntryPoint** je nastaven na **MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="651a9-342">**EntryPoint** is set to **MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="651a9-343">**PackageLinkedService** je nastaven na **AzureStorageLinkedService** který odkazuje na úložiště objektů blob, který obsahuje soubor zip vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-343">**PackageLinkedService** is set to **AzureStorageLinkedService** that points to the blob storage that contains the custom activity zip file.</span></span> <span data-ttu-id="651a9-344">Pokud používáte jiné účty Azure Storage pro vstupní a výstupní soubory a soubor zip vlastní aktivitu, vytvoříte jiné propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-344">If you are using different Azure Storage accounts for input/output files and the custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="651a9-345">Tento článek předpokládá, že používáte stejný účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-345">This article assumes that you are using the same Azure Storage account.</span></span>
   * <span data-ttu-id="651a9-346">**PackageFile** je nastaven na **customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="651a9-346">**PackageFile** is set to **customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="651a9-347">Je ve formátu: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="651a9-347">It is in the format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="651a9-348">Vlastní aktivita přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="651a9-348">The custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="651a9-349">Vlastnost linkedServiceName vlastní aktivity odkazuje na **AzureBatchLinkedService**, která říká službě Azure Data Factory, který vlastní aktivita je potřeba spustit na virtuálních počítačích Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="651a9-349">The linkedServiceName property of the custom activity points to the **AzureBatchLinkedService**, which tells Azure Data Factory that the custom activity needs to run on Azure Batch VMs.</span></span>
   * <span data-ttu-id="651a9-350">**isPaused** je nastavena na **false** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="651a9-350">**isPaused** property is set to **false** by default.</span></span> <span data-ttu-id="651a9-351">Kanál se spustí okamžitě v tomto příkladu protože řezy spustit v minulosti.</span><span class="sxs-lookup"><span data-stu-id="651a9-351">The pipeline runs immediately in this example because the slices start in the past.</span></span> <span data-ttu-id="651a9-352">Tuto vlastnost lze nastavit na hodnotu true pozastavení kanálu a nastavte ji zpět na hodnotu false restartovat.</span><span class="sxs-lookup"><span data-stu-id="651a9-352">You can set this property to true to pause the pipeline and set it back to false to restart.</span></span>
   * <span data-ttu-id="651a9-353">**Spustit** čas a **end** časy jsou **pět** hodiny od sebe a řezy vytváří každou hodinu, takže pět řezy vytváří v kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-353">The **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by the pipeline.</span></span>
3. <span data-ttu-id="651a9-354">Chcete-li nasadit kanálu, klikněte na tlačítko **nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-354">To deploy the pipeline, click **Deploy** on the command bar.</span></span>

### <a name="monitor-the-pipeline"></a><span data-ttu-id="651a9-355">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="651a9-355">Monitor the pipeline</span></span>
1. <span data-ttu-id="651a9-356">V okně Data Factory na webu Azure portal, klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="651a9-356">In the Data Factory blade in the Azure portal, click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="651a9-358">V zobrazení diagramu klikněte teď OutputDataset.</span><span class="sxs-lookup"><span data-stu-id="651a9-358">In the Diagram View, now click the OutputDataset.</span></span>

    ![Zobrazení diagramu](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="651a9-360">Měli byste vidět, že pět Výstup řezy jsou ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="651a9-360">You should see that the five output slices are in the Ready state.</span></span> <span data-ttu-id="651a9-361">Pokud nejsou ve stavu Připraveno, nebyly byla vypracována ještě.</span><span class="sxs-lookup"><span data-stu-id="651a9-361">If they are not in the Ready state, they haven't been produced yet.</span></span> 

   ![Výstup řezy](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="651a9-363">Ověřte, že jsou generovány výstupní soubory v úložišti objektů blob v **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="651a9-363">Verify that the output files are generated in the blob storage in the **adftutorial** container.</span></span>

   ![výstup z vlastní aktivity][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="651a9-365">Pokud můžete otevřít výstupní soubor, byste měli vidět výstup podobný následující výstup:</span><span class="sxs-lookup"><span data-stu-id="651a9-365">If you open the output file, you should see the output similar to the following output:</span></span>

    ```
    2 occurrences(s) of the search term "Microsoft" were found in the file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="651a9-366">Použití [portál Azure] [ azure-preview-portal] nebo rutin prostředí Azure PowerShell sledovat vaše služby data factory, kanálů a datové sady.</span><span class="sxs-lookup"><span data-stu-id="651a9-366">Use the [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets to monitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="651a9-367">Zobrazí se zprávy z **ActivityLogger** v kódu vlastní aktivity v protokoly (konkrétně uživatel 0.log), které si můžete stáhnout z portálu nebo pomocí rutin.</span><span class="sxs-lookup"><span data-stu-id="651a9-367">You can see messages from the **ActivityLogger** in the code for the custom activity in the logs (specifically user-0.log) that you can download from the portal or using cmdlets.</span></span>

   ![stažení protokolů z vlastní aktivity][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="651a9-369">V tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) podrobné pokyny pro monitorování datových sad a kanálů.</span><span class="sxs-lookup"><span data-stu-id="651a9-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="651a9-370">Projekt data Factory v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="651a9-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="651a9-371">Můžete vytvořit a publikovat entity služby Data Factory pomocí sady Visual Studio místo použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="651a9-372">Podrobné informace o vytváření a publikování entit služby Data Factory pomocí sady Visual Studio, najdete v části [sestavit svůj první kanál pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) a [kopírování dat z objektu Blob Azure do Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) články.</span><span class="sxs-lookup"><span data-stu-id="651a9-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob to Azure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="651a9-373">Pokud vytváříte projekt Data Factory v sadě Visual Studio, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="651a9-373">Do the following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="651a9-374">Přidejte do řešení sady Visual Studio, který obsahuje vlastní aktivity projektu projekt Data Factory.</span><span class="sxs-lookup"><span data-stu-id="651a9-374">Add the Data Factory project to the Visual Studio solution that contains the custom activity project.</span></span> 
2. <span data-ttu-id="651a9-375">Přidáte odkaz na projekt aktivity .NET z projekt Data Factory.</span><span class="sxs-lookup"><span data-stu-id="651a9-375">Add a reference to the .NET activity project from the Data Factory project.</span></span> <span data-ttu-id="651a9-376">Klikněte pravým tlačítkem na projekt Data Factory, přejděte na **přidat**a potom klikněte na **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="651a9-376">Right-click Data Factory project, point to **Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="651a9-377">V **přidat odkaz na** dialogové okno, vyberte **MyDotNetActivity** projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="651a9-377">In the **Add Reference** dialog box, select the **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="651a9-378">Sestavení a publikování řešení.</span><span class="sxs-lookup"><span data-stu-id="651a9-378">Build and publish the solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="651a9-379">Když publikujete entity služby Data Factory, soubor zip je pro vás automaticky vytvoří a je odeslán do kontejneru objektů blob: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="651a9-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded to the blob container: customactivitycontainer.</span></span> <span data-ttu-id="651a9-380">Pokud kontejner objektů blob neexistuje, je vytvořeno automaticky příliš.</span><span class="sxs-lookup"><span data-stu-id="651a9-380">If the blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="651a9-381">Integrace služby Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="651a9-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="651a9-382">Služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem: **adf poolname: xxx úlohy**.</span><span class="sxs-lookup"><span data-stu-id="651a9-382">The Data Factory service creates a job in Azure Batch with the name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="651a9-383">Klikněte na tlačítko **úlohy** v levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="651a9-383">Click **Jobs** from the left menu.</span></span> 

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="651a9-385">Úloha se vytvoří při každém spuštění aktivity řezu.</span><span class="sxs-lookup"><span data-stu-id="651a9-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="651a9-386">Pokud existují pět řezy, které jsou připravené ke zpracování, jsou v této úloze vytvořit pět úloh.</span><span class="sxs-lookup"><span data-stu-id="651a9-386">If there are five slices ready to be processed, five tasks are created in this job.</span></span> <span data-ttu-id="651a9-387">Pokud existují několika výpočetních uzlech ve fondu Batch, dva nebo více řezů může běžet paralelně.</span><span class="sxs-lookup"><span data-stu-id="651a9-387">If there are multiple compute nodes in the Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="651a9-388">Pokud > 1 je nastavena maximální úkolů na výpočetním uzlu, může mít více než jeden řez na stejném výpočetním spuštěna.</span><span class="sxs-lookup"><span data-stu-id="651a9-388">If the maximum tasks per compute node is set to > 1, you can also have more than one slice running on the same compute.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="651a9-390">Následující diagram znázorňuje vztah mezi Azure Data Factory a dávkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="651a9-390">The following diagram illustrates the relationship between Azure Data Factory and Batch tasks.</span></span>

![Objekt pro vytváření dat & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="651a9-392">Řešení chyb</span><span class="sxs-lookup"><span data-stu-id="651a9-392">Troubleshoot failures</span></span>
<span data-ttu-id="651a9-393">Řešení potíží se skládá z několika základní techniky:</span><span class="sxs-lookup"><span data-stu-id="651a9-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="651a9-394">Pokud se zobrazí následující chyba, můžete pomocí úložiště objektů blob aktivní/nástrojů místo použití úložiště objektů blob v Azure pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="651a9-394">If you see the following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="651a9-395">Nahrajte soubor zip **účet úložiště pro obecné účely Azure**.</span><span class="sxs-lookup"><span data-stu-id="651a9-395">Upload the zip file to a **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of the specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="651a9-396">Pokud se zobrazí následující chyba, zkontrolujte, zda název třídy v souboru CS odpovídá název zadaný pro **EntryPoint** vlastnost v kódu JSON kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-396">If you see the following error, confirm that the name of the class in the CS file matches the name you specified for the **EntryPoint** property in the pipeline JSON.</span></span> <span data-ttu-id="651a9-397">V tomto návodu je název třídy: MyDotNetActivity a vstupní bod v kódu JSON: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="651a9-397">In the walkthrough, name of the class is: MyDotNetActivity, and the EntryPoint in the JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement the type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="651a9-398">Pokud se názvy shodují, potvrďte, že jsou všechny binární soubory **kořenové složky** souboru zip.</span><span class="sxs-lookup"><span data-stu-id="651a9-398">If the names do match, confirm that all the binaries are in the **root folder** of the zip file.</span></span> <span data-ttu-id="651a9-399">To znamená když otevřete soubor zip, měli byste vidět všechny soubory v kořenové složce, ne všechny podsložek.</span><span class="sxs-lookup"><span data-stu-id="651a9-399">That is, when you open the zip file, you should see all the files in the root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="651a9-400">Pokud vstupní řez není nastaven na **připraven**, potvrďte správnost struktuře vstupní složky a **soubor.txt** existuje ve vstupní složkách.</span><span class="sxs-lookup"><span data-stu-id="651a9-400">If the input slice is not set to **Ready**, confirm that the input folder structure is correct and **file.txt** exists in the input folders.</span></span>
3. <span data-ttu-id="651a9-401">V **Execute** metoda vlastní aktivity, použijte **IActivityLogger** objektu k protokolování informací, které vám pomůže vyřešit problémy.</span><span class="sxs-lookup"><span data-stu-id="651a9-401">In the **Execute** method of your custom activity, use the **IActivityLogger** object to log information that helps you troubleshoot issues.</span></span> <span data-ttu-id="651a9-402">Zprávy zaznamenané objeví v souborech protokolu uživatele (jeden nebo více souborů s názvem: uživatel 0.log, 1.log uživatele, uživatel 2.log atd.).</span><span class="sxs-lookup"><span data-stu-id="651a9-402">The logged messages show up in the user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="651a9-403">V **OutputDataset** okně klikněte na tlačítko řez zobrazíte **datový ŘEZ** okno pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-403">In the **OutputDataset** blade, click the slice to see the **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="651a9-404">Zobrazí **běh aktivit** pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="651a9-405">Měli byste vidět jednu aktivitu spustit řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-405">You should see one activity run for the slice.</span></span> <span data-ttu-id="651a9-406">Pokud jste na panelu příkazů klikněte na tlačítko spustit, můžete spustit jiné aktivity při spuštění stejné řez.</span><span class="sxs-lookup"><span data-stu-id="651a9-406">If you click Run in the command bar, you can start another activity run for the same slice.</span></span>

   <span data-ttu-id="651a9-407">Když kliknete na aktivity při spuštění, uvidíte **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="651a9-407">When you click the activity run, you see the **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="651a9-408">Zobrazí zprávy zaznamenané v souboru user_0.log.</span><span class="sxs-lookup"><span data-stu-id="651a9-408">You see logged messages in the user_0.log file.</span></span> <span data-ttu-id="651a9-409">Když dojde k chybě, zobrazí tři běh aktivit, protože počet opakování je nastavena na hodnotu 3 v kódu JSON kanálu nebo aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-409">When an error occurs, you see three activity runs because the retry count is set to 3 in the pipeline/activity JSON.</span></span> <span data-ttu-id="651a9-410">Po kliknutí na tlačítko spustit aktivitu, zobrazí se soubory protokolů, které můžete zkontrolovat, chcete-li vyřešit chyby.</span><span class="sxs-lookup"><span data-stu-id="651a9-410">When you click the activity run, you see the log files that you can review to troubleshoot the error.</span></span>

   <span data-ttu-id="651a9-411">V seznamu souborů protokolu, klikněte na **uživatele 0.log**.</span><span class="sxs-lookup"><span data-stu-id="651a9-411">In the list of log files, click the **user-0.log**.</span></span> <span data-ttu-id="651a9-412">V pravém panelu jsou výsledky pomocí **IActivityLogger.Write** metoda.</span><span class="sxs-lookup"><span data-stu-id="651a9-412">In the right panel are the results of using the **IActivityLogger.Write** method.</span></span> <span data-ttu-id="651a9-413">Pokud nevidíte všechny zprávy, zkontrolujte, zda máte další soubory protokolu s názvem: user_1.log, user_2.log atd. Kód nebyla úspěšná, jinak hodnota po poslední přihlášení zprávy.</span><span class="sxs-lookup"><span data-stu-id="651a9-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, the code may have failed after the last logged message.</span></span>

   <span data-ttu-id="651a9-414">Kromě toho zkontrolujte **systému 0.log** pro všechny chybové zprávy systému a výjimky.</span><span class="sxs-lookup"><span data-stu-id="651a9-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="651a9-415">Zahrnout **PDB** souborů v souboru zip tak, aby podrobnosti o chybě informace, jako **zásobník volání** když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="651a9-415">Include the **PDB** file in the zip file so that the error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="651a9-416">Všechny soubory v souboru .zip pro vlastní aktivitu musí být na **nejvyšší úrovni**, bez podsložek.</span><span class="sxs-lookup"><span data-stu-id="651a9-416">All the files in the zip file for the custom activity must be at the **top level** with no sub folders.</span></span>
6. <span data-ttu-id="651a9-417">Ujistěte se, že **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (by měla odkazovat na **pro obecné účely**úložiště objektů blob v Azure, který obsahuje soubor zip) jsou nastaveny na správný hodnoty.</span><span class="sxs-lookup"><span data-stu-id="651a9-417">Ensure that the **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point to the **general-purpose**Azure blob storage that contains the zip file) are set to correct values.</span></span>
7. <span data-ttu-id="651a9-418">Pokud jste opravili chybu a chcete řez zpracovat znovu, klikněte na něj v okně **OutputDataset** pravým tlačítkem myši a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="651a9-418">If you fixed an error and want to reprocess the slice, right-click the slice in the **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="651a9-419">Pokud se zobrazí následující chyba, používáte Azure Storage balíček verze > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="651a9-419">If you see the following error, you are using the Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="651a9-420">Spouštěč služby Data Factory vyžaduje 4.3 verzi WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-420">Data Factory service launcher requires the 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="651a9-421">V tématu [izolace domény aplikace](#appdomain-isolation) části pro řešení, pokud musíte použít novější verzi sestavení Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use the later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="651a9-422">Pokud použijete 4.3.0 verzi balíčku Azure Storage, odeberte existující odkaz na balíček verze > 4.3.0 Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="651a9-422">If you can use the 4.3.0 version of Azure Storage package, remove the existing reference to Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="651a9-423">Potom spusťte následující příkaz z konzoly Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="651a9-423">Then, run the following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="651a9-424">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="651a9-424">Build the project.</span></span> <span data-ttu-id="651a9-425">Odstraňte ze složky bin\Debug Azure.Storage sestavením verze > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="651a9-425">Delete Azure.Storage assembly of version > 4.3.0 from the bin\Debug folder.</span></span> <span data-ttu-id="651a9-426">Vytvořte soubor zip s binární soubory a souboru PDB.</span><span class="sxs-lookup"><span data-stu-id="651a9-426">Create a zip file with binaries and the PDB file.</span></span> <span data-ttu-id="651a9-427">Nahraďte původní soubor zip touto v kontejneru objektů blob (customactivitycontainer).</span><span class="sxs-lookup"><span data-stu-id="651a9-427">Replace the old zip file with this one in the blob container (customactivitycontainer).</span></span> <span data-ttu-id="651a9-428">Znovu spustit datové řezy, které se nezdařilo (klikněte pravým tlačítkem na řez a klikněte na tlačítko Spustit).</span><span class="sxs-lookup"><span data-stu-id="651a9-428">Rerun the slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="651a9-429">Vlastní aktivity nepoužívá **app.config** soubor ze svého balíčku.</span><span class="sxs-lookup"><span data-stu-id="651a9-429">The custom activity does not use the **app.config** file from your package.</span></span> <span data-ttu-id="651a9-430">Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru, ale nefunguje za běhu.</span><span class="sxs-lookup"><span data-stu-id="651a9-430">Therefore, if your code reads any connection strings from the configuration file, it does not work at runtime.</span></span> <span data-ttu-id="651a9-431">Osvědčeným postupem při použití Azure Batch je pro uložení všech tajných klíčů v **Azure KeyVault**, použijte objekt služby založené na certifikátech k ochraně **keyvault**a distribuovat certifikát, který chcete Azure Batch fond.</span><span class="sxs-lookup"><span data-stu-id="651a9-431">The best practice when using Azure Batch is to hold any secrets in an **Azure KeyVault**, use a certificate-based service principal to protect the **keyvault**, and distribute the certificate to Azure Batch pool.</span></span> <span data-ttu-id="651a9-432">Vlastní aktivita .NET potom má přístup k tajným klíčům v trezoru za běhu.</span><span class="sxs-lookup"><span data-stu-id="651a9-432">The .NET custom activity then can access secrets from the KeyVault at runtime.</span></span> <span data-ttu-id="651a9-433">Toto řešení je obecný řešení a můžete škálovat k libovolnému typu tajný klíč, ne jenom připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="651a9-433">This solution is a generic solution and can scale to any type of secret, not just connection string.</span></span>

   <span data-ttu-id="651a9-434">Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** pomocí nastavení připojovacího řetězce, vytvořit datovou sadu, která používá propojené služby a řetězu datovou sadu jako fiktivní vstupní datové sady do vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="651a9-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses the linked service, and chain the dataset as a dummy input dataset to the custom .NET activity.</span></span> <span data-ttu-id="651a9-435">Máte přístup připojovací řetězec propojené služby v kódu vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-435">You can then access the linked service's connection string in the custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="651a9-436">Aktualizovat vlastní aktivitu</span><span class="sxs-lookup"><span data-stu-id="651a9-436">Update custom activity</span></span>
<span data-ttu-id="651a9-437">Pokud aktualizujete kód vlastní aktivity, sestavte jej a nahrajte soubor zip, který obsahuje nové binární soubory do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="651a9-437">If you update the code for the custom activity, build it, and upload the zip file that contains new binaries to the blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="651a9-438">Izolace domény aplikace</span><span class="sxs-lookup"><span data-stu-id="651a9-438">Appdomain isolation</span></span>
<span data-ttu-id="651a9-439">V tématu [mezi ukázka AppDomain](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) který ukazuje, jak vytvořit vlastní aktivity, které nejsou rozděleny do verze sestavení používá Spouštěče objekt pro vytváření dat (Příklad: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x atd.).</span><span class="sxs-lookup"><span data-stu-id="651a9-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how to create a custom activity that is not constrained to assembly versions used by the Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="651a9-440">Přístup rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="651a9-440">Access extended properties</span></span>
<span data-ttu-id="651a9-441">Je možné deklarovat rozšířených vlastností v kódu JSON, jak znázorňuje následující ukázka aktivity:</span><span class="sxs-lookup"><span data-stu-id="651a9-441">You can declare extended properties in the activity JSON as shown in the following sample:</span></span>

```JSON
"typeProperties": {
  "AssemblyName": "MyDotNetActivity.dll",
  "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
  "PackageLinkedService": "AzureStorageLinkedService",
  "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
  "extendedProperties": {
    "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))",
    "DataFactoryName": "CustomActivityFactory"
  }
},
```


<span data-ttu-id="651a9-442">V příkladu jsou dvě rozšířené vlastnosti: **SliceStart** a **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="651a9-442">In the example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="651a9-443">Hodnota pro SliceStart je založená na proměnnou SliceStart systému.</span><span class="sxs-lookup"><span data-stu-id="651a9-443">The value for SliceStart is based on the SliceStart system variable.</span></span> <span data-ttu-id="651a9-444">V tématu [systémové proměnné](data-factory-functions-variables.md) seznam podporovaných systémové proměnné.</span><span class="sxs-lookup"><span data-stu-id="651a9-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="651a9-445">Hodnota DataFactoryName je pevně zakódovaná na CustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="651a9-445">The value for DataFactoryName is hard-coded to CustomActivityFactory.</span></span>

<span data-ttu-id="651a9-446">Pro přístup k těmto rozšířené vlastnosti v **Execute** metoda, použít kód podobná následující kód:</span><span class="sxs-lookup"><span data-stu-id="651a9-446">To access these extended properties in the **Execute** method, use code similar to the following code:</span></span>

```csharp
// to get extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// to log all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="651a9-447">Automatické škálování služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="651a9-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="651a9-448">Můžete také vytvořit fond Azure Batch s **škálování** funkce.</span><span class="sxs-lookup"><span data-stu-id="651a9-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="651a9-449">Můžete například vytvořit fondu azure batch s 0 vyhrazených virtuálních počítačích a vzorec škálování na základě počtu úkolů čekajících na zpracování.</span><span class="sxs-lookup"><span data-stu-id="651a9-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on the number of pending tasks.</span></span> 

<span data-ttu-id="651a9-450">Vzorec ukázka tady dosahuje následující chování: při vytvoření fondu, začne s virtuálním Počítačem 1.</span><span class="sxs-lookup"><span data-stu-id="651a9-450">The sample formula here achieves the following behavior: When the pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="651a9-451">Metrika $PendingTasks definuje počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.</span><span class="sxs-lookup"><span data-stu-id="651a9-451">$PendingTasks metric defines the number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="651a9-452">Vzorec najde průměrný počet úkolů čekajících na zpracování v posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="651a9-452">The formula finds the average number of pending tasks in the last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="651a9-453">Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651a9-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="651a9-454">Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování zmenšuje těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="651a9-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and the autoscaling shrinks those VMs.</span></span> <span data-ttu-id="651a9-455">startingNumberOfVMs a maxNumberofVMs lze upravit vašim potřebám.</span><span class="sxs-lookup"><span data-stu-id="651a9-455">startingNumberOfVMs and maxNumberofVMs can be adjusted to your needs.</span></span>

<span data-ttu-id="651a9-456">Vzorec škálování:</span><span class="sxs-lookup"><span data-stu-id="651a9-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="651a9-457">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../batch/batch-automatic-scaling.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="651a9-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="651a9-458">Pokud fondu používá výchozí [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), služba Batch může trvat 15 až 30 minut Příprava virtuálního počítače před spuštěním vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-458">If the pool is using the default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), the Batch service could take 15-30 minutes to prepare the VM before running the custom activity.</span></span>  <span data-ttu-id="651a9-459">Pokud fondu používá jiný autoScaleEvaluationInterval, služba Batch může trvat autoScaleEvaluationInterval + 10 minut.</span><span class="sxs-lookup"><span data-stu-id="651a9-459">If the pool is using a different autoScaleEvaluationInterval, the Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="651a9-460">Pomocí HDInsight výpočetní služby</span><span class="sxs-lookup"><span data-stu-id="651a9-460">Use HDInsight compute service</span></span>
<span data-ttu-id="651a9-461">V tomto návodu jste použili výpočetní Azure Batch pro spuštění vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="651a9-461">In the walkthrough, you used Azure Batch compute to run the custom activity.</span></span> <span data-ttu-id="651a9-462">Můžete také použít vlastní cluster HDInsight se systémem Windows nebo mít objekt pro vytváření dat vytvořit na vyžádání clusteru HDInsight se systémem Windows a mít vlastní aktivity při spuštění v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have the custom activity run on the HDInsight cluster.</span></span> <span data-ttu-id="651a9-463">Zde jsou základní kroky pro použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-463">Here are the high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="651a9-464">Vlastní aktivity .NET spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="651a9-464">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="651a9-465">Alternativní řešení pro toto omezení je pomocí mapy snížit aktivity ke spuštění vlastního kódu Java na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="651a9-465">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="651a9-466">Další možností je použití fondu virtuálních počítačů služby Azure Batch ke spouštění vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-466">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="651a9-467">Vytvoření služby Azure HDInsight propojený.</span><span class="sxs-lookup"><span data-stu-id="651a9-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="651a9-468">Použití HDInsight propojená služba místě **AzureBatchLinkedService** v kódu JSON kanálu.</span><span class="sxs-lookup"><span data-stu-id="651a9-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in the pipeline JSON.</span></span>

<span data-ttu-id="651a9-469">Pokud chcete otestovat s návodu, změňte **spustit** a **end** časy pro kanál, tak, aby scénáře pomocí služby Azure HDInsight můžete otestovat.</span><span class="sxs-lookup"><span data-stu-id="651a9-469">If you want to test it with the walkthrough, change **start** and **end** times for the pipeline so that you can test the scenario with the Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="651a9-470">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="651a9-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="651a9-471">Služba Azure Data Factory podporuje vytváření clusteru na vyžádání a použít ho ke zpracování vstupu nevytvořila výstupní data.</span><span class="sxs-lookup"><span data-stu-id="651a9-471">The Azure Data Factory service supports creation of an on-demand cluster and use it to process input to produce output data.</span></span> <span data-ttu-id="651a9-472">Můžete také vlastní cluster provést stejný.</span><span class="sxs-lookup"><span data-stu-id="651a9-472">You can also use your own cluster to perform the same.</span></span> <span data-ttu-id="651a9-473">Pokud používáte cluster HDInsight na vyžádání, získá pro každý řez vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="651a9-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="651a9-474">Vzhledem k tomu, pokud chcete použít vlastní cluster HDInsight, cluster je připraven ke zpracování řezu se okamžitě.</span><span class="sxs-lookup"><span data-stu-id="651a9-474">Whereas, if you use your own HDInsight cluster, the cluster is ready to process the slice immediately.</span></span> <span data-ttu-id="651a9-475">Proto když používáte cluster na vyžádání, nemusíte vidět výstupní data co když použijete vlastní cluster.</span><span class="sxs-lookup"><span data-stu-id="651a9-475">Therefore, when you use on-demand cluster, you may not see the output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="651a9-476">V době běhu instance aktivity .NET funguje pouze na jeden uzel pracovního procesu v clusteru HDInsight; nelze ji škálovat ke spuštění na víc uzlů.</span><span class="sxs-lookup"><span data-stu-id="651a9-476">At runtime, an instance of a .NET activity runs only on one worker node in the HDInsight cluster; it cannot be scaled to run on multiple nodes.</span></span> <span data-ttu-id="651a9-477">Více instancí .NET aktivity může běžet paralelně na různých uzlech clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-477">Multiple instances of .NET activity can run in parallel on different nodes of the HDInsight cluster.</span></span>
>
>

##### <a name="to-use-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="651a9-478">Na používání clusteru HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="651a9-478">To use an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="651a9-479">V **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** na domovské stránce objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="651a9-479">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="651a9-480">V editoru služby Data Factory, klikněte na tlačítko **nový výpočet** z příkazového řádku a vyberte **clusteru HDInsight na vyžádání** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="651a9-480">In the Data Factory Editor, click **New compute** from the command bar and select **On-demand HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="651a9-481">Skript JSON proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="651a9-481">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="651a9-482">Pro **parametr clusterSize** vlastnost, zadejte velikost clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-482">For the **clusterSize** property, specify the size of the HDInsight cluster.</span></span>
   2. <span data-ttu-id="651a9-483">Pro **timeToLive** vlastnost, zadejte, jak dlouho zákazníka může být nečinnosti, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="651a9-483">For the **timeToLive** property, specify how long the customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="651a9-484">Pro **verze** vlastnost, zadejte verzi HDInsight, kterou chcete použít.</span><span class="sxs-lookup"><span data-stu-id="651a9-484">For the **version** property, specify the HDInsight version you want to use.</span></span> <span data-ttu-id="651a9-485">Pokud vyloučíte tuto vlastnost, použije se nejnovější verzi.</span><span class="sxs-lookup"><span data-stu-id="651a9-485">If you exclude this property, the latest version is used.</span></span>  
   4. <span data-ttu-id="651a9-486">Pro **linkedServiceName**, zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="651a9-486">For the **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

        ```JSON
        {
           "name": "HDInsightOnDemandLinkedService",
           "properties": {
               "type": "HDInsightOnDemand",
               "typeProperties": {
                   "clusterSize": 4,
                   "timeToLive": "00:05:00",
                   "osType": "Windows",
                   "linkedServiceName": "AzureStorageLinkedService",
               }
           }
        }
        ```

    > [!IMPORTANT]
    > <span data-ttu-id="651a9-487">Vlastní aktivity .NET spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="651a9-487">The custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="651a9-488">Alternativní řešení pro toto omezení je pomocí mapy snížit aktivity ke spuštění vlastního kódu Java na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="651a9-488">A workaround for this limitation is to use the Map Reduce Activity to run custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="651a9-489">Další možností je použití fondu virtuálních počítačů služby Azure Batch ke spouštění vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-489">Another option is to use an Azure Batch pool of VMs to run custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="651a9-490">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-490">Click **Deploy** on the command bar to deploy the linked service.</span></span>

##### <a name="to-use-your-own-hdinsight-cluster"></a><span data-ttu-id="651a9-491">Použití vlastní cluster HDInsight:</span><span class="sxs-lookup"><span data-stu-id="651a9-491">To use your own HDInsight cluster:</span></span>
1. <span data-ttu-id="651a9-492">V **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** na domovské stránce objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="651a9-492">In the **Azure portal**, click **Author and Deploy** in the Data Factory home page.</span></span>
2. <span data-ttu-id="651a9-493">V **editoru služby Data Factory**, klikněte na tlačítko **nový výpočet** z příkazového řádku a vyberte **clusteru HDInsight** z nabídky.</span><span class="sxs-lookup"><span data-stu-id="651a9-493">In the **Data Factory Editor**, click **New compute** from the command bar and select **HDInsight cluster** from the menu.</span></span>
3. <span data-ttu-id="651a9-494">Skript JSON proveďte následující změny:</span><span class="sxs-lookup"><span data-stu-id="651a9-494">Make the following changes to the JSON script:</span></span>

   1. <span data-ttu-id="651a9-495">Pro **clusterUri** vlastnost, zadejte adresu URL pro vaše prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-495">For the **clusterUri** property, enter the URL for your HDInsight.</span></span> <span data-ttu-id="651a9-496">Příklad: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="651a9-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="651a9-497">Pro **uživatelské jméno** vlastnost, zadejte uživatelské jméno, který má přístup ke clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-497">For the **UserName** property, enter the user name who has access to the HDInsight cluster.</span></span>
   3. <span data-ttu-id="651a9-498">Pro **heslo** vlastnost, zadejte heslo pro uživatele.</span><span class="sxs-lookup"><span data-stu-id="651a9-498">For the **Password** property, enter the password for the user.</span></span>
   4. <span data-ttu-id="651a9-499">Pro **LinkedServiceName** vlastnost, zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="651a9-499">For the **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="651a9-500">Propojenou službu nasadíte kliknutím na **Nasadit** na panelu příkazů.</span><span class="sxs-lookup"><span data-stu-id="651a9-500">Click **Deploy** on the command bar to deploy the linked service.</span></span>

<span data-ttu-id="651a9-501">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="651a9-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="651a9-502">V **kanálu JSON**, použití HDInsight (na vyžádání nebo vlastní) propojené služby:</span><span class="sxs-lookup"><span data-stu-id="651a9-502">In the **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

```JSON
{
  "name": "ADFTutorialPipelineCustom",
  "properties": {
    "description": "Use custom activity",
    "activities": [
      {
        "Name": "MyDotNetActivity",
        "Type": "DotNetActivity",
        "Inputs": [
          {
            "Name": "InputDataset"
          }
        ],
        "Outputs": [
          {
            "Name": "OutputDataset"
          }
        ],
        "LinkedServiceName": "HDInsightOnDemandLinkedService",
        "typeProperties": {
          "AssemblyName": "MyDotNetActivity.dll",
          "EntryPoint": "MyDotNetActivityNS.MyDotNetActivity",
          "PackageLinkedService": "AzureStorageLinkedService",
          "PackageFile": "customactivitycontainer/MyDotNetActivity.zip",
          "extendedProperties": {
            "SliceStart": "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"
          }
        },
        "Policy": {
          "Concurrency": 2,
          "ExecutionPriorityOrder": "OldestFirst",
          "Retry": 3,
          "Timeout": "00:30:00",
          "Delay": "00:00:00"
        }
      }
    ],
    "start": "2016-11-16T00:00:00Z",
    "end": "2016-11-16T05:00:00Z",
    "isPaused": false
  }
}
```

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="651a9-503">Vytvoření vlastní aktivity pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="651a9-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="651a9-504">V tomto návodu v tomto článku vytvoříte objekt pro vytváření dat kanál, který používá vlastní aktivity pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="651a9-504">In the walkthrough in this article, you create a data factory with a pipeline that uses the custom activity by using the Azure portal.</span></span> <span data-ttu-id="651a9-505">Následující kód ukazuje, jak vytvořit objekt pro vytváření dat pomocí .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="651a9-505">The following code shows you how to create the data factory by using .NET SDK instead.</span></span> <span data-ttu-id="651a9-506">Můžete najít další podrobnosti o použití sady SDK k vytváření prostřednictvím kódu programu kanály v [vytvoření kanálu s aktivitou kopírování pomocí rozhraní API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md) článku.</span><span class="sxs-lookup"><span data-stu-id="651a9-506">You can find more details about using SDK to programmatically create pipelines in the [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

```csharp
using System;
using System.Configuration;
using System.Collections.ObjectModel;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.Azure;
using Microsoft.Azure.Management.DataFactories;
using Microsoft.Azure.Management.DataFactories.Models;
using Microsoft.Azure.Management.DataFactories.Common.Models;

using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Collections.Generic;

namespace DataFactoryAPITestApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // create data factory management client

            // TODO: replace ADFTutorialResourceGroup with the name of your resource group.
            string resourceGroupName = "ADFTutorialResourceGroup";

            // TODO: replace APITutorialFactory with a name that is globally unique. For example: APITutorialFactory04212017
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
                ConfigurationManager.AppSettings["SubscriptionId"],
                GetAuthorizationHeader().Result);

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties()
                    }
                }
            );

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: Replace <accountname> and <accountkey> with name and key of your Azure Storage account.
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure Batch linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureBatchLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            // TODO: replace <batchaccountname> and <yourbatchaccountkey> with name and key of your Azure Batch account
                            new AzureBatchLinkedService("<batchaccountname>", "https://westus.batch.azure.com", "<yourbatchaccountkey>", "myazurebatchpool", "AzureStorageLinkedService")
                        )
                    }
                }
            );

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "InputDataset";
            string Dataset_Destination = "OutputDataset";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Source,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FolderPath = "adftutorial/customactivityinput/",
                                Format = new TextFormat()
                            },
                            External = true,
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },

                            Policy = new Policy() { }
                        }
                    }
                });

            Console.WriteLine("Creating output dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            LinkedServiceName = "AzureStorageLinkedService",
                            TypeProperties = new AzureBlobDataset()
                            {
                                FileName = "{slice}.txt",
                                FolderPath = "adftutorial/customactivityoutput/",
                                PartitionedBy = new List<Partition>()
                                {
                                    new Partition()
                                    {
                                        Name = "slice",
                                        Value = new DateTimePartitionValue()
                                        {
                                            Date = "SliceStart",
                                            Format = "yyyy-MM-dd-HH"
                                        }
                                    }
                                }
                            },
                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

            Console.WriteLine("Creating a custom activity pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2017, 3, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipelineCustom";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Use custom activity",

                            // Initial value for pipeline's active period. With this, you won't need to set slice status
                            Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,
                            IsPaused = false,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "MyDotNetActivity",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    LinkedServiceName = "AzureBatchLinkedService",
                                    TypeProperties = new DotNetActivity()
                                    {
                                        AssemblyName = "MyDotNetActivity.dll",
                                        EntryPoint = "MyDotNetActivityNS.MyDotNetActivity",
                                        PackageLinkedService = "AzureStorageLinkedService",
                                        PackageFile = "customactivitycontainer/MyDotNetActivity.zip",
                                        ExtendedProperties = new Dictionary<string, string>()
                                        {
                                            { "SliceStart", "$$Text.Format('{0:yyyyMMddHH-mm}', Time.AddMinutes(SliceStart, 0))"}
                                        }
                                    },
                                    Policy = new ActivityPolicy()
                                    {
                                        Concurrency = 2,
                                        ExecutionPriorityOrder = "OldestFirst",
                                        Retry = 3,
                                        Timeout = new TimeSpan(0,0,30,0),
                                        Delay = new TimeSpan()
                                    }
                                }
                            }
                        }
                    }
                });
        }

        public static async Task<string> GetAuthorizationHeader()
        {
            AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
            ClientCredential credential = new ClientCredential(
                ConfigurationManager.AppSettings["ApplicationId"],
                ConfigurationManager.AppSettings["Password"]);
            AuthenticationResult result = await context.AcquireTokenAsync(
                resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                clientCredential: credential);

            if (result != null)
                return result.AccessToken;

            throw new InvalidOperationException("Failed to acquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="651a9-507">Ladění vlastní aktivity v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="651a9-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="651a9-508">[Azure Data Factory - místní prostředí](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) ukázce na Githubu zahrnuje nástroj, který vám umožní ladit vlastní .NET aktivity v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="651a9-508">The [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you to debug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="651a9-509">Ukázka vlastních aktivit na Githubu</span><span class="sxs-lookup"><span data-stu-id="651a9-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="651a9-510">Ukázka</span><span class="sxs-lookup"><span data-stu-id="651a9-510">Sample</span></span> | <span data-ttu-id="651a9-511">Jaké vlastní aktivita provede</span><span class="sxs-lookup"><span data-stu-id="651a9-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="651a9-512">[Stažení programu HTTP Data](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="651a9-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="651a9-513">Stahování dat z koncový bod HTTP do Azure Blob Storage pomocí vlastní C# aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="651a9-513">Downloads data from an HTTP Endpoint to Azure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="651a9-514">Ukázka postojích Analysis služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="651a9-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="651a9-515">Vyvolá modelu Azure ML a proveďte analýzu postojích, vyhodnocování, předpovědi atd.</span><span class="sxs-lookup"><span data-stu-id="651a9-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="651a9-516">[Spustit skript jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="651a9-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="651a9-517">Vyvolá skript jazyka R spuštěním RScript.exe, který již má nainstalované R na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="651a9-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="651a9-518">Mezi domény aplikace .NET aktivity</span><span class="sxs-lookup"><span data-stu-id="651a9-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="651a9-519">Používá jiný sestavení verze z těch, které jsou používané Spouštěče objekt pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="651a9-519">Uses different assembly versions from ones used by the Data Factory launcher</span></span> |
| [<span data-ttu-id="651a9-520">Zpracování modelu v Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="651a9-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="651a9-521">Znovu zpracuje model v Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="651a9-521">Reprocesses a model in Azure Analysis Services.</span></span> |

[batch-net-library]: ../batch/batch-dotnet-get-started.md
[batch-create-account]: ../batch/batch-account-create-portal.md
[batch-technical-overview]: ../batch/batch-technical-overview.md
[batch-get-started]: ../batch/batch-dotnet-get-started.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[azure-powershell-install]: https://github.com/Azure/azure-sdk-tools/releases


[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[new-azure-batch-account]: https://msdn.microsoft.com/library/mt125880.aspx
[new-azure-batch-pool]: https://msdn.microsoft.com/library/mt125936.aspx
[azure-batch-blog]: http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx

[nuget-package]: http://go.microsoft.com/fwlink/?LinkId=517478
[adf-developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[azure-preview-portal]: https://portal.azure.com/

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[hivewalkthrough]: data-factory-data-transformation-activities.md

[image-data-factory-ouput-from-custom-activity]: ./media/data-factory-use-custom-activities/OutputFilesFromCustomActivity.png

[image-data-factory-download-logs-from-custom-activity]: ./media/data-factory-use-custom-activities/DownloadLogsFromCustomActivity.png
