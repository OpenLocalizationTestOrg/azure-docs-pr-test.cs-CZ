---
title: "aaaBuild vaše první objekt pro vytváření dat (Visual Studio) | Microsoft Docs"
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
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a><span data-ttu-id="4336a-103">Kurz: Vytvoření datové továrny pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4336a-103">Tutorial: Create a data factory by using Visual Studio</span></span>
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [<span data-ttu-id="4336a-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="4336a-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="4336a-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4336a-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="4336a-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4336a-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="4336a-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="4336a-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="4336a-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4336a-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="4336a-109">REST API</span><span class="sxs-lookup"><span data-stu-id="4336a-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)

<span data-ttu-id="4336a-110">Tento kurz ukazuje, jak toocreate objekt pro vytváření dat Azure pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4336a-110">This tutorial shows you how toocreate an Azure data factory by using Visual Studio.</span></span> <span data-ttu-id="4336a-111">Vytvořit projekt sady Visual Studio pomocí šablony projektu pro vytváření dat hello, zadejte ve formátu JSON entit služby Data Factory (propojené služby, datové sady a kanál) a pak publikování/nasazení cloudu toohello tyto entity.</span><span class="sxs-lookup"><span data-stu-id="4336a-111">You create a Visual Studio project using hello Data Factory project template, define Data Factory entities (linked services, datasets, and pipeline) in JSON format, and then publish/deploy these entities toohello cloud.</span></span> 

<span data-ttu-id="4336a-112">Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="4336a-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="4336a-113">Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="4336a-114">Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="4336a-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="4336a-115">Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-115">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="4336a-116">Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="4336a-117">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="4336a-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="4336a-118">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="4336a-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="4336a-119">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="4336a-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>


## <a name="walkthrough-create-and-publish-data-factory-entities"></a><span data-ttu-id="4336a-120">Názorný postup: Vytvoření a publikování entit Data Factory</span><span class="sxs-lookup"><span data-stu-id="4336a-120">Walkthrough: Create and publish Data Factory entities</span></span>
<span data-ttu-id="4336a-121">Zde jsou hello kroky, které můžete provádět v rámci tohoto návodu:</span><span class="sxs-lookup"><span data-stu-id="4336a-121">Here are hello steps you perform as part of this walkthrough:</span></span>

1. <span data-ttu-id="4336a-122">Vytvořte dvě propojené služby: **AzureStorageLinkedService1** a **HDInsightOnDemandLinkedService1**.</span><span class="sxs-lookup"><span data-stu-id="4336a-122">Create two linked services: **AzureStorageLinkedService1** and **HDInsightOnDemandLinkedService1**.</span></span> 
   
    <span data-ttu-id="4336a-123">V tomto kurzu vstupní a výstupní data pro aktivitu hive hello jsou v hello stejné úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-123">In this tutorial, both input and output data for hello hive activity are in hello same Azure Blob Storage.</span></span> <span data-ttu-id="4336a-124">Použijete na vyžádání HDInsight clusteru tooprocess existující vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-124">You use an on-demand HDInsight cluster tooprocess existing input data tooproduce output data.</span></span> <span data-ttu-id="4336a-125">cluster HDInsight na vyžádání Hello se automaticky vytvoří za vás službou Azure Data Factory v době běhu, pokud vstupní data hello je připraven toobe zpracovat.</span><span class="sxs-lookup"><span data-stu-id="4336a-125">hello on-demand HDInsight cluster is automatically created for you by Azure Data Factory at run time when hello input data is ready toobe processed.</span></span> <span data-ttu-id="4336a-126">Je nutné toolink ukládá data nebo vypočítá tooyour pro vytváření dat, aby se služba Data Factory hello připojovat toothem za běhu.</span><span class="sxs-lookup"><span data-stu-id="4336a-126">You need toolink your data stores or computes tooyour data factory so that hello Data Factory service can connect toothem at runtime.</span></span> <span data-ttu-id="4336a-127">Proto propojení účtu úložiště Azure služby data factory toohello pomocí hello AzureStorageLinkedService1 a propojení clusteru HDInsight na vyžádání pomocí hello HDInsightOnDemandLinkedService1.</span><span class="sxs-lookup"><span data-stu-id="4336a-127">Therefore, you link your Azure Storage Account toohello data factory by using hello AzureStorageLinkedService1, and link an on-demand HDInsight cluster by using hello HDInsightOnDemandLinkedService1.</span></span> <span data-ttu-id="4336a-128">Při publikování, je třeba zadat název hello hello data factory toobe vytvořen nebo existující datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="4336a-128">When publishing, you specify hello name for hello data factory toobe created or an existing data factory.</span></span>  
2. <span data-ttu-id="4336a-129">Vytvořte dvě datové sady: **InputDataset** a **OutputDataset**, které představují vstupní a výstupní data hello uložených v hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-129">Create two datasets: **InputDataset** and **OutputDataset**, which represent hello input/output data that is stored in hello Azure blob storage.</span></span> 
   
    <span data-ttu-id="4336a-130">Tyto definice datové sady odkazují toohello propojenou službu úložiště Azure, kterou jste vytvořili v předchozím kroku hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-130">These dataset definitions refer toohello Azure Storage linked service you created in hello previous step.</span></span> <span data-ttu-id="4336a-131">Pro hello InputDataset zadejte hello blob kontejneru (adfgetstarted) a hello složky (inptutdata), která obsahuje objekt blob se vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-131">For hello InputDataset, you specify hello blob container (adfgetstarted) and hello folder (inptutdata) that contains a blob with hello input data.</span></span> <span data-ttu-id="4336a-132">Pro hello OutputDataset zadejte hello blob kontejneru (adfgetstarted) a složku hello (partitioneddata), který obsahuje hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-132">For hello OutputDataset, you specify hello blob container (adfgetstarted) and hello folder (partitioneddata) that holds hello output data.</span></span> <span data-ttu-id="4336a-133">Zadáte také další vlastnosti, jako jsou například struktura, dostupnost a zásady.</span><span class="sxs-lookup"><span data-stu-id="4336a-133">You also specify other properties such as structure, availability, and policy.</span></span>
3. <span data-ttu-id="4336a-134">Vytvořte kanál s názvem **MyFirstPipeline**.</span><span class="sxs-lookup"><span data-stu-id="4336a-134">Create a pipeline named **MyFirstPipeline**.</span></span> 
  
    <span data-ttu-id="4336a-135">V tomto návodu hello kanálu má jenom jedna aktivita: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="4336a-135">In this walkthrough, hello pipeline has only one activity: **HDInsight Hive Activity**.</span></span> <span data-ttu-id="4336a-136">Tato aktivita transformace vstupní data tooproduce výstupní data pomocí skriptu hive v clusteru HDInsight na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="4336a-136">This activity transform input data tooproduce output data by running a hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4336a-137">toolearn Další informace o hive aktivity, najdete v části [Hive aktivity](data-factory-hive-activity.md)</span><span class="sxs-lookup"><span data-stu-id="4336a-137">toolearn more about hive activity, see [Hive Activity](data-factory-hive-activity.md)</span></span> 
4. <span data-ttu-id="4336a-138">Vytvořte datovou továrnu s názvem **DataFactoryUsingVS**.</span><span class="sxs-lookup"><span data-stu-id="4336a-138">Create a data factory named **DataFactoryUsingVS**.</span></span> <span data-ttu-id="4336a-139">Nasaďte hello data factory a všechny entity služby Data Factory (propojené služby, tabulky a kanál hello).</span><span class="sxs-lookup"><span data-stu-id="4336a-139">Deploy hello data factory and all Data Factory entities (linked services, tables, and hello pipeline).</span></span>
5. <span data-ttu-id="4336a-140">Po publikování, pomocí oken webu Azure portal a sledování aplikace a správu toomonitor hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="4336a-140">After you publish, you use Azure portal blades and Monitoring & Management App toomonitor hello pipeline.</span></span> 
  
### <a name="prerequisites"></a><span data-ttu-id="4336a-141">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4336a-141">Prerequisites</span></span>
1. <span data-ttu-id="4336a-142">Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="4336a-142">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span> <span data-ttu-id="4336a-143">Můžete také vybrat hello **přehled a požadavky** možnost v rozevíracím seznamu hello v článku toohello nejvyšší tooswitch hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-143">You can also select hello **Overview and prerequisites** option in hello drop-down list at hello top tooswitch toohello article.</span></span> <span data-ttu-id="4336a-144">Po dokončení hello požadavky přepnout zpět toothis článku výběrem **Visual Studio** možnost v rozevíracím seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-144">After you complete hello prerequisites, switch back toothis article by selecting **Visual Studio** option in hello drop-down list.</span></span>
2. <span data-ttu-id="4336a-145">instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-145">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>  
3. <span data-ttu-id="4336a-146">Musíte mít nainstalované ve vašem počítači hello tyto položky:</span><span class="sxs-lookup"><span data-stu-id="4336a-146">You must have hello following installed on your computer:</span></span>
   * <span data-ttu-id="4336a-147">Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="4336a-147">Visual Studio 2013 or Visual Studio 2015</span></span>
   * <span data-ttu-id="4336a-148">Stáhněte si sadu Azure SDK pro Visual Studio 2013 nebo Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="4336a-148">Download Azure SDK for Visual Studio 2013 or Visual Studio 2015.</span></span> <span data-ttu-id="4336a-149">Přejděte příliš[stránku položek ke stažení Azure](https://azure.microsoft.com/downloads/) a klikněte na tlačítko **VS 2013** nebo **VS 2015** v hello **.NET** části.</span><span class="sxs-lookup"><span data-stu-id="4336a-149">Navigate too[Azure Download Page](https://azure.microsoft.com/downloads/) and click **VS 2013** or **VS 2015** in hello **.NET** section.</span></span>
   * <span data-ttu-id="4336a-150">Stáhnout hello nejnovější modul plug-in Azure Data Factory pro Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) nebo [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span><span class="sxs-lookup"><span data-stu-id="4336a-150">Download hello latest Azure Data Factory plugin for Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) or [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005).</span></span> <span data-ttu-id="4336a-151">Můžete také aktualizovat modul plug-in hello provedením následujících kroků hello: hello nabídce klikněte na **nástroje** -> **rozšíření a aktualizace** -> **Online**  ->  **Galerie sady visual Studio** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="4336a-151">You can also update hello plugin by doing hello following steps: On hello menu, click **Tools** -> **Extensions and Updates** -> **Online** -> **Visual Studio Gallery** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **Update**.</span></span>

<span data-ttu-id="4336a-152">Teď umožňuje pomocí sady Visual Studio toocreate služby Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-152">Now, let's use Visual Studio toocreate an Azure data factory.</span></span>

### <a name="create-visual-studio-project"></a><span data-ttu-id="4336a-153">Vytvoření projektu v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4336a-153">Create Visual Studio project</span></span>
1. <span data-ttu-id="4336a-154">Spusťte **Visual Studio 2013** nebo **Visual Studio 2015**.</span><span class="sxs-lookup"><span data-stu-id="4336a-154">Launch **Visual Studio 2013** or **Visual Studio 2015**.</span></span> <span data-ttu-id="4336a-155">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="4336a-155">Click **File**, point too**New**, and click **Project**.</span></span> <span data-ttu-id="4336a-156">Měli byste vidět hello **nový projekt** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-156">You should see hello **New Project** dialog box.</span></span>  
2. <span data-ttu-id="4336a-157">V hello **nový projekt** dialogové okno, vyberte hello **DataFactory** šablonu a klikněte na **prázdný projekt Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="4336a-157">In hello **New Project** dialog, select hello **DataFactory** template, and click **Empty Data Factory Project**.</span></span>   

    ![Dialogové okno Nový projekt](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. <span data-ttu-id="4336a-159">Zadejte **název** pro projekt hello **umístění**a název hello **řešení**a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4336a-159">Enter a **name** for hello project, **location**, and a name for hello **solution**, and click **OK**.</span></span>

    ![Průzkumník řešení](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a><span data-ttu-id="4336a-161">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="4336a-161">Create linked services</span></span>
<span data-ttu-id="4336a-162">V tomto kroku vytvoříte dvě propojené služby, **Azure Storage** a **HDInsight na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="4336a-162">In this step, you create two linked services: **Azure Storage** and **HDInsight on-demand**.</span></span> 

<span data-ttu-id="4336a-163">Hello Azure Storage propojená služba propojuje účet úložiště Azure toohello datovou továrnu tím, že poskytuje informace o připojení hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-163">hello Azure Storage linked service links your Azure Storage account toohello data factory by providing hello connection information.</span></span> <span data-ttu-id="4336a-164">Služba data Factory používá hello připojovacího řetězce z toohello tooconnect nastavení hello propojené služby úložiště Azure za běhu.</span><span class="sxs-lookup"><span data-stu-id="4336a-164">Data Factory service uses hello connection string from hello linked service setting tooconnect toohello Azure storage at runtime.</span></span> <span data-ttu-id="4336a-165">Toto úložiště obsahuje vstupní a výstupní data pro kanál hello a hello hive soubor skriptu hive aktivitou hello používá.</span><span class="sxs-lookup"><span data-stu-id="4336a-165">This storage holds input and output data for hello pipeline, and hello hive script file used by hello hive activity.</span></span> 

<span data-ttu-id="4336a-166">S na vyžádání propojené služby HDInsight se hello clusteru HDInsight se automaticky vytvoří za běhu při hello vstupních dat je připraven tooprocessed.</span><span class="sxs-lookup"><span data-stu-id="4336a-166">With on-demand HDInsight linked service, hello HDInsight cluster is automatically created at runtime when hello input data is ready tooprocessed.</span></span> <span data-ttu-id="4336a-167">Odstranění clusteru Hello až dokončí zpracování a nečinnosti pro hello zadanou dobu.</span><span class="sxs-lookup"><span data-stu-id="4336a-167">hello cluster is deleted after it is done processing and idle for hello specified amount of time.</span></span> 

> [!NOTE]
> <span data-ttu-id="4336a-168">Vytvoříte objekt pro vytváření dat zadáním jeho název a nastavení v době hello publikování řešení Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-168">You create a data factory by specifying its name and settings at hello time of publishing your Data Factory solution.</span></span>

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="4336a-169">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="4336a-169">Create Azure Storage linked service</span></span>
1. <span data-ttu-id="4336a-170">Klikněte pravým tlačítkem na **propojené služby** v Průzkumníku řešení hello bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4336a-170">Right-click **Linked Services** in hello solution explorer, point too**Add**, and click **New Item**.</span></span>      
2. <span data-ttu-id="4336a-171">V hello **přidat novou položku** dialogové okno, vyberte **propojená služba úložiště Azure** hello seznamu a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-171">In hello **Add New Item** dialog box, select **Azure Storage Linked Service** from hello list, and click **Add**.</span></span>
    <span data-ttu-id="4336a-172">![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="4336a-172">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)</span></span>
3. <span data-ttu-id="4336a-173">Nahraďte `<accountname>` a `<accountkey>` s hello název účtu úložiště Azure a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="4336a-173">Replace `<accountname>` and `<accountkey>` with hello name of your Azure storage account and its key.</span></span> <span data-ttu-id="4336a-174">toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="4336a-174">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
    <span data-ttu-id="4336a-175">![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span><span class="sxs-lookup"><span data-stu-id="4336a-175">![Azure Storage Linked Service](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)</span></span>
4. <span data-ttu-id="4336a-176">Uložit hello **AzureStorageLinkedService1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="4336a-176">Save hello **AzureStorageLinkedService1.json** file.</span></span>

#### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="4336a-177">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="4336a-177">Create Azure HDInsight linked service</span></span>
1. <span data-ttu-id="4336a-178">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **propojené služby**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4336a-178">In hello **Solution Explorer**, right-click **Linked Services**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4336a-179">Vyberte **HDInsight On Demand Linked Service** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-179">Select **HDInsight On Demand Linked Service**, and click **Add**.</span></span>
3. <span data-ttu-id="4336a-180">Nahraďte hello **JSON** s hello následující JSON:</span><span class="sxs-lookup"><span data-stu-id="4336a-180">Replace hello **JSON** with hello following JSON:</span></span>

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

    <span data-ttu-id="4336a-181">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-181">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="4336a-182">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4336a-182">Property</span></span> | <span data-ttu-id="4336a-183">Popis</span><span class="sxs-lookup"><span data-stu-id="4336a-183">Description</span></span>
    -------- | ----------- 
    <span data-ttu-id="4336a-184">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="4336a-184">ClusterSize</span></span> | <span data-ttu-id="4336a-185">Určuje velikost hello hello clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4336a-185">Specifies hello size of hello HDInsight Hadoop cluster.</span></span>
    <span data-ttu-id="4336a-186">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="4336a-186">TimeToLive</span></span> | <span data-ttu-id="4336a-187">Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="4336a-187">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span>
    <span data-ttu-id="4336a-188">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="4336a-188">linkedServiceName</span></span> | <span data-ttu-id="4336a-189">Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem clusteru HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="4336a-189">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight Hadoop cluster.</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="4336a-190">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="4336a-190">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="4336a-191">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-191">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="4336a-192">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="4336a-192">This behavior is by design.</span></span> <span data-ttu-id="4336a-193">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="4336a-193">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="4336a-194">Hello clusteru je automaticky odstraněna po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-194">hello cluster is automatically deleted when hello processing is done.</span></span>
    > 
    > <span data-ttu-id="4336a-195">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4336a-195">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="4336a-196">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="4336a-196">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="4336a-197">vzor podle Hello názvy těchto kontejnerů: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="4336a-197">hello names of these containers follow a pattern: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`.</span></span> <span data-ttu-id="4336a-198">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4336a-198">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

    <span data-ttu-id="4336a-199">Další informace o vlastnostech JSON najdete v tématu [Propojené služby Compute](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="4336a-199">For more information about JSON properties, see [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article.</span></span> 
4. <span data-ttu-id="4336a-200">Uložit hello **HDInsightOnDemandLinkedService1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="4336a-200">Save hello **HDInsightOnDemandLinkedService1.json** file.</span></span>

### <a name="create-datasets"></a><span data-ttu-id="4336a-201">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="4336a-201">Create datasets</span></span>
<span data-ttu-id="4336a-202">V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="4336a-202">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="4336a-203">Tyto datové sady odkazují toohello **AzureStorageLinkedService1** jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4336a-203">These datasets refer toohello **AzureStorageLinkedService1** you have created earlier in this tutorial.</span></span> <span data-ttu-id="4336a-204">Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-204">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

#### <a name="create-input-dataset"></a><span data-ttu-id="4336a-205">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="4336a-205">Create input dataset</span></span>
1. <span data-ttu-id="4336a-206">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **tabulky**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4336a-206">In hello **Solution Explorer**, right-click **Tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4336a-207">Vyberte **objektů Blob v Azure** hello seznamu, změňte název hello hello souboru příliš**InputDataSet.json**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-207">Select **Azure Blob** from hello list, change hello name of hello file too**InputDataSet.json**, and click **Add**.</span></span>
3. <span data-ttu-id="4336a-208">Nahraďte hello **JSON** v editoru hello se hello následujícím fragmentu kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="4336a-208">Replace hello **JSON** in hello editor with hello following JSON snippet:</span></span>

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
    <span data-ttu-id="4336a-209">Tento fragment kódu JSON Určuje datovou sadu s názvem **AzureBlobInput** , která představuje vstupní data aktivity hello hive v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-209">This JSON snippet defines a dataset called **AzureBlobInput** that represents input data for hello hive activity in hello pipeline.</span></span> <span data-ttu-id="4336a-210">Určíte, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem `adfgetstarted` a hello složku s názvem `inputdata`.</span><span class="sxs-lookup"><span data-stu-id="4336a-210">You specify that hello input data is located in hello blob container called `adfgetstarted` and hello folder called `inputdata`.</span></span>

    <span data-ttu-id="4336a-211">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-211">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

    <span data-ttu-id="4336a-212">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="4336a-212">Property</span></span> | <span data-ttu-id="4336a-213">Popis</span><span class="sxs-lookup"><span data-stu-id="4336a-213">Description</span></span> |
    -------- | ----------- |
    <span data-ttu-id="4336a-214">type</span><span class="sxs-lookup"><span data-stu-id="4336a-214">type</span></span> |<span data-ttu-id="4336a-215">Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="4336a-215">hello type property is set too**AzureBlob** because data resides in Azure Blob Storage.</span></span>
    <span data-ttu-id="4336a-216">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="4336a-216">linkedServiceName</span></span> | <span data-ttu-id="4336a-217">Odkazuje toohello AzureStorageLinkedService1, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="4336a-217">Refers toohello AzureStorageLinkedService1 you created earlier.</span></span>
    <span data-ttu-id="4336a-218">fileName</span><span class="sxs-lookup"><span data-stu-id="4336a-218">fileName</span></span> |<span data-ttu-id="4336a-219">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="4336a-219">This property is optional.</span></span> <span data-ttu-id="4336a-220">Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="4336a-220">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="4336a-221">V takovém případě pouze hello soubor Input.log.</span><span class="sxs-lookup"><span data-stu-id="4336a-221">In this case, only hello input.log is processed.</span></span>
    <span data-ttu-id="4336a-222">type</span><span class="sxs-lookup"><span data-stu-id="4336a-222">type</span></span> | <span data-ttu-id="4336a-223">soubory protokolu Hello jsou v textovém formátu, takže používáme TextFormat.</span><span class="sxs-lookup"><span data-stu-id="4336a-223">hello log files are in text format, so we use TextFormat.</span></span> |
    <span data-ttu-id="4336a-224">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="4336a-224">columnDelimiter</span></span> | <span data-ttu-id="4336a-225">sloupce v souborech protokolů hello oddělené čárkou hello (`,`)</span><span class="sxs-lookup"><span data-stu-id="4336a-225">columns in hello log files are delimited by hello comma character (`,`)</span></span>
    <span data-ttu-id="4336a-226">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="4336a-226">frequency/interval</span></span> | <span data-ttu-id="4336a-227">frekvence nastavená tooMonth a interval je 1, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="4336a-227">frequency set tooMonth and interval is 1, which means that hello input slices are available monthly.</span></span>
    <span data-ttu-id="4336a-228">external</span><span class="sxs-lookup"><span data-stu-id="4336a-228">external</span></span> | <span data-ttu-id="4336a-229">Tato vlastnost nastavena tootrue, pokud hello vstupní data pro aktivitu hello nevygenerovala hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="4336a-229">This property is set tootrue if hello input data for hello activity is not generated by hello pipeline.</span></span> <span data-ttu-id="4336a-230">Tato vlastnost se určuje jenom pro vstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="4336a-230">This property is only specified on input datasets.</span></span> <span data-ttu-id="4336a-231">Pro hello vstupní datové sady hello první aktivitu vždy nastavte tootrue.</span><span class="sxs-lookup"><span data-stu-id="4336a-231">For hello input dataset of hello first activity, always set it tootrue.</span></span>
4. <span data-ttu-id="4336a-232">Uložit hello **InputDataset.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="4336a-232">Save hello **InputDataset.json** file.</span></span>

#### <a name="create-output-dataset"></a><span data-ttu-id="4336a-233">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="4336a-233">Create output dataset</span></span>
<span data-ttu-id="4336a-234">Teď vytvořte hello výstupní datovou sadu toorepresent výstupní data ve hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-234">Now, you create hello output dataset toorepresent output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="4336a-235">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **tabulky**, bod příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4336a-235">In hello **Solution Explorer**, right-click **tables**, point too**Add**, and click **New Item**.</span></span>
2. <span data-ttu-id="4336a-236">Vyberte **objektů Blob v Azure** hello seznamu, změňte název hello hello souboru příliš**OutputDataset.json**a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-236">Select **Azure Blob** from hello list, change hello name of hello file too**OutputDataset.json**, and click **Add**.</span></span>
3. <span data-ttu-id="4336a-237">Nahraďte hello **JSON** v editoru hello se hello následující JSON:</span><span class="sxs-lookup"><span data-stu-id="4336a-237">Replace hello **JSON** in hello editor with hello following JSON:</span></span>
    
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
    <span data-ttu-id="4336a-238">Hello fragmentu kódu JSON Určuje datovou sadu s názvem **AzureBlobOutput** , představuje výstupní data vytvořená hello hive aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-238">hello JSON snippet defines a dataset called **AzureBlobOutput** that represents output data produced by hello hive activity in hello pipeline.</span></span> <span data-ttu-id="4336a-239">Určíte, že se v kontejneru objektů blob hello názvem umístí výstup hello data je vytvořená pomocí aktivity hive hello `adfgetstarted` a hello složku s názvem `partitioneddata`.</span><span class="sxs-lookup"><span data-stu-id="4336a-239">You specify that hello output data is produced by hello hive activity is placed in hello blob container called `adfgetstarted` and hello folder called `partitioneddata`.</span></span> 
    
    <span data-ttu-id="4336a-240">Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="4336a-240">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span> <span data-ttu-id="4336a-241">plán hello Hello výstupní datovou sadu jednotky hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="4336a-241">hello output dataset drives hello schedule of hello pipeline.</span></span> <span data-ttu-id="4336a-242">kanál Hello spustí každý měsíc mezi jeho počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="4336a-242">hello pipeline runs monthly between its start and end times.</span></span> 

    <span data-ttu-id="4336a-243">V tématu **vytvoření vstupní datové sady hello** části popisy těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="4336a-243">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="4336a-244">Sady nenajdete vlastnost external hello u výstupní datové jako hello datovou sadu vytváří hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="4336a-244">You do not set hello external property on an output dataset as hello dataset is produced by hello pipeline.</span></span>
4. <span data-ttu-id="4336a-245">Uložit hello **OutputDataset.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="4336a-245">Save hello **OutputDataset.json** file.</span></span>

### <a name="create-pipeline"></a><span data-ttu-id="4336a-246">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="4336a-246">Create pipeline</span></span>
<span data-ttu-id="4336a-247">Vytvořili jste hello propojenou službu úložiště Azure a vstupní a výstupní datové sady, pokud.</span><span class="sxs-lookup"><span data-stu-id="4336a-247">You have created hello Azure Storage linked service, and input and output datasets so far.</span></span> <span data-ttu-id="4336a-248">Teď vytvoříte kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="4336a-248">Now, you create a pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="4336a-249">Hello **vstupní** pro hello hive aktivity je nastavený příliš**AzureBlobInput** a **výstup** je nastaven příliš**AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="4336a-249">hello **input** for hello hive activity is set too**AzureBlobInput** and **output** is set too**AzureBlobOutput**.</span></span> <span data-ttu-id="4336a-250">Řezy vstupní datové sady je dostupný jednou měsíčně (frekvence: Month, interval: 1), a hello výstupní řez se vytváří jednou měsíčně příliš.</span><span class="sxs-lookup"><span data-stu-id="4336a-250">A slice of an input dataset is available monthly (frequency: Month, interval: 1), and hello output slice is produced monthly too.</span></span> 

1. <span data-ttu-id="4336a-251">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na **kanály**, bod příliš**přidat**a klikněte na tlačítko **novou položku.**</span><span class="sxs-lookup"><span data-stu-id="4336a-251">In hello **Solution Explorer**, right-click **Pipelines**, point too**Add**, and click **New Item.**</span></span>
2. <span data-ttu-id="4336a-252">Vyberte **kanál transformace Hive** hello seznamu a klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-252">Select **Hive Transformation Pipeline** from hello list, and click **Add**.</span></span>
3. <span data-ttu-id="4336a-253">Nahraďte hello **JSON** s hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="4336a-253">Replace hello **JSON** with hello following snippet:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4336a-254">Nahraďte `<storageaccountname>` hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4336a-254">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

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
    > <span data-ttu-id="4336a-255">Nahraďte `<storageaccountname>` hello název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="4336a-255">Replace `<storageaccountname>` with hello name of your storage account.</span></span>

    <span data-ttu-id="4336a-256">fragmentu kódu JSON Hello definuje kanál, který se skládá z jedné aktivity (aktivita Hive).</span><span class="sxs-lookup"><span data-stu-id="4336a-256">hello JSON snippet defines a pipeline that consists of a single activity (Hive Activity).</span></span> <span data-ttu-id="4336a-257">Tato aktivita běží vstupní data Hive skriptu tooprocess na na vyžádání HDInsight clusteru tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-257">This activity runs a Hive script tooprocess input data on an on-demand HDInsight cluster tooproduce output data.</span></span> <span data-ttu-id="4336a-258">V části aktivity hello hello kódu JSON kanálu, uvidíte jenom jedna aktivita v hello pole s typem nastavit příliš**HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="4336a-258">In hello activities section of hello pipeline JSON, you see only one activity in hello array with type set too**HDInsightHive**.</span></span> 

    <span data-ttu-id="4336a-259">Ve vlastnostech typu hello, které jsou specifické tooHDInsight Hive aktivity určete, jaké propojenou službu úložiště Azure má soubor skriptu hive hello, soubor skriptu toohello hello cesty a souboru skriptu toohello parametry.</span><span class="sxs-lookup"><span data-stu-id="4336a-259">In hello type properties that are specific tooHDInsight Hive activity, you specify what Azure Storage linked service has hello hive script file, hello path toohello script file, and parameters toohello script file.</span></span> 

    <span data-ttu-id="4336a-260">soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného vlastností hello scriptLinkedService) a v hello `script` složky v kontejneru hello `adfgetstarted`.</span><span class="sxs-lookup"><span data-stu-id="4336a-260">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService), and in hello `script` folder in hello container `adfgetstarted`.</span></span>

    <span data-ttu-id="4336a-261">Hello `defines` část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span><span class="sxs-lookup"><span data-stu-id="4336a-261">hello `defines` section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.</span></span>

    <span data-ttu-id="4336a-262">Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-262">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span> <span data-ttu-id="4336a-263">Jste nakonfigurovali toobe hello datovou sadu vytváří také jednou měsíčně, tedy pouze po řez je produkovaný hello kanálu (protože měsíc hello je stejný v počátečním a koncovým datem).</span><span class="sxs-lookup"><span data-stu-id="4336a-263">You configured hello dataset toobe produced monthly, therefore, only once slice is produced by hello pipeline (because hello month is same in start and end dates).</span></span>

    <span data-ttu-id="4336a-264">V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="4336a-264">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>
4. <span data-ttu-id="4336a-265">Uložit hello **HiveActivity1.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="4336a-265">Save hello **HiveActivity1.json** file.</span></span>

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a><span data-ttu-id="4336a-266">Přidání souborů partitionweblogs.hql a input.log jako závislosti</span><span class="sxs-lookup"><span data-stu-id="4336a-266">Add partitionweblogs.hql and input.log as a dependency</span></span>
1. <span data-ttu-id="4336a-267">Klikněte pravým tlačítkem na **závislosti** v hello **Průzkumníku řešení** okně bodu příliš**přidat**a klikněte na tlačítko **existující položka**.</span><span class="sxs-lookup"><span data-stu-id="4336a-267">Right-click **Dependencies** in hello **Solution Explorer** window, point too**Add**, and click **Existing Item**.</span></span>  
2. <span data-ttu-id="4336a-268">Přejděte toohello **C:\ADFGettingStarted** a vyberte **partitionweblogs.hql**, **input.log** soubory a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-268">Navigate toohello **C:\ADFGettingStarted** and select **partitionweblogs.hql**, **input.log** files, and click **Add**.</span></span> <span data-ttu-id="4336a-269">Tyto dva soubory jste vytvořili v rámci požadavků z hello [přehled kurzu](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-269">You created these two files as part of prerequisites from hello [Tutorial Overview](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="4336a-270">Když publikujete hello řešení v dalším kroku hello, hello **partitionweblogs.hql** soubor je nahrané toohello **skriptu** složky v hello `adfgetstarted` kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4336a-270">When you publish hello solution in hello next step, hello **partitionweblogs.hql** file is uploaded toohello **script** folder in hello `adfgetstarted` blob container.</span></span>   

### <a name="publishdeploy-data-factory-entities"></a><span data-ttu-id="4336a-271">Publikování/nasazení entit služby Data Factory</span><span class="sxs-lookup"><span data-stu-id="4336a-271">Publish/deploy Data Factory entities</span></span>
<span data-ttu-id="4336a-272">V tomto kroku publikujete hello entit služby Data Factory (propojené služby, datové sady a kanál) ve vašem projektu toohello služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-272">In this step, you publish hello Data Factory entities (linked services, datasets, and pipeline) in your project toohello Azure Data Factory service.</span></span> <span data-ttu-id="4336a-273">V hello proces publikování zadejte hello název objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="4336a-273">In hello process of publishing, you specify hello name for your data factory.</span></span> 

1. <span data-ttu-id="4336a-274">Klikněte pravým tlačítkem na projekt v Průzkumníku řešení hello a klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-274">Right-click project in hello Solution Explorer, and click **Publish**.</span></span>
2. <span data-ttu-id="4336a-275">Pokud se zobrazí **přihlášení tooyour účtu Microsoft** dialogové okno, zadejte svoje přihlašovací údaje pro hello účet, který má předplatné Azure a klikněte na tlačítko **přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="4336a-275">If you see **Sign in tooyour Microsoft account** dialog box, enter your credentials for hello account that has Azure subscription, and click **sign in**.</span></span>
3. <span data-ttu-id="4336a-276">Měli byste vidět hello následující dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="4336a-276">You should see hello following dialog box:</span></span>

   ![Dialogové okno publikování](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. <span data-ttu-id="4336a-278">V hello **objekt pro vytváření dat konfigurace** stránky, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4336a-278">In hello **Configure data factory** page, do hello following steps:</span></span>

    ![Publikování – nová nastavení datové továrny](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. <span data-ttu-id="4336a-280">Vyberte možnost **Create New Data Factory** (Vytvořit nový objekt pro vytváření dat).</span><span class="sxs-lookup"><span data-stu-id="4336a-280">select **Create New Data Factory** option.</span></span>
   2. <span data-ttu-id="4336a-281">Zadejte jedinečný **název** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-281">Enter a unique **name** for hello data factory.</span></span> <span data-ttu-id="4336a-282">Příklad: **DataFactoryUsingVS09152016**.</span><span class="sxs-lookup"><span data-stu-id="4336a-282">For example: **DataFactoryUsingVS09152016**.</span></span> <span data-ttu-id="4336a-283">Název Hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="4336a-283">hello name must be globally unique.</span></span>
   3. <span data-ttu-id="4336a-284">Vyberte správné předplatné hello hello **předplatné** pole.</span><span class="sxs-lookup"><span data-stu-id="4336a-284">Select hello right subscription for hello **Subscription** field.</span></span> 
        > [!IMPORTANT]
        > <span data-ttu-id="4336a-285">Pokud se nezobrazí žádné předplatné. Ujistěte se, že jste se přihlásili pomocí účtu, který je správce nebo spolusprávce předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-285">If you do not see any subscription, ensure that you logged in using an account that is an admin or co-admin of hello subscription.</span></span>
   4. <span data-ttu-id="4336a-286">Vyberte hello **skupiny prostředků** pro hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="4336a-286">Select hello **resource group** for hello data factory toobe created.</span></span>
   5. <span data-ttu-id="4336a-287">Vyberte hello **oblast** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-287">Select hello **region** for hello data factory.</span></span>
   6. <span data-ttu-id="4336a-288">Klikněte na tlačítko **Další** tooswitch toohello **publikování položky** stránky.</span><span class="sxs-lookup"><span data-stu-id="4336a-288">Click **Next** tooswitch toohello **Publish Items** page.</span></span> <span data-ttu-id="4336a-289">(Stisknutím klávesy **KARTĚ** toomove mimo hello název pole tooif hello **Další** tlačítko je zakázané.)</span><span class="sxs-lookup"><span data-stu-id="4336a-289">(Press **TAB** toomove out of hello Name field tooif hello **Next** button is disabled.)</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4336a-290">Pokud se zobrazí chyba hello **název objektu pro vytváření dat "DataFactoryUsingVS" není k dispozici** při publikování, změňte název hello (například yournameDataFactoryUsingVS).</span><span class="sxs-lookup"><span data-stu-id="4336a-290">If you receive hello error **Data factory name “DataFactoryUsingVS” is not available** when publishing, change hello name (for example, yournameDataFactoryUsingVS).</span></span> <span data-ttu-id="4336a-291">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-291">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
1. <span data-ttu-id="4336a-292">V hello **publikování položky** stránky, ujistěte se, že všechny hello datové továrny entity jsou vybrané a klikněte na tlačítko **Další** tooswitch toohello **Souhrn** stránky.</span><span class="sxs-lookup"><span data-stu-id="4336a-292">In hello **Publish Items** page, ensure that all hello Data Factories entities are selected, and click **Next** tooswitch toohello **Summary** page.</span></span>

    ![Stránka publikování položek](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. <span data-ttu-id="4336a-294">Zkontrolujte hello shrnutí a klikněte na **Další** toostart hello nasazení proces a zobrazení hello **stav nasazení**.</span><span class="sxs-lookup"><span data-stu-id="4336a-294">Review hello summary and click **Next** toostart hello deployment process and view hello **Deployment Status**.</span></span>

    ![Stránka souhrnu](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. <span data-ttu-id="4336a-296">V hello **stav nasazení** stránky, měli byste vidět hello stav procesu nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-296">In hello **Deployment Status** page, you should see hello status of hello deployment process.</span></span> <span data-ttu-id="4336a-297">Po dokončení hello nasazení, klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="4336a-297">Click Finish after hello deployment is done.</span></span>

<span data-ttu-id="4336a-298">Toonote důležité body:</span><span class="sxs-lookup"><span data-stu-id="4336a-298">Important points toonote:</span></span>

- <span data-ttu-id="4336a-299">Pokud se zobrazí chyba hello: **toto předplatné není registrované toouse oboru názvů Microsoft.DataFactory**, proveďte jednu z následujících hello a zkuste to znovu publikovat:</span><span class="sxs-lookup"><span data-stu-id="4336a-299">If you receive hello error: **This subscription is not registered toouse namespace Microsoft.DataFactory**, do one of hello following and try publishing again:</span></span>
    - <span data-ttu-id="4336a-300">V prostředí Azure PowerShell spusťte následující příkaz tooregister hello Data Factory zprostředkovatele hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-300">In Azure PowerShell, run hello following command tooregister hello Data Factory provider.</span></span>
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        <span data-ttu-id="4336a-301">Můžete spustit následující příkaz tooconfirm hello této hello zaregistrovat poskytovatele služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-301">You can run hello following command tooconfirm that hello Data Factory provider is registered.</span></span>

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - <span data-ttu-id="4336a-302">Přihlášení pomocí hello předplatného Azure v toohello [portál Azure](https://portal.azure.com) a přejděte tooa okno objekt pro vytváření dat (nebo) vytvořte objekt pro vytváření dat v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-302">Login using hello Azure subscription in toohello [Azure portal](https://portal.azure.com) and navigate tooa Data Factory blade (or) create a data factory in hello Azure portal.</span></span> <span data-ttu-id="4336a-303">Tato akce automaticky registruje zprostředkovatele hello za vás.</span><span class="sxs-lookup"><span data-stu-id="4336a-303">This action automatically registers hello provider for you.</span></span>
- <span data-ttu-id="4336a-304">Hello název objektu pro vytváření dat hello možné zaregistrovat jako název DNS v hello budoucí a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="4336a-304">hello name of hello data factory may be registered as a DNS name in hello future and hence become publically visible.</span></span>
- <span data-ttu-id="4336a-305">instance služby Data Factory toocreate, potřebujete toobe správce nebo spolusprávce hello předplatného Azure</span><span class="sxs-lookup"><span data-stu-id="4336a-305">toocreate Data Factory instances, you need toobe an admin or co-admin of hello Azure subscription</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="4336a-306">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="4336a-306">Monitor pipeline</span></span>
<span data-ttu-id="4336a-307">V tomto kroku můžete monitorovat pomocí zobrazení diagramu objektu pro vytváření dat hello kanál hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-307">In this step, you monitor hello pipeline using Diagram View of hello data factory.</span></span> 

#### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="4336a-308">Monitorování kanálu pomocí Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="4336a-308">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="4336a-309">Přihlaste se toohello [portál Azure](https://portal.azure.com/), hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4336a-309">Log in toohello [Azure portal](https://portal.azure.com/), do hello following steps:</span></span>
   1. <span data-ttu-id="4336a-310">Klikněte na **Další služby** a poté na **Objekty pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-310">Click **More services** and click **Data factories**.</span></span>
       
        ![Procházet -> Datové továrny](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. <span data-ttu-id="4336a-312">Vyberte hello název objektu pro vytváření dat (například: **DataFactoryUsingVS09152016**) ze seznamu hello datové továrny.</span><span class="sxs-lookup"><span data-stu-id="4336a-312">Select hello name of your data factory (for example: **DataFactoryUsingVS09152016**) from hello list of data factories.</span></span>
   
       ![Výběr objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. <span data-ttu-id="4336a-314">Hello domovské stránce objektu pro vytváření dat, klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="4336a-314">In hello home page for your data factory, click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. <span data-ttu-id="4336a-316">V zobrazení diagramu hello zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4336a-316">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. <span data-ttu-id="4336a-318">tooview všechny aktivity v kanálu hello, klikněte pravým tlačítkem na kanál v hello diagram a klikněte na Otevřít kanál.</span><span class="sxs-lookup"><span data-stu-id="4336a-318">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. <span data-ttu-id="4336a-320">Zkontrolujte, jestli aktivita HDInsightHive hello v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-320">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    <span data-ttu-id="4336a-322">toonavigate zpět toohello předchozího zobrazení, klikněte na tlačítko **objekt pro vytváření dat** v nabídce hello s popisem cesty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-322">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
6. <span data-ttu-id="4336a-323">V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="4336a-323">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="4336a-324">Potvrďte, že hello řez je v **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="4336a-324">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="4336a-325">Může trvat několik minut, než se řez tooshow hello ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="4336a-325">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="4336a-326">Pokud není k tomu dojít po určité době čekat na, zjistěte, zda obsahuje hello vstupní soubor (input.log umístěny ve správném kontejneru hello) (`adfgetstarted`) a složku (`inputdata`).</span><span class="sxs-lookup"><span data-stu-id="4336a-326">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (`adfgetstarted`) and folder (`inputdata`).</span></span> <span data-ttu-id="4336a-327">A ujistěte se, že hello **externí** vlastnost v hello vstupní datové sady je nastavená příliš**true**.</span><span class="sxs-lookup"><span data-stu-id="4336a-327">And, make sure that hello **external** property on hello input dataset is set too**true**.</span></span> 

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. <span data-ttu-id="4336a-329">Klikněte na tlačítko **X** tooclose **AzureBlobInput** okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-329">Click **X** tooclose **AzureBlobInput** blade.</span></span>
8. <span data-ttu-id="4336a-330">V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="4336a-330">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="4336a-331">Uvidíte, že hello řez, který se právě zpracovává.</span><span class="sxs-lookup"><span data-stu-id="4336a-331">You see that hello slice that is currently being processed.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. <span data-ttu-id="4336a-333">Po dokončení zpracování uvidíte hello řez ve **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="4336a-333">When processing is done, you see hello slice in **Ready** state.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="4336a-334">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="4336a-334">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="4336a-335">Proto očekávat hello kanálu tootake **přibližně 30 minut** tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="4336a-335">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>  
   
    ![Datová sada](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. <span data-ttu-id="4336a-337">Pokud je řez hello v **připraven** stavu, zkontrolujte hello `partitioneddata` složky v hello `adfgetstarted` kontejneru ve službě blob storage hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-337">When hello slice is in **Ready** state, check hello `partitioneddata` folder in hello `adfgetstarted` container in your blob storage for hello output data.</span></span>  

    ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. <span data-ttu-id="4336a-339">Klikněte na tlačítko hello řez toosee podrobnosti o jeho **datový řez** okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-339">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

    ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. <span data-ttu-id="4336a-341">Klikněte na aktivitu spustit v hello **aktivita spuštěna seznamu** toosee podrobnosti o aktivitě spustit (Hive aktivitu v tomto scénáři) **podrobnosti o spuštění aktivit** okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-341">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span> 
  
    ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    <span data-ttu-id="4336a-343">Ze souborů protokolů hello se zobrazí informace o stavu a hello dotaz Hive, která byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4336a-343">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="4336a-344">Tyto protokoly jsou užitečné při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="4336a-344">These logs are useful for troubleshooting any issues.</span></span>  

<span data-ttu-id="4336a-345">V tématu [monitorování datových sad a kanálu](data-factory-monitor-manage-pipelines.md) pokyny, jak toouse hello Azure portálu toomonitor hello kanálu a datových sad jste vytvořili v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="4336a-345">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

#### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="4336a-346">Monitorování kanálu pomocí aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="4336a-346">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="4336a-347">Také můžete použít monitorování a Správa aplikací toomonitor vašeho kanálů.</span><span class="sxs-lookup"><span data-stu-id="4336a-347">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="4336a-348">Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-348">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="4336a-349">Klikněte na dlaždici Monitorování a správa.</span><span class="sxs-lookup"><span data-stu-id="4336a-349">Click Monitor & Manage tile.</span></span>

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. <span data-ttu-id="4336a-351">Měla by se zobrazit aplikace pro monitorování a správu.</span><span class="sxs-lookup"><span data-stu-id="4336a-351">You should see Monitor & Manage application.</span></span> <span data-ttu-id="4336a-352">Změna hello **počáteční čas** a **čas ukončení** toomatch spuštění (04-01-2016 12:00:00) a ukončení (04-02-2016 12:00:00) kanálu a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="4336a-352">Change hello **Start time** and **End time** toomatch start (04-01-2016 12:00 AM) and end times (04-02-2016 12:00 AM) of your pipeline, and click **Apply**.</span></span>

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. <span data-ttu-id="4336a-354">toosee podrobnosti o okno s aktivity, vyberte ho v hello **seznamu aktivity Windows** toosee podrobnosti o něm.</span><span class="sxs-lookup"><span data-stu-id="4336a-354">toosee details about an activity window, select it in hello **Activity Windows list** toosee details about it.</span></span>
    <span data-ttu-id="4336a-355">![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span><span class="sxs-lookup"><span data-stu-id="4336a-355">![Activity window details](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4336a-356">vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="4336a-356">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="4336a-357">Proto, chcete-li řez hello toorerun nebo znovu hello kurzu, nahrajte hello vstupní soubor (input.log) toohello `inputdata` složky hello `adfgetstarted` kontejneru.</span><span class="sxs-lookup"><span data-stu-id="4336a-357">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello `inputdata` folder of hello `adfgetstarted` container.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="4336a-358">Další poznámky</span><span class="sxs-lookup"><span data-stu-id="4336a-358">Additional notes</span></span>
- <span data-ttu-id="4336a-359">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="4336a-359">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="4336a-360">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="4336a-360">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="4336a-361">Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data.</span><span class="sxs-lookup"><span data-stu-id="4336a-361">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data.</span></span> <span data-ttu-id="4336a-362">V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="4336a-362">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="4336a-363">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam výpočetní služby podporovaných službou Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-363">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory.</span></span>
- <span data-ttu-id="4336a-364">Propojené služby propojují úložiště dat nebo výpočetní služby tooan pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-364">Linked services link data stores or compute services tooan Azure data factory.</span></span> <span data-ttu-id="4336a-365">V tématu [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats) pro všechny hello zdroje a jímky nepodporuje hello aktivity kopírování.</span><span class="sxs-lookup"><span data-stu-id="4336a-365">See [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for all hello sources and sinks supported by hello Copy Activity.</span></span> <span data-ttu-id="4336a-366">V tématu [výpočetní propojené služby](data-factory-compute-linked-services.md) hello seznam podporovaných službou Data Factory výpočetní služby a [aktivit transformace](data-factory-data-transformation-activities.md) , můžete spustit na ně.</span><span class="sxs-lookup"><span data-stu-id="4336a-366">See [compute linked services](data-factory-compute-linked-services.md) for hello list of compute services supported by Data Factory and [transformation activities](data-factory-data-transformation-activities.md) that can run on them.</span></span>
- <span data-ttu-id="4336a-367">V tématu [přesun dat z / tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) podrobnosti o vlastnostech JSON použitých ve hello propojená definice služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="4336a-367">See [Move data from/tooAzure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used in hello Azure Storage linked service definition.</span></span>
- <span data-ttu-id="4336a-368">Místo clusteru HDInsight na vyžádání můžete použít také vlastní cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4336a-368">You could use your own HDInsight cluster instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4336a-369">Podrobnosti najdete v tématu [Propojené výpočetní služby](data-factory-compute-linked-services.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-369">See [Compute Linked Services](data-factory-compute-linked-services.md) for details.</span></span>
-  <span data-ttu-id="4336a-370">Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello předcházející JSON.</span><span class="sxs-lookup"><span data-stu-id="4336a-370">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello preceding JSON.</span></span> <span data-ttu-id="4336a-371">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="4336a-371">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
- <span data-ttu-id="4336a-372">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (linkedServiceName).</span><span class="sxs-lookup"><span data-stu-id="4336a-372">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (linkedServiceName).</span></span> <span data-ttu-id="4336a-373">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-373">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="4336a-374">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="4336a-374">This behavior is by design.</span></span> <span data-ttu-id="4336a-375">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když se zpracuje určitý řez, pokud neexistuje aktivní cluster (timeToLive).</span><span class="sxs-lookup"><span data-stu-id="4336a-375">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (timeToLive).</span></span> <span data-ttu-id="4336a-376">Hello clusteru je automaticky odstraněna po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-376">hello cluster is automatically deleted when hello processing is done.</span></span>
    
    <span data-ttu-id="4336a-377">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="4336a-377">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="4336a-378">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="4336a-378">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="4336a-379">vzor podle Hello názvy těchto kontejnerů: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span><span class="sxs-lookup"><span data-stu-id="4336a-379">hello names of these containers follow a pattern: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`.</span></span> <span data-ttu-id="4336a-380">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="4336a-380">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>
- <span data-ttu-id="4336a-381">Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="4336a-381">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="4336a-382">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-382">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> 
- <span data-ttu-id="4336a-383">Tento kurz neukazuje, jak kopírovat data pomocí Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-383">This tutorial does not show how copy data by using Azure Data Factory.</span></span> <span data-ttu-id="4336a-384">Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-384">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>


## <a name="use-server-explorer-tooview-data-factories"></a><span data-ttu-id="4336a-385">Pomocí Průzkumníka serveru tooview datové továrny</span><span class="sxs-lookup"><span data-stu-id="4336a-385">Use Server Explorer tooview data factories</span></span>
1. <span data-ttu-id="4336a-386">V **Visual Studio**, klikněte na tlačítko **zobrazení** hello nabídce a klikněte na tlačítko **Průzkumníka serveru**.</span><span class="sxs-lookup"><span data-stu-id="4336a-386">In **Visual Studio**, click **View** on hello menu, and click **Server Explorer**.</span></span>
2. <span data-ttu-id="4336a-387">V okně hello Průzkumníka serveru rozbalte **Azure** a rozbalte **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="4336a-387">In hello Server Explorer window, expand **Azure** and expand **Data Factory**.</span></span> <span data-ttu-id="4336a-388">Pokud se zobrazí **přihlášení tooVisual Studio**, zadejte hello **účet** přidružené k vašemu předplatnému Azure a klikněte na tlačítko **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-388">If you see **Sign in tooVisual Studio**, enter hello **account** associated with your Azure subscription and click **Continue**.</span></span> <span data-ttu-id="4336a-389">Zadejte **heslo** a klikněte na **Přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="4336a-389">Enter **password**, and click **Sign in**.</span></span> <span data-ttu-id="4336a-390">Visual Studio se pokusí tooget informace o všechny objekty pro vytváření dat Azure v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4336a-390">Visual Studio tries tooget information about all Azure data factories in your subscription.</span></span> <span data-ttu-id="4336a-391">Zobrazí hello stav této operace v hello **seznam úkolů objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-391">You see hello status of this operation in hello **Data Factory Task List** window.</span></span>

    ![Průzkumník serveru](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. <span data-ttu-id="4336a-393">Klikněte pravým tlačítkem na objekt pro vytváření dat a vyberte **tooNew Export Data Factory Project** toocreate projekt sady Visual Studio podle existující datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="4336a-393">You can right-click a data factory, and select **Export Data Factory tooNew Project** toocreate a Visual Studio project based on an existing data factory.</span></span>

    ![Export objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a><span data-ttu-id="4336a-395">Aktualizace nástrojů služby Data Factory pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4336a-395">Update Data Factory tools for Visual Studio</span></span>
<span data-ttu-id="4336a-396">tooupdate Azure Data Factory tools pro Visual Studio, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4336a-396">tooupdate Azure Data Factory tools for Visual Studio, do hello following steps:</span></span>

1. <span data-ttu-id="4336a-397">Klikněte na tlačítko **nástroje** na hello nabídku a vyberte **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="4336a-397">Click **Tools** on hello menu and select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="4336a-398">Vyberte **aktualizace** v levém podokně hello a pak vyberte **Galerie sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="4336a-398">Select **Updates** in hello left pane and then select **Visual Studio Gallery**.</span></span>
3. <span data-ttu-id="4336a-399">Vyberte možnost **Azure Data Factory tools for Visual Studio** (Nástroje služby Azure Data Factory pro Visual Studio) a klikněte na **Aktualizovat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-399">Select **Azure Data Factory tools for Visual Studio** and click **Update**.</span></span> <span data-ttu-id="4336a-400">Pokud se tato položka nezobrazí, už máte nejnovější verzi nástroje hello hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-400">If you do not see this entry, you already have hello latest version of hello tools.</span></span>

## <a name="use-configuration-files"></a><span data-ttu-id="4336a-401">Použití konfiguračních souborů</span><span class="sxs-lookup"><span data-stu-id="4336a-401">Use configuration files</span></span>
<span data-ttu-id="4336a-402">Konfigurační soubory v sadě Visual Studio tooconfigure vlastnosti můžete použít pro propojených služeb/tabulek/kanálů pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="4336a-402">You can use configuration files in Visual Studio tooconfigure properties for linked services/tables/pipelines differently for each environment.</span></span>

<span data-ttu-id="4336a-403">Vezměte v úvahu následující definici JSON pro služby Azure Storage, propojené hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-403">Consider hello following JSON definition for an Azure Storage linked service.</span></span> <span data-ttu-id="4336a-404">toospecify **connectionString** s různými hodnotami pro accountname a accountkey pro různá prostředí (Dev/testovací/produkční) toowhich hello nasazujete entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-404">toospecify **connectionString** with different values for accountname and accountkey based on hello environment (Dev/Test/Production) toowhich you are deploying Data Factory entities.</span></span> <span data-ttu-id="4336a-405">Provedete to tak, že pro každé prostředí použijete samostatný konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="4336a-405">You can achieve this behavior by using separate configuration file for each environment.</span></span>

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

### <a name="add-a-configuration-file"></a><span data-ttu-id="4336a-406">Přidání konfiguračního souboru</span><span class="sxs-lookup"><span data-stu-id="4336a-406">Add a configuration file</span></span>
<span data-ttu-id="4336a-407">Přidejte konfigurační soubor pro každé prostředí provedením následujících kroků hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-407">Add a configuration file for each environment by performing hello following steps:</span></span>   

1. <span data-ttu-id="4336a-408">Klikněte pravým tlačítkem na projekt Data Factory hello v řešení sady Visual Studio, přejděte příliš**přidat**a klikněte na tlačítko **novou položku**.</span><span class="sxs-lookup"><span data-stu-id="4336a-408">Right-click hello Data Factory project in your Visual Studio solution, point too**Add**, and click **New item**.</span></span>
2. <span data-ttu-id="4336a-409">Vyberte **konfigurace** hello seznamu nainstalovaných šablon vlevo hello vyberte **konfigurační soubor**, zadejte **název** pro konfiguraci hello souboru a klikněte na tlačítko **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="4336a-409">Select **Config** from hello list of installed templates on hello left, select **Configuration File**, enter a **name** for hello configuration file, and click **Add**.</span></span>

    ![Přidání konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. <span data-ttu-id="4336a-411">Přidejte konfigurační parametry a jejich hodnoty v hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="4336a-411">Add configuration parameters and their values in hello following format:</span></span>

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

    <span data-ttu-id="4336a-412">Tento příklad umožňuje nakonfigurovat vlastnost connectionString propojené služby Azure Storage a propojené služby Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="4336a-412">This example configures connectionString property of an Azure Storage linked service and an Azure SQL linked service.</span></span> <span data-ttu-id="4336a-413">Všimněte si, že je hello syntaxe pro určení názvu [JsonPath](http://goessner.net/articles/JsonPath/).</span><span class="sxs-lookup"><span data-stu-id="4336a-413">Notice that hello syntax for specifying name is [JsonPath](http://goessner.net/articles/JsonPath/).</span></span>   

    <span data-ttu-id="4336a-414">Pokud má kód JSON vlastnost, která má pole hodnot, jak je znázorněno v následujícím kódu hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-414">If JSON has a property that has an array of values as shown in hello following code:</span></span>  

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

    <span data-ttu-id="4336a-415">Nakonfigurujte vlastnosti, jak je znázorněno v následující soubor konfigurace (použijte indexování od nuly) hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-415">Configure properties as shown in hello following configuration file (use zero-based indexing):</span></span>

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

### <a name="property-names-with-spaces"></a><span data-ttu-id="4336a-416">Názvy vlastností s mezerami</span><span class="sxs-lookup"><span data-stu-id="4336a-416">Property names with spaces</span></span>
<span data-ttu-id="4336a-417">Pokud název vlastnosti obsahuje mezery, použijte hranaté závorky, jak ukazuje následující příklad (název databázového serveru) hello:</span><span class="sxs-lookup"><span data-stu-id="4336a-417">If a property name has spaces in it, use square brackets as shown in hello following example (Database server name):</span></span>

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a><span data-ttu-id="4336a-418">Nasazení řešení pomocí konfigurace</span><span class="sxs-lookup"><span data-stu-id="4336a-418">Deploy solution using a configuration</span></span>
<span data-ttu-id="4336a-419">Když v sadě VS publikujete entity služby Azure Data Factory, abyste mohli zadat konfiguraci hello chcete toouse pro danou operaci publikování.</span><span class="sxs-lookup"><span data-stu-id="4336a-419">When you are publishing Azure Data Factory entities in VS, you can specify hello configuration that you want toouse for that publishing operation.</span></span>

<span data-ttu-id="4336a-420">toopublish entity v projektu Azure Data Factory pomocí konfiguračního souboru:</span><span class="sxs-lookup"><span data-stu-id="4336a-420">toopublish entities in an Azure Data Factory project using configuration file:</span></span>   

1. <span data-ttu-id="4336a-421">Klikněte pravým tlačítkem na projekt Data Factory a klikněte na tlačítko **publikovat** toosee hello **publikování položky** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="4336a-421">Right-click Data Factory project and click **Publish** toosee hello **Publish Items** dialog box.</span></span>
2. <span data-ttu-id="4336a-422">Vyberte existující datovou továrnu nebo zadejte hodnoty pro vytvoření služby data factory na hello **objekt pro vytváření dat konfigurace** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4336a-422">Select an existing data factory or specify values for creating a data factory on hello **Configure data factory** page, and click **Next**.</span></span>   
3. <span data-ttu-id="4336a-423">Na hello **publikování položky** stránky: zobrazí rozevírací seznam s dostupnými konfiguracemi pro hello **výběr konfigurace nasazení** pole.</span><span class="sxs-lookup"><span data-stu-id="4336a-423">On hello **Publish Items** page: you see a drop-down list with available configurations for hello **Select Deployment Config** field.</span></span>

    ![Výběr konfiguračního souboru](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. <span data-ttu-id="4336a-425">Vyberte hello **konfigurační soubor** , že chcete vytvořit toouse a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4336a-425">Select hello **configuration file** that you would like toouse and click **Next**.</span></span>
5. <span data-ttu-id="4336a-426">Zkontrolujte, jestli hello název souboru JSON v hello **Souhrn** a klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="4336a-426">Confirm that you see hello name of JSON file in hello **Summary** page and click **Next**.</span></span>
6. <span data-ttu-id="4336a-427">Klikněte na tlačítko **Dokončit** po dokončení operace nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-427">Click **Finish** after hello deployment operation is finished.</span></span>

<span data-ttu-id="4336a-428">Když nasadíte, hello hodnoty z konfiguračního souboru hello jsou použité tooset hodnoty vlastností v souborech JSON hello před hello entity jsou nasazené tooAzure služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-428">When you deploy, hello values from hello configuration file are used tooset values for properties in hello JSON files before hello entities are deployed tooAzure Data Factory service.</span></span>   

## <a name="use-azure-key-vault"></a><span data-ttu-id="4336a-429">Použití Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4336a-429">Use Azure Key Vault</span></span>
<span data-ttu-id="4336a-430">Není vhodné a často proti zabezpečení zásady toocommit citlivých dat jako úložiště kódu toohello řetězce připojení.</span><span class="sxs-lookup"><span data-stu-id="4336a-430">It is not advisable and often against security policy toocommit sensitive data such as connection strings toohello code repository.</span></span> <span data-ttu-id="4336a-431">V tématu [zabezpečeného publikování ADF](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) ukázku na Githubu toolearn o ukládání citlivých informací v Azure Key Vault a jeho použití při publikování entit služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-431">See [ADF Secure Publish](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) sample on GitHub toolearn about storing sensitive information in Azure Key Vault and using it while publishing Data Factory entities.</span></span> <span data-ttu-id="4336a-432">Hello zabezpečeného publikování rozšíření pro Visual Studio umožňuje toobe hello tajné klíče uložené v Key Vault a pouze odkazy toothem jsou určené v propojené služby nebo konfigurace nasazení.</span><span class="sxs-lookup"><span data-stu-id="4336a-432">hello Secure Publish extension for Visual Studio allows hello secrets toobe stored in Key Vault and only references toothem are specified in linked services/ deployment configurations.</span></span> <span data-ttu-id="4336a-433">Tyto odkazy se překládají při publikování tooAzure entity služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-433">These references are resolved when you publish Data Factory entities tooAzure.</span></span> <span data-ttu-id="4336a-434">Tyto soubory pak lze potvrdit toosource úložiště bez vystavení všech tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="4336a-434">These files can then be committed toosource repository without exposing any secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="4336a-435">Souhrn</span><span class="sxs-lookup"><span data-stu-id="4336a-435">Summary</span></span>
<span data-ttu-id="4336a-436">V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop.</span><span class="sxs-lookup"><span data-stu-id="4336a-436">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="4336a-437">Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="4336a-437">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="4336a-438">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="4336a-438">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="4336a-439">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="4336a-439">Created two **linked services**:</span></span>
   1. <span data-ttu-id="4336a-440">**Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-440">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="4336a-441">**Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-441">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="4336a-442">Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="4336a-442">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="4336a-443">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="4336a-443">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="4336a-444">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="4336a-444">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="4336a-445">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4336a-445">Next Steps</span></span>
<span data-ttu-id="4336a-446">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="4336a-446">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="4336a-447">jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-447">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="4336a-448">Dvě aktivity (spustit aktivitu po jiné) můžete zřetězené nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="4336a-448">You can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="4336a-449">Podrobné informace najdete v tématu s popisem [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="4336a-449">See [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md) for detailed information.</span></span> 


## <a name="see-also"></a><span data-ttu-id="4336a-450">Viz také</span><span class="sxs-lookup"><span data-stu-id="4336a-450">See Also</span></span>
| <span data-ttu-id="4336a-451">Téma</span><span class="sxs-lookup"><span data-stu-id="4336a-451">Topic</span></span> | <span data-ttu-id="4336a-452">Popis</span><span class="sxs-lookup"><span data-stu-id="4336a-452">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="4336a-453">Kanály</span><span class="sxs-lookup"><span data-stu-id="4336a-453">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="4336a-454">Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct řízené daty pracovních pro vaší situaci nebo podniku.</span><span class="sxs-lookup"><span data-stu-id="4336a-454">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="4336a-455">Datové sady</span><span class="sxs-lookup"><span data-stu-id="4336a-455">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="4336a-456">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-456">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="4336a-457">Aktivity transformace dat</span><span class="sxs-lookup"><span data-stu-id="4336a-457">Data Transformation Activities</span></span>](data-factory-data-transformation-activities.md) |<span data-ttu-id="4336a-458">Tento článek obsahuje seznam aktivit transformace dat (třeba transformaci HDInsight Hive, kterou jste použili v tomto kurzu) podporovaných službou Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-458">This article provides a list of data transformation activities (such as HDInsight Hive transformation you used in this tutorial) supported by Azure Data Factory.</span></span> |
| [<span data-ttu-id="4336a-459">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="4336a-459">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="4336a-460">Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="4336a-460">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="4336a-461">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="4336a-461">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="4336a-462">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="4336a-462">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
