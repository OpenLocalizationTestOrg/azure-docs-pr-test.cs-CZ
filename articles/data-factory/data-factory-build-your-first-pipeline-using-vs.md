---
title: "Sestavení prvního objektu pro vytváření dat (Visual Studio) | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte pomocí sady Visual Studio ukázkový kanál služby Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 77042219cbe698a33ab9447aa952586772897241
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a><span data-ttu-id="eea4a-103">Kurz: Vytvoření datové továrny pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eea4a-103">Tutorial: Create a data factory by using Visual Studio</span></span>
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [<span data-ttu-id="eea4a-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="eea4a-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="eea4a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="eea4a-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="eea4a-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eea4a-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="eea4a-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="eea4a-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="eea4a-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="eea4a-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="eea4a-109">REST API</span><span class="sxs-lookup"><span data-stu-id="eea4a-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)

<span data-ttu-id="eea4a-110">V tomto kurzu se dozvíte, jak vytvořit datovou továrnu Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eea4a-110">This tutorial shows you how to create an Azure data factory by using Visual Studio.</span></span> <span data-ttu-id="eea4a-111">Vytvoříte projektu sady Visual Studio pomocí šablony projektu Data Factory, budete definovat entity Data Factory (propojené služby, datové sady a kanál) ve formátu JSON a potom budete tyto entity publikovat/nasazovat do cloudu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-111">You create a Visual Studio project using the Data Factory project template, define Data Factory entities (linked services, datasets, and pipeline) in JSON format, and then publish/deploy these entities to the cloud.</span></span> 

<span data-ttu-id="eea4a-112">Kanál v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="eea4a-113">Tato aktivita spouští skript Hive v clusteru Azure HDInsight, který transformuje vstupní data pro vytvoření výstupních dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="eea4a-114">Spuštění kanálu je naplánované jednou za měsíc mezi zadaným počátečním a koncovým časem.</span><span class="sxs-lookup"><span data-stu-id="eea4a-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="eea4a-115">Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-115">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="eea4a-116">Kurz předvádějící způsoby kopírování dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="eea4a-117">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="eea4a-118">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="eea4a-118">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="eea4a-119">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="eea4a-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="walkthrough-create-and-publish-data-factory-entities"></a><span data-ttu-id="eea4a-120">Názorný postup: Vytvoření a publikování entit Data Factory</span><span class="sxs-lookup"><span data-stu-id="eea4a-120">Walkthrough: Create and publish Data Factory entities</span></span>
<span data-ttu-id="eea4a-121">V rámci tohoto názorného postupu provedete tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-121">Here are the steps you perform as part of this walkthrough:</span></span>

1. <span data-ttu-id="eea4a-122">Vytvořte dvě propojené služby: **AzureStorageLinkedService1** a **HDInsightOnDemandLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-122">Create two linked services: **AzureStorageLinkedService1** and **HDInsightOnDemandLinkedService1**.</span></span> 
   
    <span data-ttu-id="eea4a-123">V tomto kurzu jsou vstupní i výstupní data pro aktivitu Hive ve stejné službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="eea4a-123">In this tutorial, both input and output data for the hive activity are in the same Azure Blob Storage.</span></span> <span data-ttu-id="eea4a-124">Ke zpracování existujících vstupních dat a vytvoření výstupních dat můžete použít cluster HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="eea4a-124">You use an on-demand HDInsight cluster to process existing input data to produce output data.</span></span> <span data-ttu-id="eea4a-125">Cluster HDInsight na vyžádání za vás automaticky vytvoří služba Azure Data Factory za běhu, když jsou vstupní data připravená k zpracování.</span><span class="sxs-lookup"><span data-stu-id="eea4a-125">The on-demand HDInsight cluster is automatically created for you by Azure Data Factory at run time when the input data is ready to be processed.</span></span> <span data-ttu-id="eea4a-126">Musíte propojit úložiště dat nebo výpočetní služby k datové továrně pro vytváření dat tak, aby se k nim služba Data Factory mohla připojit za běhu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-126">You need to link your data stores or computes to your data factory so that the Data Factory service can connect to them at runtime.</span></span> <span data-ttu-id="eea4a-127">Proto propojíte účet Azure Storage k datové továrně pomocí AzureStorageLinkedService1 a připojíte cluster HDInsight na vyžádání pomocí HDInsightOnDemandLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="eea4a-127">Therefore, you link your Azure Storage Account to the data factory by using the AzureStorageLinkedService1, and link an on-demand HDInsight cluster by using the HDInsightOnDemandLinkedService1.</span></span> <span data-ttu-id="eea4a-128">Při publikování je potřeba zadat název datové továrny, která se má vytvořit, nebo název existující datové továrny.</span><span class="sxs-lookup"><span data-stu-id="eea4a-128">When publishing, you specify the name for the data factory to be created or an existing data factory.</span></span>  
2. <span data-ttu-id="eea4a-129">Vytvořte dvě datové sady, **InputDataset** a **OutputDataset**, které představují vstupní a výstupní data uložená ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="eea4a-129">Create two datasets: **InputDataset** and **OutputDataset**, which represent the input/output data that is stored in the Azure blob storage.</span></span> 
   
    <span data-ttu-id="eea4a-130">Tyto definice datové sady odkazuje na propojenou službu Azure Storage, kterou jste vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="eea4a-130">These dataset definitions refer to the Azure Storage linked service you created in the previous step.</span></span> <span data-ttu-id="eea4a-131">Jako InputDataset zadejte kontejner objektů blob (adfgetstarted) a složku (inptutdata), která obsahuje objekt blob se vstupními daty.</span><span class="sxs-lookup"><span data-stu-id="eea4a-131">For the InputDataset, you specify the blob container (adfgetstarted) and the folder (inptutdata) that contains a blob with the input data.</span></span> <span data-ttu-id="eea4a-132">Jako OutputDataset zadejte kontejner objektů blob (adfgetstarted) a složku (partitioneddata), do které se ukládají výstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-132">For the OutputDataset, you specify the blob container (adfgetstarted) and the folder (partitioneddata) that holds the output data.</span></span> <span data-ttu-id="eea4a-133">Zadáte také další vlastnosti, jako jsou například struktura, dostupnost a zásady.</span><span class="sxs-lookup"><span data-stu-id="eea4a-133">You also specify other properties such as structure, availability, and policy.</span></span>
