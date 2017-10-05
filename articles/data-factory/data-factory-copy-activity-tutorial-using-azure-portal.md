---
title: "Kurz: Vytvoření kanálu Azure Data Factory pro kopírování dat (portál Azure Portal) | Dokumentace Microsoftu"
description: "V tomto kurzu pomocí portálu Azure Portal vytvoříte kanál Azure Data Factory s aktivitou kopírování pro kopírování dat z úložiště objektů blob v Azure do databáze Azure SQL."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 8072a863fab0b304ccbbba639aa56b403e8f37c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-use-azure-portal-to-create-a-data-factory-pipeline-to-copy-data"></a><span data-ttu-id="66892-103">Kurz: Použití portálu Azure Portal k vytvoření kanálu Data Factory pro kopírování dat</span><span class="sxs-lookup"><span data-stu-id="66892-103">Tutorial: Use Azure portal to create a Data Factory pipeline to copy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66892-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="66892-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="66892-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="66892-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="66892-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="66892-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="66892-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66892-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="66892-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66892-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="66892-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="66892-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="66892-110">REST API</span><span class="sxs-lookup"><span data-stu-id="66892-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="66892-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="66892-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="66892-112">V tomto článku se naučíte, jak použít portál [Azure Portal](https://portal.azure.com) k vytvoření datové továrny s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-112">In this article, you learn how to use [Azure portal](https://portal.azure.com) to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="66892-113">Pokud s Azure Data Factory začínáte, přečtěte si článek [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto kurzem začnete.</span><span class="sxs-lookup"><span data-stu-id="66892-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="66892-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="66892-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="66892-115">Aktivita kopírování kopíruje data z podporovaného úložiště dat do podporovaného úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="66892-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="66892-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="66892-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="66892-117">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="66892-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="66892-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="66892-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="66892-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="66892-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="66892-120">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="66892-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="66892-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="66892-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="66892-122">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="66892-122">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="66892-123">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="66892-123">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66892-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66892-124">Prerequisites</span></span>
<span data-ttu-id="66892-125">Než se do tohoto kurzu pustíte, dokončete požadované kroky uvedené v [požadavcích kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="66892-125">Complete prerequisites listed in the [tutorial prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article before performing this tutorial.</span></span>

## <a name="steps"></a><span data-ttu-id="66892-126">Kroky</span><span class="sxs-lookup"><span data-stu-id="66892-126">Steps</span></span>
<span data-ttu-id="66892-127">Zde jsou kroky, které provedete v rámci tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="66892-127">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="66892-128">Vytvořte **datovou továrnu** Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-128">Create an Azure **data factory**.</span></span> <span data-ttu-id="66892-129">V tomto kroku vytvoříte datovou továrnu s názvem ADFTutorialDataFactory.</span><span class="sxs-lookup"><span data-stu-id="66892-129">In this step, you create a data factory named ADFTutorialDataFactory.</span></span> 
2. <span data-ttu-id="66892-130">V této datové továrně vytvořte **propojené služby**.</span><span class="sxs-lookup"><span data-stu-id="66892-130">Create **linked services** in the data factory.</span></span> <span data-ttu-id="66892-131">V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="66892-131">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="66892-132">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-132">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="66892-133">V rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) jste vytvořili kontejner a nahráli data do tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="66892-133">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="66892-134">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-134">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="66892-135">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="66892-135">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="66892-136">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.</span><span class="sxs-lookup"><span data-stu-id="66892-136">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   
3. <span data-ttu-id="66892-137">Vytvořte v datové továrně vstupní a výstupní **datové sady**.</span><span class="sxs-lookup"><span data-stu-id="66892-137">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="66892-138">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-138">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="66892-139">A vstupní datová sada objektu blob určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="66892-139">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="66892-140">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="66892-140">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="66892-141">A výstupní datová sada tabulky SQL určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="66892-141">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
4. <span data-ttu-id="66892-142">Vytvořte v datové továrně **kanál**.</span><span class="sxs-lookup"><span data-stu-id="66892-142">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="66892-143">V tomto kroku pomocí aktivity kopírování vytvoříte kanál.</span><span class="sxs-lookup"><span data-stu-id="66892-143">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="66892-144">Aktivita kopírování kopíruje data z objektu blob v úložišti objektů blob v Azure do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="66892-144">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="66892-145">Aktivitu kopírování můžete v kanálu použít ke kopírování dat z jakéhokoli podporovaného zdroje do jakéhokoli podporovaného cíle.</span><span class="sxs-lookup"><span data-stu-id="66892-145">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="66892-146">Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="66892-146">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
5. <span data-ttu-id="66892-147">Monitorujte kanál.</span><span class="sxs-lookup"><span data-stu-id="66892-147">Monitor the pipeline.</span></span> <span data-ttu-id="66892-148">V tomto kroku budete **monitorovat** řezy vstupních a výstupních datových sad pomocí portálu Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="66892-148">In this step, you **monitor** the slices of input and output datasets by using Azure portal.</span></span> 

## <a name="create-data-factory"></a><span data-ttu-id="66892-149">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="66892-149">Create data factory</span></span>
> [!IMPORTANT]
> <span data-ttu-id="66892-150">Pokud jste tak ještě neučinili, dokončete [požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="66892-150">Complete [prerequisites for the tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) if you haven't already done so.</span></span>   

<span data-ttu-id="66892-151">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="66892-151">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="66892-152">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="66892-152">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="66892-153">Může obsahovat například aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="66892-153">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="66892-154">V tomto kroku začneme vytvořením objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="66892-154">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="66892-155">Po přihlášení k portálu [Azure Portal](https://portal.azure.com/) klikněte v levé nabídce na **Nový**, vyberte **Data + analýza** a klikněte na **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="66892-155">After logging in to the [Azure portal](https://portal.azure.com/), click **New** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span> 
   
   ![Nový -> Objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. <span data-ttu-id="66892-157">V okně **Nový objekt pro vytváření dat**:</span><span class="sxs-lookup"><span data-stu-id="66892-157">In the **New data factory** blade:</span></span>
   
   1. <span data-ttu-id="66892-158">Do pole **Název** zadejte **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="66892-158">Enter **ADFTutorialDataFactory** for the **name**.</span></span> 
      
         ![Okno Nový objekt pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       <span data-ttu-id="66892-160">Název objektu pro vytváření dat Azure musí být **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="66892-160">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="66892-161">Pokud se zobrazí následující chyba, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="66892-161">If you receive the following error, change the name of the data factory (for example, yournameADFTutorialDataFactory) and try creating again.</span></span> <span data-ttu-id="66892-162">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66892-162">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Název objektu pro vytváření dat není k dispozici](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. <span data-ttu-id="66892-164">Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="66892-164">Select your Azure **subscription** in which you want to create the data factory.</span></span> 
   3. <span data-ttu-id="66892-165">Pro **Skupinu prostředků** proveďte jeden z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="66892-165">For the **Resource Group**, do one of the following steps:</span></span>
      
      - <span data-ttu-id="66892-166">Vyberte **Použít existující** a z rozevíracího seznamu vyberte existující skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="66892-166">Select **Use existing**, and select an existing resource group from the drop-down list.</span></span> 
      - <span data-ttu-id="66892-167">Vyberte **Vytvořit novou** a zadejte název skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="66892-167">Select **Create new**, and enter the name of a resource group.</span></span>   
         
          <span data-ttu-id="66892-168">Některé kroky v tomto kurzu vychází z předpokladu, že pro skupinu prostředků použijete název **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="66892-168">Some of the steps in this tutorial assume that you use the name: **ADFTutorialResourceGroup** for the resource group.</span></span> <span data-ttu-id="66892-169">Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="66892-169">To learn about resource groups, see [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md).</span></span>  
   4. <span data-ttu-id="66892-170">Vyberte **umístění** pro objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="66892-170">Select the **location** for the data factory.</span></span> <span data-ttu-id="66892-171">V rozevíracím seznamu jsou uvedené pouze oblasti podporované službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66892-171">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
   5. <span data-ttu-id="66892-172">Zaškrtněte **Připnout na řídicí panel**.</span><span class="sxs-lookup"><span data-stu-id="66892-172">Select **Pin to dashboard**.</span></span>     
   6. <span data-ttu-id="66892-173">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="66892-173">Click **Create**.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="66892-174">Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="66892-174">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
      > 
      > <span data-ttu-id="66892-175">Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="66892-175">The name of the data factory may be registered as a DNS name in the future and hence become publically visible.</span></span>                
      > 
      > 
3. <span data-ttu-id="66892-176">Na řídicím panelu vidíte následující dlaždice se statusem: **Nasazování datové továrny**.</span><span class="sxs-lookup"><span data-stu-id="66892-176">On the dashboard, you see the following tile with status: **Deploying data factory**.</span></span> 

    ![nasazování dlaždice datové továrny](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. <span data-ttu-id="66892-178">Po vytvoření se zobrazí okno **Objekt pro vytváření dat**, jak je znázorněno na obrázku.</span><span class="sxs-lookup"><span data-stu-id="66892-178">After the creation is complete, you see the **Data Factory** blade as shown in the image.</span></span>
   
   ![Domovská stránka objektu pro vytváření dat](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a><span data-ttu-id="66892-180">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="66892-180">Create linked services</span></span>
<span data-ttu-id="66892-181">V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-181">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="66892-182">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="66892-182">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="66892-183">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="66892-183">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="66892-184">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="66892-184">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="66892-185">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-185">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="66892-186">Tento účet úložiště je ten, ve kterém jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili kontejner a nahráli do něj data.</span><span class="sxs-lookup"><span data-stu-id="66892-186">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="66892-187">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-187">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="66892-188">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="66892-188">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="66892-189">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku emp.</span><span class="sxs-lookup"><span data-stu-id="66892-189">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="66892-190">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="66892-190">Create Azure Storage linked service</span></span>
<span data-ttu-id="66892-191">V tomto kroku propojíte se svou datovou továrnou účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-191">In this step, you link your Azure storage account to your data factory.</span></span> <span data-ttu-id="66892-192">V tomto oddílu zadáte název a klíč svého účtu služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="66892-192">You specify the name and key of your Azure storage account in this section.</span></span>  

1. <span data-ttu-id="66892-193">V okně **Data Factory** klikněte na dlaždici **Vytvořit a nasadit**.</span><span class="sxs-lookup"><span data-stu-id="66892-193">In the **Data Factory** blade, click **Author and deploy** tile.</span></span>
   
   ![Dlaždice Vytvořit a nasadit](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. <span data-ttu-id="66892-195">Zobrazí se **Data Factory Editor**, jak je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="66892-195">You see the **Data Factory Editor** as shown in the following image:</span></span> 

    ![Data Factory Editor](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. <span data-ttu-id="66892-197">V editoru klikněte na panelu nástrojů na tlačítko **Nové datové úložiště** a z rozevírací nabídky vyberte **Úložiště Azure**.</span><span class="sxs-lookup"><span data-stu-id="66892-197">In the editor, click **New data store** button on the toolbar and select **Azure storage** from the drop-down menu.</span></span> <span data-ttu-id="66892-198">V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="66892-198">You should see the JSON template for creating an Azure storage linked service in the right pane.</span></span> 
   
    ![Tlačítko Nové datové úložiště v editoru](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. <span data-ttu-id="66892-200">Hodnoty `<accountname>` a `<accountkey>` nahraďte názvem účtu služby Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="66892-200">Replace `<accountname>` and `<accountkey>` with the account name and account key values for your Azure storage account.</span></span> 
   
    ![Editor – Blob Storage – JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. <span data-ttu-id="66892-202">Na panelu nástrojů klikněte na **Nasadit**.</span><span class="sxs-lookup"><span data-stu-id="66892-202">Click **Deploy** on the toolbar.</span></span> <span data-ttu-id="66892-203">Nyní byste měli v zobrazení stromu vidět nasazenou službu **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="66892-203">You should see the deployed **AzureStorageLinkedService** in the tree view now.</span></span> 
   
    ![Editor – Blob Storage – nasazení](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    <span data-ttu-id="66892-205">Další informace o vlastnostech JSON použitých v definici propojené služby najdete v článku[Konektor služby Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="66892-205">For more information about JSON properties in the linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-a-linked-service-for-the-azure-sql-database"></a><span data-ttu-id="66892-206">Vytvoření propojené služby pro Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="66892-206">Create a linked service for the Azure SQL Database</span></span>
<span data-ttu-id="66892-207">V tomto kroku se svým objektem pro vytváření dat propojíte svou databázi SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-207">In this step, you link your Azure SQL database to your data factory.</span></span> <span data-ttu-id="66892-208">V tomto oddílu zadáte název serveru Azure SQL, název databáze, uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="66892-208">You specify the Azure SQL server name, database name, user name, and user password in this section.</span></span> 

1. <span data-ttu-id="66892-209">V **editoru služby Data Factory** klikněte na panelu nástrojů na tlačítko **Nové úložiště dat** a z rozevírací nabídky vyberte **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="66892-209">In the **Data Factory Editor**, click **New data store** button on the toolbar and select **Azure SQL Database** from the drop-down menu.</span></span> <span data-ttu-id="66892-210">V pravém podokně by se měla zobrazit šablona JSON pro vytvoření propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="66892-210">You should see the JSON template for creating the Azure SQL linked service in the right pane.</span></span>
2. <span data-ttu-id="66892-211">Hodnoty `<servername>`, `<databasename>`, `<username>@<servername>` a `<password>` nahraďte názvy serveru Azure SQL, databáze, uživatelského účtu a heslem.</span><span class="sxs-lookup"><span data-stu-id="66892-211">Replace `<servername>`, `<databasename>`, `<username>@<servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span> 
3. <span data-ttu-id="66892-212">Službu **AzureSqlLinkedService** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="66892-212">Click **Deploy** on the toolbar to create and deploy the **AzureSqlLinkedService**.</span></span>
4. <span data-ttu-id="66892-213">Zkontrolujte, jestli se služba **AzureSqlLinkedService** objevila v zobrazení stromu v části **Propojené služby**.</span><span class="sxs-lookup"><span data-stu-id="66892-213">Confirm that you see **AzureSqlLinkedService** in the tree view under **Linked services**.</span></span>  

    <span data-ttu-id="66892-214">Další informace o těchto vlastnostech JSON najdete v článku [Konektor služby Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="66892-214">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>

## <a name="create-datasets"></a><span data-ttu-id="66892-215">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="66892-215">Create datasets</span></span>
<span data-ttu-id="66892-216">V předchozím kroku jste vytvořili propojené služby, abyste propojili účet úložiště Azure a Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="66892-216">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="66892-217">V tomto kroku nadefinujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují služby AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="66892-217">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

<span data-ttu-id="66892-218">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-218">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="66892-219">A vstupní datová sada objektu blob (InputDataset) určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="66892-219">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="66892-220">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="66892-220">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="66892-221">A výstupní datová sada tabulky SQL (OutputDataset) určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="66892-221">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="66892-222">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="66892-222">Create input dataset</span></span>
<span data-ttu-id="66892-223">V tomto kroku vytvoříte datovou sadu s názvem InputDataset, která odkazuje na soubor blob (emp.txt) v kořenové složce kontejneru objektů blob (adftutorial), který se nachází ve službě Azure Storage reprezentované propojenou službou AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="66892-223">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="66892-224">Pokud neurčíte hodnotu fileName (nebo ji přeskočíte), data ze všech objektů blob ve vstupní složce se zkopírují do cíle.</span><span class="sxs-lookup"><span data-stu-id="66892-224">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="66892-225">V tomto kurzu určíte hodnotu fileName.</span><span class="sxs-lookup"><span data-stu-id="66892-225">In this tutorial, you specify a value for the fileName.</span></span> 

1. <span data-ttu-id="66892-226">V **editoru** služby Data Factory klikněte na **... Další**, klikněte na **Nová datová sada** a v rozevíracím seznamu klikněte na **Azure Blob Storage**.</span><span class="sxs-lookup"><span data-stu-id="66892-226">In the **Editor** for the Data Factory, click **... More**, click **New dataset**, and click **Azure Blob storage** from the drop-down menu.</span></span> 
   
    ![Nabídka Nová datová sada](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. <span data-ttu-id="66892-228">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="66892-228">Replace JSON in the right pane with the following JSON snippet:</span></span> 
   
    ```json
    {
      "name": "InputDataset",
      "properties": {
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
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```   

    <span data-ttu-id="66892-229">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="66892-229">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="66892-230">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="66892-230">Property</span></span> | <span data-ttu-id="66892-231">Popis</span><span class="sxs-lookup"><span data-stu-id="66892-231">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="66892-232">type</span><span class="sxs-lookup"><span data-stu-id="66892-232">type</span></span> | <span data-ttu-id="66892-233">Vlastnost type je nastavená na hodnotu **AzureBlob**, protože se data nachází ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="66892-233">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="66892-234">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="66892-234">linkedServiceName</span></span> | <span data-ttu-id="66892-235">Odkazuje na službu **AzureStorageLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="66892-235">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="66892-236">folderPath</span><span class="sxs-lookup"><span data-stu-id="66892-236">folderPath</span></span> | <span data-ttu-id="66892-237">Určuje **kontejner** objektů blob a **složku** obsahující vstupní objekty blob.</span><span class="sxs-lookup"><span data-stu-id="66892-237">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="66892-238">V tomto kurzu je adftutorial kontejnerem objektů blob a složka je kořenová složka.</span><span class="sxs-lookup"><span data-stu-id="66892-238">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="66892-239">fileName</span><span class="sxs-lookup"><span data-stu-id="66892-239">fileName</span></span> | <span data-ttu-id="66892-240">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="66892-240">This property is optional.</span></span> <span data-ttu-id="66892-241">Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath.</span><span class="sxs-lookup"><span data-stu-id="66892-241">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="66892-242">V tomto kurzu má fileName hodnotu **emp.txt**, takže se zpracuje pouze tento soubor.</span><span class="sxs-lookup"><span data-stu-id="66892-242">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="66892-243">format -> type</span><span class="sxs-lookup"><span data-stu-id="66892-243">format -> type</span></span> |<span data-ttu-id="66892-244">Vstupní soubor je v textovém formátu, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="66892-244">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="66892-245">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="66892-245">columnDelimiter</span></span> | <span data-ttu-id="66892-246">Sloupce ve vstupním souboru jsou oddělené **znakem čárky (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="66892-246">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="66892-247">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="66892-247">frequency/interval</span></span> | <span data-ttu-id="66892-248">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, takže vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="66892-248">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="66892-249">Jinými slovy služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce kontejneru objektů blob (**adftutorial**), který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="66892-249">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="66892-250">Vyhledává data v rámci kanálu mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="66892-250">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="66892-251">external</span><span class="sxs-lookup"><span data-stu-id="66892-251">external</span></span> | <span data-ttu-id="66892-252">Pokud data nevygeneroval tento kanál, je tato vlastnost nastavená na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="66892-252">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="66892-253">Vstupní data v tomto kurzu jsou v souboru emp.txt, který není generován tímto kanálem, proto jsme tuto vlastnost nastavili na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="66892-253">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="66892-254">Další informace o těchto vlastnostech JSON najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="66892-254">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>      
3. <span data-ttu-id="66892-255">Datovou sadu **InputDataset** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="66892-255">Click **Deploy** on the toolbar to create and deploy the **InputDataset** dataset.</span></span> <span data-ttu-id="66892-256">Zkontrolujte, jestli se datová sada **InputDataset** objevila v zobrazení stromu.</span><span class="sxs-lookup"><span data-stu-id="66892-256">Confirm that you see the **InputDataset** in the tree view.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="66892-257">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="66892-257">Create output dataset</span></span>
<span data-ttu-id="66892-258">Propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="66892-258">The Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="66892-259">Výstupní datová sada tabulky SQL (OutputDataset), kterou v tomto kroku vytvoříte, určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="66892-259">The output SQL table dataset (OututDataset) you create in this step specifies the table in the database to which the data from the blob storage is copied.</span></span>

1. <span data-ttu-id="66892-260">V **editoru** služby Data Factory klikněte na **... Další**, klikněte na **Nová datová sada** a v rozevíracím seznamu klikněte na **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="66892-260">In the **Editor** for the Data Factory, click **... More**, click **New dataset**, and click **Azure SQL** from the drop-down menu.</span></span> 
2. <span data-ttu-id="66892-261">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="66892-261">Replace JSON in the right pane with the following JSON snippet:</span></span>

    ```json   
    {
      "name": "OutputDataset",
      "properties": {
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
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "emp"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```     

    <span data-ttu-id="66892-262">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="66892-262">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="66892-263">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="66892-263">Property</span></span> | <span data-ttu-id="66892-264">Popis</span><span class="sxs-lookup"><span data-stu-id="66892-264">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="66892-265">type</span><span class="sxs-lookup"><span data-stu-id="66892-265">type</span></span> | <span data-ttu-id="66892-266">Vlastnost type je nastavena na hodnotu **AzureSqlTable**, protože data se kopírují do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="66892-266">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="66892-267">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="66892-267">linkedServiceName</span></span> | <span data-ttu-id="66892-268">Odkazuje na službu **AzureSqlLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="66892-268">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="66892-269">tableName</span><span class="sxs-lookup"><span data-stu-id="66892-269">tableName</span></span> | <span data-ttu-id="66892-270">Určuje **tabulku**, do které se kopírují data.</span><span class="sxs-lookup"><span data-stu-id="66892-270">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="66892-271">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="66892-271">frequency/interval</span></span> | <span data-ttu-id="66892-272">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, což znamená, že výstupní řezy se tvoří **každou hodinu** mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="66892-272">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="66892-273">V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**.</span><span class="sxs-lookup"><span data-stu-id="66892-273">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="66892-274">ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).</span><span class="sxs-lookup"><span data-stu-id="66892-274">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="66892-275">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="66892-275">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="66892-276">Datovou sadu **OutputDataset** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="66892-276">Click **Deploy** on the toolbar to create and deploy the **OutputDataset** dataset.</span></span> <span data-ttu-id="66892-277">Zkontrolujte, jestli se datová sada **OutputDataset** objevila v zobrazení stromu v části **Datové sady**.</span><span class="sxs-lookup"><span data-stu-id="66892-277">Confirm that you see the **OutputDataset** in the tree view under **Datasets**.</span></span> 

## <a name="create-pipeline"></a><span data-ttu-id="66892-278">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="66892-278">Create pipeline</span></span>
<span data-ttu-id="66892-279">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="66892-279">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="66892-280">Výstupní datové sady v současné době řídí plán.</span><span class="sxs-lookup"><span data-stu-id="66892-280">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="66892-281">V tomto kurzu je výstupní datová sada nakonfigurovaná tak, aby vytvářela řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="66892-281">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="66892-282">Kanál má čas spuštění a čas ukončení nastavený jeden den od sebe, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="66892-282">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="66892-283">Proto kanál vytvoří 24 řezů výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="66892-283">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 

1. <span data-ttu-id="66892-284">V **editoru** služby Data Factory klikněte na **... Další** a poté na **Nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="66892-284">In the **Editor** for the Data Factory, click **... More**, and click **New pipeline**.</span></span> <span data-ttu-id="66892-285">Případně můžete ve stromovém zobrazení kliknout pravým tlačítkem na **anály** a pak kliknout na **Nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="66892-285">Alternatively, you can right-click **Pipelines** in the tree view and click **New pipeline**.</span></span>
2. <span data-ttu-id="66892-286">Nahraďte kód JSON v pravém podokně následujícím fragmentem kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="66892-286">Replace JSON in the right pane with the following JSON snippet:</span></span> 

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
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
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```   
    
    <span data-ttu-id="66892-287">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="66892-287">Note the following points:</span></span>
   
    - <span data-ttu-id="66892-288">V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.</span><span class="sxs-lookup"><span data-stu-id="66892-288">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="66892-289">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="66892-289">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="66892-290">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="66892-290">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="66892-291">Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="66892-291">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="66892-292">V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="66892-292">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="66892-293">Úplný seznam úložišť dat podporovaných aktivitou kopírování jako zdroje a jímky najdete v tématu [podporovaných úložištích dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="66892-293">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="66892-294">Kliknutím na odkaz v tabulce se dozvíte, jak použít konkrétní podporovaná úložiště dat jako zdroj/jímku.</span><span class="sxs-lookup"><span data-stu-id="66892-294">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>
    - <span data-ttu-id="66892-295">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="66892-295">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="66892-296">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="66892-296">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="66892-297">Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme.</span><span class="sxs-lookup"><span data-stu-id="66892-297">The **end** time is optional, but we use it in this tutorial.</span></span> <span data-ttu-id="66892-298">Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“.</span><span class="sxs-lookup"><span data-stu-id="66892-298">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="66892-299">Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.</span><span class="sxs-lookup"><span data-stu-id="66892-299">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="66892-300">V předchozím příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="66892-300">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="66892-301">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="66892-301">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="66892-302">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="66892-302">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="66892-303">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="66892-303">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="66892-304">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="66892-304">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>
3. <span data-ttu-id="66892-305">Kanál **ADFTutorialPipeline** vytvoříte a nasadíte kliknutím na **Nasadit** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="66892-305">Click **Deploy** on the toolbar to create and deploy the **ADFTutorialPipeline**.</span></span> <span data-ttu-id="66892-306">Zkontrolujte, jestli se kanál objevil v zobrazení stromu.</span><span class="sxs-lookup"><span data-stu-id="66892-306">Confirm that you see the pipeline in the tree view.</span></span> 
4. <span data-ttu-id="66892-307">Teď zavřete okno **Editor** kliknutím na **X**. Chcete-li zobrazit domovskou stránku **objektu pro vytváření dat** pro **ADFTutorialDataFactory**, znovu klikněte na **X**.</span><span class="sxs-lookup"><span data-stu-id="66892-307">Now, close the **Editor** blade by clicking **X**. Click **X** again to see the **Data Factory** home page for the **ADFTutorialDataFactory**.</span></span>

<span data-ttu-id="66892-308">**Blahopřejeme!**</span><span class="sxs-lookup"><span data-stu-id="66892-308">**Congratulations!**</span></span> <span data-ttu-id="66892-309">Úspěšně jste vytvořili datovou továrnu Azure s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="66892-309">You have successfully created an Azure data factory with a pipeline to copy data from an Azure blob storage to an Azure SQL database.</span></span> 


## <a name="monitor-pipeline"></a><span data-ttu-id="66892-310">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="66892-310">Monitor pipeline</span></span>
<span data-ttu-id="66892-311">V tomto kroku budete pomocí webu Azure Portal monitorovat, co se děje v objektu pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-311">In this step, you use the Azure portal to monitor what’s going on in an Azure data factory.</span></span>    

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="66892-312">Monitorování kanálu pomocí aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="66892-312">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="66892-313">Následující kroky ukazují, jak v datové továrně monitorovat kanály pomocí aplikace pro monitorování a správu:</span><span class="sxs-lookup"><span data-stu-id="66892-313">The following steps show you how to monitor pipelines in your data factory by using the Monitor & Manage application:</span></span> 

1. <span data-ttu-id="66892-314">Na domovské stránce svého objektu pro vytváření dat klikněte na dlaždici **Monitorování a správa**.</span><span class="sxs-lookup"><span data-stu-id="66892-314">Click **Monitor & Manage** tile on the home page for your data factory.</span></span>
   
    ![Dlaždice Monitorování a správa](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. <span data-ttu-id="66892-316">Na samostatné kartě by se měla zobrazit **aplikace pro monitorování a správu**.</span><span class="sxs-lookup"><span data-stu-id="66892-316">You should see **Monitor & Manage application** in a separate tab.</span></span> 

    > [!NOTE]
    > <span data-ttu-id="66892-317">Pokud zjistíte, že se webový prohlížeč zasekl ve fázi „Autorizace…“, proveďte jeden z následujících kroků: zrušte zaškrtnutí políčka **Block third party cookies and site data** (Blokovat data souborů cookie a webů třetích stran) nebo vytvořte výjimku pro **login.microsoftonline.com** a potom zkuste aplikaci znovu otevřít.</span><span class="sxs-lookup"><span data-stu-id="66892-317">If you see that the web browser is stuck at "Authorizing...", do one of the following: clear the **Block third-party cookies and site data** check box (or) create an exception for **login.microsoftonline.com**, and then try to open the app again.</span></span>

    ![Aplikace pro monitorování a správu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. <span data-ttu-id="66892-319">Změňte hodnoty **Čas spuštění** a **Čas ukončení** tak, aby zahrnovaly časy spuštění (11. 5. 2017) a ukončení (12. 5. 2017) vašeho kanálu, potom klikněte na **Použít**.</span><span class="sxs-lookup"><span data-stu-id="66892-319">Change the **Start time** and **End time** to include start (2017-05-11) and end times (2017-05-12) of your pipeline, and click **Apply**.</span></span>       
3. <span data-ttu-id="66892-320">Na seznamu v prostředním podokně se zobrazí **okna aktivit** spojená s každou hodinou mezi časem spuštění a časem ukončení.</span><span class="sxs-lookup"><span data-stu-id="66892-320">You see the **activity windows** associated with each hour between pipeline start and end times in the list in the middle pane.</span></span> 
4. <span data-ttu-id="66892-321">Pokud chcete zobrazit podrobnosti o okně aktivity, vyberte ho v seznamu **Okna aktivit**.</span><span class="sxs-lookup"><span data-stu-id="66892-321">To see details about an activity window, select the activity window in the **Activity Windows** list.</span></span> 
    <span data-ttu-id="66892-322">![Podrobnosti o okně aktivity](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="66892-322">![Activity window details](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)</span></span>

    <span data-ttu-id="66892-323">V Průzkumníku okna aktivity napravo uvidíte, že řezy do aktuálního času UTC (20:12) jsou všechny zpracované (v zelené barvě).</span><span class="sxs-lookup"><span data-stu-id="66892-323">In Activity Window Explorer on the right, you see that the slices up to the current UTC time (8:12 PM) are all processed (in green color).</span></span> <span data-ttu-id="66892-324">Řezy v časech 20:00–21:00, 21:00–22:00, 22:00–23:00 a 23:00–00:00 ještě zpracované nebyly.</span><span class="sxs-lookup"><span data-stu-id="66892-324">The 8-9 PM, 9 - 10 PM, 10 - 11 PM, 11 PM - 12 AM slices are not processed yet.</span></span>

    <span data-ttu-id="66892-325">Oddíl **Pokusy** v pravém podokně poskytuje informace o spuštění aktivit pro datové řezy.</span><span class="sxs-lookup"><span data-stu-id="66892-325">The **Attempts** section in the right pane provides information about the activity run for the data slice.</span></span> <span data-ttu-id="66892-326">V případě, že došlo k chybě, poskytuje o ní podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="66892-326">If there was an error, it provides details about the error.</span></span> <span data-ttu-id="66892-327">Například pokud vstupní složka nebo kontejner neexistuje a zpracování řezu se nezdaří, zobrazí chybová zpráva s oznámením, že kontejner nebo složka neexistuje.</span><span class="sxs-lookup"><span data-stu-id="66892-327">For example, if the input folder or container does not exist and the slice processing fails, you see an error message stating that the container or folder does not exist.</span></span>

    ![Pokusy spuštění aktivit](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. <span data-ttu-id="66892-329">Spusťte **SQL Server Management Studio**, připojte se ke službě Azure SQL Database a ověřte, že se řádky vložily do tabulky **emp** v databázi.</span><span class="sxs-lookup"><span data-stu-id="66892-329">Launch **SQL Server Management Studio**, connect to the Azure SQL Database, and verify that the rows are inserted in to the **emp** table in the database.</span></span>
    
    ![Výsledky dotazu SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

<span data-ttu-id="66892-331">Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="66892-331">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="66892-332">Monitorování kanálu pomocí Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="66892-332">Monitor pipeline using Diagram View</span></span>
<span data-ttu-id="66892-333">Datové kanály můžete také monitorovat pomocí zobrazení diagramu.</span><span class="sxs-lookup"><span data-stu-id="66892-333">You can also monitor data pipelines by using the diagram view.</span></span>  

1. <span data-ttu-id="66892-334">V okně **Objekt pro vytváření dat** klikněte na **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="66892-334">In the **Data Factory** blade, click **Diagram**.</span></span>
   
    ![Okno objekt pro vytváření dat – dlaždice Diagram](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. <span data-ttu-id="66892-336">Zobrazený diagram by měl vypadat přibližně jako na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="66892-336">You should see the diagram similar to the following image:</span></span> 
   
    ![Zobrazení diagramu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. <span data-ttu-id="66892-338">V zobrazení diagramu si dvojitým kliknutím na **InputDataset** zobrazíte řezy pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="66892-338">In the diagram view, double-click **InputDataset** to see slices for the dataset.</span></span>  
   
    ![Datové sady s vybranou sadou InputDataset](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. <span data-ttu-id="66892-340">Kliknutím na odkaz **Zobrazit více** zobrazíte všechny datové řezy.</span><span class="sxs-lookup"><span data-stu-id="66892-340">Click **See more** link to see all the data slices.</span></span> <span data-ttu-id="66892-341">Uvidíte 24 hodinových řezů mezi časem spuštění a ukončení.</span><span class="sxs-lookup"><span data-stu-id="66892-341">You see 24 hourly slices between pipeline start and end times.</span></span> 
   
    ![Všechny vstupní datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    <span data-ttu-id="66892-343">Všimněte si, že všechny datové řezy až do aktuálního času UTC jsou ve stavu **Připraveno**, protože soubor **emp.txt** celou dobu existuje v kontejneru objektů blob: **adftutorial\input**.</span><span class="sxs-lookup"><span data-stu-id="66892-343">Notice that all the data slices up to the current UTC time are **Ready** because the **emp.txt** file exists all the time in the blob container: **adftutorial\input**.</span></span> <span data-ttu-id="66892-344">Řezy budoucích časů ještě ve stavu připraveno nejsou.</span><span class="sxs-lookup"><span data-stu-id="66892-344">The slices for the future times are not in ready state yet.</span></span> <span data-ttu-id="66892-345">Potvrďte, že se žádné řezy nezobrazují v části **Řezy, které v poslední době selhaly** dole.</span><span class="sxs-lookup"><span data-stu-id="66892-345">Confirm that no slices show up in the **Recently failed slices** section at the bottom.</span></span>
6. <span data-ttu-id="66892-346">Pozavírejte tato okna, dokud neuvidíte zobrazení diagramu nebo si zobrazení diagramu zobrazte posunutím vlevo.</span><span class="sxs-lookup"><span data-stu-id="66892-346">Close the blades until you see the diagram view (or) scroll left to see the diagram view.</span></span> <span data-ttu-id="66892-347">Potom dvakrát klikněte na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="66892-347">Then, double-click **OutputDataset**.</span></span> 
8. <span data-ttu-id="66892-348">Pokud chcete zobrazit všechny řezy, klikněte na odkaz **Zobrazit více** v okně **tabulky** pro **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="66892-348">Click **See more** link on the **Table** blade for **OutputDataset** to see all the slices.</span></span>

    ![okno datové řezy](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. <span data-ttu-id="66892-350">Všimněte si, že všechny řezy až do aktuálního času UTC se přesunuly ze stavu **Čeká na provedení** => **Probíhá** ==> **Připraveno**.</span><span class="sxs-lookup"><span data-stu-id="66892-350">Notice that all the slices up to the current UTC time move from **pending execution** state => **In progress** ==> **Ready** state.</span></span> <span data-ttu-id="66892-351">Ve výchozím nastavení se řezy z minulosti (před aktuálním časem) zpracovávají od nejnovějšího po nejstarší.</span><span class="sxs-lookup"><span data-stu-id="66892-351">The slices from the past (before current time) are processed from latest to oldest by default.</span></span> <span data-ttu-id="66892-352">Například pokud je aktuální čas UTC 20:12, řez pro dobu 19:00–20:00 se zpracuje před řezem z doby 18:00–19:00.</span><span class="sxs-lookup"><span data-stu-id="66892-352">For example, if the current time is 8:12 PM UTC, the slice for 7 PM - 8 PM is processed ahead of the 6 PM - 7 PM slice.</span></span> <span data-ttu-id="66892-353">Ve výchozím nastavení se řez z doby 20:00–21:00 zpracuje na konci časového intervalu, což je po 21:00.</span><span class="sxs-lookup"><span data-stu-id="66892-353">The 8 PM - 9 PM slice is processed at the end of the time interval by default, that is after 9 PM.</span></span>  
10. <span data-ttu-id="66892-354">Klikněte na libovolný datový řez ze seznamu. Mělo by se zobrazit okno **Datový řez**.</span><span class="sxs-lookup"><span data-stu-id="66892-354">Click any data slice from the list and you should see the **Data slice** blade.</span></span> <span data-ttu-id="66892-355">Část dat spojená s oknem aktivity se nazývá řez.</span><span class="sxs-lookup"><span data-stu-id="66892-355">A piece of data associated with an activity window is called a slice.</span></span> <span data-ttu-id="66892-356">Řez může tvořit jeden soubor nebo více souborů.</span><span class="sxs-lookup"><span data-stu-id="66892-356">A slice can be one file or multiple files.</span></span>  
    
     ![okno datový řez](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     <span data-ttu-id="66892-358">Není-li řez ve stavu **Připraveno**, zobrazí se v seznamu **Upstreamové datové řezy, které nejsou připraveny** upstreamové datové řezy, které nejsou připravené a které blokují spuštění aktuálního řezu.</span><span class="sxs-lookup"><span data-stu-id="66892-358">If the slice is not in the **Ready** state, you can see the upstream slices that are not Ready and are blocking the current slice from executing in the **Upstream slices that are not ready** list.</span></span>
11. <span data-ttu-id="66892-359">V okně **DATOVÝ ŘEZ**byste měli v seznamu dole vidět všechna spuštění aktivit.</span><span class="sxs-lookup"><span data-stu-id="66892-359">In the **DATA SLICE** blade, you should see all activity runs in the list at the bottom.</span></span> <span data-ttu-id="66892-360">Kliknutím na **spuštění aktivit** zobrazíte okno **Podrobnosti o spuštění aktivit**.</span><span class="sxs-lookup"><span data-stu-id="66892-360">Click an **activity run** to see the **Activity run details** blade.</span></span> 
    
    ![Podrobnosti o spuštění aktivit](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    <span data-ttu-id="66892-362">V tomto okně vidíte, jak dlouho kopírování trvalo, jaká je propustnost, kolik bajtů dat se přečetlo a zapsalo, počáteční čas spuštění, koncový čas spuštění atd.</span><span class="sxs-lookup"><span data-stu-id="66892-362">In this blade, you see how long the copy operation took, what throughput is, how many bytes of data were read and written, run start time, run end time etc.</span></span>  
12. <span data-ttu-id="66892-363">Klikejte na tlačítko **X**, dokud nezavřete všechna okna a nevrátíte se zpátky do domovského okna pro **ADFTutorialDataFactory**.</span><span class="sxs-lookup"><span data-stu-id="66892-363">Click **X** to close all the blades until you get back to the home blade for the **ADFTutorialDataFactory**.</span></span>
13. <span data-ttu-id="66892-364">(volitelné) pokud chcete zobrazit okna, která jste viděli u předchozích kroků, klikněte na dlaždici **Datové sady** nebo dlaždici **Kanály**.</span><span class="sxs-lookup"><span data-stu-id="66892-364">(optional) click the **Datasets** tile or **Pipelines** tile to get the blades you have seen the preceding steps.</span></span> 
14. <span data-ttu-id="66892-365">Spusťte **SQL Server Management Studio**, připojte se ke službě Azure SQL Database a ověřte, že se řádky vložily do tabulky **emp** v databázi.</span><span class="sxs-lookup"><span data-stu-id="66892-365">Launch **SQL Server Management Studio**, connect to the Azure SQL Database, and verify that the rows are inserted in to the **emp** table in the database.</span></span>
    
    ![Výsledky dotazu SQL](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a><span data-ttu-id="66892-367">Souhrn</span><span class="sxs-lookup"><span data-stu-id="66892-367">Summary</span></span>
<span data-ttu-id="66892-368">V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="66892-368">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="66892-369">Použili jste web Azure Portal k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="66892-369">You used the Azure portal to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="66892-370">Zde jsou základní kroky, které jste v tomto kurzu provedli:</span><span class="sxs-lookup"><span data-stu-id="66892-370">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="66892-371">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="66892-371">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="66892-372">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="66892-372">Created **linked services**:</span></span>
   1. <span data-ttu-id="66892-373">Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="66892-373">An **Azure Storage** linked service to link your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="66892-374">Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data.</span><span class="sxs-lookup"><span data-stu-id="66892-374">An **Azure SQL** linked service to link your Azure SQL database that holds the output data.</span></span> 
3. <span data-ttu-id="66892-375">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="66892-375">Created **datasets** that describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="66892-376">Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="66892-376">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="66892-377">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66892-377">Next steps</span></span>
<span data-ttu-id="66892-378">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="66892-378">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="66892-379">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="66892-379">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="66892-380">Kliknutím na odkaz úložiště dat v tabulce získáte další informace o kopírování dat do nebo z úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="66892-380">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>