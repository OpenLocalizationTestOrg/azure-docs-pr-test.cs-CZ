---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál služby Azure Data Factory s aktivitou kopírování pomocí sady Visual Studio."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: f90b158e45a3679210685765b23c8299eb76ed50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a><span data-ttu-id="f5481-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5481-103">Tutorial: Create a pipeline with Copy Activity using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5481-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="f5481-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="f5481-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="f5481-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="f5481-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f5481-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="f5481-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5481-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="f5481-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f5481-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="f5481-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f5481-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="f5481-110">REST API</span><span class="sxs-lookup"><span data-stu-id="f5481-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="f5481-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="f5481-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="f5481-112">V tomto článku se naučíte, jak používat Microsoft Visual Studio, abyste vytvořili datovou továrnu s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-112">In this article, you learn how to use the Microsoft Visual Studio to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="f5481-113">Pokud s Azure Data Factory začínáte, přečtěte si článek [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto kurzem začnete.</span><span class="sxs-lookup"><span data-stu-id="f5481-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="f5481-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="f5481-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="f5481-115">Aktivita kopírování kopíruje data z podporovaného úložiště dat do podporovaného úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="f5481-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="f5481-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f5481-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="f5481-117">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="f5481-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="f5481-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="f5481-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="f5481-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="f5481-120">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="f5481-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="f5481-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="f5481-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE] 
> <span data-ttu-id="f5481-122">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="f5481-122">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="f5481-123">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-123">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5481-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f5481-124">Prerequisites</span></span>
1. <span data-ttu-id="f5481-125">Přečtěte si článek [Přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a proveďte **nutné** kroky.</span><span class="sxs-lookup"><span data-stu-id="f5481-125">Read through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article and complete the **prerequisite** steps.</span></span>       
2. <span data-ttu-id="f5481-126">Chcete-li vytvářet instance služby Data Factory, musíte být členem role [Přispěvatel Data Factory](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) na úrovni předplatného a skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="f5481-126">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
3. <span data-ttu-id="f5481-127">Na počítači musíte mít nainstalované tyto položky:</span><span class="sxs-lookup"><span data-stu-id="f5481-127">You must have the following installed on your computer:</span></span> 
   * <span data-ttu-id="f5481-128">Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="f5481-128">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="f5481-129">Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="f5481-129">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="f5481-130">Přejděte na [stránku položek ke stažení pro Azure](https://azure.microsoft.com/downloads/) a klikněte na **VS 2013** nebo **VS 2015** v části **.NET**.</span><span class="sxs-lookup"><span data-stu-id="f5481-130">Navigate to [Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in the **.NET** section.</span></span>
   * <span data-ttu-id="f5481-131">Stáhněte si nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="f5481-131">Download the latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="f5481-132">Modul plug-in můžete taky aktualizovat, a to pomocí tohoto postupu: V nabídce klikněte na **Nástroje** -> **Rozšíření a aktualizace** -> **Online** -> **Galerie sady Visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** (Nástroje Microsoft Azure Data Factory pro Visual Studio) -> **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-132">You can also update the plugin by doing the following steps: On the menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

## <a name="steps"></a><span data-ttu-id="f5481-133">Kroky</span><span class="sxs-lookup"><span data-stu-id="f5481-133">Steps</span></span>
<span data-ttu-id="f5481-134">Zde jsou kroky, které provedete v rámci tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f5481-134">Here are the steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="f5481-135">V této datové továrně vytvořte **propojené služby**.</span><span class="sxs-lookup"><span data-stu-id="f5481-135">Create **linked services** in the data factory.</span></span> <span data-ttu-id="f5481-136">V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f5481-136">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="f5481-137">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-137">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="f5481-138">V rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) jste vytvořili kontejner a nahráli data do tohoto účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="f5481-138">You created a container and uploaded data to this storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="f5481-139">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-139">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="f5481-140">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="f5481-140">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="f5481-141">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.</span><span class="sxs-lookup"><span data-stu-id="f5481-141">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>     
2. <span data-ttu-id="f5481-142">Vytvořte v datové továrně vstupní a výstupní **datové sady**.</span><span class="sxs-lookup"><span data-stu-id="f5481-142">Create input and output **datasets** in the data factory.</span></span>  
    
    <span data-ttu-id="f5481-143">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-143">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="f5481-144">A vstupní datová sada objektu blob určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="f5481-144">And, the input blob dataset specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="f5481-145">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="f5481-145">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="f5481-146">A výstupní datová sada tabulky SQL určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="f5481-146">And, the output SQL table dataset specifies the table in the database to which the data from the blob storage is copied.</span></span>
3. <span data-ttu-id="f5481-147">Vytvořte v datové továrně **kanál**.</span><span class="sxs-lookup"><span data-stu-id="f5481-147">Create a **pipeline** in the data factory.</span></span> <span data-ttu-id="f5481-148">V tomto kroku pomocí aktivity kopírování vytvoříte kanál.</span><span class="sxs-lookup"><span data-stu-id="f5481-148">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="f5481-149">Aktivita kopírování kopíruje data z objektu blob v úložišti objektů blob v Azure do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="f5481-149">The copy activity copies data from a blob in the Azure blob storage to a table in the Azure SQL database.</span></span> <span data-ttu-id="f5481-150">Aktivitu kopírování můžete v kanálu použít ke kopírování dat z jakéhokoli podporovaného zdroje do jakéhokoli podporovaného cíle.</span><span class="sxs-lookup"><span data-stu-id="f5481-150">You can use a copy activity in a pipeline to copy data from any supported source to any supported destination.</span></span> <span data-ttu-id="f5481-151">Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f5481-151">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
4. <span data-ttu-id="f5481-152">Vytvořte **datovou továrnu** Azure, když nasazujete entity služby Data Factory (propojené služby, datové sady / tabulky a kanály).</span><span class="sxs-lookup"><span data-stu-id="f5481-152">Create an Azure **data factory** when deploying Data Factory entities (linked services, datasets/tables, and pipelines).</span></span> 

## <a name="create-visual-studio-project"></a><span data-ttu-id="f5481-153">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5481-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="f5481-154">Spusťte **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="f5481-154">Launch **Visual Studio 2015**.</span></span> <span data-ttu-id="f5481-155">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="f5481-155">Click **File**, point to **New**, and click **Project**.</span></span> <span data-ttu-id="f5481-156">Mělo by se zobrazit dialogové okno **Nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="f5481-156">You should see the **New Project** dialog box.</span></span>  
2. <span data-ttu-id="f5481-157">V dialogovém okně **Nový projekt** vyberte šablonu **DataFactory** a klikněte na **Empty Data Factory Project** (Prázdný projekt Data Factory).</span><span class="sxs-lookup"><span data-stu-id="f5481-157">In the **New Project** dialog, select the **DataFactory** template, and click **Empty Data Factory Project**.</span></span>  
   
    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. <span data-ttu-id="f5481-159">Zadejte název projektu, umístění pro řešení a název tohoto řešení a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="f5481-159">Specify the name of the project, location for the solution, and name of the solution, and then click **OK**.</span></span>
   
    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a><span data-ttu-id="f5481-161">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="f5481-161">Create linked services</span></span>
<span data-ttu-id="f5481-162">V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-162">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="f5481-163">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="f5481-163">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="f5481-164">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="f5481-164">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="f5481-165">Proto vytvoříte dvě propojené služby typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="f5481-165">Therefore, you create two linked services of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="f5481-166">Propojená služba AzureStorage propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-166">The Azure Storage linked service links your Azure storage account to the data factory.</span></span> <span data-ttu-id="f5481-167">Tento účet úložiště je ten, ve kterém jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili kontejner a nahráli do něj data.</span><span class="sxs-lookup"><span data-stu-id="f5481-167">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="f5481-168">Propojená služba Azure SQL propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-168">Azure SQL linked service links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="f5481-169">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="f5481-169">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="f5481-170">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku emp.</span><span class="sxs-lookup"><span data-stu-id="f5481-170">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="f5481-171">Propojené služby propojují úložiště dat nebo výpočetní služby s objektem pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-171">Linked services link data stores or compute services to an Azure data factory.</span></span> <span data-ttu-id="f5481-172">Všechny zdroje a jímky podporované aktivitou kopírování najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f5481-172">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all the sources and sinks supported by the Copy Activity.</span></span> <span data-ttu-id="f5481-173">Seznam výpočetních služeb podporovaných službou Data Factory najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-173">See [compute linked services](data-factory-compute-linked-services.md) for the list of compute services supported by Data Factory.</span></span> <span data-ttu-id="f5481-174">V tomto kurzu žádné výpočetní služby nepoužijete.</span><span class="sxs-lookup"><span data-stu-id="f5481-174">In this tutorial, you do not use any compute service.</span></span> 

### <a name="create-the-azure-storage-linked-service"></a><span data-ttu-id="f5481-175">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="f5481-175">Create the Azure Storage linked service</span></span>
1. <span data-ttu-id="f5481-176">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-176">In **Solution Explorer**, right-click **Linked Services**, point to **Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="f5481-177">V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Storage Linked Service** (Propojená služba Azure Storage) a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-177">In the **Add New Item** dialog box, select **Azure Storage Linked Service** from the list, and click **Add**.</span></span> 
   
    ![Nová propojená služba](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. <span data-ttu-id="f5481-179">Hodnoty `<accountname>` a `<accountkey>`* nahraďte názvem účtu služby Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="f5481-179">Replace `<accountname>` and `<accountkey>`* with the name of your Azure storage account and its key.</span></span> 
   
    ![Propojená služba Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. <span data-ttu-id="f5481-181">Uložte soubor **AzureStorageLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="f5481-181">Save the **AzureStorageLinkedService1.json** file.</span></span>

    <span data-ttu-id="f5481-182">Další informace o vlastnostech JSON použitých v definici propojené služby najdete v článku[Konektor služby Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f5481-182">For more information about JSON properties in the linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-the-azure-sql-linked-service"></a><span data-ttu-id="f5481-183">Vytvoření propojené služby Azure SQL</span><span class="sxs-lookup"><span data-stu-id="f5481-183">Create the Azure SQL linked service</span></span>
1. <span data-ttu-id="f5481-184">V **Průzkumníku řešení** znovu klikněte pravým tlačítkem myši na uzel **Propojené služby**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-184">Right-click on **Linked Services** node in the **Solution Explorer** again, point to **Add**, and click **New Item**.</span></span> 
2. <span data-ttu-id="f5481-185">Tentokrát vyberte **Propojená služba Azure SQL** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-185">This time, select **Azure SQL Linked Service**, and click **Add**.</span></span> 
3. <span data-ttu-id="f5481-186">V souboru **AzureSqlLinkedService1.json** nahraďte hodnoty `<servername>`, `<databasename>`, `<username@servername>` a `<password>` názvy svého serveru Azure SQL, databáze, uživatelského účtu a heslem.</span><span class="sxs-lookup"><span data-stu-id="f5481-186">In the **AzureSqlLinkedService1.json file**, replace `<servername>`, `<databasename>`, `<username@servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span>    
4. <span data-ttu-id="f5481-187">Uložte soubor **AzureSqlLinkedService1.json**.</span><span class="sxs-lookup"><span data-stu-id="f5481-187">Save the **AzureSqlLinkedService1.json** file.</span></span> 
    
    <span data-ttu-id="f5481-188">Další informace o těchto vlastnostech JSON najdete v článku [Konektor služby Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="f5481-188">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>


## <a name="create-datasets"></a><span data-ttu-id="f5481-189">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="f5481-189">Create datasets</span></span>
<span data-ttu-id="f5481-190">V předchozím kroku jste vytvořili propojené služby, abyste propojili účet úložiště Azure a Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="f5481-190">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="f5481-191">V tomto kroku nadefinujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují služby AzureStorageLinkedService1 a AzureSqlLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="f5481-191">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService1 and AzureSqlLinkedService1 respectively.</span></span>

<span data-ttu-id="f5481-192">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-192">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="f5481-193">A vstupní datová sada objektu blob (InputDataset) určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="f5481-193">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

<span data-ttu-id="f5481-194">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="f5481-194">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="f5481-195">A výstupní datová sada tabulky SQL (OutputDataset) určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="f5481-195">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="f5481-196">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="f5481-196">Create input dataset</span></span>
<span data-ttu-id="f5481-197">V tomto kroku vytvoříte datovou sadu s názvem InputDataset, která odkazuje na soubor blob (emp.txt) v kořenové složce kontejneru objektů blob (adftutorial), který se nachází ve službě Azure Storage reprezentované propojenou službou AzureStorageLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="f5481-197">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService1 linked service.</span></span> <span data-ttu-id="f5481-198">Pokud neurčíte hodnotu fileName (nebo ji přeskočíte), data ze všech objektů blob ve vstupní složce se zkopírují do cíle.</span><span class="sxs-lookup"><span data-stu-id="f5481-198">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="f5481-199">V tomto kurzu určíte hodnotu fileName.</span><span class="sxs-lookup"><span data-stu-id="f5481-199">In this tutorial, you specify a value for the fileName.</span></span> 

<span data-ttu-id="f5481-200">Zde raději použijte termín „tabulky“ než „datové sady“.</span><span class="sxs-lookup"><span data-stu-id="f5481-200">Here, you use the term "tables" rather than "datasets".</span></span> <span data-ttu-id="f5481-201">Tabulka je obdélníková datová sada a je jediným typem datové sady, který je nyní podporovaný.</span><span class="sxs-lookup"><span data-stu-id="f5481-201">A table is a rectangular dataset and is the only type of dataset supported right now.</span></span> 

1. <span data-ttu-id="f5481-202">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-202">Right-click **Tables** in the **Solution Explorer**, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="f5481-203">V dialogovém okně **Přidat novou položku** vyberte v seznamu možnost **Azure Blob** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-203">In the **Add New Item** dialog box, select **Azure Blob**, and click **Add**.</span></span>   
3. <span data-ttu-id="f5481-204">Nahraďte text JSON následujícím textem a uložte soubor **AzureBlobLocation1.json**.</span><span class="sxs-lookup"><span data-stu-id="f5481-204">Replace the JSON text with the following text and save the **AzureBlobLocation1.json** file.</span></span> 

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
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
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
    <span data-ttu-id="f5481-205">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="f5481-205">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="f5481-206">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f5481-206">Property</span></span> | <span data-ttu-id="f5481-207">Popis</span><span class="sxs-lookup"><span data-stu-id="f5481-207">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="f5481-208">type</span><span class="sxs-lookup"><span data-stu-id="f5481-208">type</span></span> | <span data-ttu-id="f5481-209">Vlastnost type je nastavená na hodnotu **AzureBlob**, protože se data nachází ve službě Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="f5481-209">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="f5481-210">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="f5481-210">linkedServiceName</span></span> | <span data-ttu-id="f5481-211">Odkazuje na službu **AzureStorageLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="f5481-211">Refers to the **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="f5481-212">folderPath</span><span class="sxs-lookup"><span data-stu-id="f5481-212">folderPath</span></span> | <span data-ttu-id="f5481-213">Určuje **kontejner** objektů blob a **složku** obsahující vstupní objekty blob.</span><span class="sxs-lookup"><span data-stu-id="f5481-213">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> <span data-ttu-id="f5481-214">V tomto kurzu je adftutorial kontejnerem objektů blob a složka je kořenová složka.</span><span class="sxs-lookup"><span data-stu-id="f5481-214">In this tutorial, adftutorial is the blob container and folder is the root folder.</span></span> | 
    | <span data-ttu-id="f5481-215">fileName</span><span class="sxs-lookup"><span data-stu-id="f5481-215">fileName</span></span> | <span data-ttu-id="f5481-216">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="f5481-216">This property is optional.</span></span> <span data-ttu-id="f5481-217">Pokud ji vynecháte, vyberou se všechny soubory v cestě folderPath.</span><span class="sxs-lookup"><span data-stu-id="f5481-217">If you omit this property, all files from the folderPath are picked.</span></span> <span data-ttu-id="f5481-218">V tomto kurzu má fileName hodnotu **emp.txt**, takže se zpracuje pouze tento soubor.</span><span class="sxs-lookup"><span data-stu-id="f5481-218">In this tutorial, **emp.txt** is specified for the fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="f5481-219">format -> type</span><span class="sxs-lookup"><span data-stu-id="f5481-219">format -> type</span></span> |<span data-ttu-id="f5481-220">Vstupní soubor je v textovém formátu, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-220">The input file is in the text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="f5481-221">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="f5481-221">columnDelimiter</span></span> | <span data-ttu-id="f5481-222">Sloupce ve vstupním souboru jsou oddělené **znakem čárky (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="f5481-222">The columns in the input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="f5481-223">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="f5481-223">frequency/interval</span></span> | <span data-ttu-id="f5481-224">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, takže vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="f5481-224">The frequency is set to **Hour** and interval is  set to **1**, which means that the input slices are available **hourly**.</span></span> <span data-ttu-id="f5481-225">Jinými slovy služba Data Factory každou hodinu vyhledá vstupní data v kořenové složce kontejneru objektů blob (**adftutorial**), který jste zadali.</span><span class="sxs-lookup"><span data-stu-id="f5481-225">In other words, the Data Factory service looks for input data every hour in the root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="f5481-226">Vyhledává data v rámci kanálu mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="f5481-226">It looks for the data within the pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="f5481-227">external</span><span class="sxs-lookup"><span data-stu-id="f5481-227">external</span></span> | <span data-ttu-id="f5481-228">Pokud data nevygeneroval tento kanál, je tato vlastnost nastavená na hodnotu **true**.</span><span class="sxs-lookup"><span data-stu-id="f5481-228">This property is set to **true** if the data is not generated by this pipeline.</span></span> <span data-ttu-id="f5481-229">Vstupní data v tomto kurzu jsou v souboru emp.txt, který není generován tímto kanálem, proto jsme tuto vlastnost nastavili na hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="f5481-229">The input data in this tutorial is in the emp.txt file, which is not generated by this pipeline, so we set this property to true.</span></span> |

    <span data-ttu-id="f5481-230">Další informace o těchto vlastnostech JSON najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f5481-230">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>   

### <a name="create-output-dataset"></a><span data-ttu-id="f5481-231">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="f5481-231">Create output dataset</span></span>
<span data-ttu-id="f5481-232">V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="f5481-232">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="f5481-233">Tato datová sada odkazuje na tabulku SQL ve službě Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="f5481-233">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService1**.</span></span> 

1. <span data-ttu-id="f5481-234">V **Průzkumníku řešení** klikněte opět pravým tlačítkem myši na **Tabulky**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-234">Right-click **Tables** in the **Solution Explorer** again, point to **Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="f5481-235">V dialogovém okně **Přidat novou položku** vyberte **Azure SQL** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-235">In the **Add New Item** dialog box, select **Azure SQL**, and click **Add**.</span></span> 
3. <span data-ttu-id="f5481-236">Nahraďte text JSON následujícím textem JSON a uložte soubor **AzureSqlTableLocation1.json**.</span><span class="sxs-lookup"><span data-stu-id="f5481-236">Replace the JSON text with the following JSON and save the **AzureSqlTableLocation1.json** file.</span></span>

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
       "linkedServiceName": "AzureSqlLinkedService1",
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
    <span data-ttu-id="f5481-237">Následující tabulka obsahuje popis vlastností použitých v tomto fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="f5481-237">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

    | <span data-ttu-id="f5481-238">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="f5481-238">Property</span></span> | <span data-ttu-id="f5481-239">Popis</span><span class="sxs-lookup"><span data-stu-id="f5481-239">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="f5481-240">type</span><span class="sxs-lookup"><span data-stu-id="f5481-240">type</span></span> | <span data-ttu-id="f5481-241">Vlastnost type je nastavena na hodnotu **AzureSqlTable**, protože data se kopírují do tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="f5481-241">The type property is set to **AzureSqlTable** because data is copied to a table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="f5481-242">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="f5481-242">linkedServiceName</span></span> | <span data-ttu-id="f5481-243">Odkazuje na službu **AzureSqlLinkedService**, kterou jste vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="f5481-243">Refers to the **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="f5481-244">tableName</span><span class="sxs-lookup"><span data-stu-id="f5481-244">tableName</span></span> | <span data-ttu-id="f5481-245">Určuje **tabulku**, do které se kopírují data.</span><span class="sxs-lookup"><span data-stu-id="f5481-245">Specified the **table** to which the data is copied.</span></span> | 
    | <span data-ttu-id="f5481-246">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="f5481-246">frequency/interval</span></span> | <span data-ttu-id="f5481-247">Frekvence je nastavená na hodnotu **Hour** (hodina) a interval je **1**, což znamená, že výstupní řezy se tvoří **každou hodinu** mezi časy spuštění a ukončení, ne před nebo po této době.</span><span class="sxs-lookup"><span data-stu-id="f5481-247">The frequency is set to **Hour** and interval is **1**, which means that the output slices are produced **hourly** between the pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="f5481-248">V tabulce emp v databázi jsou k dispozici tři sloupce – **ID**, **FirstName** a **LastName**.</span><span class="sxs-lookup"><span data-stu-id="f5481-248">There are three columns – **ID**, **FirstName**, and **LastName** – in the emp table in the database.</span></span> <span data-ttu-id="f5481-249">ID je sloupec identity, takže je zde třeba zadat pouze položky **FirstName** (Jméno) a **LastName** (Příjmení).</span><span class="sxs-lookup"><span data-stu-id="f5481-249">ID is an identity column, so you need to specify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="f5481-250">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="f5481-250">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

## <a name="create-pipeline"></a><span data-ttu-id="f5481-251">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="f5481-251">Create pipeline</span></span>
<span data-ttu-id="f5481-252">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="f5481-252">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="f5481-253">Výstupní datové sady v současné době řídí plán.</span><span class="sxs-lookup"><span data-stu-id="f5481-253">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="f5481-254">V tomto kurzu je výstupní datová sada nakonfigurovaná tak, aby vytvářela řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="f5481-254">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="f5481-255">Kanál má čas spuštění a čas ukončení nastavený jeden den od sebe, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="f5481-255">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="f5481-256">Proto kanál vytvoří 24 řezů výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="f5481-256">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span> 

1. <span data-ttu-id="f5481-257">V **Průzkumníku řešení** klikněte pravým tlačítkem myši na **Kanály**, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-257">Right-click **Pipelines** in the **Solution Explorer**, point to **Add**, and click **New Item**.</span></span>  
2. <span data-ttu-id="f5481-258">V dialogovém okně **Přidat novou položku** vyberte **Copy Data Pipeline** (Kanál kopírování dat) a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-258">Select **Copy Data Pipeline** in the **Add New Item** dialog box and click **Add**.</span></span> 
3. <span data-ttu-id="f5481-259">Nahraďte text JSON následujícím textem JSON a uložte soubor **CopyActivity1.json**.</span><span class="sxs-lookup"><span data-stu-id="f5481-259">Replace the JSON with the following JSON and save the **CopyActivity1.json** file.</span></span>

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
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - <span data-ttu-id="f5481-260">V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.</span><span class="sxs-lookup"><span data-stu-id="f5481-260">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="f5481-261">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-261">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="f5481-262">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-262">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="f5481-263">Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="f5481-263">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="f5481-264">V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="f5481-264">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="f5481-265">Úplný seznam úložišť dat podporovaných aktivitou kopírování jako zdroje a jímky najdete v tématu [podporovaných úložištích dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="f5481-265">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="f5481-266">Kliknutím na odkaz v tabulce se dozvíte, jak použít konkrétní podporovaná úložiště dat jako zdroj/jímku.</span><span class="sxs-lookup"><span data-stu-id="f5481-266">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
     
    <span data-ttu-id="f5481-267">Nahraďte hodnotu vlastnosti **start** aktuálním dnem a **end** následujícím dnem.</span><span class="sxs-lookup"><span data-stu-id="f5481-267">Replace the value of the **start** property with the current day and **end** value with the next day.</span></span> <span data-ttu-id="f5481-268">Můžete zadat jenom část data a přeskočit část času.</span><span class="sxs-lookup"><span data-stu-id="f5481-268">You can specify only the date part and skip the time part of the date time.</span></span> <span data-ttu-id="f5481-269">Například „2016-02-03“ je ekvivalentní hodnotě „2016-02-03T00:00:00Z“.</span><span class="sxs-lookup"><span data-stu-id="f5481-269">For example, "2016-02-03", which is equivalent to "2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="f5481-270">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="f5481-270">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="f5481-271">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="f5481-271">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="f5481-272">Čas hodnoty **end** je nepovinný, ale my ho v tomto kurzu použijeme.</span><span class="sxs-lookup"><span data-stu-id="f5481-272">The **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="f5481-273">Pokud nezadáte hodnotu vlastnosti **end**, vypočítá se jako „**start + 48 hodin**“.</span><span class="sxs-lookup"><span data-stu-id="f5481-273">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="f5481-274">Pokud chcete kanál spouštět bez omezení, zadejte vlastnosti **end** hodnotu **9999-09-09**.</span><span class="sxs-lookup"><span data-stu-id="f5481-274">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span>
     
    <span data-ttu-id="f5481-275">V předchozím příkladu je 24 datových řezů, protože se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="f5481-275">In the preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="f5481-276">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-276">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="f5481-277">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-277">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="f5481-278">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-278">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="f5481-279">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-279">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="f5481-280">Publikování/nasazení entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="f5481-280">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="f5481-281">V tomto kroku publikujete entity služby Data Factory (propojené služby, datové sady a kanál), které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="f5481-281">In this step, you publish Data Factory entities (linked services, datasets, and pipeline) you created earlier.</span></span> <span data-ttu-id="f5481-282">Také zadáte název nově vytvořeného objektu pro vytváření dat, do kterého se tyto entity načtou.</span><span class="sxs-lookup"><span data-stu-id="f5481-282">You also specify the name of the new data factory to be created to hold these entities.</span></span>  

1. <span data-ttu-id="f5481-283">V Průzkumníku řešení klikněte pravým tlačítkem na požadovaný projekt a poté klikněte na **Publikovat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-283">Right-click project in the Solution Explorer, and click **Publish**.</span></span> 
2. <span data-ttu-id="f5481-284">Pokud se zobrazí dialogové okno **Přihlásit se pomocí účtu Microsoft**, zadejte přihlašovací údaje k účtu s předplatným Azure a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="f5481-284">If you see **Sign in to your Microsoft account** dialog box, enter your credentials for the account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="f5481-285">Mělo by se zobrazit následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="f5481-285">You should see the following dialog box:</span></span>
   
   ![Dialogové okno publikování](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. <span data-ttu-id="f5481-287">Na stránce Konfigurace objektu pro vytváření dat proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5481-287">In the Configure data factory page, do the following steps:</span></span> 
   
   1. <span data-ttu-id="f5481-288">Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="f5481-288">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="f5481-289">Zadejte **VSTutorialFactory** jako **Název**.</span><span class="sxs-lookup"><span data-stu-id="f5481-289">Enter **VSTutorialFactory** for **Name**.</span></span>  
      
      > [!IMPORTANT]
      > <span data-ttu-id="f5481-290">Název objektu pro vytváření dat Azure musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="f5481-290">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="f5481-291">Pokud se při publikování zobrazí chyba týkající se názvu objektu pro vytváření dat, změňte název objektu pro vytváření dat (třeba na váš_název_VSTutorialFactory) a zkuste publikování znovu.</span><span class="sxs-lookup"><span data-stu-id="f5481-291">If you receive an error about the name of data factory when publishing, change the name of the data factory (for example, yournameVSTutorialFactory) and try publishing again.</span></span> <span data-ttu-id="f5481-292">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f5481-292">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>        
      > 
      > 
   3. <span data-ttu-id="f5481-293">V poli **Předplatné** vyberte své předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-293">Select your Azure subscription for the **Subscription** field.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="f5481-294">Pokud nevidíte žádné předplatné, ujistěte se, že jste přihlášeni pomocí účtu, který je správce nebo spolusprávce předplatného.</span><span class="sxs-lookup"><span data-stu-id="f5481-294">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of the subscription.</span></span>  
      > 
      > 
   4. <span data-ttu-id="f5481-295">Vyberte **skupinu prostředků** pro objekt pro vytváření dat, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="f5481-295">Select the **resource group** for the data factory to be created.</span></span> 
   5. <span data-ttu-id="f5481-296">Vyberte **oblast** pro objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="f5481-296">Select the **region** for the data factory.</span></span> <span data-ttu-id="f5481-297">V rozevíracím seznamu jsou uvedené pouze oblasti podporované službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f5481-297">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
   6. <span data-ttu-id="f5481-298">Kliknutím na **Další** přejděte na stránku **Publish Items** (Publikovat položky).</span><span class="sxs-lookup"><span data-stu-id="f5481-298">Click **Next** to switch to the **Publish Items** page.</span></span>
      
       ![Stránka konfigurace služby Data Factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. <span data-ttu-id="f5481-300">Na stránce **Publish Items** (Publikovat položky) zkontrolujte, jestli jsou vybrané všechny entity služby Data Factory, a kliknutím na **Další** přejděte na stránku **Souhrn**.</span><span class="sxs-lookup"><span data-stu-id="f5481-300">In the **Publish Items** page, ensure that all the Data Factories entities are selected, and click **Next** to switch to the **Summary** page.</span></span>
   
   ![Stránka publikování položek](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. <span data-ttu-id="f5481-302">Zkontrolujte souhrn a klikněte na **Další**. Spustí se proces nasazení a zobrazí se **Stav nasazení**.</span><span class="sxs-lookup"><span data-stu-id="f5481-302">Review the summary and click **Next** to start the deployment process and view the **Deployment Status**.</span></span>
   
   ![Stránka souhrnu publikování](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. <span data-ttu-id="f5481-304">Na stránce **Stav nasazení** byste měli vidět stav procesu nasazení.</span><span class="sxs-lookup"><span data-stu-id="f5481-304">In the **Deployment Status** page, you should see the status of the deployment process.</span></span> <span data-ttu-id="f5481-305">Až se nasazení dokončí, klikněte na Dokončit.</span><span class="sxs-lookup"><span data-stu-id="f5481-305">Click Finish after the deployment is done.</span></span>
 
   ![Stránka stavu nasazení](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

<span data-ttu-id="f5481-307">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="f5481-307">Note the following points:</span></span> 

* <span data-ttu-id="f5481-308">Pokud se zobrazí chyba „Pro předplatné není zaregistrované používání oboru názvů Microsoft.DataFactory“, proveďte některý z těchto kroků a znovu zkuste název publikovat:</span><span class="sxs-lookup"><span data-stu-id="f5481-308">If you receive the error: "This subscription is not registered to use namespace Microsoft.DataFactory", do one of the following and try publishing again:</span></span> 
  
  * <span data-ttu-id="f5481-309">V prostředí Azure PowerShell zaregistrujte zprostředkovatele služby Data Factory pomocí následujícího příkazu.</span><span class="sxs-lookup"><span data-stu-id="f5481-309">In Azure PowerShell, run the following command to register the Data Factory provider.</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="f5481-310">Spuštěním následujícího příkazu si můžete ověřit, jestli je zprostředkovatel služby Data Factory zaregistrovaný.</span><span class="sxs-lookup"><span data-stu-id="f5481-310">You can run the following command to confirm that the Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="f5481-311">Přihlaste se na web [Azure Portal ](https://portal.azure.com) pomocí předplatného Azure a přejděte do okna Objekt pro vytváření dat nebo na webu Azure Portal vytvořte objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="f5481-311">Login using the Azure subscription into the [Azure portal](https://portal.azure.com) and navigate to a Data Factory blade (or) create a data factory in the Azure portal.</span></span> <span data-ttu-id="f5481-312">Zprostředkovatel se při takovém postupu zaregistruje automaticky.</span><span class="sxs-lookup"><span data-stu-id="f5481-312">This action automatically registers the provider for you.</span></span>
* <span data-ttu-id="f5481-313">Název objektu pro vytváření dat se může v budoucnu zaregistrovat jako název DNS, takže pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="f5481-313">The name of the data factory may be registered as a DNS name in the future and hence become publically visible.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f5481-314">Chcete-li vytvářet instance služby Data Factory, musíte být správce nebo spolusprávce předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-314">To create Data Factory instances, you need to be a admin/co-admin of the Azure subscription</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="f5481-315">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="f5481-315">Monitor pipeline</span></span>
<span data-ttu-id="f5481-316">Přejděte na domovskou stránku své datové továrny:</span><span class="sxs-lookup"><span data-stu-id="f5481-316">Navigate to the home page for your data factory:</span></span>

1. <span data-ttu-id="f5481-317">Přihlaste se k portálu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f5481-317">Log in to [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f5481-318">Klikněte v levé nabídce na **Další služby** a poté na **Datové továrny**.</span><span class="sxs-lookup"><span data-stu-id="f5481-318">Click **More services** on the left menu, and click **Data factories**.</span></span>

    ![Procházet -> Datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. <span data-ttu-id="f5481-320">Začněte zadávat název své datové továrny.</span><span class="sxs-lookup"><span data-stu-id="f5481-320">Start typing the name of your data factory.</span></span>

    ![Název datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. <span data-ttu-id="f5481-322">Klikněte na svou datovou továrnu v seznamu výsledků, abyste si zobrazili domovskou stránku datové továrny.</span><span class="sxs-lookup"><span data-stu-id="f5481-322">Click your data factory in the results list to see the home page for your data factory.</span></span>

    ![Domovská stránka objektu pro vytváření dat](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. <span data-ttu-id="f5481-324">Postupujte podle pokynů v tématu [Monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) k monitorování kanálu a datových sad, které jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="f5481-324">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) to monitor the pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="f5481-325">V současné době Visual Studio monitorování kanálů Data Factory nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="f5481-325">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span> 

## <a name="summary"></a><span data-ttu-id="f5481-326">Souhrn</span><span class="sxs-lookup"><span data-stu-id="f5481-326">Summary</span></span>
<span data-ttu-id="f5481-327">V tomto kurzu jste vytvořili objekt pro vytváření dat Azure pro zkopírování dat z objektu blob Azure do Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f5481-327">In this tutorial, you created an Azure data factory to copy data from an Azure blob to an Azure SQL database.</span></span> <span data-ttu-id="f5481-328">Visual Studio jste použili k vytvoření objektu pro vytváření dat, propojených služeb, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="f5481-328">You used Visual Studio to create the data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="f5481-329">Zde jsou základní kroky, které jste v tomto kurzu provedli:</span><span class="sxs-lookup"><span data-stu-id="f5481-329">Here are the high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="f5481-330">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="f5481-330">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="f5481-331">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="f5481-331">Created **linked services**:</span></span>
   1. <span data-ttu-id="f5481-332">Propojená služba **Azure Storage** připojující účet úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="f5481-332">An **Azure Storage** linked service to link your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="f5481-333">Propojená služba **Azure SQL** připojující službu Azure SQL Database, která obsahuje výstupní data.</span><span class="sxs-lookup"><span data-stu-id="f5481-333">An **Azure SQL** linked service to link your Azure SQL database that holds the output data.</span></span> 
3. <span data-ttu-id="f5481-334">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="f5481-334">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="f5481-335">Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="f5481-335">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span> 

<span data-ttu-id="f5481-336">Informace o tom, jak používat aktivitu HDInsight Hive pomocí clusteru Azure HDInsight najdete v tématu popisujícím [kurz vytvoření prvního kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-336">To see how to use a HDInsight Hive Activity to transform data by using Azure HDInsight cluster, see [ Tutorial: Build your first pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="f5481-337">Dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="f5481-337">You can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="f5481-338">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="f5481-338">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 

## <a name="view-all-data-factories-in-server-explorer"></a><span data-ttu-id="f5481-339">Zobrazení všech datových továren v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="f5481-339">View all data factories in Server Explorer</span></span>
<span data-ttu-id="f5481-340">Tato část popisuje, jak použít Průzkumník serveru ve Visual Studiu k zobrazení všech datových továren v předplatném Azure a jak vytvořit ve Visual Studiu projekt na základě existující datové továrny.</span><span class="sxs-lookup"><span data-stu-id="f5481-340">This section describes how to use the Server Explorer in Visual Studio to view all the data factories in your Azure subscription and create a Visual Studio project based on an existing data factory.</span></span> 

1. <span data-ttu-id="f5481-341">V sadě **Visual Studio** klikněte v nabídce na **Zobrazit** a potom klikněte na **Průzkumník serveru**.</span><span class="sxs-lookup"><span data-stu-id="f5481-341">In **Visual Studio**, click **View** on the menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="f5481-342">V okně Průzkumníka serveru rozbalte položku **Azure** a potom **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="f5481-342">In the Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="f5481-343">Pokud se zobrazí text **Přihlásit se k Visual Studiu**, zadejte **účet** přidružený k vašemu předplatnému Azure a klikněte na **Pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-343">If you see **Sign in to Visual Studio**, enter the **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="f5481-344">Zadejte **heslo** a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="f5481-344">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="f5481-345">Visual Studio se pokusí získat informace o všech objektech pro vytváření dat Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="f5481-345">Visual Studio tries to get information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="f5481-346">Stav této operace se zobrazuje v okně **Data Factory Task List** (Seznam úkolů služby Data Factory).</span><span class="sxs-lookup"><span data-stu-id="f5481-346">You see the status of this operation in the **Data Factory Task List** window.</span></span>

    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a><span data-ttu-id="f5481-348">Vytvoření projektu ve Visual Studiu pro existující datovou továrnu</span><span class="sxs-lookup"><span data-stu-id="f5481-348">Create a Visual Studio project for an existing data factory</span></span>

- <span data-ttu-id="f5481-349">Kliknutím na datovou továrnu pravým tlačítkem v Průzkumníku serveru a výběrem **Export Data Factory to New Project** (Exportovat továrnu dat do nového projektu) vytvoříte projekt sady Visual Studio založený na existující datové továrně.</span><span class="sxs-lookup"><span data-stu-id="f5481-349">Right-click a data factory in Server Explorer, and select **Export Data Factory to New Project** to create a Visual Studio project based on an existing data factory.</span></span>

    ![Export objektu pro vytváření dat do projektu VS](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="f5481-351">Aktualizace nástrojů služby Data Factory pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f5481-351">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="f5481-352">Chcete-li aktualizovat nástroje služby Azure Data Factory pro Visual Studio, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="f5481-352">To update Azure Data Factory tools for Visual Studio, do the following steps:</span></span>

1. <span data-ttu-id="f5481-353">V nabídce klikněte na **Nástroje** a vyberte **Rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="f5481-353">Click **Tools** on the menu and select **Extensions and Updates**.</span></span> 
2. <span data-ttu-id="f5481-354">V levém podokně vyberte **Aktualizace** a vyberte **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="f5481-354">Select **Updates** in the left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="f5481-355">Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-355">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="f5481-356">Pokud se tato položka nezobrazí, znamená to, že máte nejnovější verzi těchto nástrojů.</span><span class="sxs-lookup"><span data-stu-id="f5481-356">If you do not see this entry, you already have the latest version of the tools.</span></span> 

## <a name="use-configuration-files"></a><span data-ttu-id="f5481-357">Použití konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="f5481-357">Use configuration files</span></span>
<span data-ttu-id="f5481-358">Pomocí konfiguračních souborů v sadě Visual Studio můžete pro různá prostředí nakonfigurovat různé vlastnosti propojených služeb / tabulek / kanálů.</span><span class="sxs-lookup"><span data-stu-id="f5481-358">You can use configuration files in Visual Studio to configure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="f5481-359">Podívejte se na následující definici JSON pro propojenou službu Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="f5481-359">Consider the following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="f5481-360">U položky **connectionString** můžete určit jiné hodnoty vlastností accountname a accountkey pro různá prostředí (vývojové/testovací/produkční), do kterých nasazujete entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f5481-360">To specify **connectionString** with different values for accountname and accountkey based on the environment (Dev/Test/Production) to which you are deploying Data Factory entities.</span></span> <span data-ttu-id="f5481-361">Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="f5481-361">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="f5481-362">Přidání konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="f5481-362">Add a configuration file</span></span>
<span data-ttu-id="f5481-363">Následující postup umožňuje přidat pro každé prostředí jiný konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="f5481-363">Add a configuration file for each environment by performing the following steps:</span></span>   

1. <span data-ttu-id="f5481-364">V řešení v sadě Visual Studio klikněte pravým tlačítkem na svůj projekt Data Factory, přejděte na **Přidat** a klikněte na **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="f5481-364">Right-click the Data Factory project in your Visual Studio solution, point to **Add**, and click **New item**.</span></span>
2. <span data-ttu-id="f5481-365">V seznamu nainstalovaných šablon vlevo vyberte šablonu **Config**, vyberte možnost **Konfigurační soubor**, zadejte jeho **název** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="f5481-365">Select **Config** from the list of installed templates on the left, select **Configuration File**, enter a **name** for the configuration file, and click **Add**.</span></span>

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="f5481-367">Přidejte parametry konfigurace a jejich hodnoty v následujícím formátu:</span><span class="sxs-lookup"><span data-stu-id="f5481-367">Add configuration parameters and their values in the following format:</span></span>

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

    <span data-ttu-id="f5481-368">Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="f5481-368">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="f5481-369">Všimněte si, že syntaxe pro určení názvu je [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="f5481-369">Notice that the syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="f5481-370">Pokud má kód JSON vlastnost s polem hodnot, jak je znázorněno v následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="f5481-370">If JSON has a property that has an array of values as shown in the following code:</span></span>  

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

    <span data-ttu-id="f5481-371">Nakonfigurujte vlastnosti tak, jak je znázorněno v následujícím konfiguračním souboru (použijte indexování počítané od nuly):</span><span class="sxs-lookup"><span data-stu-id="f5481-371">Configure properties as shown in the following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="f5481-372">Názvy vlastností s mezerami</span><span class="sxs-lookup"><span data-stu-id="f5481-372">Property names with spaces</span></span>
<span data-ttu-id="f5481-373">Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru):</span><span class="sxs-lookup"><span data-stu-id="f5481-373">If a property name has spaces in it, use square brackets as shown in the following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="f5481-374">Nasazení řešení pomocí konfigurace</span><span class="sxs-lookup"><span data-stu-id="f5481-374">Deploy solution using a configuration</span></span>
<span data-ttu-id="f5481-375">Když v sadě VS publikujete entity služby Azure Data Factory, můžete určit, jakou konfiguraci chcete pro danou operaci publikování použít.</span><span class="sxs-lookup"><span data-stu-id="f5481-375">When you are publishing Azure Data Factory entities in VS, you can specify the configuration that you want to use for that publishing operation.</span></span>

<span data-ttu-id="f5481-376">Pokud chcete publikovat entity v projektu Azure Data Factory pomocí konfiguračního souboru, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="f5481-376">To publish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="f5481-377">Klikněte pravým tlačítkem na projekt Data Factory a kliknutím na **Publikovat** zobrazte dialogové okno **Publish Items** (Publikovat položky).</span><span class="sxs-lookup"><span data-stu-id="f5481-377">Right-click Data Factory project and click **Publish** to see the **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="f5481-378">Vyberte existující objekt pro vytváření dat nebo zadejte hodnoty pro vytvoření objektu pro vytváření dat na stránce **Configure data factory** (Konfigurace objektu pro vytváření dat) a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f5481-378">Select an existing data factory or specify values for creating a data factory on the **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="f5481-379">Na stránce **Publish Items** (Publikovat položky) se zobrazí rozevírací seznam s dostupnými konfiguracemi pro pole **Select Deployment Config** (Výběr konfigurace nasazení).</span><span class="sxs-lookup"><span data-stu-id="f5481-379">On the **Publish Items** page: you see a drop-down list with available configurations for the **Select Deployment Config** field.</span></span>

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="f5481-381">Vyberte **konfigurační soubor**, který chcete použít, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f5481-381">Select the **configuration file** that you would like to use and click **Next**.</span></span>
5. <span data-ttu-id="f5481-382">Ujistěte se, že se na stránce **Souhrn** zobrazil název souboru JSON, a klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="f5481-382">Confirm that you see the name of JSON file in the **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="f5481-383">Po dokončení operace nasazení klikněte na **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="f5481-383">Click **Finish** after the deployment operation is finished.</span></span>

<span data-ttu-id="f5481-384">Při nasazení se hodnoty z konfiguračního souboru použijí k nastavení hodnot vlastností v souborech JSON před samotným nasazením entit do služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f5481-384">When you deploy, the values from the configuration file are used to set values for properties in the JSON files before the entities are deployed to Azure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="f5481-385">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f5481-385">Use Azure Key Vault</span></span>
<span data-ttu-id="f5481-386">Není vhodné a často je to proti zásadám zabezpečení ukládat citlivá data, jako jsou například připojovací řetězce, do úložišti kódu.</span><span class="sxs-lookup"><span data-stu-id="f5481-386">It is not advisable and often against security policy to commit sensitive data such as connection strings to the code repository.</span></span> <span data-ttu-id="f5481-387">V ukázce [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) na Githubu získáte další informace o ukládání citlivých informací v Azure Key Vault a jejich používání při publikování entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="f5481-387">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub to learn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="f5481-388">Rozšíření zabezpečeného publikování pro Visual Studio umožňuje ukládat tajné klíče v Key Vault a v konfiguracích propojených služeb a nasazení uvádět pouze odkazy.</span><span class="sxs-lookup"><span data-stu-id="f5481-388">The Secure Publish extension for Visual Studio allows the secrets to be stored in Key Vault and only references to them are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="f5481-389">Tyto odkazy se při publikování entit služby Data Factory do Azure vyhodnotí.</span><span class="sxs-lookup"><span data-stu-id="f5481-389">These references are resolved when you publish Data Factory entities to Azure.</span></span> <span data-ttu-id="f5481-390">Tyto soubory pak lze uložit do úložiště zdrojového kódu bez vystavení jakýchkoli tajných kódů.</span><span class="sxs-lookup"><span data-stu-id="f5481-390">These files can then be committed to source repository without exposing any secrets.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f5481-391">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f5481-391">Next steps</span></span>
<span data-ttu-id="f5481-392">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="f5481-392">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="f5481-393">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="f5481-393">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="f5481-394">Kliknutím na odkaz úložiště dat v tabulce získáte další informace o kopírování dat do nebo z úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="f5481-394">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>