3. <span data-ttu-id="eea4a-134">Vytvořte kanál s názvem **MyFirstPipeline**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-134">Create a pipeline named **MyFirstPipeline**.</span></span> 
  
    <span data-ttu-id="eea4a-135">V tomto názorném postupu má kanál má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-135">In this walkthrough, the pipeline has only one activity: **HDInsight Hive Activity**.</span></span> <span data-ttu-id="eea4a-136">Tato aktivita spouští skript Hive v clusteru HDInsight na vyžádání, který transformuje vstupní data pro vytvoření výstupních dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-136">This activity transform input data to produce output data by running a hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="eea4a-137">Další informace o aktivitě Hive najdete v tématu [Aktivita Hive](data-factory-hive-activity.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-137">To learn more about hive activity, see [Hive Activity](data-factory-hive-activity.md)</span></span> 
4. <span data-ttu-id="eea4a-138">Vytvořte datovou továrnu s názvem **DataFactoryUsingVS**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-138">Create a data factory named **DataFactoryUsingVS**.</span></span> <span data-ttu-id="eea4a-139">Nasaďte objekt pro vytváření dat a všechny entity služby Data Factory (propojené služby, tabulky a kanál).</span><span class="sxs-lookup"><span data-stu-id="eea4a-139">Deploy the data factory and all Data Factory entities (linked services, tables, and the pipeline).</span></span>
5. <span data-ttu-id="eea4a-140">Po publikování použijte okna na webu Azure Portal a aplikaci Monitorování a správa k monitorování kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-140">After you publish, you use Azure portal blades and Monitoring & Management App to monitor the pipeline.</span></span> 
  
### <a name="prerequisites"></a><span data-ttu-id="eea4a-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="eea4a-141">Prerequisites</span></span>
1. <span data-ttu-id="eea4a-142">Přečtěte si článek [Přehled kurzu](data-factory-build-your-first-pipeline.md) a proveďte **nutné** kroky.</span><span class="sxs-lookup"><span data-stu-id="eea4a-142">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span> <span data-ttu-id="eea4a-143">Pokud chcete přepnout na tento článek, můžete taky v rozevíracím seznamu v horní části okna vybrat možnost **Přehled a požadavky**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-143">You can also select the **Overview and prerequisites** option in the drop-down list at the top to switch to the article.</span></span> <span data-ttu-id="eea4a-144">Po dokončení požadavků přepněte zpátky na tento článek výběrem možnosti **Visual Studio** v rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-144">After you complete the prerequisites, switch back to this article by selecting **Visual Studio** option in the drop-down list.</span></span>
2. <span data-ttu-id="eea4a-145">Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="eea4a-145">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>  
3. <span data-ttu-id="eea4a-146">Na počítači musíte mít nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-146">You must have the following installed on your computer:</span></span>
   * <span data-ttu-id="eea4a-147">Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="eea4a-147">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="eea4a-148">Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="eea4a-148">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="eea4a-149">Přejděte na [stránku položek ke stažení pro Azure](https://azure.microsoft.com/downloads/) a klikněte na **VS 2013** nebo **VS 2015** v části **.NET**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-149">Navigate to [Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in the **.NET** section.</span></span>
   * <span data-ttu-id="eea4a-150">Stáhněte si nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="eea4a-150">Download the latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="eea4a-151">Modul plug-in můžete taky aktualizovat, a to pomocí tohoto postupu: V nabídce klikněte na **Nástroje** -> **Rozšíření a aktualizace** -> **Online** -> **Galerie sady Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** (Nástroje Microsoft Azure Data Factory pro Visual Studio) -> **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-151">You can also update the plugin by doing the following steps: On the menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

<span data-ttu-id="eea4a-152">Nyní použijeme sadu Visual Studio k vytvoření objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="eea4a-152">Now, let's use Visual Studio to create an Azure data factory.</span></span>

### <a name="create-visual-studio-project"></a><span data-ttu-id="eea4a-153">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eea4a-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="eea4a-154">Spusťte **Visual Studio 2013** nebo **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-154">Launch **Visual Studio 2013** or **Visual Studio 2015**.</span></span> <span data-ttu-id="eea4a-155">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-155">Click **File**, point to **New**, and click **Project**.</span></span> <span data-ttu-id="eea4a-156">Mělo by se zobrazit dialogové okno **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-156">You should see the **New Project** dialog box.</span></span>  
2. <span data-ttu-id="eea4a-157">V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Empty Data Factory Project** (Prázdný projekt Data Factory).</span><span class="sxs-lookup"><span data-stu-id="eea4a-157">In the **New Project** dialog, select the **DataFactory** template, and click **Empty Data Factory Project**.</span></span>   

    ![Dialogové okno Nový projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. <span data-ttu-id="eea4a-159">Zadejte **název** projektu, **umístění** a název **řešení** a klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-159">Enter a **name** for the project, **location**, and a name for the **solution**, and click **OK**.</span></span>

    ![Průzkumník řešení](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a><span data-ttu-id="eea4a-161">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="eea4a-161">Create linked services</span></span>
<span data-ttu-id="eea4a-162">V tomto kroku vytvoříte dvě propojené služby, **Azure Storage** a **HDInsight na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-162">In this step, you create two linked services: **Azure Storage** and **HDInsight on-demand**.</span></span> 

<span data-ttu-id="eea4a-163">Propojená služba Azure Storage propojuje účet úložiště Azure k datové továrně tím, že poskytuje informace o připojení.</span><span class="sxs-lookup"><span data-stu-id="eea4a-163">The Azure Storage linked service links your Azure Storage account to the data factory by providing the connection information.</span></span> <span data-ttu-id="eea4a-164">Služba Data Factory používá připojovací řetězec z nastavení propojené služby pro připojení k úložišti Azure za běhu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-164">Data Factory service uses the connection string from the linked service setting to connect to the Azure storage at runtime.</span></span> <span data-ttu-id="eea4a-165">V tomto úložišti jsou uložená vstupní a výstupní data pro kanál a soubor skriptu Hive, který používá aktivita Hive.</span><span class="sxs-lookup"><span data-stu-id="eea4a-165">This storage holds input and output data for the pipeline, and the hive script file used by the hive activity.</span></span> 

<span data-ttu-id="eea4a-166">S využitím propojené služby HDInsight na vyžádání se cluster HDInsight automaticky vytvoří za běhu, když jsou vstupní data připravená k zpracování.</span><span class="sxs-lookup"><span data-stu-id="eea4a-166">With on-demand HDInsight linked service, The HDInsight cluster is automatically created at runtime when the input data is ready to processed.</span></span> <span data-ttu-id="eea4a-167">Až cluster dokončí zpracování, po určité zadané době nečinnosti se odstraní.</span><span class="sxs-lookup"><span data-stu-id="eea4a-167">The cluster is deleted after it is done processing and idle for the specified amount of time.</span></span> 

> [!NOTE]
> <span data-ttu-id="eea4a-168">Datovou továrnu vytvoříte zadáním názvu a nastavení v době publikování řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-168">You create a data factory by specifying its name and settings at the time of publishing your Data Factory solution.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="eea4a-169">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="eea4a-169">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="eea4a-170">V Průzkumníku řešení klikněte pravým tlačítkem myši na **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-170">Right-click **Linked Services** in the solution explorer, point to **Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="eea4a-171">V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Storage Linked Service** (Propojená služba Azure Storage) a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-171">In the **Add New Item** dialog box, select **Azure Storage Linked Service** from the list, and click **Add**.</span></span>
    <span data-ttu-id="eea4a-172">![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="eea4a-172">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span></span>
3. <span data-ttu-id="eea4a-173">Hodnoty `<accountname>` a `<accountkey>` nahraďte názvem účtu služby Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="eea4a-173">Replace `<accountname>` and `<accountkey>` with the name of your Azure storage account and its key.</span></span> <span data-ttu-id="eea4a-174">Chcete-li zjistit, jak získat přístupový klíč k úložišti, přečtěte si informace o zobrazení, kopírování a opětovném vygenerování přístupových klíčů k úložišti v tématu [Správa účtu úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="eea4a-174">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
    <span data-ttu-id="eea4a-175">![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="eea4a-175">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span></span>
4. <span data-ttu-id="eea4a-176">Uložte soubor **AzureStorageLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-176">Save the **AzureStorageLinkedService1.json** file.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="eea4a-177">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="eea4a-177">Create Azure HDInsight linked service</span></span>
1. <span data-ttu-id="eea4a-178">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-178">In the **Solution Explorer**, right-click **Linked Services**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="eea4a-179">Vyberte **HDInsight On Demand Linked Service** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-179">Select **HDInsight On Demand Linked Service**, and click **Add**.</span></span>
3. <span data-ttu-id="eea4a-180">Nahraďte kód **JSON** následujícím kódem JSON:</span><span class="sxs-lookup"><span data-stu-id="eea4a-180">Replace the **JSON** with the following JSON:</span></span>

     ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
        "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    <span data-ttu-id="eea4a-181">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="eea4a-181">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    <span data-ttu-id="eea4a-182">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="eea4a-182">Property</span></span> | <span data-ttu-id="eea4a-183">Popis</span><span class="sxs-lookup"><span data-stu-id="eea4a-183">Description</span></span>
    -------- | ----------- 
    <span data-ttu-id="eea4a-184">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="eea4a-184">ClusterSize</span></span> | <span data-ttu-id="eea4a-185">Určuje velikost clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="eea4a-185">Specifies the size of the HDInsight Hadoop cluster.</span></span>
    <span data-ttu-id="eea4a-186">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="eea4a-186">TimeToLive</span></span> | <span data-ttu-id="eea4a-187">Určuje dobu nečinnosti před odstraněním clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eea4a-187">Specifies that the idle time for the HDInsight cluster, before it is deleted.</span></span>
    <span data-ttu-id="eea4a-188">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="eea4a-188">linkedServiceName</span></span> | <span data-ttu-id="eea4a-189">Určuje účet úložiště, který se používá k ukládání protokolů generovaných clusterem HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="eea4a-189">Specifies the storage account that is used to store the logs that are generated by HDInsight Hadoop cluster.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="eea4a-190">Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="eea4a-190">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (linkedServiceName).</span></span> <span data-ttu-id="eea4a-191">Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eea4a-191">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="eea4a-192">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="eea4a-192">This behavior is by design.</span></span> <span data-ttu-id="eea4a-193">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="eea4a-193">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="eea4a-194">Po dokončení zpracování se cluster automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="eea4a-194">The cluster is automatically deleted when the processing is done.</span></span>
    > 
    > <span data-ttu-id="eea4a-195">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-195">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="eea4a-196">Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="eea4a-196">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="eea4a-197">Názvy těchto kontejnerů se řídí vzorem: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-197">The names of these containers follow a pattern: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span></span> <span data-ttu-id="eea4a-198">K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="eea4a-198">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

    <span data-ttu-id="eea4a-199">Další informace o vlastnostech JSON najdete v tématu [Propojené služby Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="eea4a-199">For more information about JSON properties, see [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article.</span></span> 
4. <span data-ttu-id="eea4a-200">Uložte soubor **HDInsightOnDemandLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-200">Save the **HDInsightOnDemandLinkedService1.json** file.</span></span>

### <a name="create-datasets"></a><span data-ttu-id="eea4a-201">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="eea4a-201">Create datasets</span></span>
<span data-ttu-id="eea4a-202">V tomto kroku vytvoříte datové sady, které představují vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="eea4a-202">In this step, you create datasets to represent the input and output data for Hive processing.</span></span> <span data-ttu-id="eea4a-203">Tyto datové sady odkazují na službu **AzureStorageLinkedService1**, kterou už jste v tomto kurzu vytvořili.</span><span class="sxs-lookup"><span data-stu-id="eea4a-203">These datasets refer to the **AzureStorageLinkedService1** you have created earlier in this tutorial.</span></span> <span data-ttu-id="eea4a-204">Propojená služba odkazuje na účet služby Azure Storage a datové sady určují kontejner, složku a název souboru v úložišti, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-204">The linked service points to an Azure Storage account and datasets specify container, folder, file name in the storage that holds input and output data.</span></span>   

#### <a name="create-input-dataset"></a><span data-ttu-id="eea4a-205">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="eea4a-205">Create input dataset</span></span>
1. <span data-ttu-id="eea4a-206">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-206">In the **Solution Explorer**, right-click **Tables**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="eea4a-207">V seznamu vyberte **Azure Blob**, změňte název souboru na **InputDataSet.json** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-207">Select **Azure Blob** from the list, change the name of the file to **InputDataSet.json**, and click **Add**.</span></span>
3. <span data-ttu-id="eea4a-208">Nahraďte kód **JSON** v editoru následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="eea4a-208">Replace the **JSON** in the editor with the following JSON snippet:</span></span>

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    <span data-ttu-id="eea4a-209">Tento fragment kódu JSON definuje datovou sadu s názvem **AzureBlobInput**, která představuje vstupní data pro aktivitu Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-209">This JSON snippet defines a dataset called **AzureBlobInput** that represents input data for the hive activity in the pipeline.</span></span> <span data-ttu-id="eea4a-210">Určíte, že vstupní data se nacházejí v kontejneru objektů blob s názvem `adfgetstarted` ve složce `inputdata`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-210">You specify that the input data is located in the blob container called `adfgetstarted` and the folder called `inputdata`.</span></span>

    <span data-ttu-id="eea4a-211">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="eea4a-211">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    <span data-ttu-id="eea4a-212">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="eea4a-212">Property</span></span> | <span data-ttu-id="eea4a-213">Popis</span><span class="sxs-lookup"><span data-stu-id="eea4a-213">Description</span></span> |
    -------- | ----------- |
    <span data-ttu-id="eea4a-214">type</span><span class="sxs-lookup"><span data-stu-id="eea4a-214">type</span></span> |<span data-ttu-id="eea4a-215">Vlastnost type je nastavená na hodnotu **AzureBlob**, protože se data nachází ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="eea4a-215">The type property is set to **AzureBlob** because data resides in Azure Blob Storage.</span></span>
    <span data-ttu-id="eea4a-216">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="eea4a-216">linkedServiceName</span></span> | <span data-ttu-id="eea4a-217">Odkazuje na službu AzureStorageLinkedService1, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="eea4a-217">Refers to the AzureStorageLinkedService1 you created earlier.</span></span>
    <span data-ttu-id="eea4a-218">fileName</span><span class="sxs-lookup"><span data-stu-id="eea4a-218">fileName</span></span> |<span data-ttu-id="eea4a-219">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="eea4a-219">This property is optional.</span></span> <span data-ttu-id="eea4a-220">Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath.</span><span class="sxs-lookup"><span data-stu-id="eea4a-220">If you omit this property, all the files from the folderPath are picked.</span></span> <span data-ttu-id="eea4a-221">V tomto případě se zpracovává jenom soubor input.log.</span><span class="sxs-lookup"><span data-stu-id="eea4a-221">In this case, only the input.log is processed.</span></span>
    <span data-ttu-id="eea4a-222">type</span><span class="sxs-lookup"><span data-stu-id="eea4a-222">type</span></span> | <span data-ttu-id="eea4a-223">Soubory protokolů jsou v textovém formátu, takže použijeme hodnotu TextFormat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-223">The log files are in text format, so we use TextFormat.</span></span> |
    <span data-ttu-id="eea4a-224">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="eea4a-224">columnDelimiter</span></span> | <span data-ttu-id="eea4a-225">Sloupce v souborech protokolu jsou oddělené znakem čárky (`,`).</span><span class="sxs-lookup"><span data-stu-id="eea4a-225">columns in the log files are delimited by the comma character (`,`)</span></span>
    <span data-ttu-id="eea4a-226">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="eea4a-226">frequency/interval</span></span> | <span data-ttu-id="eea4a-227">Frekvence je nastavená na hodnotu Month (Měsíc) a interval je 1, takže vstupní řezy jsou dostupné jednou za měsíc.</span><span class="sxs-lookup"><span data-stu-id="eea4a-227">frequency set to Month and interval is 1, which means that the input slices are available monthly.</span></span>
    <span data-ttu-id="eea4a-228">external</span><span class="sxs-lookup"><span data-stu-id="eea4a-228">external</span></span> | <span data-ttu-id="eea4a-229">Pokud vstupní data pro tuto aktivitu nevygeneroval kanál, je tato vlastnost nastavená na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="eea4a-229">This property is set to true if the input data for the activity is not generated by the pipeline.</span></span> <span data-ttu-id="eea4a-230">Tato vlastnost se určuje jenom pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="eea4a-230">This property is only specified on input datasets.</span></span> <span data-ttu-id="eea4a-231">U vstupní datové sady první aktivity ji vždycky nastavte na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="eea4a-231">For the input dataset of the first activity, always set it to true.</span></span>
4. <span data-ttu-id="eea4a-232">Uložte soubor **InputDataset.json**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-232">Save the **InputDataset.json** file.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="eea4a-233">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="eea4a-233">Create output dataset</span></span>
<span data-ttu-id="eea4a-234">Teď vytvoříte výstupní datovou sadu, která bude představovat výstupní data ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="eea4a-234">Now, you create the output dataset to represent output data stored in the Azure Blob storage.</span></span>

1. <span data-ttu-id="eea4a-235">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-235">In the **Solution Explorer**, right-click **tables**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="eea4a-236">V seznamu vyberte **Azure Blob**, změňte název souboru na **OutputDataSet.json** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-236">Select **Azure Blob** from the list, change the name of the file to **OutputDataset.json**, and click **Add**.</span></span>
3. <span data-ttu-id="eea4a-237">Nahraďte kód **JSON** v editoru následujícím kódem JSON:</span><span class="sxs-lookup"><span data-stu-id="eea4a-237">Replace the **JSON** in the editor with the following JSON:</span></span>
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
                "folderPath": "adfgetstarted/partitioneddata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            }
        }
    }
    ```
    <span data-ttu-id="eea4a-238">Tento fragment kódu JSON definuje datovou sadu s názvem **AzureBlobOutput**, která představuje výstupní data vytvořená aktivitou Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-238">The JSON snippet defines a dataset called **AzureBlobOutput** that represents output data produced by the hive activity in the pipeline.</span></span> <span data-ttu-id="eea4a-239">Určíte, že výstupní data vytvořená aktivitou Hive se nacházejí v kontejneru objektů blob s názvem `adfgetstarted` ve složce `partitioneddata`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-239">You specify that the output data is produced by the hive activity is placed in the blob container called `adfgetstarted` and the folder called `partitioneddata`.</span></span> 
    
    <span data-ttu-id="eea4a-240">Oddíl **availability** určuje, že se výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="eea4a-240">The **availability** section specifies that the output dataset is produced on a monthly basis.</span></span> <span data-ttu-id="eea4a-241">Výstupní datovou sadu řídí plán kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-241">The output dataset drives the schedule of the pipeline.</span></span> <span data-ttu-id="eea4a-242">Kanál se spouští každý měsíc mezi zadaným počátečním a koncovým časem.</span><span class="sxs-lookup"><span data-stu-id="eea4a-242">The pipeline runs monthly between its start and end times.</span></span> 

    <span data-ttu-id="eea4a-243">Popisy těchto vlastností najdete v části **Vytvoření vstupní datové sady**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-243">See **Create the input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="eea4a-244">U výstupní datové sady nenajdete vlastnost external, protože datovou sadu vytváří kanál.</span><span class="sxs-lookup"><span data-stu-id="eea4a-244">You do not set the external property on an output dataset as the dataset is produced by the pipeline.</span></span>
4. <span data-ttu-id="eea4a-245">Uložte soubor **OutputDataset.json**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-245">Save the **OutputDataset.json** file.</span></span>

### <a name="create-pipeline"></a><span data-ttu-id="eea4a-246">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="eea4a-246">Create pipeline</span></span>
<span data-ttu-id="eea4a-247">Zatím jste vytvořili propojenou službu Azure Storage a vstupní a výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-247">You have created the Azure Storage linked service, and input and output datasets so far.</span></span> <span data-ttu-id="eea4a-248">Teď vytvoříte kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-248">Now, you create a pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="eea4a-249">**Vstup** aktivity Hive je nastavený na **AzureBlobInput** a **výstup aktivity** je nastavený na **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-249">The **input** for the hive activity is set to **AzureBlobInput** and **output** is set to **AzureBlobOutput**.</span></span> <span data-ttu-id="eea4a-250">Řez vstupní datové sady je dostupný každý měsíc (frekvence: Měsíc, interval: 1). Výstupní datová sada se taky vytváří každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="eea4a-250">A slice of an input dataset is available monthly (frequency: Month, interval: 1), and the output slice is produced monthly too.</span></span> 

1. <span data-ttu-id="eea4a-251">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Kanály**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-251">In the **Solution Explorer**, right-click **Pipelines**, point to **Add**, and click **New Item.**</span></span>
2. <span data-ttu-id="eea4a-252">V seznamu vyberte **Hive Transformation Pipeline** (Kanál transformace Hive) a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-252">Select **Hive Transformation Pipeline** from the list, and click **Add**.</span></span>
3. <span data-ttu-id="eea4a-253">Nahraďte kód **JSON** tímto fragmentem kódu:</span><span class="sxs-lookup"><span data-stu-id="eea4a-253">Replace the **JSON** with the following snippet:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eea4a-254">Nahraďte `<storageaccountname>` názvem vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="eea4a-254">Replace `<storageaccountname>` with the name of your storage account.</span></span>

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService1",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="eea4a-255">Nahraďte `<storageaccountname>` názvem vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="eea4a-255">Replace `<storageaccountname>` with the name of your storage account.</span></span>

    <span data-ttu-id="eea4a-256">Fragment JSON definuje kanál, který se skládá z jediné aktivity (aktivita Hive).</span><span class="sxs-lookup"><span data-stu-id="eea4a-256">The JSON snippet defines a pipeline that consists of a single activity (Hive Activity).</span></span> <span data-ttu-id="eea4a-257">Tato aktivita spouští skript Hive pro zpracování vstupních dat v clusteru HDInsight na vyžádání s cílem vytvořit výstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-257">This activity runs a Hive script to process input data on an on-demand HDInsight cluster to produce output data.</span></span> <span data-ttu-id="eea4a-258">V oddílu aktivit JSON kanálu uvidíte jenom jednu aktivitu s nastaveným typem **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-258">In the activities section of the pipeline JSON, you see only one activity in the array with type set to **HDInsightHive**.</span></span> 

    <span data-ttu-id="eea4a-259">Ve vlastnostech type, které jsou specifické pro aktivitu HDInsight Hive, zadáte, kterou propojenou službu Azure Storage má soubor skriptu Hive, cestu k tomuto souboru skriptu a parametry pro něj.</span><span class="sxs-lookup"><span data-stu-id="eea4a-259">In the type properties that are specific to HDInsight Hive activity, you specify what Azure Storage linked service has the hive script file, the path to the script file, and parameters to the script file.</span></span> 

    <span data-ttu-id="eea4a-260">Soubor skriptu Hive **partitionweblogs.hql** je uložený v účtu služby Azure Storage (který určuje služba scriptLinkedService) a ve složce `script` v kontejneru `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-260">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService), and in the `script` folder in the container `adfgetstarted`.</span></span>

    <span data-ttu-id="eea4a-261">Oddíl `defines` určuje nastavení běhového prostředí, které se předá skriptu Hive jako konfigurační hodnoty Hive (např. `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`).</span><span class="sxs-lookup"><span data-stu-id="eea4a-261">The `defines` section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span></span>

    <span data-ttu-id="eea4a-262">Vlastnosti kanálu **start** a **end** určují období aktivity kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-262">The **start** and **end** properties of the pipeline specifies the active period of the pipeline.</span></span> <span data-ttu-id="eea4a-263">Nakonfigurovali jste vytváření datové sady jednou měsíčně. Proto taky kanál vytvoří jenom jeden řez (protože měsíc je pro počáteční a koncové datum stejný).</span><span class="sxs-lookup"><span data-stu-id="eea4a-263">You configured the dataset to be produced monthly, therefore, only once slice is produced by the pipeline (because the month is same in start and end dates).</span></span>

    <span data-ttu-id="eea4a-264">V kódu JSON aktivity určujete, že má skript Hive běžet ve výpočetní službě určené vlastností **linkedServiceName**, tedy **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-264">In the activity JSON, you specify that the Hive script runs on the compute specified by the **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>
4. <span data-ttu-id="eea4a-265">Uložte soubor **HiveActivity1.json**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-265">Save the **HiveActivity1.json** file.</span></span>

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a><span data-ttu-id="eea4a-266">Přidání souborů partitionweblogs.hql a input.log jako závislosti</span><span class="sxs-lookup"><span data-stu-id="eea4a-266">Add partitionweblogs.hql and input.log as a dependency</span></span>
1. <span data-ttu-id="eea4a-267">Klikněte pravým tlačítkem myši na **Závislosti** v okně **Průzkumník řešení**, přejděte na **Přidat** a klikněte na **Existující položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-267">Right-click **Dependencies** in the **Solution Explorer** window, point to **Add**, and click **Existing Item**.</span></span>  
2. <span data-ttu-id="eea4a-268">Přejděte do složky **C:\ADFGettingStarted**, vyberte soubory **partitionweblogs.hql** a **input.log** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-268">Navigate to the **C:\ADFGettingStarted** and select **partitionweblogs.hql**, **input.log** files, and click **Add**.</span></span> <span data-ttu-id="eea4a-269">Tyto dva soubory jste vytvořili v rámci požadavků uvedených v článku [Přehled kurzu](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-269">You created these two files as part of prerequisites from the [Tutorial Overview](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="eea4a-270">Až řešení v dalším kroku publikujete, soubor **partitionweblogs.hql** se nahraje do složky **script** v kontejneru objektů blob `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-270">When you publish the solution in the next step, the **partitionweblogs.hql** file is uploaded to the **script** folder in the `adfgetstarted` blob container.</span></span>   

### <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="eea4a-271">Publikování/nasazení entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="eea4a-271">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="eea4a-272">V tomto kroku publikujete entity služby Data Factory (propojené služby, datové sady a kanál) ve vašem projektu do služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-272">In this step, you publish the Data Factory entities (linked services, datasets, and pipeline) in your project to the Azure Data Factory service.</span></span> <span data-ttu-id="eea4a-273">Během publikování zadáte název vaší datové továrny.</span><span class="sxs-lookup"><span data-stu-id="eea4a-273">In the process of publishing, you specify the name for your data factory.</span></span> 

1. <span data-ttu-id="eea4a-274">V Průzkumníku řešení klikněte pravým tlačítkem na požadovaný projekt a poté klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-274">Right-click project in the Solution Explorer, and click **Publish**.</span></span>
2. <span data-ttu-id="eea4a-275">Pokud se zobrazí dialogové okno **Přihlásit se pomocí účtu Microsoft**, zadejte přihlašovací údaje k účtu s předplatným Azure a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-275">If you see **Sign in to your Microsoft account** dialog box, enter your credentials for the account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="eea4a-276">Mělo by se zobrazit následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="eea4a-276">You should see the following dialog box:</span></span>

   ![Dialogové okno publikování](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. <span data-ttu-id="eea4a-278">Na stránce **Konfigurace datové továrny** proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-278">In the **Configure data factory** page, do the following steps:</span></span>

    ![Publikování – nová nastavení datové továrny](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. <span data-ttu-id="eea4a-280">Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="eea4a-280">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="eea4a-281">Zadejte jedinečný **název** objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-281">Enter a unique **name** for the data factory.</span></span> <span data-ttu-id="eea4a-282">Příklad: **DataFactoryUsingVS09152016**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-282">For example: **DataFactoryUsingVS09152016**.</span></span> <span data-ttu-id="eea4a-283">Název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="eea4a-283">The name must be globally unique.</span></span>
   3. <span data-ttu-id="eea4a-284">V poli **Předplatné** vyberte správné předplatné.</span><span class="sxs-lookup"><span data-stu-id="eea4a-284">Select the right subscription for the **Subscription** field.</span></span> 
        > [!IMPORTANT]
        > <span data-ttu-id="eea4a-285">Pokud nevidíte žádné předplatné, ujistěte se, že jste přihlášeni pomocí účtu, který je správce nebo spolusprávce předplatného.</span><span class="sxs-lookup"><span data-stu-id="eea4a-285">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of the subscription.</span></span>
   4. <span data-ttu-id="eea4a-286">Vyberte **skupinu prostředků** pro objekt pro vytváření dat, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="eea4a-286">Select the **resource group** for the data factory to be created.</span></span>
   5. <span data-ttu-id="eea4a-287">Vyberte **oblast** pro objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-287">Select the **region** for the data factory.</span></span>
   6. <span data-ttu-id="eea4a-288">Kliknutím na **Další** přejděte na stránku **Publish Items** (Publikovat položky).</span><span class="sxs-lookup"><span data-stu-id="eea4a-288">Click **Next** to switch to the **Publish Items** page.</span></span> <span data-ttu-id="eea4a-289">(Pokud je tlačítko **Další** neaktivní, opusťte pole Název stisknutím klávesy **TAB**.)</span><span class="sxs-lookup"><span data-stu-id="eea4a-289">(Press **TAB** to move out of the Name field to if the **Next** button is disabled.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eea4a-290">Pokud se při publikování zobrazí chyba typu **Název objektu pro vytváření dat FirstDataFactoryUsingVS není dostupný**, název změňte (třeba na vaše_jméno_FirstDataFactoryUsingVS).</span><span class="sxs-lookup"><span data-stu-id="eea4a-290">If you receive the error **Data factory name “DataFactoryUsingVS” is not available** when publishing, change the name (for example, yournameDataFactoryUsingVS).</span></span> <span data-ttu-id="eea4a-291">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-291">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
1. <span data-ttu-id="eea4a-292">Na stránce **Publish Items** (Publikovat položky) zkontrolujte, jestli jsou vybrané všechny entity služby Data Factory, a kliknutím na **Další** přejděte na stránku **Souhrn**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-292">In the **Publish Items** page, ensure that all the Data Factories entities are selected, and click **Next** to switch to the **Summary** page.</span></span>

    ![Stránka publikování položek](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. <span data-ttu-id="eea4a-294">Zkontrolujte souhrn a klikněte na **Další**. Spustí se proces nasazení a zobrazí se **Stav nasazení**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-294">Review the summary and click **Next** to start the deployment process and view the **Deployment Status**.</span></span>

    ![Stránka souhrnu](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. <span data-ttu-id="eea4a-296">Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="eea4a-296">In the **Deployment Status** page, you should see the status of the deployment process.</span></span> <span data-ttu-id="eea4a-297">Až se nasazení dokončí, klikněte na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="eea4a-297">Click Finish after the deployment is done.</span></span>

<span data-ttu-id="eea4a-298">Všimněte si těchto důležitých bodů:</span><span class="sxs-lookup"><span data-stu-id="eea4a-298">Important points to note:</span></span>

- <span data-ttu-id="eea4a-299">Pokud se zobrazí chyba **Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory**, proveďte některý z těchto kroků a znovu zkuste publikovat:</span><span class="sxs-lookup"><span data-stu-id="eea4a-299">If you receive the error: **This subscription is not registered to use namespace Microsoft.DataFactory**, do one of the following and try publishing again:</span></span>
    - <span data-ttu-id="eea4a-300">V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-300">In Azure PowerShell, run the following command to register the Data Factory provider.</span></span>
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        <span data-ttu-id="eea4a-301">Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="eea4a-301">You can run the following command to confirm that the Data Factory provider is registered.</span></span>

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - <span data-ttu-id="eea4a-302">Přihlaste se na web [Azure Portal](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-302">Login using the Azure subscription in to the [Azure portal](https://portal.azure.com) and navigate to a Data Factory blade (or) create a data factory in the Azure portal.</span></span> <span data-ttu-id="eea4a-303">Zprostředkovatel se při takovém postupu zaregistruje automaticky.</span><span class="sxs-lookup"><span data-stu-id="eea4a-303">This action automatically registers the provider for you.</span></span>
- <span data-ttu-id="eea4a-304">Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="eea4a-304">The name of the data factory may be registered as a DNS name in the future and hence become publically visible.</span></span>
- <span data-ttu-id="eea4a-305">Chcete-li vytvářet instance služby Data Factory, musíte být správce nebo spolusprávce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="eea4a-305">To create Data Factory instances, you need to be an admin or co-admin of the Azure subscription</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="eea4a-306">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="eea4a-306">Monitor pipeline</span></span>
<span data-ttu-id="eea4a-307">V tomto kroku monitorujete kanál pomocí zobrazení diagramu datové továrny.</span><span class="sxs-lookup"><span data-stu-id="eea4a-307">In this step, you monitor the pipeline using Diagram View of the data factory.</span></span> 

#### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="eea4a-308">Monitorování kanálu pomocí Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="eea4a-308">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="eea4a-309">Přihlaste se na web [Azure Portal](https://portal.azure.com/) a proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-309">Log in to the [Azure portal](https://portal.azure.com/), do the following steps:</span></span>
   1. <span data-ttu-id="eea4a-310">Klikněte na **Další služby** a poté na **Objekty pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-310">Click **More services** and click **Data factories**.</span></span>
       
        ![Procházet -> Datové továrny](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. <span data-ttu-id="eea4a-312">Ze seznamu datových továren vyberte název své datové továrny (příklad: **DataFactoryUsingVS09152016**).</span><span class="sxs-lookup"><span data-stu-id="eea4a-312">Select the name of your data factory (for example: **DataFactoryUsingVS09152016**) from the list of data factories.</span></span>
   
       ![Výběr objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. <span data-ttu-id="eea4a-314">Na domovské stránce objektu pro vytváření dat klikněte na **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-314">In the home page for your data factory, click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. <span data-ttu-id="eea4a-316">V zobrazení diagramu uvidíte přehled kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-316">In the Diagram View, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. <span data-ttu-id="eea4a-318">Pokud chcete zobrazit všechny aktivity v kanálu, klikněte na kanál v diagramu pravým tlačítkem myši a potom klikněte na Otevřít kanál.</span><span class="sxs-lookup"><span data-stu-id="eea4a-318">To view all activities in the pipeline, right-click pipeline in the diagram and click Open Pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. <span data-ttu-id="eea4a-320">Zkontrolujte, jestli je v kanálu vidět aktivita HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="eea4a-320">Confirm that you see the HDInsightHive activity in the pipeline.</span></span>

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    <span data-ttu-id="eea4a-322">Pokud se chcete vrátit do předchozího zobrazení, klikněte v nabídce navigace s popisem cesty v horní části na **Objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-322">To navigate back to the previous view, click **Data factory** in the breadcrumb menu at the top.</span></span>
6. <span data-ttu-id="eea4a-323">V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-323">In the **Diagram View**, double-click the dataset **AzureBlobInput**.</span></span> <span data-ttu-id="eea4a-324">Zkontrolujte, jestli je řez ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-324">Confirm that the slice is in **Ready** state.</span></span> <span data-ttu-id="eea4a-325">Než se řez zobrazí ve stavu Připraveno, může to několik minut trvat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-325">It may take a couple of minutes for the slice to show up in Ready state.</span></span> <span data-ttu-id="eea4a-326">Pokud se to do nějaké doby nestane, zkontrolujte, jestli je vstupní soubor (input.log) umístěný ve správném kontejneru (`adfgetstarted`) a složce (`inputdata`).</span><span class="sxs-lookup"><span data-stu-id="eea4a-326">If it does not happen after you wait for sometime, see if you have the input file (input.log) placed in the right container (`adfgetstarted`) and folder (`inputdata`).</span></span> <span data-ttu-id="eea4a-327">Zkontrolujte taky, že vlastnost **external** vstupní datové sady je nastavená na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-327">And, make sure that the **external** property on the input dataset is set to **true**.</span></span> 

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. <span data-ttu-id="eea4a-329">Kliknutím na tlačítko **X** zavřete okno **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-329">Click **X** to close **AzureBlobInput** blade.</span></span>
8. <span data-ttu-id="eea4a-330">V **zobrazení diagramu** dvakrát klikněte na datovou sadu **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-330">In the **Diagram View**, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="eea4a-331">Zobrazí se řez, který se právě zpracovává.</span><span class="sxs-lookup"><span data-stu-id="eea4a-331">You see that the slice that is currently being processed.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. <span data-ttu-id="eea4a-333">Po dokončení zpracování bude řez ve stavu **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-333">When processing is done, you see the slice in **Ready** state.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="eea4a-334">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="eea4a-334">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="eea4a-335">Proto počítejte s tím, že zpracování řezu kanálem bude trvat **přibližně 30 minut**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-335">Therefore, expect the pipeline to take **approximately 30 minutes** to process the slice.</span></span>  
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. <span data-ttu-id="eea4a-337">Pokud je řez ve stavu **Připraveno**, zkontrolujte, jestli se ve složce `partitioneddata` v kontejneru `adfgetstarted` ve službě Blob Storage nachází výstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-337">When the slice is in **Ready** state, check the `partitioneddata` folder in the `adfgetstarted` container in your blob storage for the output data.</span></span>  

    ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. <span data-ttu-id="eea4a-339">Kliknutím na řez otevřete okno **Datový řez** s podrobnostmi o řezu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-339">Click the slice to see details about it in a **Data slice** blade.</span></span>

    ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. <span data-ttu-id="eea4a-341">Kliknutím na spuštění aktivity v **seznamu spuštění aktivit** zobrazíte podrobnosti o spuštění aktivity (v našem scénáři aktivity Hivu) v okně **Podrobnosti o spuštění aktivit**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-341">Click an activity run in the **Activity runs list** to see details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span> 
  
    ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    <span data-ttu-id="eea4a-343">V souborech protokolů můžete zobrazit provedený dotaz Hivu s informacemi o jeho stavu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-343">From the log files, you can see the Hive query that was executed and status information.</span></span> <span data-ttu-id="eea4a-344">Tyto protokoly jsou užitečné při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="eea4a-344">These logs are useful for troubleshooting any issues.</span></span>  

<span data-ttu-id="eea4a-345">Pokyny k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu, pomocí webu Azure Portal najdete v článku [Monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-345">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how to use the Azure portal to monitor the pipeline and datasets you have created in this tutorial.</span></span>

#### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="eea4a-346">Monitorování kanálu pomocí aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="eea4a-346">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="eea4a-347">K monitorování kanálů můžete také použít aplikaci pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-347">You can also use Monitor & Manage application to monitor your pipelines.</span></span> <span data-ttu-id="eea4a-348">Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-348">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="eea4a-349">Klikněte na dlaždici Monitorování a správa.</span><span class="sxs-lookup"><span data-stu-id="eea4a-349">Click Monitor & Manage tile.</span></span>

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. <span data-ttu-id="eea4a-351">Měla by se zobrazit aplikace pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-351">You should see Monitor & Manage application.</span></span> <span data-ttu-id="eea4a-352">Změňte hodnoty **Čas spuštění** a **Čas ukončení** tak, aby odpovídaly času spuštění (01.04.2016 00:00) a ukončení (02.04.2016 00:00) vašeho kanálu, a klikněte na **Použít**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-352">Change the **Start time** and **End time** to match start (04-01-2016 12:00 AM) and end times (04-02-2016 12:00 AM) of your pipeline, and click **Apply**.</span></span>

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. <span data-ttu-id="eea4a-354">Pokud chcete zobrazit podrobnosti o okně aktivity, vyberte ho v seznamu **Okna aktivit**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-354">To see details about an activity window, select it in the **Activity Windows list** to see details about it.</span></span>
    <span data-ttu-id="eea4a-355">![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="eea4a-355">![Activity window details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="eea4a-356">Po úspěšném zpracování řezu se vstupní soubor odstraní.</span><span class="sxs-lookup"><span data-stu-id="eea4a-356">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="eea4a-357">Pokud tedy chcete spustit řez znovu nebo si znovu projít tento kurz, načtěte vstupní soubor (input.log) do složky `inputdata` v kontejneru `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-357">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the `inputdata` folder of the `adfgetstarted` container.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="eea4a-358">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="eea4a-358">Additional notes</span></span>
- <span data-ttu-id="eea4a-359">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-359">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="eea4a-360">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="eea4a-360">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="eea4a-361">Může obsahovat například aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu Hivu HDInsight pro spuštění skriptu Hive, který umožňuje transformovat vstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-361">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data.</span></span> <span data-ttu-id="eea4a-362">Všechny zdroje a jímky podporované aktivitou kopírování najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="eea4a-362">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="eea4a-363">Seznam výpočetních služeb podporovaných službou Data Factory najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-363">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory.</span></span>
- <span data-ttu-id="eea4a-364">Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="eea4a-364">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="eea4a-365">Všechny zdroje a jímky podporované aktivitou kopírování najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="eea4a-365">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="eea4a-366">Seznam výpočetních služeb podporovaných službou Data Factory a [aktivity transformace](data-factory-data-transformation-activities.md), které se v nich dají spustit, najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-366">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory and [transformation activities](data-factory-data-transformation-activities.md) that can run on them.</span></span>
- <span data-ttu-id="eea4a-367">Podrobné informace o vlastnostech JSON použitých v definici propojené služby Azure Storage najdete v tématu [Přesun dat do a z Azure Blobu](data-factory-azure-blob-connector.md#azure-storage-linked-service).</span><span class="sxs-lookup"><span data-stu-id="eea4a-367">See [Move data from/to Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used in the Azure Storage linked service definition.</span></span>
- <span data-ttu-id="eea4a-368">Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="eea4a-368">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="eea4a-369">Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-369">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>
-  <span data-ttu-id="eea4a-370">Pomocí výše uvedeného kódu JSON služba Data Factory vytvoří cluster HDInsight **se systémem Linux** za vás.</span><span class="sxs-lookup"><span data-stu-id="eea4a-370">The Data Factory creates a **Linux-based** HDInsight cluster for you with the preceding JSON.</span></span> <span data-ttu-id="eea4a-371">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="eea4a-371">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
- <span data-ttu-id="eea4a-372">Cluster HDInsight vytvoří **výchozí kontejner** ve službě Blob Storage, kterou jste určili v kódu JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="eea4a-372">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (linkedServiceName).</span></span> <span data-ttu-id="eea4a-373">Při odstranění clusteru HDInsight neprovede odstranění tohoto kontejneru.</span><span class="sxs-lookup"><span data-stu-id="eea4a-373">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="eea4a-374">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="eea4a-374">This behavior is by design.</span></span> <span data-ttu-id="eea4a-375">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="eea4a-375">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="eea4a-376">Po dokončení zpracování se cluster automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="eea4a-376">The cluster is automatically deleted when the processing is done.</span></span>
    
    <span data-ttu-id="eea4a-377">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-377">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="eea4a-378">Pokud je nepotřebujete k řešení potíží s úlohami, můžete je odstranit, abyste snížili náklady na úložiště.</span><span class="sxs-lookup"><span data-stu-id="eea4a-378">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="eea4a-379">Názvy těchto kontejnerů se řídí vzorem: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="eea4a-379">The names of these containers follow a pattern: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span></span> <span data-ttu-id="eea4a-380">K odstranění kontejnerů ze služby Azure Blob Storage můžete použít nástroje, jako je třeba [Průzkumník úložišť od Microsoftu](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="eea4a-380">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>
- <span data-ttu-id="eea4a-381">V současnosti určuje plán výstupní datová sada, takže musíte výstupní datovou sadu vytvořit i v případě, že aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="eea4a-381">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="eea4a-382">Pokud aktivita nemá žádný vstup, vstupní datovou sadu vytvářet nemusíte.</span><span class="sxs-lookup"><span data-stu-id="eea4a-382">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> 
- <span data-ttu-id="eea4a-383">Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-383">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="eea4a-384">Kurz předvádějící způsoby kopírování dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz kopírování dat z Blob Storage do SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-384">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>


## <a name="use-server-explorer-to-view-data-factories"></a><span data-ttu-id="eea4a-385">Zobrazení objektů pro vytváření dat pomocí Průzkumníka serveru</span><span class="sxs-lookup"><span data-stu-id="eea4a-385">Use Server Explorer to view data factories</span></span>
1. <span data-ttu-id="eea4a-386">V sadě **Visual Studio** klikněte v nabídce na **Zobrazit** a potom klikněte na **Průzkumník serveru**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-386">In **Visual Studio**, click **View** on the menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="eea4a-387">V okně Průzkumníka serveru rozbalte položku **Azure** a potom **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-387">In the Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="eea4a-388">Pokud se zobrazí text **Přihlásit se k Visual Studiu**, zadejte **účet** přidružený k vašemu předplatnému Azure a klikněte na **Pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-388">If you see **Sign in to Visual Studio**, enter the **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="eea4a-389">Zadejte **heslo** a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-389">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="eea4a-390">Visual Studio se pokusí získat informace o všech objektech pro vytváření dat Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="eea4a-390">Visual Studio tries to get information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="eea4a-391">Stav této operace se zobrazuje v okně **Data Factory Task List** (Seznam úkolů služby Data Factory).</span><span class="sxs-lookup"><span data-stu-id="eea4a-391">You see the status of this operation in the **Data Factory Task List** window.</span></span>

    ![Průzkumník serveru](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. <span data-ttu-id="eea4a-393">Kliknutím na objekt pro vytváření dat pravým tlačítkem a výběrem **Export Data Factory to New Project** (Exportovat objekt pro vytváření dat do nového projektu) můžete vytvořit projekt sady Visual Studio založený na existujícím objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-393">You can right-click a data factory, and select **Export Data Factory to New Project** to create a Visual Studio project based on an existing data factory.</span></span>

    ![Export objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="eea4a-395">Aktualizace nástrojů služby Data Factory pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eea4a-395">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="eea4a-396">Chcete-li aktualizovat nástroje služby Azure Data Factory pro Visual Studio, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-396">To update Azure Data Factory tools for Visual Studio, do the following steps:</span></span>

1. <span data-ttu-id="eea4a-397">V nabídce klikněte na **Nástroje** a vyberte **Rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-397">Click **Tools** on the menu and select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="eea4a-398">V levém podokně vyberte **Aktualizace** a vyberte **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-398">Select **Updates** in the left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="eea4a-399">Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-399">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="eea4a-400">Pokud se tato položka nezobrazí, znamená to, že máte nejnovější verzi těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-400">If you do not see this entry, you already have the latest version of the tools.</span></span>

## <a name="use-configuration-files"></a><span data-ttu-id="eea4a-401">Použití konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="eea4a-401">Use configuration files</span></span>
<span data-ttu-id="eea4a-402">Pomocí konfiguračních souborů v sadě Visual Studio můžete pro různá prostředí nakonfigurovat různé vlastnosti propojených služeb / tabulek / kanálů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-402">You can use configuration files in Visual Studio to configure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="eea4a-403">Podívejte se na následující definici JSON pro propojenou službu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="eea4a-403">Consider the following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="eea4a-404">U položky **connectionString** můžete určit jiné hodnoty vlastností accountname a accountkey pro různá prostředí (vývojové/testovací/produkční), do kterých nasazujete entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-404">To specify **connectionString** with different values for accountname and accountkey based on the environment (Dev/Test/Production) to which you are deploying Data Factory entities.</span></span> <span data-ttu-id="eea4a-405">Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="eea4a-405">You can achieve this behavior by using separate configuration file for each environment.</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a><span data-ttu-id="eea4a-406">Přidání konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="eea4a-406">Add a configuration file</span></span>
<span data-ttu-id="eea4a-407">Následující postup umožňuje přidat pro každé prostředí jiný konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="eea4a-407">Add a configuration file for each environment by performing the following steps:</span></span>   

1. <span data-ttu-id="eea4a-408">V řešení v sadě Visual Studio klikněte pravým tlačítkem na svůj projekt Data Factory, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-408">Right-click the Data Factory project in your Visual Studio solution, point to **Add**, and click **New item**.</span></span>
2. <span data-ttu-id="eea4a-409">V seznamu nainstalovaných šablon vlevo vyberte šablonu **Config**, vyberte možnost **Konfigurační soubor**, zadejte jeho **název** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-409">Select **Config** from the list of installed templates on the left, select **Configuration File**, enter a **name** for the configuration file, and click **Add**.</span></span>

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="eea4a-411">Přidejte parametry konfigurace a jejich hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="eea4a-411">Add configuration parameters and their values in the following format:</span></span>

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    <span data-ttu-id="eea4a-412">Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="eea4a-412">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="eea4a-413">Všimněte si, že syntaxe pro určení názvu je [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="eea4a-413">Notice that the syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="eea4a-414">Pokud má kód JSON vlastnost s polem hodnot, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="eea4a-414">If JSON has a property that has an array of values as shown in the following code:</span></span>  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    <span data-ttu-id="eea4a-415">Nakonfigurujte vlastnosti tak, jak je znázorněno v následujícím konfiguračním souboru (použijte indexování počítané od nuly):</span><span class="sxs-lookup"><span data-stu-id="eea4a-415">Configure properties as shown in the following configuration file (use zero-based indexing):</span></span>

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a><span data-ttu-id="eea4a-416">Názvy vlastností s mezerami</span><span class="sxs-lookup"><span data-stu-id="eea4a-416">Property names with spaces</span></span>
<span data-ttu-id="eea4a-417">Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru):</span><span class="sxs-lookup"><span data-stu-id="eea4a-417">If a property name has spaces in it, use square brackets as shown in the following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="eea4a-418">Nasazení řešení pomocí konfigurace</span><span class="sxs-lookup"><span data-stu-id="eea4a-418">Deploy solution using a configuration</span></span>
<span data-ttu-id="eea4a-419">Když v sadě VS publikujete entity služby Azure Data Factory, můžete určit, jakou konfiguraci chcete pro danou operaci publikování použít.</span><span class="sxs-lookup"><span data-stu-id="eea4a-419">When you are publishing Azure Data Factory entities in VS, you can specify the configuration that you want to use for that publishing operation.</span></span>

<span data-ttu-id="eea4a-420">Pokud chcete publikovat entity v projektu Azure Data Factory pomocí konfiguračního souboru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="eea4a-420">To publish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="eea4a-421">Klikněte pravým tlačítkem na projekt Data Factory a kliknutím na **Publikovat** zobrazte dialogové okno **Publish Items** (Publikovat položky).</span><span class="sxs-lookup"><span data-stu-id="eea4a-421">Right-click Data Factory project and click **Publish** to see the **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="eea4a-422">Vyberte existující objekt pro vytváření dat nebo zadejte hodnoty pro vytvoření objektu pro vytváření dat na stránce **Configure data factory** (Konfigurace objektu pro vytváření dat) a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-422">Select an existing data factory or specify values for creating a data factory on the **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="eea4a-423">Na stránce **Publish Items** (Publikovat položky) se zobrazí rozevírací seznam s dostupnými konfiguracemi pro pole **Select Deployment Config** (Výběr konfigurace nasazení).</span><span class="sxs-lookup"><span data-stu-id="eea4a-423">On the **Publish Items** page: you see a drop-down list with available configurations for the **Select Deployment Config** field.</span></span>

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="eea4a-425">Vyberte **konfigurační soubor**, který chcete použít, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-425">Select the **configuration file** that you would like to use and click **Next**.</span></span>
5. <span data-ttu-id="eea4a-426">Ujistěte se, že se na stránce **Souhrn** zobrazil název souboru JSON, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-426">Confirm that you see the name of JSON file in the **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="eea4a-427">Po dokončení operace nasazení klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-427">Click **Finish** after the deployment operation is finished.</span></span>

<span data-ttu-id="eea4a-428">Při nasazení se hodnoty z konfiguračního souboru použijí k nastavení hodnot vlastností v souborech JSON před samotným nasazením entit do služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-428">When you deploy, the values from the configuration file are used to set values for properties in the JSON files before the entities are deployed to Azure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="eea4a-429">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="eea4a-429">Use Azure Key Vault</span></span>
<span data-ttu-id="eea4a-430">Není vhodné a často je to proti zásadám zabezpečení ukládat citlivá data, jako jsou například připojovací řetězce, do úložišti kódu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-430">It is not advisable and often against security policy to commit sensitive data such as connection strings to the code repository.</span></span> <span data-ttu-id="eea4a-431">V ukázce [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) na Githubu získáte další informace o ukládání citlivých informací v Azure Key Vault a jejich používání při publikování entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-431">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub to learn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="eea4a-432">Rozšíření zabezpečeného publikování pro Visual Studio umožňuje ukládat tajné klíče v Key Vault a v konfiguracích propojených služeb a nasazení uvádět pouze odkazy.</span><span class="sxs-lookup"><span data-stu-id="eea4a-432">The Secure Publish extension for Visual Studio allows the secrets to be stored in Key Vault and only references to them are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="eea4a-433">Tyto odkazy se při publikování entit služby Data Factory do Azure vyhodnotí.</span><span class="sxs-lookup"><span data-stu-id="eea4a-433">These references are resolved when you publish Data Factory entities to Azure.</span></span> <span data-ttu-id="eea4a-434">Tyto soubory pak lze uložit do úložiště zdrojového kódu bez vystavení jakýchkoli tajných kódů.</span><span class="sxs-lookup"><span data-stu-id="eea4a-434">These files can then be committed to source repository without exposing any secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="eea4a-435">Souhrn</span><span class="sxs-lookup"><span data-stu-id="eea4a-435">Summary</span></span>
<span data-ttu-id="eea4a-436">V tomto kurzu jste vytvořili objekt pro zpracování dat Azure, který zpracovává data pomocí skriptu Hive v clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="eea4a-436">In this tutorial, you created an Azure data factory to process data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="eea4a-437">Pomocí editoru služby Data Factory na webu Azure Portal jste provedli tyto kroky:</span><span class="sxs-lookup"><span data-stu-id="eea4a-437">You used the Data Factory Editor in the Azure portal to do the following steps:</span></span>  

1. <span data-ttu-id="eea4a-438">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="eea4a-438">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="eea4a-439">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="eea4a-439">Created two **linked services**:</span></span>
   1. <span data-ttu-id="eea4a-440">Propojená služba **Azure Storage** spojuje úložiště objektů blob Azure, které obsahuje vstupní/výstupní soubory, s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-440">**Azure Storage** linked service to link your Azure blob storage that holds input/output files to the data factory.</span></span>
   2. <span data-ttu-id="eea4a-441">Propojená služba na vyžádání **Azure HDInsight** spojuje cluster na vyžádání HDInsight Hadoop s objektem pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="eea4a-441">**Azure HDInsight** on-demand linked service to link an on-demand HDInsight Hadoop cluster to the data factory.</span></span> <span data-ttu-id="eea4a-442">Služba Azure Data Factory vytvoří cluster HDInsight Hadoop ve chvíli, kdy je potřeba zpracovat vstupní data a vygenerovat výstupní data.</span><span class="sxs-lookup"><span data-stu-id="eea4a-442">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time to process input data and produce output data.</span></span>
3. <span data-ttu-id="eea4a-443">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-443">Created two **datasets**, which describe input and output data for HDInsight Hive activity in the pipeline.</span></span>
4. <span data-ttu-id="eea4a-444">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="eea4a-444">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="eea4a-445">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eea4a-445">Next Steps</span></span>
<span data-ttu-id="eea4a-446">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="eea4a-446">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="eea4a-447">Pokud chcete zjistit, jak pomocí aktivity kopírování zkopírovat data z Azure Blob do Azure SQL, projděte si článek [Kurz: Kopírování dat z Azure Blob do Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-447">To see how to use a Copy Activity to copy data from an Azure Blob to Azure SQL, see [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="eea4a-448">Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="eea4a-448">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="eea4a-449">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="eea4a-449">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 


## <a name="see-also"></a><span data-ttu-id="eea4a-450">Viz také</span><span class="sxs-lookup"><span data-stu-id="eea4a-450">See Also</span></span>
| <span data-ttu-id="eea4a-451">Téma</span><span class="sxs-lookup"><span data-stu-id="eea4a-451">Topic</span></span> | <span data-ttu-id="eea4a-452">Popis</span><span class="sxs-lookup"><span data-stu-id="eea4a-452">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="eea4a-453">Kanály</span><span class="sxs-lookup"><span data-stu-id="eea4a-453">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="eea4a-454">Tento článek vám pomůže pochopit kanály a aktivity ve službě Azure Data Factory a porozumět tomu, jak se dají ve vaší situaci nebo firmě použít k sestavení pracovních postupů založených na datech.</span><span class="sxs-lookup"><span data-stu-id="eea4a-454">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="eea4a-455">Datové sady</span><span class="sxs-lookup"><span data-stu-id="eea4a-455">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="eea4a-456">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-456">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="eea4a-457">Aktivity transformace dat</span><span class="sxs-lookup"><span data-stu-id="eea4a-457">Data Transformation Activities</span></span>](data-factory-data-transformation-activities.md) |<span data-ttu-id="eea4a-458">Tento článek obsahuje seznam aktivit transformace dat (třeba transformaci HDInsight Hive, kterou jste použili v tomto kurzu) podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-458">This article provides a list of data transformation activities (such as HDInsight Hive transformation you used in this tutorial) supported by Azure Data Factory.</span></span> |
| [<span data-ttu-id="eea4a-459">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="eea4a-459">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="eea4a-460">Tento článek vysvětluje aspekty plánování a provádění aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="eea4a-460">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="eea4a-461">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="eea4a-461">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="eea4a-462">Tento článek popisuje, jak monitorovat, spravovat a ladit kanály pomocí aplikace pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="eea4a-462">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |
