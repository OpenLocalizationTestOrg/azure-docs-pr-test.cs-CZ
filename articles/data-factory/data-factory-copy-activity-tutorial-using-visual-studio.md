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
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a><span data-ttu-id="ca4ab-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca4ab-103">Tutorial: Create a pipeline with Copy Activity using Visual Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ca4ab-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="ca4ab-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="ca4ab-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="ca4ab-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="ca4ab-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ca4ab-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="ca4ab-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca4ab-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="ca4ab-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ca4ab-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="ca4ab-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="ca4ab-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="ca4ab-110">REST API</span><span class="sxs-lookup"><span data-stu-id="ca4ab-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="ca4ab-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="ca4ab-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="ca4ab-112">V tomto článku se dozvíte, jak toouse hello Microsoft Visual Studio toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-112">In this article, you learn how toouse hello Microsoft Visual Studio toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="ca4ab-113">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="ca4ab-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="ca4ab-115">Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="ca4ab-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="ca4ab-117">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="ca4ab-118">Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="ca4ab-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="ca4ab-120">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="ca4ab-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

> [!NOTE] 
> <span data-ttu-id="ca4ab-122">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-122">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="ca4ab-123">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-123">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca4ab-124">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ca4ab-124">Prerequisites</span></span>
1. <span data-ttu-id="ca4ab-125">Pročtěte [přehled kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-125">Read through [Tutorial Overview](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) article and complete hello **prerequisite** steps.</span></span>       
2. <span data-ttu-id="ca4ab-126">instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-126">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
3. <span data-ttu-id="ca4ab-127">Musíte mít nainstalované ve vašem počítači hello tyto položky:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-127">You must have hello following installed on your computer:</span></span> 
   * <span data-ttu-id="ca4ab-128">Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-128">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="ca4ab-129">Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-129">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="ca4ab-130">Přejděte příliš[stránku položek ke stažení Azure](https://azure.microsoft.com/downloads/) a klikněte na tlačítko **VS 2013** nebo **VS 2015** v hello **.NET** části.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-130">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="ca4ab-131">Stáhnout hello nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-131">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="ca4ab-132">Můžete také aktualizovat modul plug-in hello provedením následujících kroků hello: hello nabídce klikněte na **nástroje** -> **rozšíření a aktualizace** -> **Online**  ->  **Galerie sady visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-132">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

## <a name="steps"></a><span data-ttu-id="ca4ab-133">Kroky</span><span class="sxs-lookup"><span data-stu-id="ca4ab-133">Steps</span></span>
<span data-ttu-id="ca4ab-134">Zde jsou hello kroky, které můžete provádět v rámci tohoto kurzu:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-134">Here are hello steps you perform as part of this tutorial:</span></span>

1. <span data-ttu-id="ca4ab-135">Vytvoření **propojené služby** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-135">Create **linked services** in hello data factory.</span></span> <span data-ttu-id="ca4ab-136">V tomto kroku vytvoříte dvě propojené služby typu: Azure Storage a Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-136">In this step, you create two linked services of types: Azure Storage and Azure SQL Database.</span></span> 
    
    <span data-ttu-id="ca4ab-137">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-137">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="ca4ab-138">Vytvořit kontejner a účet úložiště dat toothis odesílané jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-138">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

    <span data-ttu-id="ca4ab-139">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-139">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="ca4ab-140">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-140">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="ca4ab-141">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku SQL.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-141">You created a SQL table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>     
2. <span data-ttu-id="ca4ab-142">Vytvoření vstupní a výstupní **datové sady** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-142">Create input and output **datasets** in hello data factory.</span></span>  
    
    <span data-ttu-id="ca4ab-143">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-143">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="ca4ab-144">A určuje datovou sadu vstupního objektu blob hello hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-144">And, hello input blob dataset specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="ca4ab-145">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-145">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="ca4ab-146">A hello výstupní SQL tabulky datovou sadu Určuje, že se zkopíruje hello tabulky v hello databáze toowhich hello dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-146">And, hello output SQL table dataset specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>
3. <span data-ttu-id="ca4ab-147">Vytvoření **kanálu** v datové továrně hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-147">Create a **pipeline** in hello data factory.</span></span> <span data-ttu-id="ca4ab-148">V tomto kroku pomocí aktivity kopírování vytvoříte kanál.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-148">In this step, you create a pipeline with a copy activity.</span></span>   
    
    <span data-ttu-id="ca4ab-149">Aktivita kopírování Hello zkopíruje data z objektu blob ve tabulku tooa úložiště objektů blob v Azure hello ve hello Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-149">hello copy activity copies data from a blob in hello Azure blob storage tooa table in hello Azure SQL database.</span></span> <span data-ttu-id="ca4ab-150">Můžete vytvořit aktivity kopírování v kanálu toocopy data z cílového všechny podporované zdrojové tooany podporována.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-150">You can use a copy activity in a pipeline toocopy data from any supported source tooany supported destination.</span></span> <span data-ttu-id="ca4ab-151">Seznam podporovaných úložišť dat najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-151">For a list of supported data stores, see [data movement activities](data-factory-data-movement-activities.md#supported-data-stores-and-formats) article.</span></span> 
4. <span data-ttu-id="ca4ab-152">Vytvořte **datovou továrnu** Azure, když nasazujete entity služby Data Factory (propojené služby, datové sady / tabulky a kanály).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-152">Create an Azure **data factory** when deploying Data Factory entities (linked services, datasets/tables, and pipelines).</span></span> 

## <a name="create-visual-studio-project"></a><span data-ttu-id="ca4ab-153">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca4ab-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="ca4ab-154">Spusťte **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-154">Launch **Visual Studio 2015**.</span></span> <span data-ttu-id="ca4ab-155">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="ca4ab-156">Měli byste vidět hello **nový projekt** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="ca4ab-157">V hello **nový projekt** dialogové okno, vyberte hello **DataFactory** šablonu a klikněte na **prázdný projekt Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>  
   
    ![Dialogové okno Nový projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. <span data-ttu-id="ca4ab-159">Zadejte název hello hello projektu, umístění pro hello řešení a název hello řešení a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-159">Specify hello name of hello project, location for hello solution, and name of hello solution, and then click **OK**.</span></span>
   
    ![Průzkumník řešení](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a><span data-ttu-id="ca4ab-161">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="ca4ab-161">Create linked services</span></span>
<span data-ttu-id="ca4ab-162">Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-162">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="ca4ab-163">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-163">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="ca4ab-164">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-164">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

<span data-ttu-id="ca4ab-165">Proto vytvoříte dvě propojené služby typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-165">Therefore, you create two linked services of types: AzureStorage and AzureSqlDatabase.</span></span>  

<span data-ttu-id="ca4ab-166">Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-166">hello Azure Storage linked service links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="ca4ab-167">Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-167">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>   

<span data-ttu-id="ca4ab-168">Azure SQL propojená služba propojuje toohello datovou továrnu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-168">Azure SQL linked service links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="ca4ab-169">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-169">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="ca4ab-170">Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-170">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="ca4ab-171">Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-171">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="ca4ab-172">V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-172">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="ca4ab-173">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam výpočetní služby podporovaných službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-173">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span> <span data-ttu-id="ca4ab-174">V tomto kurzu žádné výpočetní služby nepoužijete.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-174">In this tutorial, you do not use any compute service.</span></span> 

### <a name="create-hello-azure-storage-linked-service"></a><span data-ttu-id="ca4ab-175">Vytvoření hello propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="ca4ab-175">Create hello Azure Storage linked service</span></span>
1. <span data-ttu-id="ca4ab-176">V **Průzkumníku řešení**, klikněte pravým tlačítkem na **propojené služby**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-176">In **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="ca4ab-177">V hello **přidat novou položku** dialogové okno, vyberte **propojená služba úložiště Azure** hello seznamu a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-177">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span> 
   
    ![Nová propojená služba](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. <span data-ttu-id="ca4ab-179">Nahraďte `<accountname>` a `<accountkey>`* s hello název účtu úložiště Azure a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-179">Replace `<accountname>` and `<accountkey>`* with hello name of your Azure storage account and its key.</span></span> 
   
    ![Propojená služba Azure Storage](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. <span data-ttu-id="ca4ab-181">Uložit hello **AzureStorageLinkedService1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-181">Save hello **AzureStorageLinkedService1.json** file.</span></span>

    <span data-ttu-id="ca4ab-182">Další informace o vlastnostech JSON v definici hello propojené služby najdete v tématu [konektor Azure Blob Storage](data-factory-azure-blob-connector.md#linked-service-properties) článku.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-182">For more information about JSON properties in hello linked service definition, see [Azure Blob Storage connector](data-factory-azure-blob-connector.md#linked-service-properties) article.</span></span>

### <a name="create-hello-azure-sql-linked-service"></a><span data-ttu-id="ca4ab-183">Vytvoření hello propojená služba Azure SQL</span><span class="sxs-lookup"><span data-stu-id="ca4ab-183">Create hello Azure SQL linked service</span></span>
1. <span data-ttu-id="ca4ab-184">Klikněte pravým tlačítkem na **propojené služby** uzel v hello **Průzkumníku řešení** znovu bodu příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-184">Right-click on **Linked Services** node in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span> 
2. <span data-ttu-id="ca4ab-185">Tentokrát vyberte **Propojená služba Azure SQL** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-185">This time, select **Azure SQL Linked Service**, and click **Add**.</span></span> 
3. <span data-ttu-id="ca4ab-186">V hello **AzureSqlLinkedService1.json soubor**, nahraďte `<servername>`, `<databasename>`, `<username@servername>`, a `<password>` s názvy serveru Azure SQL, databáze, uživatelský účet a heslo.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-186">In hello **AzureSqlLinkedService1.json file**, replace `<servername>`, `<databasename>`, `<username@servername>`, and `<password>` with names of your Azure SQL server, database, user account, and password.</span></span>    
4. <span data-ttu-id="ca4ab-187">Uložit hello **AzureSqlLinkedService1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-187">Save hello **AzureSqlLinkedService1.json** file.</span></span> 
    
    <span data-ttu-id="ca4ab-188">Další informace o těchto vlastnostech JSON najdete v článku [Konektor služby Azure SQL Database](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-188">For more information about these JSON properties, see [Azure SQL Database connector](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>


## <a name="create-datasets"></a><span data-ttu-id="ca4ab-189">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="ca4ab-189">Create datasets</span></span>
<span data-ttu-id="ca4ab-190">V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-190">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="ca4ab-191">V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService1 a AzureSqlLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-191">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService1 and AzureSqlLinkedService1 respectively.</span></span>

<span data-ttu-id="ca4ab-192">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-192">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="ca4ab-193">A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-193">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

<span data-ttu-id="ca4ab-194">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-194">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="ca4ab-195">A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-195">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span> 

### <a name="create-input-dataset"></a><span data-ttu-id="ca4ab-196">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="ca4ab-196">Create input dataset</span></span>
<span data-ttu-id="ca4ab-197">V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService1 propojené služby.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-197">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService1 linked service.</span></span> <span data-ttu-id="ca4ab-198">Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-198">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="ca4ab-199">V tomto kurzu zadejte hodnotu pro název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-199">In this tutorial, you specify a value for hello fileName.</span></span> 

<span data-ttu-id="ca4ab-200">Zde je použít hello termín "tabulky" místo "datové sady".</span><span class="sxs-lookup"><span data-stu-id="ca4ab-200">Here, you use hello term "tables" rather than "datasets".</span></span> <span data-ttu-id="ca4ab-201">Tabulka je obdélníková datová sada a je jediným typem datové sady, které jsou nyní podporovány hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-201">A table is a rectangular dataset and is hello only type of dataset supported right now.</span></span> 

1. <span data-ttu-id="ca4ab-202">Klikněte pravým tlačítkem na **tabulky** v hello **Průzkumníku řešení**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-202">Right-click **Tables** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="ca4ab-203">V hello **přidat novou položku** dialogové okno, vyberte **objektů Blob v Azure**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-203">In hello **Add New Item** dialog box, select **Azure Blob**, and click **Add**.</span></span>   
3. <span data-ttu-id="ca4ab-204">Nahraďte hello JSON text hello následující text a uložte hello **AzureBlobLocation1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-204">Replace hello JSON text with hello following text and save hello **AzureBlobLocation1.json** file.</span></span> 

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
    <span data-ttu-id="ca4ab-205">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-205">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="ca4ab-206">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ca4ab-206">Property</span></span> | <span data-ttu-id="ca4ab-207">Popis</span><span class="sxs-lookup"><span data-stu-id="ca4ab-207">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="ca4ab-208">type</span><span class="sxs-lookup"><span data-stu-id="ca4ab-208">type</span></span> | <span data-ttu-id="ca4ab-209">Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-209">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
    | <span data-ttu-id="ca4ab-210">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="ca4ab-210">linkedServiceName</span></span> | <span data-ttu-id="ca4ab-211">Odkazuje toohello **AzureStorageLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-211">Refers toohello **AzureStorageLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="ca4ab-212">folderPath</span><span class="sxs-lookup"><span data-stu-id="ca4ab-212">folderPath</span></span> | <span data-ttu-id="ca4ab-213">Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-213">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> <span data-ttu-id="ca4ab-214">V tomto kurzu adftutorial hello kontejner objektů blob, složka hello kořenové složky.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-214">In this tutorial, adftutorial is hello blob container and folder is hello root folder.</span></span> | 
    | <span data-ttu-id="ca4ab-215">fileName</span><span class="sxs-lookup"><span data-stu-id="ca4ab-215">fileName</span></span> | <span data-ttu-id="ca4ab-216">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-216">This property is optional.</span></span> <span data-ttu-id="ca4ab-217">Pokud ji vynecháte, jsou vybrány všechny soubory z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-217">If you omit this property, all files from hello folderPath are picked.</span></span> <span data-ttu-id="ca4ab-218">V tomto kurzu **emp.txt** zadaný pro hello název souboru, takže pouze tento soubor je převzata pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-218">In this tutorial, **emp.txt** is specified for hello fileName, so only that file is picked up for processing.</span></span> |
    | <span data-ttu-id="ca4ab-219">format -> type</span><span class="sxs-lookup"><span data-stu-id="ca4ab-219">format -> type</span></span> |<span data-ttu-id="ca4ab-220">vstupní soubor Hello je ve formátu textu hello, takže použijeme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-220">hello input file is in hello text format, so we use **TextFormat**.</span></span> |
    | <span data-ttu-id="ca4ab-221">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="ca4ab-221">columnDelimiter</span></span> | <span data-ttu-id="ca4ab-222">Hello sloupců ve vstupním souboru hello oddělené **čárku (`,`)**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-222">hello columns in hello input file are delimited by **comma character (`,`)**.</span></span> |
    | <span data-ttu-id="ca4ab-223">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="ca4ab-223">frequency/interval</span></span> | <span data-ttu-id="ca4ab-224">je příliš nastavena frekvence Hello**hodinu** a interval je nastaven příliš**1**, což znamená, že hello vstupní řezy jsou dostupné **každou hodinu**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-224">hello frequency is set too**Hour** and interval is  set too**1**, which means that hello input slices are available **hourly**.</span></span> <span data-ttu-id="ca4ab-225">Jinými slovy, hello služba Data Factory hledá vstupní data každou hodinu v kořenové složce hello kontejner objektů blob (**adftutorial**) jste zadali.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-225">In other words, hello Data Factory service looks for input data every hour in hello root folder of blob container (**adftutorial**) you specified.</span></span> <span data-ttu-id="ca4ab-226">Hledá hello dat v rámci hello kanálu počáteční a koncové dobu, není před nebo po této doby.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-226">It looks for hello data within hello pipeline start and end times, not before or after these times.</span></span>  |
    | <span data-ttu-id="ca4ab-227">external</span><span class="sxs-lookup"><span data-stu-id="ca4ab-227">external</span></span> | <span data-ttu-id="ca4ab-228">Tato vlastnost nastavena příliš**true** Pokud hello data nevygenerovala pomocí tohoto kanálu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-228">This property is set too**true** if hello data is not generated by this pipeline.</span></span> <span data-ttu-id="ca4ab-229">Hello vstupní data v tomto kurzu je v souboru emp.txt hello, který není generované tímto kanálem, abyste nám nastavit tuto vlastnost tootrue.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-229">hello input data in this tutorial is in hello emp.txt file, which is not generated by this pipeline, so we set this property tootrue.</span></span> |

    <span data-ttu-id="ca4ab-230">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-230">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>   

### <a name="create-output-dataset"></a><span data-ttu-id="ca4ab-231">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="ca4ab-231">Create output dataset</span></span>
<span data-ttu-id="ca4ab-232">V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-232">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="ca4ab-233">Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-233">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService1**.</span></span> 

1. <span data-ttu-id="ca4ab-234">Klikněte pravým tlačítkem na **tabulky** v hello **Průzkumníku řešení** znovu bodu příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-234">Right-click **Tables** in hello **Solution Explorer** again, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="ca4ab-235">V hello **přidat novou položku** dialogové okno, vyberte **Azure SQL**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-235">In hello **Add New Item** dialog box, select **Azure SQL**, and click **Add**.</span></span> 
3. <span data-ttu-id="ca4ab-236">Nahraďte hello JSON text hello následující JSON a uložte hello **AzureSqlTableLocation1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-236">Replace hello JSON text with hello following JSON and save hello **AzureSqlTableLocation1.json** file.</span></span>

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
    <span data-ttu-id="ca4ab-237">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-237">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    | <span data-ttu-id="ca4ab-238">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="ca4ab-238">Property</span></span> | <span data-ttu-id="ca4ab-239">Popis</span><span class="sxs-lookup"><span data-stu-id="ca4ab-239">Description</span></span> |
    |:--- |:--- |
    | <span data-ttu-id="ca4ab-240">type</span><span class="sxs-lookup"><span data-stu-id="ca4ab-240">type</span></span> | <span data-ttu-id="ca4ab-241">Hello vlastnost Typ nastavena příliš**AzureSqlTable** protože data jsou zkopírované tooa tabulky v databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-241">hello type property is set too**AzureSqlTable** because data is copied tooa table in an Azure SQL database.</span></span> |
    | <span data-ttu-id="ca4ab-242">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="ca4ab-242">linkedServiceName</span></span> | <span data-ttu-id="ca4ab-243">Odkazuje toohello **AzureSqlLinkedService** kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-243">Refers toohello **AzureSqlLinkedService** that you created earlier.</span></span> |
    | <span data-ttu-id="ca4ab-244">tableName</span><span class="sxs-lookup"><span data-stu-id="ca4ab-244">tableName</span></span> | <span data-ttu-id="ca4ab-245">Zadaný hello **tabulky** toowhich hello data budou zkopírována.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-245">Specified hello **table** toowhich hello data is copied.</span></span> | 
    | <span data-ttu-id="ca4ab-246">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="ca4ab-246">frequency/interval</span></span> | <span data-ttu-id="ca4ab-247">Hello je nastavena frekvence příliš**hodinu** a interval je **1**, což znamená, že vytváří řezy výstup hello **každou hodinu** mezi hello kanálu počáteční a koncové krát, ne dřív nebo Po této doby.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-247">hello frequency is set too**Hour** and interval is **1**, which means that hello output slices are produced **hourly** between hello pipeline start and end times, not before or after these times.</span></span>  |

    <span data-ttu-id="ca4ab-248">Existují tři sloupce – **ID**, **FirstName**, a **LastName** – v tabulce emp hello v databázi hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-248">There are three columns – **ID**, **FirstName**, and **LastName** – in hello emp table in hello database.</span></span> <span data-ttu-id="ca4ab-249">ID je sloupec identity, takže je nutné pouze toospecify **FirstName** a **LastName** sem.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-249">ID is an identity column, so you need toospecify only **FirstName** and **LastName** here.</span></span>

    <span data-ttu-id="ca4ab-250">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure SQL](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-250">For more information about these JSON properties, see [Azure SQL connector article](data-factory-azure-sql-connector.md#dataset-properties).</span></span>

## <a name="create-pipeline"></a><span data-ttu-id="ca4ab-251">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="ca4ab-251">Create pipeline</span></span>
<span data-ttu-id="ca4ab-252">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-252">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

<span data-ttu-id="ca4ab-253">Výstupní datové sady v současné době je, jaké jednotky hello plán.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-253">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="ca4ab-254">V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-254">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="ca4ab-255">Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-255">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="ca4ab-256">Proto 24 řezů výstupní datové sady vyprodukované hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-256">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span> 

1. <span data-ttu-id="ca4ab-257">Klikněte pravým tlačítkem na **kanály** v hello **Průzkumníku řešení**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-257">Right-click **Pipelines** in hello **Solution Explorer**, point too**Add**, and click **New Item**.</span></span>  
2. <span data-ttu-id="ca4ab-258">Vyberte **kanál kopírování dat** v hello **přidat novou položku** dialogové okno a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-258">Select **Copy Data Pipeline** in hello **Add New Item** dialog box and click **Add**.</span></span> 
3. <span data-ttu-id="ca4ab-259">Hello JSON nahraďte hello následující JSON a uložte hello **CopyActivity1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-259">Replace hello JSON with hello following JSON and save hello **CopyActivity1.json** file.</span></span>

  ```json   
    {
     "name": "ADFTutorialPipeline",
     "properties": {
       "description": "Copy data from a blob tooAzure SQL table",
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
    - <span data-ttu-id="ca4ab-260">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-260">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="ca4ab-261">Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-261">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="ca4ab-262">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-262">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="ca4ab-263">Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-263">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="ca4ab-264">V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-264">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="ca4ab-265">Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-265">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="ca4ab-266">toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-266">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
     
    <span data-ttu-id="ca4ab-267">Nahraďte hodnotu hello hello **spustit** vlastnost s hello aktuální den a **end** hodnotu s hello další den.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-267">Replace hello value of hello **start** property with hello current day and **end** value with hello next day.</span></span> <span data-ttu-id="ca4ab-268">Můžete zadat jenom část data hello a přeskočit část času hello hello datum a čas.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-268">You can specify only hello date part and skip hello time part of hello date time.</span></span> <span data-ttu-id="ca4ab-269">Například "2016-02-03", který je ekvivalentní příliš "2016-02-03T00:00:00Z"</span><span class="sxs-lookup"><span data-stu-id="ca4ab-269">For example, "2016-02-03", which is equivalent too"2016-02-03T00:00:00Z"</span></span>
     
    <span data-ttu-id="ca4ab-270">Počáteční a koncové hodnoty data a času musí být ve [formátu ISO](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-270">Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="ca4ab-271">Například: 2016-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-271">For example: 2016-10-14T16:32:41Z.</span></span> <span data-ttu-id="ca4ab-272">Hello **end** čas je nepovinný, ale používáme v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-272">hello **end** time is optional, but we use it in this tutorial.</span></span> 
     
    <span data-ttu-id="ca4ab-273">Pokud nezadáte hodnotu hello **end** vlastnost, vypočítá se jako "**start + 48 hodin**".</span><span class="sxs-lookup"><span data-stu-id="ca4ab-273">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours**".</span></span> <span data-ttu-id="ca4ab-274">bez omezení, zadejte toorun hello kanálu **9999-09-09** hello hodnotu hello **end** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-274">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span>
     
    <span data-ttu-id="ca4ab-275">V předchozím příkladu hello jsou 24 datových řezů jako se vytvářejí každou hodinu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-275">In hello preceding example, there are 24 data slices as each data slice is produced hourly.</span></span>

    <span data-ttu-id="ca4ab-276">Popisy vlastností JSON použitých v definici kanálu najdete v článku [Vytvoření kanálů](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-276">For descriptions of JSON properties in a pipeline definition, see [create pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="ca4ab-277">Popisy vlastností JSON použitých v definici aktivity kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-277">For descriptions of JSON properties in a copy activity definition, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="ca4ab-278">Popisy vlastností JSON podporovaných zdrojem BlobSource najdete v článku [Konektor Azure Blob](data-factory-azure-blob-connector.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-278">For descriptions of JSON properties supported by BlobSource, see [Azure Blob connector article](data-factory-azure-blob-connector.md).</span></span> <span data-ttu-id="ca4ab-279">Popisy vlastností JSON podporovaných jímkou SqlSink najdete v článku [Konektor Azure SQL Database](data-factory-azure-sql-connector.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-279">For descriptions of JSON properties supported by SqlSink, see [Azure SQL Database connector article](data-factory-azure-sql-connector.md).</span></span>

## <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="ca4ab-280">Publikování/nasazení entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="ca4ab-280">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="ca4ab-281">V tomto kroku publikujete entity služby Data Factory (propojené služby, datové sady a kanál), které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-281">In this step, you publish Data Factory entities (linked services, datasets, and pipeline) you created earlier.</span></span> <span data-ttu-id="ca4ab-282">Můžete také určit, že název hello hello nové data factory toobe vytvořili toohold tyto entity.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-282">You also specify hello name of hello new data factory toobe created toohold these entities.</span></span>  

1. <span data-ttu-id="ca4ab-283">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-283">Right-click project in hello Solution Explorer, and click **Publish**.</span></span> 
2. <span data-ttu-id="ca4ab-284">Pokud se zobrazí **přihlášení tooyour účtu Microsoft** dialogové okno, zadejte svoje přihlašovací údaje pro hello účet, který má předplatné Azure a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-284">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="ca4ab-285">Měli byste vidět hello následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-285">You should see hello following dialog box:</span></span>
   
   ![Dialogové okno publikování](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. <span data-ttu-id="ca4ab-287">Na stránce objektu pro vytváření dat konfigurace hello hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-287">In hello Configure data factory page, do hello following steps:</span></span> 
   
   1. <span data-ttu-id="ca4ab-288">Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-288">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="ca4ab-289">Zadejte **VSTutorialFactory** jako **Název**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-289">Enter **VSTutorialFactory** for **Name**.</span></span>  
      
      > [!IMPORTANT]
      > <span data-ttu-id="ca4ab-290">Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-290">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="ca4ab-291">Pokud se při publikování zobrazí chybu o hello název objektu pro vytváření dat, změňte název hello hello objekt pro vytváření dat (třeba na Váš_název_vstutorialfactory) a zkuste publikování znovu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-291">If you receive an error about hello name of data factory when publishing, change hello name of hello data factory (for example, yournameVSTutorialFactory) and try publishing again.</span></span> <span data-ttu-id="ca4ab-292">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-292">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>        
      > 
      > 
   3. <span data-ttu-id="ca4ab-293">Vyberte předplatné Azure pro hello **předplatné** pole.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-293">Select your Azure subscription for hello **Subscription** field.</span></span>
      
      > [!IMPORTANT]
      > <span data-ttu-id="ca4ab-294">Pokud se nezobrazí žádné předplatné. Ujistěte se, že jste se přihlásili pomocí účtu, který je správce nebo spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-294">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>  
      > 
      > 
   4. <span data-ttu-id="ca4ab-295">Vyberte hello **skupiny prostředků** pro hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-295">Select hello **resource group** for hello data factory toobe created.</span></span> 
   5. <span data-ttu-id="ca4ab-296">Vyberte hello **oblast** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-296">Select hello **region** for hello data factory.</span></span> <span data-ttu-id="ca4ab-297">V rozevíracím seznamu hello jsou uvedeny pouze oblasti nepodporuje hello služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-297">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
   6. <span data-ttu-id="ca4ab-298">Klikněte na tlačítko **Další** tooswitch toohello **publikování položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-298">Click **Next** tooswitch toohello **Publish Items** page.</span></span>
      
       ![Stránka konfigurace služby Data Factory](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. <span data-ttu-id="ca4ab-300">V hello **publikování položky** stránky, ujistěte se, že všechny hello datové továrny entity jsou vybrané a klikněte na tlačítko **Další** tooswitch toohello **Souhrn** stránky.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-300">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>
   
   ![Stránka publikování položek](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. <span data-ttu-id="ca4ab-302">Zkontrolujte hello shrnutí a klikněte na **Další** toostart hello nasazení proces a zobrazení hello **stav nasazení**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-302">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>
   
   ![Stránka souhrnu publikování](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. <span data-ttu-id="ca4ab-304">V hello **stav nasazení** stránky, měli byste vidět hello stav procesu nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-304">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="ca4ab-305">Po dokončení hello nasazení, klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-305">Click Finish after hello deployment is done.</span></span>
 
   ![Stránka stavu nasazení](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

<span data-ttu-id="ca4ab-307">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-307">Note hello following points:</span></span> 

* <span data-ttu-id="ca4ab-308">Pokud se zobrazí chyba hello: "Toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory", proveďte jednu z následujících hello a zkuste znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-308">If you receive hello error: "This subscription is not registered toouse namespace Microsoft.DataFactory", do one of hello following and try publishing again:</span></span> 
  
  * <span data-ttu-id="ca4ab-309">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-309">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span> 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    <span data-ttu-id="ca4ab-310">Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-310">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span> 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * <span data-ttu-id="ca4ab-311">Přihlášení pomocí hello předplatného Azure hello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-311">Login using hello Azure subscription into hello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="ca4ab-312">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-312">This action automatically registers hello provider for you.</span></span>
* <span data-ttu-id="ca4ab-313">Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-313">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ca4ab-314">instance služby Data Factory toocreate, je třeba toobe správce nebo spolusprávce předplatného Azure Dobrý den</span><span class="sxs-lookup"><span data-stu-id="ca4ab-314">toocreate Data Factory instances, you need toobe a admin/co-admin of hello Azure subscription</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="ca4ab-315">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="ca4ab-315">Monitor pipeline</span></span>
<span data-ttu-id="ca4ab-316">Přejděte toohello Domovská stránka objektu pro vytváření dat:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-316">Navigate toohello home page for your data factory:</span></span>

1. <span data-ttu-id="ca4ab-317">Přihlaste se příliš[portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-317">Log in too[Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ca4ab-318">Klikněte na tlačítko **další služby** hello levé nabídce a klikněte na tlačítko **datové továrny**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-318">Click **More services** on hello left menu, and click **Data factories**.</span></span>

    ![Procházet -> Datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. <span data-ttu-id="ca4ab-320">Začněte psát název vaší služby data factory hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-320">Start typing hello name of your data factory.</span></span>

    ![Název datové továrny](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. <span data-ttu-id="ca4ab-322">Klikněte na datovou továrnu hello výsledky seznamu toosee hello domovské stránce objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-322">Click your data factory in hello results list toosee hello home page for your data factory.</span></span>

    ![Domovská stránka objektu pro vytváření dat](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. <span data-ttu-id="ca4ab-324">Postupujte podle pokynů v [monitorování datových sad a kanálu](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello kanálu a datových sad v tomto kurzu jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-324">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="ca4ab-325">V současné době Visual Studio monitorování kanálů Data Factory nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-325">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span> 

## <a name="summary"></a><span data-ttu-id="ca4ab-326">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ca4ab-326">Summary</span></span>
<span data-ttu-id="ca4ab-327">V tomto kurzu jste vytvořili Azure data factory toocopy dat z Azure SQL database tooan objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-327">In this tutorial, you created an Azure data factory toocopy data from an Azure blob tooan Azure SQL database.</span></span> <span data-ttu-id="ca4ab-328">Můžete použít Visual Studio toocreate hello objekt pro vytváření dat, propojených služeb, datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-328">You used Visual Studio toocreate hello data factory, linked services, datasets, and a pipeline.</span></span> <span data-ttu-id="ca4ab-329">Zde jsou hello kroků, které jste provedli v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-329">Here are hello high-level steps you performed in this tutorial:</span></span>  

1. <span data-ttu-id="ca4ab-330">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-330">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="ca4ab-331">Vytvořili jste **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-331">Created **linked services**:</span></span>
   1. <span data-ttu-id="ca4ab-332">**Azure Storage** propojené služby toolink účtu úložiště Azure, který obsahuje vstupní data.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-332">An **Azure Storage** linked service toolink your Azure Storage account that holds input data.</span></span>     
   2. <span data-ttu-id="ca4ab-333">**Azure SQL** propojené toolink služby Azure SQL database, který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-333">An **Azure SQL** linked service toolink your Azure SQL database that holds hello output data.</span></span> 
3. <span data-ttu-id="ca4ab-334">Vytvořili jste **datové sady**, které popisují vstupní data a výstupní data pro kanály.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-334">Created **datasets**, which describe input data and output data for pipelines.</span></span>
4. <span data-ttu-id="ca4ab-335">Vytvořili jste **kanál** s **aktivitou kopírování**, která má jako zdroj **BlobSource** a jako jímku **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-335">Created a **pipeline** with a **Copy Activity** with **BlobSource** as source and **SqlSink** as sink.</span></span> 

<span data-ttu-id="ca4ab-336">toosee jak zjistit, toouse tootransform data aktivity HDInsight Hive pomocí clusteru Azure HDInsight [ kurz: vytvoření vaší první dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-336">toosee how toouse a HDInsight Hive Activity tootransform data by using Azure HDInsight cluster, see [ Tutorial: Build your first pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="ca4ab-337">Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-337">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="ca4ab-338">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-338">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 

## <a name="view-all-data-factories-in-server-explorer"></a><span data-ttu-id="ca4ab-339">Zobrazení všech datových továren v Průzkumníku serveru</span><span class="sxs-lookup"><span data-stu-id="ca4ab-339">View all data factories in Server Explorer</span></span>
<span data-ttu-id="ca4ab-340">Tato část popisuje, jak toouse hello Průzkumníka serveru v sadě Visual Studio tooview všechny datové továrny hello ve vašem předplatném Azure a vytvořit projekt sady Visual Studio založený na existujícím objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-340">This section describes how toouse hello Server Explorer in Visual Studio tooview all hello data factories in your Azure subscription and create a Visual Studio project based on an existing data factory.</span></span> 

1. <span data-ttu-id="ca4ab-341">V **Visual Studio**, klikněte na tlačítko **zobrazení** hello nabídce a klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-341">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="ca4ab-342">V okně hello Průzkumníka serveru rozbalte **Azure** a rozbalte **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-342">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="ca4ab-343">Pokud se zobrazí **přihlášení tooVisual Studio**, zadejte hello **účet** přidružené k vašemu předplatnému Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-343">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="ca4ab-344">Zadejte **heslo** a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-344">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="ca4ab-345">Visual Studio se pokusí tooget informace o všechny objekty pro vytváření dat Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-345">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="ca4ab-346">Zobrazí hello stav této operace v hello **seznam úkolů objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-346">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Průzkumník serveru](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a><span data-ttu-id="ca4ab-348">Vytvoření projektu ve Visual Studiu pro existující datovou továrnu</span><span class="sxs-lookup"><span data-stu-id="ca4ab-348">Create a Visual Studio project for an existing data factory</span></span>

- <span data-ttu-id="ca4ab-349">Klikněte pravým tlačítkem na objekt pro vytváření dat v Průzkumníku serveru a vyberte **tooNew Export Data Factory Project** toocreate projekt sady Visual Studio podle existující datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-349">Right-click a data factory in Server Explorer, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Export data factory tooa VS projekt](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="ca4ab-351">Aktualizace nástrojů služby Data Factory pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ca4ab-351">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="ca4ab-352">tooupdate Azure Data Factory tools pro Visual Studio, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-352">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="ca4ab-353">Klikněte na tlačítko **nástroje** na hello nabídku a vyberte **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-353">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span> 
2. <span data-ttu-id="ca4ab-354">Vyberte **aktualizace** v levém podokně hello a pak vyberte **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-354">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="ca4ab-355">Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-355">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="ca4ab-356">Pokud se tato položka nezobrazí, už máte nejnovější verzi nástroje hello hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-356">If you do not see this entry, you already have hello latest version of hello tools.</span></span> 

## <a name="use-configuration-files"></a><span data-ttu-id="ca4ab-357">Použití konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="ca4ab-357">Use configuration files</span></span>
<span data-ttu-id="ca4ab-358">Konfigurační soubory v sadě Visual Studio tooconfigure vlastnosti můžete použít pro propojených služeb/tabulek/kanálů pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-358">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="ca4ab-359">Vezměte v úvahu následující definici JSON pro služby Azure Storage, propojené hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-359">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="ca4ab-360">toospecify **connectionString** s různými hodnotami pro accountname a accountkey pro různá prostředí (Dev/testovací/produkční) toowhich hello nasazujete entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-360">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="ca4ab-361">Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-361">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="ca4ab-362">Přidání konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="ca4ab-362">Add a configuration file</span></span>
<span data-ttu-id="ca4ab-363">Přidejte konfigurační soubor pro každé prostředí provedením následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-363">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="ca4ab-364">Klikněte pravým tlačítkem na projekt Data Factory hello v řešení sady Visual Studio, přejděte příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-364">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="ca4ab-365">Vyberte **konfigurace** hello seznamu nainstalovaných šablon vlevo hello vyberte **konfigurační soubor**, zadejte **název** pro konfiguraci hello souboru a klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-365">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="ca4ab-367">Přidejte konfigurační parametry a jejich hodnoty v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-367">Add configuration parameters and their values in hello following format:</span></span>

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

    <span data-ttu-id="ca4ab-368">Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-368">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="ca4ab-369">Všimněte si, že je hello syntaxe pro určení názvu [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="ca4ab-369">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="ca4ab-370">Pokud má kód JSON vlastnost, která má pole hodnot, jak je znázorněno v následujícím kódu hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-370">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

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

    <span data-ttu-id="ca4ab-371">Nakonfigurujte vlastnosti, jak je znázorněno v následující soubor konfigurace (použijte indexování od nuly) hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-371">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="ca4ab-372">Názvy vlastností s mezerami</span><span class="sxs-lookup"><span data-stu-id="ca4ab-372">Property names with spaces</span></span>
<span data-ttu-id="ca4ab-373">Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru) hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-373">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="ca4ab-374">Nasazení řešení pomocí konfigurace</span><span class="sxs-lookup"><span data-stu-id="ca4ab-374">Deploy solution using a configuration</span></span>
<span data-ttu-id="ca4ab-375">Když v sadě VS publikujete entity služby Azure Data Factory, abyste mohli zadat konfiguraci hello chcete toouse pro danou operaci publikování.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-375">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="ca4ab-376">toopublish entity v projektu Azure Data Factory pomocí konfiguračního souboru:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-376">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="ca4ab-377">Klikněte pravým tlačítkem na projekt Data Factory a klikněte na tlačítko **publikovat** toosee hello **publikování položky** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-377">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="ca4ab-378">Vyberte existující datovou továrnu nebo zadejte hodnoty pro vytvoření služby data factory na hello **objekt pro vytváření dat konfigurace** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-378">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="ca4ab-379">Na hello **publikování položky** stránky: zobrazí rozevírací seznam s dostupnými konfiguracemi pro hello **výběr konfigurace nasazení** pole.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-379">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="ca4ab-381">Vyberte hello **konfigurační soubor** , že chcete vytvořit toouse a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-381">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="ca4ab-382">Zkontrolujte, jestli hello název souboru JSON v hello **Souhrn** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-382">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="ca4ab-383">Klikněte na tlačítko **Dokončit** po dokončení operace nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-383">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="ca4ab-384">Když nasadíte, hello hodnoty z konfiguračního souboru hello jsou použité tooset hodnoty vlastností v souborech JSON hello před hello entity jsou nasazené tooAzure služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-384">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="ca4ab-385">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ca4ab-385">Use Azure Key Vault</span></span>
<span data-ttu-id="ca4ab-386">Není vhodné a často proti zabezpečení zásady toocommit citlivých dat jako úložiště kódu toohello řetězce připojení.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-386">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="ca4ab-387">V tématu [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) ukázku na Githubu toolearn o ukládání citlivých informací v Azure Key Vault a jeho použití při publikování entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-387">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="ca4ab-388">Hello zabezpečeného publikování rozšíření pro Visual Studio umožňuje toobe hello tajné klíče uložené v Key Vault a pouze odkazy toothem jsou určené v propojené služby nebo konfigurace nasazení.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-388">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="ca4ab-389">Tyto odkazy se překládají při publikování tooAzure entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-389">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="ca4ab-390">Tyto soubory pak lze potvrdit toosource úložiště bez vystavení všech tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-390">These files can then be committed toosource repository without exposing any secrets.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ca4ab-391">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ca4ab-391">Next steps</span></span>
<span data-ttu-id="ca4ab-392">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-392">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="ca4ab-393">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="ca4ab-393">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="ca4ab-394">toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="ca4ab-394">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
