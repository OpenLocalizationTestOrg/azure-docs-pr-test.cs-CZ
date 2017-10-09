---
title: "aaaUse vlastní aktivity v kanálu Azure Data Factory"
description: "Zjistěte, jak toocreate vlastní aktivity a použijte je v kanál služby Azure Data Factory."
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
ms.openlocfilehash: 23e33727b2160541ab40938ffd911fdd484b3daa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-custom-activities-in-an-azure-data-factory-pipeline"></a><span data-ttu-id="c266e-103">Použití vlastních aktivit v kanálu Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c266e-103">Use custom activities in an Azure Data Factory pipeline</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="c266e-104">Aktivita Hive</span><span class="sxs-lookup"><span data-stu-id="c266e-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="c266e-105">Pig aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="c266e-106">Činnost MapReduce</span><span class="sxs-lookup"><span data-stu-id="c266e-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="c266e-107">Streamované aktivitě Hadoop</span><span class="sxs-lookup"><span data-stu-id="c266e-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="c266e-108">Spark aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="c266e-109">Aktivita Provedení dávky služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c266e-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="c266e-110">Aktivita Aktualizace prostředků služby Machine Learning</span><span class="sxs-lookup"><span data-stu-id="c266e-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="c266e-111">Aktivita Uložená procedura</span><span class="sxs-lookup"><span data-stu-id="c266e-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="c266e-112">Aktivita U-SQL služby Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="c266e-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="c266e-113">Vlastní aktivity rozhraní .NET</span><span class="sxs-lookup"><span data-stu-id="c266e-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="c266e-114">Existují dva typy aktivit, které můžete použít v kanál služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c266e-114">There are two types of activities that you can use in an Azure Data Factory pipeline.</span></span>

- <span data-ttu-id="c266e-115">[Aktivity přesunu dat](data-factory-data-movement-activities.md) toomove dat mezi [podporovanými úložišti dat zdroj a jímka](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="c266e-115">[Data Movement Activities](data-factory-data-movement-activities.md) toomove data between [supported source and sink data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span>
- <span data-ttu-id="c266e-116">[Aktivity transformace dat](data-factory-data-transformation-activities.md) tootransform dat pomocí výpočetních služeb například Azure HDInsight, Azure Batch a Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="c266e-116">[Data Transformation Activities](data-factory-data-transformation-activities.md) tootransform data using compute services such as Azure HDInsight, Azure Batch, and Azure Machine Learning.</span></span> 

<span data-ttu-id="c266e-117">Vytvoření toomove data do nebo z úložiště dat, které objekt pro vytváření dat nepodporuje, **vlastní aktivity** s vlastní data přesun logiku a použití hello aktivitu v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c266e-117">toomove data to/from a data store that Data Factory does not support, create a **custom activity** with your own data movement logic and use hello activity in a pipeline.</span></span> <span data-ttu-id="c266e-118">Podobně tootransform nebo zpracování dat způsobem, který není podporován službou Data Factory vytvořit vlastní aktivity s vlastní logikou transformaci dat a použít hello aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c266e-118">Similarly, tootransform/process data in a way that isn't supported by Data Factory, create a custom activity with your own data transformation logic and use hello activity in a pipeline.</span></span> 

<span data-ttu-id="c266e-119">Můžete nakonfigurovat vlastní aktivity toorun na **Azure Batch** fondu virtuálních počítačů nebo systémem Windows **Azure HDInsight** clusteru.</span><span class="sxs-lookup"><span data-stu-id="c266e-119">You can configure a custom activity toorun on an **Azure Batch** pool of virtual machines or a Windows-based **Azure HDInsight** cluster.</span></span> <span data-ttu-id="c266e-120">Pokud používáte Azure Batch, můžete použít pouze existujícího fondu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="c266e-120">When using Azure Batch, you can use only an existing Azure Batch pool.</span></span> <span data-ttu-id="c266e-121">Vzhledem k tomu, při použití HDInsight, můžete použít stávající cluster HDInsight nebo clusteru, který se automaticky vytvoří pro můžete na vyžádání za běhu.</span><span class="sxs-lookup"><span data-stu-id="c266e-121">Whereas, when using HDInsight, you can use an existing HDInsight cluster or a cluster that is automatically created for you on-demand at runtime.</span></span>  

<span data-ttu-id="c266e-122">Hello následující názorný postup obsahuje podrobné pokyny pro vytváření vlastních aktivit rozhraní .NET a používání hello vlastní aktivity v kanálu.</span><span class="sxs-lookup"><span data-stu-id="c266e-122">hello following walkthrough provides step-by-step instructions for creating a custom .NET activity and using hello custom activity in a pipeline.</span></span> <span data-ttu-id="c266e-123">návod Hello používá **Azure Batch** propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c266e-123">hello walkthrough uses an **Azure Batch** linked service.</span></span> <span data-ttu-id="c266e-124">toouse Azure HDInsight propojená služba místo, vytvoření propojené služby typu **HDInsight** (vlastní cluster HDInsight) nebo **HDInsightOnDemand** (Data Factory vytvoří cluster služby HDInsight na vyžádání).</span><span class="sxs-lookup"><span data-stu-id="c266e-124">toouse an Azure HDInsight linked service instead, you create a linked service of type **HDInsight** (your own HDInsight cluster) or **HDInsightOnDemand** (Data Factory creates an HDInsight cluster on-demand).</span></span> <span data-ttu-id="c266e-125">Potom nakonfigurujte vlastní aktivity toouse hello propojené služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-125">Then, configure custom activity toouse hello HDInsight linked service.</span></span> <span data-ttu-id="c266e-126">V tématu [použití Azure HDInsight propojené služby](#use-hdinsight-compute-service) část Podrobnosti o použití Azure HDInsight toorun hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-126">See [Use Azure HDInsight linked services](#use-hdinsight-compute-service) section for details on using Azure HDInsight toorun hello custom activity.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="c266e-127">vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c266e-127">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c266e-128">Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c266e-128">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c266e-129">Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-129">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
> - <span data-ttu-id="c266e-130">Není možné toouse Brána pro správu dat z vlastní aktivity tooaccess místní datové zdroje.</span><span class="sxs-lookup"><span data-stu-id="c266e-130">It is not possible toouse a Data Management Gateway from a custom activity tooaccess on-premises data sources.</span></span> <span data-ttu-id="c266e-131">V současné době [Brána pro správu dat](data-factory-data-management-gateway.md) podporuje pouze aktivity kopírování hello a aktivity uložené procedury v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="c266e-131">Currently, [Data Management Gateway](data-factory-data-management-gateway.md) supports only hello copy activity and stored procedure activity in Data Factory.</span></span>   

## <a name="walkthrough-create-a-custom-activity"></a><span data-ttu-id="c266e-132">Návod: vytvoření vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-132">Walkthrough: create a custom activity</span></span>
### <a name="prerequisites"></a><span data-ttu-id="c266e-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c266e-133">Prerequisites</span></span>
* <span data-ttu-id="c266e-134">Visual Studio 2012/2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="c266e-134">Visual Studio 2012/2013/2015</span></span>
* <span data-ttu-id="c266e-135">Stáhněte sadu [Azure .NET SDK](https://azure.microsoft.com/downloads/) a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="c266e-135">Download and install [Azure .NET SDK](https://azure.microsoft.com/downloads/)</span></span>

### <a name="azure-batch-prerequisites"></a><span data-ttu-id="c266e-136">Požadavky Azure Batch</span><span class="sxs-lookup"><span data-stu-id="c266e-136">Azure Batch prerequisites</span></span>
<span data-ttu-id="c266e-137">V Průvodci hello spustíte vlastní aktivitu .NET pomocí Azure Batch jako výpočetní prostředek.</span><span class="sxs-lookup"><span data-stu-id="c266e-137">In hello walkthrough, you run your custom .NET activities using Azure Batch as a compute resource.</span></span> <span data-ttu-id="c266e-138">**Azure Batch** je platforma služby pro spuštění rozsáhlé paralelní a vysoce výkonné aplikace (HPC) efektivně v hello cloud computing.</span><span class="sxs-lookup"><span data-stu-id="c266e-138">**Azure Batch** is a platform service for running large-scale parallel and high-performance computing (HPC) applications efficiently in hello cloud.</span></span> <span data-ttu-id="c266e-139">Azure Batch plány toorun výpočetně náročných na spravované **kolekci virtuálních počítačů**, a dokáže automaticky škálovat výpočetní prostředky toomeet hello potřeby vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="c266e-139">Azure Batch schedules compute-intensive work toorun on a managed **collection of virtual machines**, and can automatically scale compute resources toomeet hello needs of your jobs.</span></span> <span data-ttu-id="c266e-140">V tématu [základy Azure Batch] [ batch-technical-overview] článku podrobnější přehled služby Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-140">See [Azure Batch basics][batch-technical-overview] article for a detailed overview of hello Azure Batch service.</span></span>

<span data-ttu-id="c266e-141">Hello kurzu vytvoříte účet Azure Batch se fondu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c266e-141">For hello tutorial, create an Azure Batch account with a pool of VMs.</span></span> <span data-ttu-id="c266e-142">Zde jsou kroky hello:</span><span class="sxs-lookup"><span data-stu-id="c266e-142">Here are hello steps:</span></span>

1. <span data-ttu-id="c266e-143">Vytvoření **účtu Azure Batch** pomocí hello [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c266e-143">Create an **Azure Batch account** using hello [Azure portal](http://portal.azure.com).</span></span> <span data-ttu-id="c266e-144">V tématu [vytvoření a Správa účtu Azure Batch] [ batch-create-account] pokyny najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="c266e-144">See [Create and manage an Azure Batch account][batch-create-account] article for instructions.</span></span>
2. <span data-ttu-id="c266e-145">Poznamenejte si název účtu Azure Batch hello, klíč účtu, identifikátor URI a název fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-145">Note down hello Azure Batch account name, account key, URI, and pool name.</span></span> <span data-ttu-id="c266e-146">Musíte je toocreate služby Azure Batch propojený.</span><span class="sxs-lookup"><span data-stu-id="c266e-146">You need them toocreate an Azure Batch linked service.</span></span>
    1. <span data-ttu-id="c266e-147">Na domovské stránce hello pro účet Azure Batch, uvidíte **URL** v hello následující formát: `https://myaccount.westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c266e-147">On hello home page for Azure Batch account, you see a **URL** in hello following format: `https://myaccount.westus.batch.azure.com`.</span></span> <span data-ttu-id="c266e-148">V tomto příkladu **stránku Můj účet** je název hello hello účtu Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="c266e-148">In this example, **myaccount** is hello name of hello Azure Batch account.</span></span> <span data-ttu-id="c266e-149">Identifikátor URI, které můžete použít v definici hello propojené služby je adresa URL hello bez názvu hello hello účtu.</span><span class="sxs-lookup"><span data-stu-id="c266e-149">URI you use in hello linked service definition is hello URL without hello name of hello account.</span></span> <span data-ttu-id="c266e-150">Například: `https://<region>.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c266e-150">For example: `https://<region>.batch.azure.com`.</span></span>
    2. <span data-ttu-id="c266e-151">Klikněte na tlačítko **klíče** v levé nabídce hello a kopírování hello **primární přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="c266e-151">Click **Keys** on hello left menu, and copy hello **PRIMARY ACCESS KEY**.</span></span>
    3. <span data-ttu-id="c266e-152">Klikněte na existující fond, toouse **fondy** v nabídce hello a zapište hello **ID** hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-152">toouse an existing pool, click **Pools** on hello menu, and note down hello **ID** of hello pool.</span></span> <span data-ttu-id="c266e-153">Pokud nemáte existující fond, přesuňte toohello další krok.</span><span class="sxs-lookup"><span data-stu-id="c266e-153">If you don't have an existing pool, move toohello next step.</span></span>     
2. <span data-ttu-id="c266e-154">Vytvoření **fondu Azure Batch**.</span><span class="sxs-lookup"><span data-stu-id="c266e-154">Create an **Azure Batch pool**.</span></span>

   1. <span data-ttu-id="c266e-155">V hello [portál Azure](https://portal.azure.com), klikněte na tlačítko **Procházet** v levé nabídce text hello a klikněte na tlačítko **účty Batch**.</span><span class="sxs-lookup"><span data-stu-id="c266e-155">In hello [Azure portal](https://portal.azure.com), click **Browse** in hello left menu, and click **Batch Accounts**.</span></span>
   2. <span data-ttu-id="c266e-156">Vyberte vaše hello tooopen účet Azure Batch **účtu Batch** okno.</span><span class="sxs-lookup"><span data-stu-id="c266e-156">Select your Azure Batch account tooopen hello **Batch Account** blade.</span></span>
   3. <span data-ttu-id="c266e-157">Klikněte na tlačítko **fondy** dlaždici.</span><span class="sxs-lookup"><span data-stu-id="c266e-157">Click **Pools** tile.</span></span>
   4. <span data-ttu-id="c266e-158">V hello **fondy** okně klikněte na tlačítko Přidat na panelu nástrojů tooadd hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-158">In hello **Pools** blade, click Add button on hello toolbar tooadd a pool.</span></span>
      1. <span data-ttu-id="c266e-159">Zadejte ID fondu hello (ID fondu).</span><span class="sxs-lookup"><span data-stu-id="c266e-159">Enter an ID for hello pool (Pool ID).</span></span> <span data-ttu-id="c266e-160">Poznámka: hello **ID fondu hello**; je nutné při vytváření řešení Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-160">Note hello **ID of hello pool**; you need it when creating hello Data Factory solution.</span></span>
      2. <span data-ttu-id="c266e-161">Zadejte **Windows Server 2012 R2** pro nastavení hello řada operačního systému.</span><span class="sxs-lookup"><span data-stu-id="c266e-161">Specify **Windows Server 2012 R2** for hello Operating System Family setting.</span></span>
      3. <span data-ttu-id="c266e-162">Vyberte **cenovou úroveň uzlu**.</span><span class="sxs-lookup"><span data-stu-id="c266e-162">Select a **node pricing tier**.</span></span>
      4. <span data-ttu-id="c266e-163">Zadejte **2** jako hodnota pro hello **cíl vyhrazené** nastavení.</span><span class="sxs-lookup"><span data-stu-id="c266e-163">Enter **2** as value for hello **Target Dedicated** setting.</span></span>
      5. <span data-ttu-id="c266e-164">Zadejte **2** jako hodnota pro hello **maximální počet úloh na uzel** nastavení.</span><span class="sxs-lookup"><span data-stu-id="c266e-164">Enter **2** as value for hello **Max tasks per node** setting.</span></span>
   5. <span data-ttu-id="c266e-165">Klikněte na tlačítko **OK** toocreate hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-165">Click **OK** toocreate hello pool.</span></span>
   6. <span data-ttu-id="c266e-166">Zapište hello **ID** hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-166">Note down hello **ID** of hello pool.</span></span> 



### <a name="high-level-steps"></a><span data-ttu-id="c266e-167">Postup vysoké úrovně</span><span class="sxs-lookup"><span data-stu-id="c266e-167">High-level steps</span></span>
<span data-ttu-id="c266e-168">Zde jsou hello dva základní kroky, které můžete provádět v rámci tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="c266e-168">Here are hello two high-level steps you perform as part of this walkthrough:</span></span> 

1. <span data-ttu-id="c266e-169">Vytvořte vlastní aktivitu, která obsahuje jednoduché datové transformaci/zpracování logiky.</span><span class="sxs-lookup"><span data-stu-id="c266e-169">Create a custom activity that contains simple data transformation/processing logic.</span></span>
2. <span data-ttu-id="c266e-170">Vytvořte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-170">Create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

### <a name="create-a-custom-activity"></a><span data-ttu-id="c266e-171">Vytvoření vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-171">Create a custom activity</span></span>
<span data-ttu-id="c266e-172">Vytvoření toocreate .NET vlastní aktivitu, **knihovna tříd rozhraní .NET** projektu s třídou, která implementuje **IDotNetActivity** rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c266e-172">toocreate a .NET custom activity, create a **.NET Class Library** project with a class that implements that **IDotNetActivity** interface.</span></span> <span data-ttu-id="c266e-173">Toto rozhraní obsahuje pouze jednu metodu: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) a jeho podpis je:</span><span class="sxs-lookup"><span data-stu-id="c266e-173">This interface has only one method: [Execute](https://msdn.microsoft.com/library/azure/mt603945.aspx) and its signature is:</span></span>

```csharp
public IDictionary<string, string> Execute(
        IEnumerable<LinkedService> linkedServices,
        IEnumerable<Dataset> datasets,
        Activity activity,
        IActivityLogger logger)
```


<span data-ttu-id="c266e-174">Metoda Hello přijímá čtyř parametrů:</span><span class="sxs-lookup"><span data-stu-id="c266e-174">hello method takes four parameters:</span></span>

- <span data-ttu-id="c266e-175">**linkedServices**.</span><span class="sxs-lookup"><span data-stu-id="c266e-175">**linkedServices**.</span></span> <span data-ttu-id="c266e-176">Tato vlastnost je vyčíslitelná seznam úložiště dat propojené služby odkazuje vstupní a výstupní datové sady pro aktivitu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-176">This property is an enumerable list of Data Store linked services referenced by input/output datasets for hello activity.</span></span>   
- <span data-ttu-id="c266e-177">**datové sady**.</span><span class="sxs-lookup"><span data-stu-id="c266e-177">**datasets**.</span></span> <span data-ttu-id="c266e-178">Tato vlastnost je vyčíslitelná seznam vstupní a výstupní datové sady pro aktivitu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-178">This property is an enumerable list of input/output datasets for hello activity.</span></span> <span data-ttu-id="c266e-179">Můžete použít tento parametr tooget hello umístění a schémata definované vstupní a výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="c266e-179">You can use this parameter tooget hello locations and schemas defined by input and output datasets.</span></span>
- <span data-ttu-id="c266e-180">**aktivita**.</span><span class="sxs-lookup"><span data-stu-id="c266e-180">**activity**.</span></span> <span data-ttu-id="c266e-181">Tato vlastnost představuje hello aktuální aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-181">This property represents hello current activity.</span></span> <span data-ttu-id="c266e-182">Dá se použít tooaccess rozšířené vlastnosti související s hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-182">It can be used tooaccess extended properties associated with hello custom activity.</span></span> <span data-ttu-id="c266e-183">V tématu [přístup rozšířené vlastnosti](#access-extended-properties) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c266e-183">See [Access extended properties](#access-extended-properties) for details.</span></span>
- <span data-ttu-id="c266e-184">**protokolovač**.</span><span class="sxs-lookup"><span data-stu-id="c266e-184">**logger**.</span></span> <span data-ttu-id="c266e-185">Tento objekt umožňuje zapisovat ladění komentáře tento prostor v protokolu uživatele hello hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="c266e-185">This object lets you write debug comments that surface in hello user log for hello pipeline.</span></span>

<span data-ttu-id="c266e-186">Hello metoda vrátí slovník, který může být vlastní aktivity používané toochain společně v budoucnosti hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-186">hello method returns a dictionary that can be used toochain custom activities together in hello future.</span></span> <span data-ttu-id="c266e-187">Tato funkce není dosud implementována, takže vrátí prázdný slovník z metody hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-187">This feature is not implemented yet, so return an empty dictionary from hello method.</span></span>  

### <a name="procedure"></a><span data-ttu-id="c266e-188">Postup</span><span class="sxs-lookup"><span data-stu-id="c266e-188">Procedure</span></span>
1. <span data-ttu-id="c266e-189">Vytvoření **knihovna tříd rozhraní .NET** projektu.</span><span class="sxs-lookup"><span data-stu-id="c266e-189">Create a **.NET Class Library** project.</span></span>
   <ol type="a">
     <li><span data-ttu-id="c266e-190">Spusťte <b>Visual Studio 2017</b> nebo <b>sady Visual Studio 2015</b> nebo <b>Visual Studio 2013</b> nebo <b>sady Visual Studio 2012</b>.</span><span class="sxs-lookup"><span data-stu-id="c266e-190">Launch <b>Visual Studio 2017</b> or <b>Visual Studio 2015</b> or <b>Visual Studio 2013</b> or <b>Visual Studio 2012</b>.</span></span></li>
     <li><span data-ttu-id="c266e-191">Klikněte na tlačítko <b>soubor</b>, bod příliš<b>nový</b>a klikněte na tlačítko <b>projektu</b>.</span><span class="sxs-lookup"><span data-stu-id="c266e-191">Click <b>File</b>, point too<b>New</b>, and click <b>Project</b>.</span></span></li>
     <li><span data-ttu-id="c266e-192">Rozbalte <b>Šablony</b> a vyberte <b>Visual C#</b>.</span><span class="sxs-lookup"><span data-stu-id="c266e-192">Expand <b>Templates</b>, and select <b>Visual C#</b>.</span></span> <span data-ttu-id="c266e-193">V tomto návodu použijete C#, ale můžete použít libovolné .NET jazyk toodevelop hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-193">In this walkthrough, you use C#, but you can use any .NET language toodevelop hello custom activity.</span></span></li>
     <li><span data-ttu-id="c266e-194">Vyberte <b>knihovny tříd</b> hello seznamu typů projektu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="c266e-194">Select <b>Class Library</b> from hello list of project types on hello right.</span></span> <span data-ttu-id="c266e-195">V VS 2017, zvolte <b>knihovny tříd (rozhraní .NET Framework)</b> </span><span class="sxs-lookup"><span data-stu-id="c266e-195">In VS 2017, choose <b>Class Library (.NET Framework)</b> </span></span></li>
     <li><span data-ttu-id="c266e-196">Zadejte <b>MyDotNetActivity</b> pro hello <b>název</b>.</span><span class="sxs-lookup"><span data-stu-id="c266e-196">Enter <b>MyDotNetActivity</b> for hello <b>Name</b>.</span></span></li>
     <li><span data-ttu-id="c266e-197">Vyberte <b>C:\ADFGetStarted</b> pro hello <b>umístění</b>.</span><span class="sxs-lookup"><span data-stu-id="c266e-197">Select <b>C:\ADFGetStarted</b> for hello <b>Location</b>.</span></span></li>
     <li><span data-ttu-id="c266e-198">Klikněte na tlačítko <b>OK</b> toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="c266e-198">Click <b>OK</b> toocreate hello project.</span></span></li>
   </ol><span data-ttu-id="c266e-199">
2.Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="c266e-199">
2. Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
<span data-ttu-id="c266e-200">3.</span><span class="sxs-lookup"><span data-stu-id="c266e-200">3.</span></span> <span data-ttu-id="c266e-201">V hello Konzola správce balíčků, spustit následující příkaz tooimport hello **Microsoft.Azure.Management.DataFactories**.</span><span class="sxs-lookup"><span data-stu-id="c266e-201">In hello Package Manager Console, execute hello following command tooimport **Microsoft.Azure.Management.DataFactories**.</span></span>

    ```PowerShell
    Install-Package Microsoft.Azure.Management.DataFactories
    ```
4. <span data-ttu-id="c266e-202">Import hello **Azure Storage** balíček NuGet v toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="c266e-202">Import hello **Azure Storage** NuGet package in toohello project.</span></span>

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="c266e-203">Spouštěč služby Data Factory vyžaduje hello 4.3 verzi WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="c266e-203">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="c266e-204">Pokud přidáte odkaz tooa novější verze sestavení Azure Storage ve vašem projektu vlastní aktivitu, zobrazí chybu když hello aktivita provede.</span><span class="sxs-lookup"><span data-stu-id="c266e-204">If you add a reference tooa later version of Azure Storage assembly in your custom activity project, you see an error when hello activity executes.</span></span> <span data-ttu-id="c266e-205">Chyba hello tooresolve, najdete v části [izolace domény aplikace](#appdomain-isolation) části.</span><span class="sxs-lookup"><span data-stu-id="c266e-205">tooresolve hello error, see [Appdomain isolation](#appdomain-isolation) section.</span></span> 
5. <span data-ttu-id="c266e-206">Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-206">Add hello following **using** statements toohello source file in hello project.</span></span>

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
6. <span data-ttu-id="c266e-207">Změnit název hello hello **obor názvů** příliš**MyDotNetActivityNS**.</span><span class="sxs-lookup"><span data-stu-id="c266e-207">Change hello name of hello **namespace** too**MyDotNetActivityNS**.</span></span>

    ```csharp
    namespace MyDotNetActivityNS
    ```
7. <span data-ttu-id="c266e-208">Změňte název hello třídy hello příliš**MyDotNetActivity** a odvodí z hello **IDotNetActivity** rozhraní, jak je znázorněno v následujícím fragmentu kódu hello:</span><span class="sxs-lookup"><span data-stu-id="c266e-208">Change hello name of hello class too**MyDotNetActivity** and derive it from hello **IDotNetActivity** interface as shown in hello following code snippet:</span></span>

    ```csharp
    public class MyDotNetActivity : IDotNetActivity
    ```
8. <span data-ttu-id="c266e-209">Implementace (Přidat) hello **Execute** metoda hello **IDotNetActivity** rozhraní toohello **MyDotNetActivity** hello třídy a zkopírujte následující ukázka kódu toohello metoda.</span><span class="sxs-lookup"><span data-stu-id="c266e-209">Implement (Add) hello **Execute** method of hello **IDotNetActivity** interface toohello **MyDotNetActivity** class and copy hello following sample code toohello method.</span></span>

    <span data-ttu-id="c266e-210">Hello následující příklad vrátí hello počet výskytů hello hledaný termín ("Microsoft") v jednotlivých objektů blob přidružený datový řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-210">hello following sample counts hello number of occurrences of hello search term (“Microsoft”) in each blob associated with a data slice.</span></span>

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
        // get extended properties defined in activity JSON definition
        // (for example: SliceStart)
        DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
        string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];
    
        // toolog information, use hello logger object
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

        // get hello input dataset
        Dataset inputDataset = datasets.Single(dataset => dataset.Name == activity.Inputs.Single().Name);
    
        // declare variables toohold type properties of input/output datasets
        AzureBlobDataset inputTypeProperties, outputTypeProperties;
        
        // get type properties from hello dataset object
        inputTypeProperties = inputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // log linked services passed in linkedServices parameter
        // you will see two linked services of type: AzureStorage
        // one for input dataset and hello other for output dataset 
        foreach (LinkedService ls in linkedServices)
            logger.Write("linkedService.Name {0}", ls.Name);
    
        // get hello first Azure Storate linked service from linkedServices object
        // using First method instead of Single since we are using hello same
        // Azure Storage linked service for input and output.
        inputLinkedService = linkedServices.First(
            linkedService =>
            linkedService.Name ==
            inputDataset.Properties.LinkedServiceName).Properties.TypeProperties
            as AzureStorageLinkedService;
    
        // get hello connection string in hello linked service
        string connectionString = inputLinkedService.ConnectionString;
    
        // get hello folder path from hello input dataset definition
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
               // with hello data slice. definition of hello method is shown in hello next step.
    
            output = Calculate(blobList, logger, folderPath, ref continuationToken, "Microsoft");
    
        } while (continuationToken != null);
    
        // get hello output dataset using hello name of hello dataset matched tooa name in hello Activity output collection.
        Dataset outputDataset = datasets.Single(dataset => dataset.Name == activity.Outputs.Single().Name);

        // get type properties for hello output dataset
        outputTypeProperties = outputDataset.Properties.TypeProperties as AzureBlobDataset;
    
        // get hello folder path from hello output dataset definition
        folderPath = GetFolderPath(outputDataset);

        // log hello output folder path 
        logger.Write("Writing blob toohello folder: {0}", folderPath);
    
        // create a storage object for hello output blob.
        CloudStorageAccount outputStorageAccount = CloudStorageAccount.Parse(connectionString);
        // write hello name of hello file.
        Uri outputBlobUri = new Uri(outputStorageAccount.BlobEndpoint, folderPath + "/" + GetFileName(outputDataset));
    
        // log hello output file name
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
9. <span data-ttu-id="c266e-211">Přidejte následující metody helper hello:</span><span class="sxs-lookup"><span data-stu-id="c266e-211">Add hello following helper methods:</span></span> 

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

        // get type properties of hello dataset 
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello folder path found in hello type properties
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
    
        // get type properties of hello dataset
        AzureBlobDataset blobDataset = dataArtifact.Properties.TypeProperties as AzureBlobDataset;
        if (blobDataset == null)
        {
            return null;
        }
    
        // return hello blob/file name in hello type properties
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

    <span data-ttu-id="c266e-212">Hello GetFolderPath metoda vrátí hello cesta toohello složky, která hello datová sada odkazuje tooand hello GetFileName metoda vrátí hello název hello objektů blob nebo souboru, který hello body datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="c266e-212">hello GetFolderPath method returns hello path toohello folder that hello dataset points tooand hello GetFileName method returns hello name of hello blob/file that hello dataset points to.</span></span> <span data-ttu-id="c266e-213">Pokud jste havefolderPath definuje pomocí proměnných, například {Year}, {Month}, {Day} atd., hello metoda vrátí hello řetězec, jako je bez jejich nahrazení s hodnotami modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="c266e-213">If you havefolderPath defines using variables such as {Year}, {Month}, {Day} etc., hello method returns hello string as it is without replacing them with runtime values.</span></span> <span data-ttu-id="c266e-214">V tématu [přístup rozšířené vlastnosti](#access-extended-properties) část Podrobnosti o přístup k SliceStart, SliceEnd atd.</span><span class="sxs-lookup"><span data-stu-id="c266e-214">See [Access extended properties](#access-extended-properties) section for details on accessing SliceStart, SliceEnd, etc.</span></span>    

    ```JSON
    "name": "InputDataset",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "file.txt",
            "folderPath": "adftutorial/inputfolder/",
    ```

    <span data-ttu-id="c266e-215">Hello Calculate metoda vypočítá hello počet instancí – klíčové slovo Microsoft v hello vstupní soubory (objekty BLOB ve složce hello).</span><span class="sxs-lookup"><span data-stu-id="c266e-215">hello Calculate method calculates hello number of instances of keyword Microsoft in hello input files (blobs in hello folder).</span></span> <span data-ttu-id="c266e-216">hledaný termín Hello ("Microsoft") je pevně zakódovaná v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-216">hello search term (“Microsoft”) is hard-coded in hello code.</span></span>
10. <span data-ttu-id="c266e-217">Kompilace projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-217">Compile hello project.</span></span> <span data-ttu-id="c266e-218">Klikněte na tlačítko **sestavení** hello nabídky a klikněte na **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c266e-218">Click **Build** from hello menu and click **Build Solution**.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c266e-219">Nastavit 4.5.2 verzi rozhraní .NET Framework jako hello cílové rozhraní pro svůj projekt: klikněte pravým tlačítkem na projekt hello a klikněte na tlačítko **vlastnosti** tooset hello cílové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c266e-219">Set 4.5.2 version of .NET Framework as hello target framework for your project: right-click hello project, and click **Properties** tooset hello target framework.</span></span> <span data-ttu-id="c266e-220">Objekt pro vytváření dat nepodporuje vlastní aktivity, které jsou novější než 4.5.2 zkompilovat proti verze rozhraní .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c266e-220">Data Factory does not support custom activities compiled against .NET Framework versions later than 4.5.2.</span></span>

11. <span data-ttu-id="c266e-221">Spusťte **Průzkumníka Windows**a přejděte příliš**bin\debug** nebo **bin\release** složku v závislosti na typu hello sestavení.</span><span class="sxs-lookup"><span data-stu-id="c266e-221">Launch **Windows Explorer**, and navigate too**bin\debug** or **bin\release** folder depending on hello type of build.</span></span>
12. <span data-ttu-id="c266e-222">Vytvořte soubor zip **MyDotNetActivity.zip** obsahující všechny binární soubory hello v hello <project folder>\bin\Debug složka.</span><span class="sxs-lookup"><span data-stu-id="c266e-222">Create a zip file **MyDotNetActivity.zip** that contains all hello binaries in hello <project folder>\bin\Debug folder.</span></span> <span data-ttu-id="c266e-223">Zahrnout hello **MyDotNetActivity.pdb** tak, aby získat další podrobnosti, jako je například číslo řádku v hello zdrojového kódu, která způsobila problém hello, pokud došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="c266e-223">Include hello **MyDotNetActivity.pdb** file so that you get additional details such as line number in hello source code that caused hello issue if there was a failure.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="c266e-224">Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s žádné podsložek.</span><span class="sxs-lookup"><span data-stu-id="c266e-224">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>

    ![Binární výstupní soubory](./media/data-factory-use-custom-activities/Binaries.png)
14. <span data-ttu-id="c266e-226">Vytvořte kontejner objektů blob s názvem **customactivitycontainer** Pokud ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c266e-226">Create a blob container named **customactivitycontainer** if it does not already exist.</span></span> 
15. <span data-ttu-id="c266e-227">Nahrát MyDotNetActivity.zip jako customactivitycontainer toohello objektů blob v **pro obecné účely** úložiště objektů blob v Azure (úložiště objektů Blob není za provozu nebo nástrojů), který odkazuje AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="c266e-227">Upload MyDotNetActivity.zip as a blob toohello customactivitycontainer in a **general-purpose** Azure blob storage (not hot/cool Blob storage) that is referred by AzureStorageLinkedService.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="c266e-228">Pokud přidáte toto rozhraní .NET aktivity projektu tooa řešení v sadě Visual Studio, který obsahuje projekt Data Factory a přidat odkaz na too.NET aktivity projekt z projektu aplikace hello objekt pro vytváření dat, není nutné tooperform hello poslední dva kroky ručně vytváření soubor zip Hello a pak ho nahrát toohello úložiště objektů blob v Azure pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="c266e-228">If you add this .NET activity project tooa solution in Visual Studio that contains a Data Factory project, and add a reference too.NET activity project from hello Data Factory application project, you do not need tooperform hello last two steps of manually creating hello zip file and uploading it toohello general-purpose Azure blob storage.</span></span> <span data-ttu-id="c266e-229">Když publikujete entity služby Data Factory pomocí sady Visual Studio, tyto kroky automaticky provádí proces publikování hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-229">When you publish Data Factory entities using Visual Studio, these steps are automatically done by hello publishing process.</span></span> <span data-ttu-id="c266e-230">Další informace najdete v tématu [projekt Data Factory v sadě Visual Studio](#data-factory-project-in-visual-studio) části.</span><span class="sxs-lookup"><span data-stu-id="c266e-230">For more information, see [Data Factory project in Visual Studio](#data-factory-project-in-visual-studio) section.</span></span>

## <a name="create-a-pipeline-with-custom-activity"></a><span data-ttu-id="c266e-231">Vytvoření kanálu s vlastní aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-231">Create a pipeline with custom activity</span></span>
<span data-ttu-id="c266e-232">Vytvoření vlastní aktivity a nahrát soubor zip hello se binární soubory tooa kontejneru objektů blob v **pro obecné účely** účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-232">You have created a custom activity and uploaded hello zip file with binaries tooa blob container in a **general-purpose** Azure Storage Account.</span></span> <span data-ttu-id="c266e-233">V této části vytvoříte objekt pro vytváření dat Azure s kanál, který používá vlastní aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-233">In this section, you create an Azure data factory with a pipeline that uses hello custom activity.</span></span>

<span data-ttu-id="c266e-234">vstupní datové sady Hello vlastní aktivity hello představuje objekty BLOB (soubory) ve složce customactivityinput hello adftutorial kontejneru v úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-234">hello input dataset for hello custom activity represents blobs (files) in hello customactivityinput folder of adftutorial container in hello blob storage.</span></span> <span data-ttu-id="c266e-235">Hello výstupní datovou sadu aktivity hello představuje výstupní objekty BLOB ve složce customactivityoutput hello adftutorial kontejneru v úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-235">hello output dataset for hello activity represents output blobs in hello customactivityoutput folder of adftutorial container in hello blob storage.</span></span>

<span data-ttu-id="c266e-236">Vytvoření **soubor.txt** soubor s hello následující obsahu a nahrajte ho příliš**customactivityinput** složky hello **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c266e-236">Create **file.txt** file with hello following content and upload it too**customactivityinput** folder of hello **adftutorial** container.</span></span> <span data-ttu-id="c266e-237">Vytvoření kontejneru adftutorial hello, pokud ho ještě neexistuje.</span><span class="sxs-lookup"><span data-stu-id="c266e-237">Create hello adftutorial container if it does not exist already.</span></span> 

```
test custom activity Microsoft test custom activity Microsoft
```

<span data-ttu-id="c266e-238">vstupní složky Hello odpovídá tooa řez v Azure Data Factory i v případě, že složka hello obsahuje dvě nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="c266e-238">hello input folder corresponds tooa slice in Azure Data Factory even if hello folder has two or more files.</span></span> <span data-ttu-id="c266e-239">Při zpracování kanálu hello se každý řez iteruje hello vlastní aktivity všech objektů BLOB hello v hello vstupní složky pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-239">When each slice is processed by hello pipeline, hello custom activity iterates through all hello blobs in hello input folder for that slice.</span></span>

<span data-ttu-id="c266e-240">Uvidíte, že jednu výstupní soubor s ve složce adftutorial\customactivityoutput hello s jedním nebo více řádky (stejný jako počet objektů BLOB ve složce vstupní hello):</span><span class="sxs-lookup"><span data-stu-id="c266e-240">You see one output file with in hello adftutorial\customactivityoutput folder with one or more lines (same as number of blobs in hello input folder):</span></span>

```
2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
```


<span data-ttu-id="c266e-241">Zde jsou hello kroky, které můžete provést v této části:</span><span class="sxs-lookup"><span data-stu-id="c266e-241">Here are hello steps you perform in this section:</span></span>

1. <span data-ttu-id="c266e-242">Vytvoření **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="c266e-242">Create a **data factory**.</span></span>
2. <span data-ttu-id="c266e-243">Vytvoření **propojené služby** fondu Azure Batch hello virtuálních počítačů, na které hello vlastní aktivity spouští a hello Azure Storage, který obsahuje objekty BLOB vstupu a výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-243">Create **Linked services** for hello Azure Batch pool of VMs on which hello custom activity runs and hello Azure Storage that holds hello input/output blobs.</span></span>
3. <span data-ttu-id="c266e-244">Vytvoření vstupní a výstupní **datové sady** které představují vstupní a výstupní hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-244">Create input and output **datasets** that represent input and output of hello custom activity.</span></span>
4. <span data-ttu-id="c266e-245">Vytvoření **kanálu** používající hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-245">Create a **pipeline** that uses hello custom activity.</span></span>

> [!NOTE]
> <span data-ttu-id="c266e-246">Vytvoření hello **soubor.txt** a nahrajte ho tooa kontejner objektů blob, pokud jste tak již neučinili.</span><span class="sxs-lookup"><span data-stu-id="c266e-246">Create hello **file.txt** and upload it tooa blob container if you haven't already done so.</span></span> <span data-ttu-id="c266e-247">Najdete v pokynech v předcházející části hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-247">See instructions in hello preceding section.</span></span>   

### <a name="step-1-create-hello-data-factory"></a><span data-ttu-id="c266e-248">Krok 1: Vytvoření objektu pro vytváření dat hello</span><span class="sxs-lookup"><span data-stu-id="c266e-248">Step 1: Create hello data factory</span></span>
1. <span data-ttu-id="c266e-249">Po přihlášení toohello portálu Azure, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c266e-249">After logging in toohello Azure portal, do hello following steps:</span></span>
   1. <span data-ttu-id="c266e-250">Klikněte na tlačítko **nový** v levé nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-250">Click **NEW** on hello left menu.</span></span>
   2. <span data-ttu-id="c266e-251">Klikněte na tlačítko **Data + analýzy** v hello **nový** okno.</span><span class="sxs-lookup"><span data-stu-id="c266e-251">Click **Data + Analytics** in hello **New** blade.</span></span>
   3. <span data-ttu-id="c266e-252">Klikněte na tlačítko **Data Factory** na hello **analýzy dat** okno.</span><span class="sxs-lookup"><span data-stu-id="c266e-252">Click **Data Factory** on hello **Data analytics** blade.</span></span>
   
    ![Nové nabídky Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-menu.png)
2. <span data-ttu-id="c266e-254">V hello **nový objekt pro vytváření dat** okno, zadejte **CustomActivityFactory** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="c266e-254">In hello **New data factory** blade, enter **CustomActivityFactory** for hello Name.</span></span> <span data-ttu-id="c266e-255">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="c266e-255">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="c266e-256">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "CustomActivityFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například **yournameCustomActivityFactory**) a zkuste vytvořit znovu.</span><span class="sxs-lookup"><span data-stu-id="c266e-256">If you receive hello error: **Data factory name “CustomActivityFactory” is not available**, change hello name of hello data factory (for example, **yournameCustomActivityFactory**) and try creating again.</span></span>

    ![Nové okno Azure Data Factory](media/data-factory-use-custom-activities/new-azure-data-factory-blade.png)
3. <span data-ttu-id="c266e-258">Klikněte na tlačítko **název skupiny prostředků**a vyberte existující skupinu prostředků nebo vytvořte skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="c266e-258">Click **RESOURCE GROUP NAME**, and select an existing resource group or create a resource group.</span></span>
4. <span data-ttu-id="c266e-259">Ověřte, že používáte správné hello **předplatné** a **oblast** místo hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="c266e-259">Verify that you are using hello correct **subscription** and **region** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="c266e-260">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="c266e-260">Click **Create** on hello **New data factory** blade.</span></span>
6. <span data-ttu-id="c266e-261">Zobrazí hello objekt pro vytváření dat vytváří v hello **řídicí panel** z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-261">You see hello data factory being created in hello **Dashboard** of hello Azure portal.</span></span>
7. <span data-ttu-id="c266e-262">Po úspěšném vytvoření objektu pro vytváření dat hello, zobrazí okno hello objekt pro vytváření dat, se zobrazí hello obsah objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-262">After hello data factory has been created successfully, you see hello Data Factory blade, which shows you hello contents of hello data factory.</span></span>
    
    ![Okno Objekt pro vytváření dat](media/data-factory-use-custom-activities/data-factory-blade.png)

### <a name="step-2-create-linked-services"></a><span data-ttu-id="c266e-264">Krok 2: Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="c266e-264">Step 2: Create linked services</span></span>
<span data-ttu-id="c266e-265">Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-265">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="c266e-266">V tomto kroku propojíte svůj účet úložiště Azure a účet Azure Batch tooyour služby data factory.</span><span class="sxs-lookup"><span data-stu-id="c266e-266">In this step, you link your Azure Storage account and Azure Batch account tooyour data factory.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="c266e-267">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c266e-267">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="c266e-268">Klikněte na tlačítko hello **vytvořit a nasadit** na hello dlaždici **DATA FACTORY** okno pro **CustomActivityFactory**.</span><span class="sxs-lookup"><span data-stu-id="c266e-268">Click hello **Author and deploy** tile on hello **DATA FACTORY** blade for **CustomActivityFactory**.</span></span> <span data-ttu-id="c266e-269">Zobrazí hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c266e-269">You see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="c266e-270">Klikněte na tlačítko **nové datové úložiště** na panelu příkazů hello a zvolte **úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="c266e-270">Click **New data store** on hello command bar and choose **Azure storage**.</span></span> <span data-ttu-id="c266e-271">Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-271">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>
    
    ![Nové datové úložiště - Azure Storage](media/data-factory-use-custom-activities/new-data-store-menu.png)
3. <span data-ttu-id="c266e-273">Nahraďte `<accountname>` s názvem účtu úložiště Azure a `<accountkey>` s přístupovým klíčem hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-273">Replace `<accountname>` with name of your Azure storage account and `<accountkey>` with access key of hello Azure storage account.</span></span> <span data-ttu-id="c266e-274">toolearn tooget úložiště přístupu klíče najdete v tématu [zobrazení, kopírování a opětovné vytváření přístupových klíčů úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="c266e-274">toolearn how tooget your storage access key, see [View, copy and regenerate storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>

    ![Úložiště Azure líbilo služby](media/data-factory-use-custom-activities/azure-storage-linked-service.png)
4. <span data-ttu-id="c266e-276">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c266e-276">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

#### <a name="create-azure-batch-linked-service"></a><span data-ttu-id="c266e-277">Vytvoření služby Azure Batch propojené</span><span class="sxs-lookup"><span data-stu-id="c266e-277">Create Azure Batch linked service</span></span>
1. <span data-ttu-id="c266e-278">V editoru služby Data Factory hello, klikněte na **... Další** na panelu příkazů hello, klikněte na tlačítko **nový výpočet**a potom vyberte **Azure Batch** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-278">In hello Data Factory Editor, click **... More** on hello command bar, click **New compute**, and then select **Azure Batch** from hello menu.</span></span>

    ![Nový výpočet - Azure Batch](media/data-factory-use-custom-activities/new-azure-compute-batch.png)
2. <span data-ttu-id="c266e-280">Ujistěte se, hello následující skript JSON toohello změny:</span><span class="sxs-lookup"><span data-stu-id="c266e-280">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="c266e-281">Zadejte název účtu Azure Batch pro hello **accountName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c266e-281">Specify Azure Batch account name for hello **accountName** property.</span></span> <span data-ttu-id="c266e-282">Hello **URL** z hello **okně účtu Azure Batch** je ve formátu hello: `http://accountname.region.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c266e-282">hello **URL** from hello **Azure Batch account blade** is in hello following format: `http://accountname.region.batch.azure.com`.</span></span> <span data-ttu-id="c266e-283">Pro hello **batchUri** vlastnost hello JSON, bude nutné tooremove `accountname.` z adresy URL a použijte hello hello `accountname` pro hello `accountName` vlastnost JSON.</span><span class="sxs-lookup"><span data-stu-id="c266e-283">For hello **batchUri** property in hello JSON, you need tooremove `accountname.` from hello URL and use hello `accountname` for hello `accountName` JSON property.</span></span>
   2. <span data-ttu-id="c266e-284">Zadejte klíč účtu Azure Batch hello pro hello **accessKey** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c266e-284">Specify hello Azure Batch account key for hello **accessKey** property.</span></span>
   3. <span data-ttu-id="c266e-285">Zadejte název hello hello fondu, které jste vytvořili v rámci požadavků pro hello **poolName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c266e-285">Specify hello name of hello pool you created as part of prerequisites for hello **poolName** property.</span></span> <span data-ttu-id="c266e-286">Můžete také zadat hello ID fondu hello místo názvu hello hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-286">You can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>
   4. <span data-ttu-id="c266e-287">Zadejte identifikátor URI Azure Batch pro hello **batchUri** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c266e-287">Specify Azure Batch URI for hello **batchUri** property.</span></span> <span data-ttu-id="c266e-288">Příklad: `https://westus.batch.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="c266e-288">Example: `https://westus.batch.azure.com`.</span></span>  
   5. <span data-ttu-id="c266e-289">Zadejte hello **AzureStorageLinkedService** pro hello **linkedServiceName** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="c266e-289">Specify hello **AzureStorageLinkedService** for hello **linkedServiceName** property.</span></span>

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

       <span data-ttu-id="c266e-290">Pro hello **poolName** vlastnost, můžete také zadat hello ID fondu hello místo názvu hello hello fondu.</span><span class="sxs-lookup"><span data-stu-id="c266e-290">For hello **poolName** property, you can also specify hello ID of hello pool instead of hello name of hello pool.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="c266e-291">Hello služba Data Factory nepodporuje možnost na vyžádání pro Azure Batch, stejně jako pro HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-291">hello Data Factory service does not support an on-demand option for Azure Batch as it does for HDInsight.</span></span> <span data-ttu-id="c266e-292">Vlastní fondu Azure Batch můžete použít pouze v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-292">You can only use your own Azure Batch pool in an Azure data factory.</span></span>   
    

### <a name="step-3-create-datasets"></a><span data-ttu-id="c266e-293">Krok 3: Vytvoření datové sady</span><span class="sxs-lookup"><span data-stu-id="c266e-293">Step 3: Create datasets</span></span>
<span data-ttu-id="c266e-294">V tomto kroku vytvoříte datové sady toorepresent vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="c266e-294">In this step, you create datasets toorepresent input and output data.</span></span>

#### <a name="create-input-dataset"></a><span data-ttu-id="c266e-295">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="c266e-295">Create input dataset</span></span>
1. <span data-ttu-id="c266e-296">V hello **Editor** hello objekt pro vytváření dat, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-296">In hello **Editor** for hello Data Factory, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage** from hello drop-down menu.</span></span>
2. <span data-ttu-id="c266e-297">Nahraďte hello JSON v pravém podokně hello hello následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="c266e-297">Replace hello JSON in hello right pane with hello following JSON snippet:</span></span>

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

   <span data-ttu-id="c266e-298">Vytvoření kanálu dále v tomto návodu se časem spuštění: 2016-11-16T00:00:00Z a koncový čas: 2016-11-16T05:00:00Z.</span><span class="sxs-lookup"><span data-stu-id="c266e-298">You create a pipeline later in this walkthrough with start time: 2016-11-16T00:00:00Z and end time: 2016-11-16T05:00:00Z.</span></span> <span data-ttu-id="c266e-299">Je naplánované tooproduce dat každou hodinu, takže se pět řezy vstupní a výstupní (mezi **00**: -> 00:00 **05**: 00:00).</span><span class="sxs-lookup"><span data-stu-id="c266e-299">It is scheduled tooproduce data hourly, so there are five input/output slices (between **00**:00:00 -> **05**:00:00).</span></span>

   <span data-ttu-id="c266e-300">Hello **frekvence** a **interval** hello vstupní datové sady je nastavený příliš**hodinu** a **1**, což znamená, že hello vstupní řez je k dispozici každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="c266e-300">hello **frequency** and **interval** for hello input dataset is set too**Hour** and **1**, which means that hello input slice is available hourly.</span></span> <span data-ttu-id="c266e-301">V této ukázce je hello stejný soubor (soubor.txt) v hello intputfolder.</span><span class="sxs-lookup"><span data-stu-id="c266e-301">In this sample, it is hello same file (file.txt) in hello intputfolder.</span></span>

   <span data-ttu-id="c266e-302">Tady jsou hello času zahájení pro každý řez, která je reprezentována SliceStart systémové proměnné v hello výše fragmentu kódu JSON.</span><span class="sxs-lookup"><span data-stu-id="c266e-302">Here are hello start times for each slice, which is represented by SliceStart system variable in hello above JSON snippet.</span></span>
3. <span data-ttu-id="c266e-303">Klikněte na tlačítko **nasadit** hello toocreate panelu nástrojů a nasadit hello **InputDataset**.</span><span class="sxs-lookup"><span data-stu-id="c266e-303">Click **Deploy** on hello toolbar toocreate and deploy hello **InputDataset**.</span></span> <span data-ttu-id="c266e-304">Zkontrolujte, jestli hello **úspěšné vytvoření tabulky** zprávu na hello záhlaví hello Editor.</span><span class="sxs-lookup"><span data-stu-id="c266e-304">Confirm that you see hello **TABLE CREATED SUCCESSFULLY** message on hello title bar of hello Editor.</span></span>

#### <a name="create-an-output-dataset"></a><span data-ttu-id="c266e-305">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="c266e-305">Create an output dataset</span></span>
1. <span data-ttu-id="c266e-306">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a potom vyberte **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="c266e-306">In hello **Data Factory editor**, click **... More** on hello command bar, click **New dataset**, and then select **Azure Blob storage**.</span></span>
2. <span data-ttu-id="c266e-307">Nahraďte hello skript JSON v pravém podokně hello hello následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="c266e-307">Replace hello JSON script in hello right pane with hello following JSON script:</span></span>

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

     <span data-ttu-id="c266e-308">Výstupní umístění je **adftutorial/customactivityoutput/** a název výstupního souboru je rrrr MM-dd-HH.txt, kde je rrrr MM-dd HH hello rok, měsíc, datum a hodinu hello řez se vytváří.</span><span class="sxs-lookup"><span data-stu-id="c266e-308">Output location is **adftutorial/customactivityoutput/** and output file name is yyyy-MM-dd-HH.txt where yyyy-MM-dd-HH is hello year, month, date, and hour of hello slice being produced.</span></span> <span data-ttu-id="c266e-309">V tématu [referenční informace pro vývojáře] [ adf-developer-reference] podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c266e-309">See [Developer Reference][adf-developer-reference] for details.</span></span>

    <span data-ttu-id="c266e-310">Objekt blob nebo soubor výstupu se generuje pro každý vstupní řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-310">An output blob/file is generated for each input slice.</span></span> <span data-ttu-id="c266e-311">Zde je, jak je výstupní soubor s názvem pro každý řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-311">Here is how an output file is named for each slice.</span></span> <span data-ttu-id="c266e-312">Všechny soubory výstup hello vygenerují na jednu výstupní složky: **adftutorial\customactivityoutput**.</span><span class="sxs-lookup"><span data-stu-id="c266e-312">All hello output files are generated in one output folder: **adftutorial\customactivityoutput**.</span></span>

   | <span data-ttu-id="c266e-313">Řez</span><span class="sxs-lookup"><span data-stu-id="c266e-313">Slice</span></span> | <span data-ttu-id="c266e-314">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="c266e-314">Start time</span></span> | <span data-ttu-id="c266e-315">Výstupní soubor</span><span class="sxs-lookup"><span data-stu-id="c266e-315">Output file</span></span> |
   |:--- |:--- |:--- |
   | <span data-ttu-id="c266e-316">1</span><span class="sxs-lookup"><span data-stu-id="c266e-316">1</span></span> |<span data-ttu-id="c266e-317">2016-11-16T00:00:00</span><span class="sxs-lookup"><span data-stu-id="c266e-317">2016-11-16T00:00:00</span></span> |<span data-ttu-id="c266e-318">2016 11 16 00.txt</span><span class="sxs-lookup"><span data-stu-id="c266e-318">2016-11-16-00.txt</span></span> |
   | <span data-ttu-id="c266e-319">2</span><span class="sxs-lookup"><span data-stu-id="c266e-319">2</span></span> |<span data-ttu-id="c266e-320">2016-11-16T01:00:00</span><span class="sxs-lookup"><span data-stu-id="c266e-320">2016-11-16T01:00:00</span></span> |<span data-ttu-id="c266e-321">2016 11 16 01.txt</span><span class="sxs-lookup"><span data-stu-id="c266e-321">2016-11-16-01.txt</span></span> |
   | <span data-ttu-id="c266e-322">3</span><span class="sxs-lookup"><span data-stu-id="c266e-322">3</span></span> |<span data-ttu-id="c266e-323">2016-11-16T02:00:00</span><span class="sxs-lookup"><span data-stu-id="c266e-323">2016-11-16T02:00:00</span></span> |<span data-ttu-id="c266e-324">2016 11 16 02.txt</span><span class="sxs-lookup"><span data-stu-id="c266e-324">2016-11-16-02.txt</span></span> |
   | <span data-ttu-id="c266e-325">4</span><span class="sxs-lookup"><span data-stu-id="c266e-325">4</span></span> |<span data-ttu-id="c266e-326">2016-11-16T03:00:00</span><span class="sxs-lookup"><span data-stu-id="c266e-326">2016-11-16T03:00:00</span></span> |<span data-ttu-id="c266e-327">2016 11 16 03.txt</span><span class="sxs-lookup"><span data-stu-id="c266e-327">2016-11-16-03.txt</span></span> |
   | <span data-ttu-id="c266e-328">5</span><span class="sxs-lookup"><span data-stu-id="c266e-328">5</span></span> |<span data-ttu-id="c266e-329">2016-11-16T04:00:00</span><span class="sxs-lookup"><span data-stu-id="c266e-329">2016-11-16T04:00:00</span></span> |<span data-ttu-id="c266e-330">2016 11 16 04.txt</span><span class="sxs-lookup"><span data-stu-id="c266e-330">2016-11-16-04.txt</span></span> |

    <span data-ttu-id="c266e-331">Mějte na paměti, že všechny soubory hello ve vstupní složky jsou součástí řez s časy zahájení hello uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="c266e-331">Remember that all hello files in an input folder are part of a slice with hello start times mentioned above.</span></span> <span data-ttu-id="c266e-332">Při zpracování této řezu se vlastní aktivity hello prohledává každý soubor a vytvoří řádek v souboru výstup hello s hello počtu výskytů hledaný termín ("Microsoft").</span><span class="sxs-lookup"><span data-stu-id="c266e-332">When this slice is processed, hello custom activity scans through each file and produces a line in hello output file with hello number of occurrences of search term (“Microsoft”).</span></span> <span data-ttu-id="c266e-333">Pokud existují tři soubory v hello inputfolder, jsou tři řádky v hello výstupní soubor pro každou hodinové řez: 2016-11-16-00.txt 2016-11-16:01:00:00.txt atd.</span><span class="sxs-lookup"><span data-stu-id="c266e-333">If there are three files in hello inputfolder, there are three lines in hello output file for each hourly slice: 2016-11-16-00.txt, 2016-11-16:01:00:00.txt, etc.</span></span>
3. <span data-ttu-id="c266e-334">toodeploy hello **OutputDataset**, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-334">toodeploy hello **OutputDataset**, click **Deploy** on hello command bar.</span></span>

### <a name="create-and-run-a-pipeline-that-uses-hello-custom-activity"></a><span data-ttu-id="c266e-335">Vytvoření a spuštění kanálu, který používá vlastní aktivity hello</span><span class="sxs-lookup"><span data-stu-id="c266e-335">Create and run a pipeline that uses hello custom activity</span></span>
1. <span data-ttu-id="c266e-336">V editoru služby Data Factory hello, klikněte na **... Další**a potom vyberte **nový kanál** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-336">In hello Data Factory Editor, click **... More**, and then select **New pipeline** on hello command bar.</span></span> 
2. <span data-ttu-id="c266e-337">Nahraďte hello JSON v pravém podokně hello hello následující skript JSON:</span><span class="sxs-lookup"><span data-stu-id="c266e-337">Replace hello JSON in hello right pane with hello following JSON script:</span></span>

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

    <span data-ttu-id="c266e-338">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="c266e-338">Note hello following points:</span></span>

   * <span data-ttu-id="c266e-339">**Concurrency** je nastaven příliš**2** tak, aby dva řezy jsou zpracovávány paralelně 2 virtuální počítače ve fondu Azure Batch hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-339">**Concurrency** is set too**2** so that two slices are processed in parallel by 2 VMs in hello Azure Batch pool.</span></span>
   * <span data-ttu-id="c266e-340">V části aktivity hello je jedna aktivita a je typu: **DotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c266e-340">There is one activity in hello activities section and it is of type: **DotNetActivity**.</span></span>
   * <span data-ttu-id="c266e-341">**AssemblyName** nastavena toohello název hello knihovny DLL: **MyDotnetActivity.dll**.</span><span class="sxs-lookup"><span data-stu-id="c266e-341">**AssemblyName** is set toohello name of hello DLL: **MyDotnetActivity.dll**.</span></span>
   * <span data-ttu-id="c266e-342">**EntryPoint** je nastaven příliš**MyDotNetActivityNS.MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c266e-342">**EntryPoint** is set too**MyDotNetActivityNS.MyDotNetActivity**.</span></span>
   * <span data-ttu-id="c266e-343">**PackageLinkedService** je nastaven příliš**AzureStorageLinkedService** který odkazuje toohello úložiště objektů blob, který obsahuje soubor zip hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-343">**PackageLinkedService** is set too**AzureStorageLinkedService** that points toohello blob storage that contains hello custom activity zip file.</span></span> <span data-ttu-id="c266e-344">Pokud používáte různé účty Azure Storage pro vstupní a výstupní soubory a hello soubor zip vlastní aktivitu, vytvoříte jiné propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c266e-344">If you are using different Azure Storage accounts for input/output files and hello custom activity zip file, you create another Azure Storage linked service.</span></span> <span data-ttu-id="c266e-345">Tento článek předpokládá, že používáte hello stejný účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-345">This article assumes that you are using hello same Azure Storage account.</span></span>
   * <span data-ttu-id="c266e-346">**PackageFile** je nastaven příliš**customactivitycontainer/MyDotNetActivity.zip**.</span><span class="sxs-lookup"><span data-stu-id="c266e-346">**PackageFile** is set too**customactivitycontainer/MyDotNetActivity.zip**.</span></span> <span data-ttu-id="c266e-347">Je ve formátu hello: containerforthezip/nameofthezip.zip.</span><span class="sxs-lookup"><span data-stu-id="c266e-347">It is in hello format: containerforthezip/nameofthezip.zip.</span></span>
   * <span data-ttu-id="c266e-348">vlastní aktivita Hello přijímá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="c266e-348">hello custom activity takes **InputDataset** as input and **OutputDataset** as output.</span></span>
   * <span data-ttu-id="c266e-349">Vlastnost linkedServiceName Hello vlastní aktivity hello ukazuje toohello **AzureBatchLinkedService**, která informuje Azure Data Factory hello vlastní aktivity musí toorun na virtuálních počítačích Azure Batch.</span><span class="sxs-lookup"><span data-stu-id="c266e-349">hello linkedServiceName property of hello custom activity points toohello **AzureBatchLinkedService**, which tells Azure Data Factory that hello custom activity needs toorun on Azure Batch VMs.</span></span>
   * <span data-ttu-id="c266e-350">**isPaused** vlastnost je nastavena příliš**false** ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="c266e-350">**isPaused** property is set too**false** by default.</span></span> <span data-ttu-id="c266e-351">kanál Hello spustí hned v tomto příkladu, protože hello řezy spustit v posledních hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-351">hello pipeline runs immediately in this example because hello slices start in hello past.</span></span> <span data-ttu-id="c266e-352">Můžete nastavit tuto vlastnost tootrue toopause hello kanálu a nastavte ji zpět toofalse toorestart.</span><span class="sxs-lookup"><span data-stu-id="c266e-352">You can set this property tootrue toopause hello pipeline and set it back toofalse toorestart.</span></span>
   * <span data-ttu-id="c266e-353">Hello **spustit** čas a **end** časy jsou **pět** hodiny od sebe a řezy vytváří každou hodinu, takže pět řezy vytváří hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="c266e-353">hello **start** time and **end** times are **five** hours apart and slices are produced hourly, so five slices are produced by hello pipeline.</span></span>
3. <span data-ttu-id="c266e-354">toodeploy hello kanálu, klikněte na tlačítko **nasadit** na panelu příkazů hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-354">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="c266e-355">Monitorování kanálu hello</span><span class="sxs-lookup"><span data-stu-id="c266e-355">Monitor hello pipeline</span></span>
1. <span data-ttu-id="c266e-356">V okně Data Factory hello v hello portálu Azure, klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="c266e-356">In hello Data Factory blade in hello Azure portal, click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-use-custom-activities/DataFactoryBlade.png)
2. <span data-ttu-id="c266e-358">V hello zobrazení diagramu klikněte teď hello OutputDataset.</span><span class="sxs-lookup"><span data-stu-id="c266e-358">In hello Diagram View, now click hello OutputDataset.</span></span>

    ![Zobrazení diagramu](./media/data-factory-use-custom-activities/diagram.png)
3. <span data-ttu-id="c266e-360">Měli byste vidět, že hello pět Výstup řezy jsou ve stavu Připraveno hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-360">You should see that hello five output slices are in hello Ready state.</span></span> <span data-ttu-id="c266e-361">Pokud nejsou ve stavu Připraveno hello, nebyly byla vypracována ještě.</span><span class="sxs-lookup"><span data-stu-id="c266e-361">If they are not in hello Ready state, they haven't been produced yet.</span></span> 

   ![Výstup řezy](./media/data-factory-use-custom-activities/OutputSlices.png)
4. <span data-ttu-id="c266e-363">Ověřte, že jsou generovány hello výstupní soubory v úložišti objektů blob hello v hello **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="c266e-363">Verify that hello output files are generated in hello blob storage in hello **adftutorial** container.</span></span>

   ![výstup z vlastní aktivity][image-data-factory-ouput-from-custom-activity]
5. <span data-ttu-id="c266e-365">Pokud otevřete hello výstupního souboru, měli byste vidět, že hello výstup podobný toohello následující výstup:</span><span class="sxs-lookup"><span data-stu-id="c266e-365">If you open hello output file, you should see hello output similar toohello following output:</span></span>

    ```
    2 occurrences(s) of hello search term "Microsoft" were found in hello file inputfolder/2016-11-16-00/file.txt.
    ```
6. <span data-ttu-id="c266e-366">Použití hello [portál Azure] [ azure-preview-portal] nebo toomonitor rutin prostředí Azure PowerShell vaše služby data factory, kanálů a datové sady.</span><span class="sxs-lookup"><span data-stu-id="c266e-366">Use hello [Azure portal][azure-preview-portal] or Azure PowerShell cmdlets toomonitor your data factory, pipelines, and data sets.</span></span> <span data-ttu-id="c266e-367">Zobrazí se zprávy z hello **ActivityLogger** v kódu hello hello vlastní aktivity v protokolech hello (konkrétně uživatel 0.log), které si můžete stáhnout z portálu hello nebo pomocí rutin.</span><span class="sxs-lookup"><span data-stu-id="c266e-367">You can see messages from hello **ActivityLogger** in hello code for hello custom activity in hello logs (specifically user-0.log) that you can download from hello portal or using cmdlets.</span></span>

   ![stažení protokolů z vlastní aktivity][image-data-factory-download-logs-from-custom-activity]

<span data-ttu-id="c266e-369">V tématu [monitorování a Správa kanálů](data-factory-monitor-manage-pipelines.md) podrobné pokyny pro monitorování datových sad a kanálů.</span><span class="sxs-lookup"><span data-stu-id="c266e-369">See [Monitor and Manage Pipelines](data-factory-monitor-manage-pipelines.md) for detailed steps for monitoring datasets and pipelines.</span></span>      

## <a name="data-factory-project-in-visual-studio"></a><span data-ttu-id="c266e-370">Projekt data Factory v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c266e-370">Data Factory project in Visual Studio</span></span>  
<span data-ttu-id="c266e-371">Můžete vytvořit a publikovat entity služby Data Factory pomocí sady Visual Studio místo použití portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-371">You can create and publish Data Factory entities by using Visual Studio instead of using Azure portal.</span></span> <span data-ttu-id="c266e-372">Podrobné informace o vytváření a publikování entit služby Data Factory pomocí sady Visual Studio, najdete v části [sestavit svůj první kanál pomocí sady Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) a [kopírování dat z Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) články.</span><span class="sxs-lookup"><span data-stu-id="c266e-372">For detailed information about creating and publishing Data Factory entities by using Visual Studio, See [Build your first pipeline using Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) and [Copy data from Azure Blob tooAzure SQL](data-factory-copy-activity-tutorial-using-visual-studio.md) articles.</span></span>

<span data-ttu-id="c266e-373">Hello následující další kroky, pokud vytváříte projekt Data Factory v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c266e-373">Do hello following additional steps if you are creating Data Factory project in Visual Studio:</span></span>
 
1. <span data-ttu-id="c266e-374">Přidejte toohello projekt Data Factory hello řešení sady Visual Studio, které obsahuje hello vlastní aktivity projektu.</span><span class="sxs-lookup"><span data-stu-id="c266e-374">Add hello Data Factory project toohello Visual Studio solution that contains hello custom activity project.</span></span> 
2. <span data-ttu-id="c266e-375">Přidáte odkaz na toohello .NET aktivity projekt z projektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-375">Add a reference toohello .NET activity project from hello Data Factory project.</span></span> <span data-ttu-id="c266e-376">Klikněte pravým tlačítkem na projekt Data Factory, přejděte příliš**přidat**a potom klikněte na **odkaz**.</span><span class="sxs-lookup"><span data-stu-id="c266e-376">Right-click Data Factory project, point too**Add**, and then click **Reference**.</span></span> 
3. <span data-ttu-id="c266e-377">V hello **přidat odkaz na** dialogové okno, vyberte hello **MyDotNetActivity** projektu a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c266e-377">In hello **Add Reference** dialog box, select hello **MyDotNetActivity** project, and click **OK**.</span></span>
4. <span data-ttu-id="c266e-378">Sestavení a publikování řešení hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-378">Build and publish hello solution.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c266e-379">Když publikujete entity služby Data Factory, soubor zip je je automaticky vytvořen a je kontejner objektů blob nahrané toohello: customactivitycontainer.</span><span class="sxs-lookup"><span data-stu-id="c266e-379">When you publish Data Factory entities, a zip file is automatically created for you and is uploaded toohello blob container: customactivitycontainer.</span></span> <span data-ttu-id="c266e-380">Pokud kontejner objektů blob hello neexistuje, je vytvořeno automaticky příliš.</span><span class="sxs-lookup"><span data-stu-id="c266e-380">If hello blob container does not exist, it is automatically created too.</span></span>  


## <a name="data-factory-and-batch-integration"></a><span data-ttu-id="c266e-381">Integrace služby Data Factory a Batch</span><span class="sxs-lookup"><span data-stu-id="c266e-381">Data Factory and Batch integration</span></span>
<span data-ttu-id="c266e-382">Hello služba Data Factory vytvoří úlohu ve službě Azure Batch s názvem hello: **adf poolname: xxx úlohy**.</span><span class="sxs-lookup"><span data-stu-id="c266e-382">hello Data Factory service creates a job in Azure Batch with hello name: **adf-poolname: job-xxx**.</span></span> <span data-ttu-id="c266e-383">Klikněte na tlačítko **úlohy** hello levé nabídce.</span><span class="sxs-lookup"><span data-stu-id="c266e-383">Click **Jobs** from hello left menu.</span></span> 

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-jobs.png)

<span data-ttu-id="c266e-385">Úloha se vytvoří při každém spuštění aktivity řezu.</span><span class="sxs-lookup"><span data-stu-id="c266e-385">A task is created for each activity run of a slice.</span></span> <span data-ttu-id="c266e-386">Pokud existují pět toobe připraven řezy, které se zpracovat, jsou v této úloze vytvořit pět úloh.</span><span class="sxs-lookup"><span data-stu-id="c266e-386">If there are five slices ready toobe processed, five tasks are created in this job.</span></span> <span data-ttu-id="c266e-387">Pokud existují několika výpočetních uzlech v hello fondu Batch, dva nebo více řezů může běžet paralelně.</span><span class="sxs-lookup"><span data-stu-id="c266e-387">If there are multiple compute nodes in hello Batch pool, two or more slices can run in parallel.</span></span> <span data-ttu-id="c266e-388">Pokud hello maximální úkolů na výpočetní uzel je nastaven příliš > 1, můžete také máte více než jeden řez systémem hello stejné výpočty.</span><span class="sxs-lookup"><span data-stu-id="c266e-388">If hello maximum tasks per compute node is set too> 1, you can also have more than one slice running on hello same compute.</span></span>

![Azure Data Factory - úlohy Batch](media/data-factory-use-custom-activities/data-factory-batch-job-tasks.png)

<span data-ttu-id="c266e-390">Hello následující diagram znázorňuje hello vztah mezi Azure Data Factory a dávkové úlohy.</span><span class="sxs-lookup"><span data-stu-id="c266e-390">hello following diagram illustrates hello relationship between Azure Data Factory and Batch tasks.</span></span>

![Objekt pro vytváření dat & Batch](./media/data-factory-use-custom-activities/DataFactoryAndBatch.png)

## <a name="troubleshoot-failures"></a><span data-ttu-id="c266e-392">Řešení chyb</span><span class="sxs-lookup"><span data-stu-id="c266e-392">Troubleshoot failures</span></span>
<span data-ttu-id="c266e-393">Řešení potíží se skládá z několika základní techniky:</span><span class="sxs-lookup"><span data-stu-id="c266e-393">Troubleshooting consists of a few basic techniques:</span></span>

1. <span data-ttu-id="c266e-394">Pokud se zobrazí následující chyba hello, můžete pomocí úložiště objektů blob aktivní/nástrojů místo použití úložiště objektů blob v Azure pro obecné účely.</span><span class="sxs-lookup"><span data-stu-id="c266e-394">If you see hello following error, you may be using a Hot/Cool blob storage instead of using a general-purpose Azure blob storage.</span></span> <span data-ttu-id="c266e-395">Nahrát tooa soubor zip hello **účet úložiště pro obecné účely Azure**.</span><span class="sxs-lookup"><span data-stu-id="c266e-395">Upload hello zip file tooa **general-purpose Azure Storage Account**.</span></span> 
 
    ```
    Error in Activity: Job encountered scheduling error. Code: BlobDownloadMiscError Category: ServerError Message: Miscellaneous error encountered while downloading one of hello specified Azure Blob(s).
    ``` 
2. <span data-ttu-id="c266e-396">Pokud se zobrazí následující chyba hello, ověřte, že hello název třídy hello v hello CS odpovídá hello zadaný název souboru je pro hello **EntryPoint** vlastnost v kódu JSON kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-396">If you see hello following error, confirm that hello name of hello class in hello CS file matches hello name you specified for hello **EntryPoint** property in hello pipeline JSON.</span></span> <span data-ttu-id="c266e-397">V návodu hello je název třídy hello: MyDotNetActivity a hello EntryPoint v hello JSON je: MyDotNetActivityNS. **MyDotNetActivity**.</span><span class="sxs-lookup"><span data-stu-id="c266e-397">In hello walkthrough, name of hello class is: MyDotNetActivity, and hello EntryPoint in hello JSON is: MyDotNetActivityNS.**MyDotNetActivity**.</span></span>

    ```
    MyDotNetActivity assembly does not exist or doesn't implement hello type Microsoft.DataFactories.Runtime.IDotNetActivity properly
    ```

   <span data-ttu-id="c266e-398">Pokud se názvy hello shodují, potvrďte, že všechny binární soubory hello jsou v hello **kořenové složky** hello souboru zip.</span><span class="sxs-lookup"><span data-stu-id="c266e-398">If hello names do match, confirm that all hello binaries are in hello **root folder** of hello zip file.</span></span> <span data-ttu-id="c266e-399">To znamená když otevřete soubor zip hello, měli byste vidět všechny soubory hello hello kořenové složky, není ve všech podsložek.</span><span class="sxs-lookup"><span data-stu-id="c266e-399">That is, when you open hello zip file, you should see all hello files in hello root folder, not in any sub folders.</span></span>   
3. <span data-ttu-id="c266e-400">Pokud vstupní řez hello není nastaven příliš**připraven**, potvrďte správnost hello vstupní složky struktura a **soubor.txt** existuje ve vstupní složkách hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-400">If hello input slice is not set too**Ready**, confirm that hello input folder structure is correct and **file.txt** exists in hello input folders.</span></span>
3. <span data-ttu-id="c266e-401">V hello **Execute** metoda vlastní aktivity, použijte hello **IActivityLogger** toolog informace o objektu, který vám pomůže vyřešit problémy.</span><span class="sxs-lookup"><span data-stu-id="c266e-401">In hello **Execute** method of your custom activity, use hello **IActivityLogger** object toolog information that helps you troubleshoot issues.</span></span> <span data-ttu-id="c266e-402">zprávy Hello přihlášení zobrazí v souborech protokolů uživatele hello (jeden nebo více souborů s názvem: uživatel 0.log, 1.log uživatele, uživatel 2.log atd.).</span><span class="sxs-lookup"><span data-stu-id="c266e-402">hello logged messages show up in hello user log files (one or more files named: user-0.log, user-1.log, user-2.log, etc.).</span></span>

   <span data-ttu-id="c266e-403">V hello **OutputDataset** okně klikněte na tlačítko hello řez toosee hello **datový ŘEZ** okno pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-403">In hello **OutputDataset** blade, click hello slice toosee hello **DATA SLICE** blade for that slice.</span></span> <span data-ttu-id="c266e-404">Zobrazí **běh aktivit** pro tento řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-404">You see **activity runs** for that slice.</span></span> <span data-ttu-id="c266e-405">Měli byste vidět jednu aktivitu spustit pro řez hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-405">You should see one activity run for hello slice.</span></span> <span data-ttu-id="c266e-406">Pokud kliknete na tlačítko spustit v hello příkazového řádku, můžete spustit jinou aktivitu spustit pro hello stejné řez.</span><span class="sxs-lookup"><span data-stu-id="c266e-406">If you click Run in hello command bar, you can start another activity run for hello same slice.</span></span>

   <span data-ttu-id="c266e-407">Po kliknutí na tlačítko hello aktivity při spuštění, se zobrazí hello **podrobnosti o spuštění aktivit** okno se seznamem souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="c266e-407">When you click hello activity run, you see hello **ACTIVITY RUN DETAILS** blade with a list of log files.</span></span> <span data-ttu-id="c266e-408">Zobrazí zprávy zaznamenané v souboru user_0.log hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-408">You see logged messages in hello user_0.log file.</span></span> <span data-ttu-id="c266e-409">Pokud dojde k chybě, uvidíte tři běh aktivit protože hello počet opakování je nastavena too3 v kanálu nebo aktivity hello JSON.</span><span class="sxs-lookup"><span data-stu-id="c266e-409">When an error occurs, you see three activity runs because hello retry count is set too3 in hello pipeline/activity JSON.</span></span> <span data-ttu-id="c266e-410">Po kliknutí na tlačítko hello aktivity při spuštění, zobrazí hello soubory protokolů, abyste si prošli tootroubleshoot hello chyby.</span><span class="sxs-lookup"><span data-stu-id="c266e-410">When you click hello activity run, you see hello log files that you can review tootroubleshoot hello error.</span></span>

   <span data-ttu-id="c266e-411">V seznamu hello protokolových souborů, klikněte na tlačítko hello **uživatele 0.log**.</span><span class="sxs-lookup"><span data-stu-id="c266e-411">In hello list of log files, click hello **user-0.log**.</span></span> <span data-ttu-id="c266e-412">V pravém panelu hello jsou výsledky hello pomocí hello **IActivityLogger.Write** metoda.</span><span class="sxs-lookup"><span data-stu-id="c266e-412">In hello right panel are hello results of using hello **IActivityLogger.Write** method.</span></span> <span data-ttu-id="c266e-413">Pokud nevidíte všechny zprávy, zkontrolujte, zda máte další soubory protokolu s názvem: user_1.log, user_2.log atd. Kód hello nebyla úspěšná, jinak hodnota po hello posledního přihlášení zpráva.</span><span class="sxs-lookup"><span data-stu-id="c266e-413">If you don't see all messages, check if you have more log files named: user_1.log, user_2.log etc. Otherwise, hello code may have failed after hello last logged message.</span></span>

   <span data-ttu-id="c266e-414">Kromě toho zkontrolujte **systému 0.log** pro všechny chybové zprávy systému a výjimky.</span><span class="sxs-lookup"><span data-stu-id="c266e-414">In addition, check **system-0.log** for any system error messages and exceptions.</span></span>
4. <span data-ttu-id="c266e-415">Zahrnout hello **PDB** souborů v souboru zip hello tak, aby podrobnosti o chybě hello informace, jako **zásobník volání** když dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="c266e-415">Include hello **PDB** file in hello zip file so that hello error details have information such as **call stack** when an error occurs.</span></span>
5. <span data-ttu-id="c266e-416">Všechny soubory v souboru zip hello hello hello vlastní aktivity musí být v hello **top úroveň** s žádné podsložek.</span><span class="sxs-lookup"><span data-stu-id="c266e-416">All hello files in hello zip file for hello custom activity must be at hello **top level** with no sub folders.</span></span>
6. <span data-ttu-id="c266e-417">Ujistěte se, že hello **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer / MyDotNetActivity.zip), a **packageLinkedService** (by měla odkazovat toohello **pro obecné účely**úložiště objektů blob v Azure, která obsahuje soubor zip hello) jsou nastavené toocorrect hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c266e-417">Ensure that hello **assemblyName** (MyDotNetActivity.dll), **entryPoint**(MyDotNetActivityNS.MyDotNetActivity), **packageFile** (customactivitycontainer/MyDotNetActivity.zip), and **packageLinkedService** (should point toohello **general-purpose**Azure blob storage that contains hello zip file) are set toocorrect values.</span></span>
7. <span data-ttu-id="c266e-418">Pokud můžete opravit chyby a chcete řez hello tooreprocess, klikněte pravým tlačítkem na hello řez ve hello **OutputDataset** a klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c266e-418">If you fixed an error and want tooreprocess hello slice, right-click hello slice in hello **OutputDataset** blade and click **Run**.</span></span>
8. <span data-ttu-id="c266e-419">Pokud se zobrazí následující chyba hello, používáte hello Azure Storage balíčku verze > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="c266e-419">If you see hello following error, you are using hello Azure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="c266e-420">Spouštěč služby Data Factory vyžaduje hello 4.3 verzi WindowsAzure.Storage.</span><span class="sxs-lookup"><span data-stu-id="c266e-420">Data Factory service launcher requires hello 4.3 version of WindowsAzure.Storage.</span></span> <span data-ttu-id="c266e-421">V tématu [izolace domény aplikace](#appdomain-isolation) části pro řešení, pokud je nutné použít hello novější verze sestavení Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c266e-421">See [Appdomain isolation](#appdomain-isolation) section for a work-around if you must use hello later version of Azure Storage assembly.</span></span> 

    ```
    Error in Activity: Unknown error in module: System.Reflection.TargetInvocationException: Exception has been thrown by hello target of an invocation. ---> System.TypeLoadException: Could not load type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from assembly 'Microsoft.WindowsAzure.Storage, Version=4.3.0.0, Culture=neutral, 
    ```

    <span data-ttu-id="c266e-422">Pokud používáte hello 4.3.0 verzi balíčku Azure Storage, odeberte hello existující odkaz tooAzure úložiště balíčku verze > 4.3.0.</span><span class="sxs-lookup"><span data-stu-id="c266e-422">If you can use hello 4.3.0 version of Azure Storage package, remove hello existing reference tooAzure Storage package of version > 4.3.0.</span></span> <span data-ttu-id="c266e-423">Potom spusťte následující příkaz z konzoly Správce balíčků NuGet hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-423">Then, run hello following command from NuGet Package Manager Console.</span></span> 

    ```PowerShell
    Install-Package WindowsAzure.Storage -Version 4.3.0
    ```

    <span data-ttu-id="c266e-424">Sestavení projektu hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-424">Build hello project.</span></span> <span data-ttu-id="c266e-425">Odstraňte Azure.Storage sestavením verze > 4.3.0 ze složky bin\Debug hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-425">Delete Azure.Storage assembly of version > 4.3.0 from hello bin\Debug folder.</span></span> <span data-ttu-id="c266e-426">Vytvořte soubor zip s binární soubory a souboru PDB hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-426">Create a zip file with binaries and hello PDB file.</span></span> <span data-ttu-id="c266e-427">Nahraďte původní soubor zip hello touto v kontejneru objektů blob hello (customactivitycontainer).</span><span class="sxs-lookup"><span data-stu-id="c266e-427">Replace hello old zip file with this one in hello blob container (customactivitycontainer).</span></span> <span data-ttu-id="c266e-428">Spusťte znovu hello řezy, které se nezdařilo (klikněte pravým tlačítkem na řez a klikněte na tlačítko Spustit).</span><span class="sxs-lookup"><span data-stu-id="c266e-428">Rerun hello slices that failed (right-click slice, and click Run).</span></span>   
8. <span data-ttu-id="c266e-429">vlastní aktivity Hello nepoužívá hello **app.config** soubor ze svého balíčku.</span><span class="sxs-lookup"><span data-stu-id="c266e-429">hello custom activity does not use hello **app.config** file from your package.</span></span> <span data-ttu-id="c266e-430">Proto pokud váš kód čte libovolné púřipojovací řetězce z konfiguračního souboru hello, ale nefunguje za běhu.</span><span class="sxs-lookup"><span data-stu-id="c266e-430">Therefore, if your code reads any connection strings from hello configuration file, it does not work at runtime.</span></span> <span data-ttu-id="c266e-431">Hello osvědčený postup, při použití Azure Batch je toohold všech tajných klíčů v **Azure KeyVault**, používat služeb na základě certifikátu hlavní tooprotect hello **keyvault**a distribuovat certifikát hello tooAzure fondu služby Batch.</span><span class="sxs-lookup"><span data-stu-id="c266e-431">hello best practice when using Azure Batch is toohold any secrets in an **Azure KeyVault**, use a certificate-based service principal tooprotect hello **keyvault**, and distribute hello certificate tooAzure Batch pool.</span></span> <span data-ttu-id="c266e-432">Hello vlastní aktivity .NET pak k dispozici tajné klíče z hello KeyVault za běhu.</span><span class="sxs-lookup"><span data-stu-id="c266e-432">hello .NET custom activity then can access secrets from hello KeyVault at runtime.</span></span> <span data-ttu-id="c266e-433">Toto řešení je obecný řešení a můžete škálovat tooany typ tajný klíč, ne jenom připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="c266e-433">This solution is a generic solution and can scale tooany type of secret, not just connection string.</span></span>

   <span data-ttu-id="c266e-434">Je snazší alternativní řešení (ale není z hlediska): můžete vytvořit **propojená služba Azure SQL** s nastavení připojovacího řetězce, vytvořit datovou sadu, zda text hello používá propojené služby a řetězu hello datovou sadu jako fiktivní vstupní datové sady toohello vlastní aktivity .NET.</span><span class="sxs-lookup"><span data-stu-id="c266e-434">There is an easier workaround (but not a best practice): you can create an **Azure SQL linked service** with connection string settings, create a dataset that uses hello linked service, and chain hello dataset as a dummy input dataset toohello custom .NET activity.</span></span> <span data-ttu-id="c266e-435">Můžete pak přístup hello propojené služby připojovací řetězec v kódu hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-435">You can then access hello linked service's connection string in hello custom activity code.</span></span>  

## <a name="update-custom-activity"></a><span data-ttu-id="c266e-436">Aktualizovat vlastní aktivitu</span><span class="sxs-lookup"><span data-stu-id="c266e-436">Update custom activity</span></span>
<span data-ttu-id="c266e-437">Pokud aktualizujete hello kód hello vlastní aktivity, sestavte jej a nahrajte hello soubor zip, který obsahuje nové úložiště objektů blob toohello binární soubory.</span><span class="sxs-lookup"><span data-stu-id="c266e-437">If you update hello code for hello custom activity, build it, and upload hello zip file that contains new binaries toohello blob storage.</span></span>

## <a name="appdomain-isolation"></a><span data-ttu-id="c266e-438">Izolace domény aplikace</span><span class="sxs-lookup"><span data-stu-id="c266e-438">Appdomain isolation</span></span>
<span data-ttu-id="c266e-439">Najdete v části [mezi ukázka AppDomain](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) který ukazuje, jak toocreate vlastní aktivitu, která není omezené tooassembly verze používané Spouštěče hello objekt pro vytváření dat (Příklad: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x atd.).</span><span class="sxs-lookup"><span data-stu-id="c266e-439">See [Cross AppDomain Sample](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) that shows you how toocreate a custom activity that is not constrained tooassembly versions used by hello Data Factory launcher (example: WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, etc.).</span></span>

## <a name="access-extended-properties"></a><span data-ttu-id="c266e-440">Přístup rozšířené vlastnosti</span><span class="sxs-lookup"><span data-stu-id="c266e-440">Access extended properties</span></span>
<span data-ttu-id="c266e-441">V kódu JSON aktivity hello můžou deklarovat rozšířených vlastností, jak je znázorněno v hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="c266e-441">You can declare extended properties in hello activity JSON as shown in hello following sample:</span></span>

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


<span data-ttu-id="c266e-442">V příkladu hello existují dvě rozšířené vlastnosti: **SliceStart** a **DataFactoryName**.</span><span class="sxs-lookup"><span data-stu-id="c266e-442">In hello example, there are two extended properties: **SliceStart** and **DataFactoryName**.</span></span> <span data-ttu-id="c266e-443">Hodnota Hello SliceStart vychází hello SliceStart systémové proměnné.</span><span class="sxs-lookup"><span data-stu-id="c266e-443">hello value for SliceStart is based on hello SliceStart system variable.</span></span> <span data-ttu-id="c266e-444">V tématu [systémové proměnné](data-factory-functions-variables.md) seznam podporovaných systémové proměnné.</span><span class="sxs-lookup"><span data-stu-id="c266e-444">See [System Variables](data-factory-functions-variables.md) for a list of supported system variables.</span></span> <span data-ttu-id="c266e-445">Hodnota Hello DataFactoryName je pevně tooCustomActivityFactory.</span><span class="sxs-lookup"><span data-stu-id="c266e-445">hello value for DataFactoryName is hard-coded tooCustomActivityFactory.</span></span>

<span data-ttu-id="c266e-446">tooaccess tyto rozšířené vlastnosti v hello **Execute** metodu, pomocí kódu podobné toohello následující kód:</span><span class="sxs-lookup"><span data-stu-id="c266e-446">tooaccess these extended properties in hello **Execute** method, use code similar toohello following code:</span></span>

```csharp
// tooget extended properties (for example: SliceStart)
DotNetActivity dotNetActivity = (DotNetActivity)activity.TypeProperties;
string sliceStartString = dotNetActivity.ExtendedProperties["SliceStart"];

// toolog all extended properties                               
IDictionary<string, string> extendedProperties = dotNetActivity.ExtendedProperties;
logger.Write("Logging extended properties if any...");
foreach (KeyValuePair<string, string> entry in extendedProperties)
{
    logger.Write("<key:{0}> <value:{1}>", entry.Key, entry.Value);
}
```

## <a name="auto-scaling-of-azure-batch"></a><span data-ttu-id="c266e-447">Automatické škálování služby Azure Batch</span><span class="sxs-lookup"><span data-stu-id="c266e-447">Auto-scaling of Azure Batch</span></span>
<span data-ttu-id="c266e-448">Můžete také vytvořit fond Azure Batch s **škálování** funkce.</span><span class="sxs-lookup"><span data-stu-id="c266e-448">You can also create an Azure Batch pool with **autoscale** feature.</span></span> <span data-ttu-id="c266e-449">Můžete například vytvořit fondu azure batch s 0 vyhrazených virtuálních počítačích a vzorec škálování na základě hello počtu úkolů čekajících na zpracování.</span><span class="sxs-lookup"><span data-stu-id="c266e-449">For example, you could create an azure batch pool with 0 dedicated VMs and an autoscale formula based on hello number of pending tasks.</span></span> 

<span data-ttu-id="c266e-450">jednoduchý vzorec Hello dosahuje hello následující chování: při počátečním vytvoření fondu hello začíná 1 virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c266e-450">hello sample formula here achieves hello following behavior: When hello pool is initially created, it starts with 1 VM.</span></span> <span data-ttu-id="c266e-451">Metrika $PendingTasks definuje hello počet úloh v spuštěná + aktivní (zařazených do fronty) stavu.</span><span class="sxs-lookup"><span data-stu-id="c266e-451">$PendingTasks metric defines hello number of tasks in running + active (queued) state.</span></span>  <span data-ttu-id="c266e-452">Vzorec Hello najde hello průměrný počet úkolů čekajících na zpracování v hello posledních 180 sekund a nastaví TargetDedicated odpovídajícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="c266e-452">hello formula finds hello average number of pending tasks in hello last 180 seconds and sets TargetDedicated accordingly.</span></span> <span data-ttu-id="c266e-453">Zajišťuje, že TargetDedicated nikdy překročí 25 virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c266e-453">It ensures that TargetDedicated never goes beyond 25 VMs.</span></span> <span data-ttu-id="c266e-454">Tak, jako jsou odeslány nové úkoly, fondu automaticky zvětšování a jako dokončení úkolů, virtuální počítače stane volné jeden po druhém a automatické škálování hello zmenšuje těchto virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c266e-454">So, as new tasks are submitted, pool automatically grows and as tasks complete, VMs become free one by one and hello autoscaling shrinks those VMs.</span></span> <span data-ttu-id="c266e-455">startingNumberOfVMs a maxNumberofVMs může být upravenou tooyour potřebám.</span><span class="sxs-lookup"><span data-stu-id="c266e-455">startingNumberOfVMs and maxNumberofVMs can be adjusted tooyour needs.</span></span>

<span data-ttu-id="c266e-456">Vzorec škálování:</span><span class="sxs-lookup"><span data-stu-id="c266e-456">Autoscale formula:</span></span>

``` 
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs,pendingTaskSamples);
```

<span data-ttu-id="c266e-457">V tématu [automatické škálování výpočetních uzlů ve fondu Azure Batch](../batch/batch-automatic-scaling.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c266e-457">See [Automatically scale compute nodes in an Azure Batch pool](../batch/batch-automatic-scaling.md) for details.</span></span>

<span data-ttu-id="c266e-458">Pokud fond hello používá výchozí hello [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello služba Batch může trvat 15 až 30 minut tooprepare hello virtuálních počítačů před spuštěním hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-458">If hello pool is using hello default [autoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/dn820173.aspx), hello Batch service could take 15-30 minutes tooprepare hello VM before running hello custom activity.</span></span>  <span data-ttu-id="c266e-459">Pokud fond hello používá jiný autoScaleEvaluationInterval, může trvat hello služba Batch autoScaleEvaluationInterval + 10 minut.</span><span class="sxs-lookup"><span data-stu-id="c266e-459">If hello pool is using a different autoScaleEvaluationInterval, hello Batch service could take autoScaleEvaluationInterval + 10 minutes.</span></span>

## <a name="use-hdinsight-compute-service"></a><span data-ttu-id="c266e-460">Pomocí HDInsight výpočetní služby</span><span class="sxs-lookup"><span data-stu-id="c266e-460">Use HDInsight compute service</span></span>
<span data-ttu-id="c266e-461">V Průvodci hello používá Azure Batch výpočetní toorun hello vlastní aktivity.</span><span class="sxs-lookup"><span data-stu-id="c266e-461">In hello walkthrough, you used Azure Batch compute toorun hello custom activity.</span></span> <span data-ttu-id="c266e-462">Můžete také použít vlastní cluster HDInsight se systémem Windows nebo mít objekt pro vytváření dat vytvořit na vyžádání clusteru HDInsight se systémem Windows a mít hello vlastní aktivity při spuštění v clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-462">You can also use your own Windows-based HDInsight cluster or have Data Factory create an on-demand Windows-based HDInsight cluster and have hello custom activity run on hello HDInsight cluster.</span></span> <span data-ttu-id="c266e-463">Tady jsou hlavní kroky hello pomocí clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-463">Here are hello high-level steps for using an HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c266e-464">vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c266e-464">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c266e-465">Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c266e-465">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c266e-466">Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-466">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>
 

1. <span data-ttu-id="c266e-467">Vytvoření služby Azure HDInsight propojený.</span><span class="sxs-lookup"><span data-stu-id="c266e-467">Create an Azure HDInsight linked service.</span></span>   
2. <span data-ttu-id="c266e-468">Použití HDInsight propojená služba místě **AzureBatchLinkedService** v hello kanálu JSON.</span><span class="sxs-lookup"><span data-stu-id="c266e-468">Use HDInsight linked service in place of **AzureBatchLinkedService** in hello pipeline JSON.</span></span>

<span data-ttu-id="c266e-469">Pokud chcete, aby tootest její hello návod, změna **spustit** a **end** časy pro kanál hello tak, aby hello scénář s hello služby Azure HDInsight můžete otestovat.</span><span class="sxs-lookup"><span data-stu-id="c266e-469">If you want tootest it with hello walkthrough, change **start** and **end** times for hello pipeline so that you can test hello scenario with hello Azure HDInsight service.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="c266e-470">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="c266e-470">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="c266e-471">Hello služby Azure Data Factory podporuje vytváření clusteru na vyžádání a použít ho tooprocess vstupní tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="c266e-471">hello Azure Data Factory service supports creation of an on-demand cluster and use it tooprocess input tooproduce output data.</span></span> <span data-ttu-id="c266e-472">Můžete také vlastní cluster tooperform hello stejné.</span><span class="sxs-lookup"><span data-stu-id="c266e-472">You can also use your own cluster tooperform hello same.</span></span> <span data-ttu-id="c266e-473">Pokud používáte cluster HDInsight na vyžádání, získá pro každý řez vytvořit cluster.</span><span class="sxs-lookup"><span data-stu-id="c266e-473">When you use on-demand HDInsight cluster, a cluster gets created for each slice.</span></span> <span data-ttu-id="c266e-474">Vzhledem k tomu, pokud chcete použít vlastní cluster HDInsight, hello cluster je připraven hello tooprocess řez okamžitě.</span><span class="sxs-lookup"><span data-stu-id="c266e-474">Whereas, if you use your own HDInsight cluster, hello cluster is ready tooprocess hello slice immediately.</span></span> <span data-ttu-id="c266e-475">Proto když používáte cluster na vyžádání, nemusíte vidět hello výstupní data co když použijete vlastní cluster.</span><span class="sxs-lookup"><span data-stu-id="c266e-475">Therefore, when you use on-demand cluster, you may not see hello output data as quickly as when you use your own cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="c266e-476">V době běhu instance aktivity .NET funguje pouze na jeden uzel pracovního procesu v clusteru HDInsight hello; nelze ji škálovat toorun ve více uzlech.</span><span class="sxs-lookup"><span data-stu-id="c266e-476">At runtime, an instance of a .NET activity runs only on one worker node in hello HDInsight cluster; it cannot be scaled toorun on multiple nodes.</span></span> <span data-ttu-id="c266e-477">Více instancí .NET aktivity může běžet paralelně na různých uzlech clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-477">Multiple instances of .NET activity can run in parallel on different nodes of hello HDInsight cluster.</span></span>
>
>

##### <a name="toouse-an-on-demand-hdinsight-cluster"></a><span data-ttu-id="c266e-478">toouse clusteru HDInsight na vyžádání</span><span class="sxs-lookup"><span data-stu-id="c266e-478">toouse an on-demand HDInsight cluster</span></span>
1. <span data-ttu-id="c266e-479">V hello **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** domovské stránce objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-479">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="c266e-480">V editoru služby Data Factory hello, klikněte na **nový výpočet** z příkazu hello panelu a vyberte možnost **clusteru HDInsight na vyžádání** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-480">In hello Data Factory Editor, click **New compute** from hello command bar and select **On-demand HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="c266e-481">Ujistěte se, hello následující skript JSON toohello změny:</span><span class="sxs-lookup"><span data-stu-id="c266e-481">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="c266e-482">Pro hello **parametr clusterSize** vlastnost, zadejte velikost hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-482">For hello **clusterSize** property, specify hello size of hello HDInsight cluster.</span></span>
   2. <span data-ttu-id="c266e-483">Pro hello **timeToLive** vlastnost, zadejte, jak dlouho hello zákazníka může být nečinnosti, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="c266e-483">For hello **timeToLive** property, specify how long hello customer can be idle before it is deleted.</span></span>
   3. <span data-ttu-id="c266e-484">Pro hello **verze** vlastnost, zadejte verzi HDInsight hello chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="c266e-484">For hello **version** property, specify hello HDInsight version you want toouse.</span></span> <span data-ttu-id="c266e-485">Pokud vyloučíte tuto vlastnost, použije se nejnovější verze hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-485">If you exclude this property, hello latest version is used.</span></span>  
   4. <span data-ttu-id="c266e-486">Pro hello **linkedServiceName**, zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c266e-486">For hello **linkedServiceName**, specify **AzureStorageLinkedService**.</span></span>

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
    > <span data-ttu-id="c266e-487">vlastní aktivity .NET Hello spustit pouze na clusterech HDInsight se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="c266e-487">hello custom .NET activities run only on Windows-based HDInsight clusters.</span></span> <span data-ttu-id="c266e-488">Alternativní řešení pro toto omezení je toouse hello mapy snížit aktivity toorun vlastní Java kód na clusteru HDInsight se systémem Linux.</span><span class="sxs-lookup"><span data-stu-id="c266e-488">A workaround for this limitation is toouse hello Map Reduce Activity toorun custom Java code on a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="c266e-489">Další možností je toouse fondu Azure Batch virtuální počítače toorun vlastních aktivit místo použití clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-489">Another option is toouse an Azure Batch pool of VMs toorun custom activities instead of using a HDInsight cluster.</span></span>

4. <span data-ttu-id="c266e-490">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c266e-490">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

##### <a name="toouse-your-own-hdinsight-cluster"></a><span data-ttu-id="c266e-491">toouse vlastní cluster HDInsight:</span><span class="sxs-lookup"><span data-stu-id="c266e-491">toouse your own HDInsight cluster:</span></span>
1. <span data-ttu-id="c266e-492">V hello **portál Azure**, klikněte na tlačítko **vytvořit a nasadit** domovské stránce objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-492">In hello **Azure portal**, click **Author and Deploy** in hello Data Factory home page.</span></span>
2. <span data-ttu-id="c266e-493">V hello **editoru služby Data Factory**, klikněte na tlačítko **nový výpočet** z příkazu hello panelu a vyberte možnost **clusteru HDInsight** nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="c266e-493">In hello **Data Factory Editor**, click **New compute** from hello command bar and select **HDInsight cluster** from hello menu.</span></span>
3. <span data-ttu-id="c266e-494">Ujistěte se, hello následující skript JSON toohello změny:</span><span class="sxs-lookup"><span data-stu-id="c266e-494">Make hello following changes toohello JSON script:</span></span>

   1. <span data-ttu-id="c266e-495">Pro hello **clusterUri** vlastnost, zadejte adresu URL hello pro vaše prostředí HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-495">For hello **clusterUri** property, enter hello URL for your HDInsight.</span></span> <span data-ttu-id="c266e-496">Příklad: https://<clustername>.azurehdinsight.net/</span><span class="sxs-lookup"><span data-stu-id="c266e-496">For example: https://<clustername>.azurehdinsight.net/</span></span>     
   2. <span data-ttu-id="c266e-497">Pro hello **uživatelské jméno** vlastnost, zadejte uživatelské jméno hello, který má cluster HDInsight toohello přístup.</span><span class="sxs-lookup"><span data-stu-id="c266e-497">For hello **UserName** property, enter hello user name who has access toohello HDInsight cluster.</span></span>
   3. <span data-ttu-id="c266e-498">Pro hello **heslo** vlastnost, zadejte heslo hello hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="c266e-498">For hello **Password** property, enter hello password for hello user.</span></span>
   4. <span data-ttu-id="c266e-499">Pro hello **LinkedServiceName** vlastnost, zadejte **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="c266e-499">For hello **LinkedServiceName** property, enter **AzureStorageLinkedService**.</span></span>
4. <span data-ttu-id="c266e-500">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="c266e-500">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

<span data-ttu-id="c266e-501">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c266e-501">See [Compute linked services](data-factory-compute-linked-services.md) for details.</span></span>

<span data-ttu-id="c266e-502">V hello **kanálu JSON**, použití HDInsight (na vyžádání nebo vlastní) propojené služby:</span><span class="sxs-lookup"><span data-stu-id="c266e-502">In hello **pipeline JSON**, use HDInsight (on-demand or your own) linked service:</span></span>

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

## <a name="create-a-custom-activity-by-using-net-sdk"></a><span data-ttu-id="c266e-503">Vytvoření vlastní aktivity pomocí .NET SDK</span><span class="sxs-lookup"><span data-stu-id="c266e-503">Create a custom activity by using .NET SDK</span></span>
<span data-ttu-id="c266e-504">V Průvodci hello v tomto článku vytvořit objekt pro vytváření dat kanál, který používá vlastní aktivity hello pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c266e-504">In hello walkthrough in this article, you create a data factory with a pipeline that uses hello custom activity by using hello Azure portal.</span></span> <span data-ttu-id="c266e-505">Hello následující kód ukazuje, jak toocreate hello objekt pro vytváření dat pomocí .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="c266e-505">hello following code shows you how toocreate hello data factory by using .NET SDK instead.</span></span> <span data-ttu-id="c266e-506">Můžete najít další podrobnosti o použití sady SDK tooprogrammatically vytvořit kanály v hello [vytvoření kanálu s aktivitou kopírování pomocí rozhraní API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md) článku.</span><span class="sxs-lookup"><span data-stu-id="c266e-506">You can find more details about using SDK tooprogrammatically create pipelines in hello [create a pipeline with copy activity by using .NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md) article.</span></span> 

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

            // TODO: replace ADFTutorialResourceGroup with hello name of your resource group.
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

                            // Initial value for pipeline's active period. With this, you won't need tooset slice status
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

            throw new InvalidOperationException("Failed tooacquire token");
        }
    }
}
```

## <a name="debug-custom-activity-in-visual-studio"></a><span data-ttu-id="c266e-507">Ladění vlastní aktivity v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c266e-507">Debug custom activity in Visual Studio</span></span>
<span data-ttu-id="c266e-508">Hello [Azure Data Factory - místní prostředí](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) ukázce na Githubu zahrnuje nástroj, který vám umožní toodebug vlastní .NET aktivity v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c266e-508">hello [Azure Data Factory - local environment](https://github.com/gbrueckl/Azure.DataFactory.LocalEnvironment) sample on GitHub includes a tool that allows you toodebug custom .NET activities within Visual Studio.</span></span>  


## <a name="sample-custom-activities-on-github"></a><span data-ttu-id="c266e-509">Ukázka vlastních aktivit na Githubu</span><span class="sxs-lookup"><span data-stu-id="c266e-509">Sample custom activities on GitHub</span></span>
| <span data-ttu-id="c266e-510">Ukázka</span><span class="sxs-lookup"><span data-stu-id="c266e-510">Sample</span></span> | <span data-ttu-id="c266e-511">Jaké vlastní aktivita provede</span><span class="sxs-lookup"><span data-stu-id="c266e-511">What custom activity does</span></span> |
| --- | --- |
| <span data-ttu-id="c266e-512">[Stažení programu HTTP Data](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span><span class="sxs-lookup"><span data-stu-id="c266e-512">[HTTP Data Downloader](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample).</span></span> |<span data-ttu-id="c266e-513">Stáhne data koncový bod HTTP tooAzure Blob Storage pomocí vlastní C# aktivitu v datové továrně.</span><span class="sxs-lookup"><span data-stu-id="c266e-513">Downloads data from an HTTP Endpoint tooAzure Blob Storage using custom C# Activity in Data Factory.</span></span> |
| [<span data-ttu-id="c266e-514">Ukázka postojích Analysis služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="c266e-514">Twitter Sentiment Analysis sample</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |<span data-ttu-id="c266e-515">Vyvolá modelu Azure ML a proveďte analýzu postojích, vyhodnocování, předpovědi atd.</span><span class="sxs-lookup"><span data-stu-id="c266e-515">Invokes an Azure ML model and do sentiment analysis, scoring, prediction etc.</span></span> |
| <span data-ttu-id="c266e-516">[Spustit skript jazyka R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span><span class="sxs-lookup"><span data-stu-id="c266e-516">[Run R Script](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).</span></span> |<span data-ttu-id="c266e-517">Vyvolá skript jazyka R spuštěním RScript.exe, který již má nainstalované R na clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c266e-517">Invokes R script by running RScript.exe on your HDInsight cluster that already has R Installed on it.</span></span> |
| [<span data-ttu-id="c266e-518">Mezi domény aplikace .NET aktivity</span><span class="sxs-lookup"><span data-stu-id="c266e-518">Cross AppDomain .NET Activity</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |<span data-ttu-id="c266e-519">Používá jiný sestavení verze z těch, které jsou používané Spouštěče hello objekt pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="c266e-519">Uses different assembly versions from ones used by hello Data Factory launcher</span></span> |
| [<span data-ttu-id="c266e-520">Zpracování modelu v Azure Analysis Services</span><span class="sxs-lookup"><span data-stu-id="c266e-520">Reprocess a model in Azure Analysis Services</span></span>](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/AzureAnalysisServicesProcessSample) |  <span data-ttu-id="c266e-521">Znovu zpracuje model v Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="c266e-521">Reprocesses a model in Azure Analysis Services.</span></span> |

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
