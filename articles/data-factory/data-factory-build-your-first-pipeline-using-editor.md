---
title: "aaaBuild vaše první objekt pro vytváření dat (portál Azure) | Microsoft Docs"
description: "V tomto kurzu vytvoříte pomocí editoru služby Data Factory v hello Azure portal ukázkový kanál Azure Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="27b50-103">Kurz: Sestavení prvního objektu pro vytváření dat Azure pomocí webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27b50-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="27b50-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="27b50-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="27b50-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="27b50-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="27b50-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="27b50-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="27b50-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="27b50-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="27b50-108">Šablona Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="27b50-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="27b50-109">REST API</span><span class="sxs-lookup"><span data-stu-id="27b50-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="27b50-110">V tomto článku se dozvíte, jak toouse [portál Azure](https://portal.azure.com/) toocreate první objekt pro vytváření dat Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-110">In this article, you learn how toouse [Azure portal](https://portal.azure.com/) toocreate your first Azure data factory.</span></span> <span data-ttu-id="27b50-111">kurz hello toodo pomocí jiných nástrojů nebo sady SDK, vyberte jednu z možností hello hello rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="27b50-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span> 

<span data-ttu-id="27b50-112">Hello kanálu v tomto kurzu má jednu aktivitu: **aktivitu HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="27b50-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="27b50-113">Tato aktivita spouští skript hive v clusteru Azure HDInsight, transformací vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="27b50-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="27b50-114">Hello kanálu není naplánované toorun po měsíci mezi hello zadaný počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="27b50-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="27b50-115">Hello datovém kanálu v tomto kurzu transformuje vstupní data tooproduce výstupní data.</span><span class="sxs-lookup"><span data-stu-id="27b50-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="27b50-116">Kurz týkající se jak toocopy dat pomocí Azure Data Factory najdete v části [kurz: kopírování dat z úložiště objektů Blob tooSQL databáze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="27b50-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="27b50-117">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="27b50-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="27b50-118">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="27b50-118">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="27b50-119">Další informace najdete v tématu [plánování a provádění ve službě Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="27b50-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="27b50-120">Požadavky</span><span class="sxs-lookup"><span data-stu-id="27b50-120">Prerequisites</span></span>
1. <span data-ttu-id="27b50-121">Pročtěte [přehled kurzu](data-factory-build-your-first-pipeline.md) článek a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="27b50-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
2. <span data-ttu-id="27b50-122">Tento článek neposkytuje koncepční přehled hello služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-122">This article does not provide a conceptual overview of hello Azure Data Factory service.</span></span> <span data-ttu-id="27b50-123">Doporučujeme projít si [Úvod tooAzure Data Factory](data-factory-introduction.md) článku podrobnější přehled služby hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-123">We recommend that you go through [Introduction tooAzure Data Factory](data-factory-introduction.md) article for a detailed overview of hello service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="27b50-124">Vytvoření objektu pro vytváření dat</span><span class="sxs-lookup"><span data-stu-id="27b50-124">Create data factory</span></span>
<span data-ttu-id="27b50-125">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="27b50-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="27b50-126">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="27b50-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="27b50-127">Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data.</span><span class="sxs-lookup"><span data-stu-id="27b50-127">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="27b50-128">Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="27b50-128">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="27b50-129">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="27b50-129">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="27b50-130">Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Data + analýzy**a klikněte na tlačítko **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="27b50-130">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![Okno Vytvořit](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="27b50-132">V hello **nový objekt pro vytváření dat** okno, zadejte **GetStartedDF** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="27b50-132">In hello **New data factory** blade, enter **GetStartedDF** for hello Name.</span></span>

   ![Okno Nový objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="27b50-134">musí být Hello název objektu pro vytváření dat Azure hello **globálně jedinečný**.</span><span class="sxs-lookup"><span data-stu-id="27b50-134">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="27b50-135">Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "GetStartedDF" není k dispozici**.</span><span class="sxs-lookup"><span data-stu-id="27b50-135">If you receive hello error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="27b50-136">Změňte hello název objektu pro vytváření dat hello (třeba na Váš_název_getstarteddf) a zkuste to znovu.</span><span class="sxs-lookup"><span data-stu-id="27b50-136">Change hello name of hello data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="27b50-137">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="27b50-138">Hello název objektu pro vytváření dat hello může být registrován jako **DNS** název v budoucnosti hello a proto pak bude veřejně viditelný.</span><span class="sxs-lookup"><span data-stu-id="27b50-138">hello name of hello data factory may be registered as a **DNS** name in hello future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="27b50-139">Vyberte hello **předplatného Azure** místo hello data factory toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="27b50-139">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="27b50-140">Vyberte existující **skupinu prostředků** nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="27b50-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="27b50-141">Hello kurzu vytvořte skupinu prostředků s názvem: **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="27b50-141">For hello tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="27b50-142">Vyberte hello **umístění** hello data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-142">Select hello **location** for hello data factory.</span></span> <span data-ttu-id="27b50-143">V rozevíracím seznamu hello jsou uvedeny pouze oblasti nepodporuje hello služba Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-143">Only regions supported by hello Data Factory service are shown in hello drop-down list.</span></span>
7. <span data-ttu-id="27b50-144">Vyberte **Pin toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="27b50-144">Select **Pin toodashboard**.</span></span> 
8. <span data-ttu-id="27b50-145">Klikněte na tlačítko **vytvořit** na hello **nový objekt pro vytváření dat** okno.</span><span class="sxs-lookup"><span data-stu-id="27b50-145">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="27b50-146">instance služby Data Factory toocreate, musíte být členem skupiny hello [Přispěvatel objekt pro vytváření dat](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role na úrovni předplatného nebo prostředků skupiny hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-146">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="27b50-147">Na řídicím panelu hello, uvidíte následující hello dlaždici se stavem: nasazení služby data factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-147">On hello dashboard, you see hello following tile with status: Deploying data factory.</span></span>    

   ![Stav vytváření objektu pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="27b50-149">Blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="27b50-149">Congratulations!</span></span> <span data-ttu-id="27b50-150">Úspěšně jste vytvořili první objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="27b50-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="27b50-151">Po úspěšném vytvoření objektu pro vytváření dat hello, se zobrazí stránka objektu pro vytváření dat hello, se zobrazí hello obsah objektu pro vytváření dat hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-151">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>     

    ![Okno Objekt pro vytváření dat](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="27b50-153">Před vytvořením kanálu v hello data factory, je nutné toocreate několik entit služby Data Factory nejdřív.</span><span class="sxs-lookup"><span data-stu-id="27b50-153">Before creating a pipeline in hello data factory, you need toocreate a few Data Factory entities first.</span></span> <span data-ttu-id="27b50-154">Nejdřív vytvoříte propojené služby toolink dat úložiště nebo výpočtů tooyour data ukládat, definujete vstupní a výstupní datové sady toorepresent vstupní a výstupní data v propojených úložištích dat a poté vytvořit hello kanálu s aktivitou, která používá tyto datové sady.</span><span class="sxs-lookup"><span data-stu-id="27b50-154">You first create linked services toolink data stores/computes tooyour data store, define input and output datasets toorepresent input/output data in linked data stores, and then create hello pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="27b50-155">Vytvoření propojených služeb</span><span class="sxs-lookup"><span data-stu-id="27b50-155">Create linked services</span></span>
<span data-ttu-id="27b50-156">V tomto kroku propojíte účtu úložiště Azure a služby na vyžádání Azure HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="27b50-157">blokování účtu Azure Storage Hello hello vstupní a výstupní data pro kanál hello v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="27b50-157">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> <span data-ttu-id="27b50-158">Hello propojené služby HDInsight je použité toorun skriptu Hive určeného v hello aktivitu hello kanál v této ukázce.</span><span class="sxs-lookup"><span data-stu-id="27b50-158">hello HDInsight linked service is used toorun a Hive script specified in hello activity of hello pipeline in this sample.</span></span> <span data-ttu-id="27b50-159">Určit, co [úložiště dat](data-factory-data-movement-activities.md)/[výpočetní služby](data-factory-compute-linked-services.md) se používají ve vašem scénáři a propojit tyto služby toohello objekt pro vytváření dat vytvořením propojených služeb.</span><span class="sxs-lookup"><span data-stu-id="27b50-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services toohello data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="27b50-160">Vytvoření propojené služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="27b50-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="27b50-161">V tomto kroku propojíte datovou továrnu tooyour účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-161">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="27b50-162">V tomto kurzu použijete hello stejný účet úložiště Azure toostore vstupní a výstupní data a hello HQL soubor skriptu.</span><span class="sxs-lookup"><span data-stu-id="27b50-162">In this tutorial, you use hello same Azure Storage account toostore input/output data and hello HQL script file.</span></span>

1. <span data-ttu-id="27b50-163">Klikněte na tlačítko **vytvořit a nasadit** na hello **DATA FACTORY** okno pro **GetStartedDF**.</span><span class="sxs-lookup"><span data-stu-id="27b50-163">Click **Author and deploy** on hello **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="27b50-164">Měli byste vidět hello editoru služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-164">You should see hello Data Factory Editor.</span></span>

   ![Dlaždice Vytvořit a nasadit](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="27b50-166">Klikněte na **Nové datové úložiště** a zvolte **Účet Azure**.</span><span class="sxs-lookup"><span data-stu-id="27b50-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Nové úložiště dat – Azure Storage – nabídka](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="27b50-168">Měli byste vidět hello skript JSON pro vytvoření Azure Storage propojená služba v editoru hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-168">You should see hello JSON script for creating an Azure Storage linked service in hello editor.</span></span>

   ![Propojená služba Azure Storage](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="27b50-170">Nahraďte **název účtu** hello název účtu úložiště Azure a **klíč účtu** s přístupovým klíčem hello hello účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-170">Replace **account name** with hello name of your Azure storage account and **account key** with hello access key of hello Azure storage account.</span></span> <span data-ttu-id="27b50-171">toolearn jak přístup tooget úložiště klíčů najdete v tématu hello informace o tom, jak tooview, kopírování a opětovné vytváření úložiště přístupové klíče v [spravovat váš účet úložiště](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="27b50-171">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="27b50-172">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="27b50-172">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![Tlačítko Nasadit](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="27b50-174">Po hello propojené služby nasazení úspěšně hello **koncept-1** by měl zmizet okně a zobrazí **AzureStorageLinkedService** v hello stromovém zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-174">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

    ![Propojená služba úložiště v nabídce](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="27b50-176">Vytvoření propojené služby Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="27b50-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="27b50-177">V tomto kroku propojíte služby na vyžádání HDInsight clusteru tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-177">In this step, you link an on-demand HDInsight cluster tooyour data factory.</span></span> <span data-ttu-id="27b50-178">Hello HDInsight cluster je automaticky vytvoří za běhu a až dokončí zpracování a nečinnosti hello zadaného množství času se odstraní.</span><span class="sxs-lookup"><span data-stu-id="27b50-178">hello HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for hello specified amount of time.</span></span>

1. <span data-ttu-id="27b50-179">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další**, klikněte na **Nový výpočet** a vyberte **Cluster HDInsight na vyžádání**.</span><span class="sxs-lookup"><span data-stu-id="27b50-179">In hello **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![Nový výpočet](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="27b50-181">Zkopírujte a vložte následující fragment kódu toohello hello **koncept-1** okno.</span><span class="sxs-lookup"><span data-stu-id="27b50-181">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="27b50-182">Hello fragmentu kódu JSON popisuje vlastnosti hello, které jsou používané toocreate hello HDInsight clusteru na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="27b50-182">hello JSON snippet describes hello properties that are used toocreate hello HDInsight cluster on-demand.</span></span>

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    <span data-ttu-id="27b50-183">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="27b50-183">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="27b50-184">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="27b50-184">Property</span></span> | <span data-ttu-id="27b50-185">Popis</span><span class="sxs-lookup"><span data-stu-id="27b50-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="27b50-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="27b50-186">ClusterSize</span></span> |<span data-ttu-id="27b50-187">Určuje velikost hello hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="27b50-187">Specifies hello size of hello HDInsight cluster.</span></span> |
   | <span data-ttu-id="27b50-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="27b50-188">TimeToLive</span></span> | <span data-ttu-id="27b50-189">Nastavení této hello nečinnosti pro hello HDInsight cluster, než se odstraní.</span><span class="sxs-lookup"><span data-stu-id="27b50-189">Specifies that hello idle time for hello HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="27b50-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="27b50-190">linkedServiceName</span></span> | <span data-ttu-id="27b50-191">Určuje účet úložiště hello, který je použité toostore hello protokoly, které jsou generovány nástrojem HDInsight.</span><span class="sxs-lookup"><span data-stu-id="27b50-191">Specifies hello storage account that is used toostore hello logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="27b50-192">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="27b50-192">Note hello following points:</span></span>

   * <span data-ttu-id="27b50-193">Hello Data Factory vytvoří **systémem Linux** cluster HDInsight s hello JSON.</span><span class="sxs-lookup"><span data-stu-id="27b50-193">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello JSON.</span></span> <span data-ttu-id="27b50-194">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="27b50-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="27b50-195">Místo clusteru HDInsight na vyžádání můžete použít také **vlastní cluster HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="27b50-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="27b50-196">Podrobnosti najdete v tématu [Propojená služba HDInsight](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).</span><span class="sxs-lookup"><span data-stu-id="27b50-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="27b50-197">Vytvoří Hello HDInsight cluster **výchozí kontejner** v úložišti objektů blob hello jste zadali v hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="27b50-197">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="27b50-198">HDInsight neprovede odstranění tohoto kontejneru při odstranění clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-198">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="27b50-199">Toto chování je záměrné.</span><span class="sxs-lookup"><span data-stu-id="27b50-199">This behavior is by design.</span></span> <span data-ttu-id="27b50-200">Díky propojené službě HDInsight na vyžádání se cluster HDInsight vytvoří pokaždé, když je zpracován určitý řez, pokud neexistuje aktivní cluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="27b50-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="27b50-201">Hello clusteru je automaticky odstraněna po dokončení zpracování hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-201">hello cluster is automatically deleted when hello processing is done.</span></span>

       <span data-ttu-id="27b50-202">Po zpracování dalších řezů se vám ve službě Azure Blob Storage objeví spousta kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="27b50-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="27b50-203">Pokud pro řešení potíží s hello úloh je nepotřebujete, můžete toodelete je úložiště hello tooreduce náklady.</span><span class="sxs-lookup"><span data-stu-id="27b50-203">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="27b50-204">vzor podle Hello názvy těchto kontejnerů: "adf**název_vašeho_objektu_pro_vytváření_dat**-**linkedservicename**- razítko_data_a_času".</span><span class="sxs-lookup"><span data-stu-id="27b50-204">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="27b50-205">Pomocí nástrojů, jako [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete kontejnery ve vaší službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="27b50-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="27b50-206">Podrobnosti najdete v tématu [Propojená služba HDInsight na vyžádání](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).</span><span class="sxs-lookup"><span data-stu-id="27b50-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="27b50-207">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello propojené služby.</span><span class="sxs-lookup"><span data-stu-id="27b50-207">Click **Deploy** on hello command bar toodeploy hello linked service.</span></span>

    ![Nasazení propojené služby HDInsight na vyžádání](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="27b50-209">Zkontrolujte, jestli obě **AzureStorageLinkedService** a **HDInsightOnDemandLinkedService** v hello stromovém zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in hello tree view on hello left.</span></span>

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="27b50-211">Vytvoření datových sad</span><span class="sxs-lookup"><span data-stu-id="27b50-211">Create datasets</span></span>
<span data-ttu-id="27b50-212">V tomto kroku vytvoříte datové sady toorepresent hello vstupní a výstupní data pro zpracování Hive.</span><span class="sxs-lookup"><span data-stu-id="27b50-212">In this step, you create datasets toorepresent hello input and output data for Hive processing.</span></span> <span data-ttu-id="27b50-213">Tyto datové sady odkazují toohello **AzureStorageLinkedService** jste vytvořili dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="27b50-213">These datasets refer toohello **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="27b50-214">Hello propojené služby body tooan účet služby Azure Storage a datové sady zadejte kontejner, složku a název souboru v úložišti hello, který obsahuje vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="27b50-214">hello linked service points tooan Azure Storage account and datasets specify container, folder, file name in hello storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="27b50-215">Vytvoření vstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="27b50-215">Create input dataset</span></span>
1. <span data-ttu-id="27b50-216">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="27b50-216">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![Nová datová sada](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="27b50-218">Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-218">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="27b50-219">V hello fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobInput** , která představuje vstupní data pro aktivitu v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-219">In hello JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in hello pipeline.</span></span> <span data-ttu-id="27b50-220">Kromě toho určíte, zda text hello vstupní data nachází v kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="27b50-220">In addition, you specify that hello input data is located in hello blob container called **adfgetstarted** and hello folder called **inputdata**.</span></span>

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
    <span data-ttu-id="27b50-221">Hello následující tabulka obsahuje popis vlastností hello JSON použitých ve fragmentu hello:</span><span class="sxs-lookup"><span data-stu-id="27b50-221">hello following table provides descriptions for hello JSON properties used in hello snippet:</span></span>

   | <span data-ttu-id="27b50-222">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="27b50-222">Property</span></span> | <span data-ttu-id="27b50-223">Popis</span><span class="sxs-lookup"><span data-stu-id="27b50-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="27b50-224">type</span><span class="sxs-lookup"><span data-stu-id="27b50-224">type</span></span> |<span data-ttu-id="27b50-225">Hello vlastnost Typ nastavena příliš**AzureBlob** protože data se nachází v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="27b50-225">hello type property is set too**AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="27b50-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="27b50-226">linkedServiceName</span></span> |<span data-ttu-id="27b50-227">Odkazuje toohello **AzureStorageLinkedService** jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="27b50-227">Refers toohello **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="27b50-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="27b50-228">folderPath</span></span> | <span data-ttu-id="27b50-229">Určuje objekt blob hello **kontejneru** a hello **složky** , který obsahuje vstupní objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="27b50-229">Specifies hello blob **container** and hello **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="27b50-230">fileName</span><span class="sxs-lookup"><span data-stu-id="27b50-230">fileName</span></span> |<span data-ttu-id="27b50-231">Tato vlastnost je nepovinná.</span><span class="sxs-lookup"><span data-stu-id="27b50-231">This property is optional.</span></span> <span data-ttu-id="27b50-232">Pokud ji vynecháte, jsou vybrány všechny soubory hello z hello folderPath.</span><span class="sxs-lookup"><span data-stu-id="27b50-232">If you omit this property, all hello files from hello folderPath are picked.</span></span> <span data-ttu-id="27b50-233">V tomto kurzu pouze hello **input.log** zpracování.</span><span class="sxs-lookup"><span data-stu-id="27b50-233">In this tutorial, only hello **input.log** is processed.</span></span> |
   | <span data-ttu-id="27b50-234">type</span><span class="sxs-lookup"><span data-stu-id="27b50-234">type</span></span> |<span data-ttu-id="27b50-235">soubory protokolu Hello jsou v textovém formátu, takže používáme **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="27b50-235">hello log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="27b50-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="27b50-236">columnDelimiter</span></span> |<span data-ttu-id="27b50-237">sloupce v souborech protokolů hello oddělené **čárku (`,`)**</span><span class="sxs-lookup"><span data-stu-id="27b50-237">columns in hello log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="27b50-238">frequency/interval</span><span class="sxs-lookup"><span data-stu-id="27b50-238">frequency/interval</span></span> |<span data-ttu-id="27b50-239">frekvence nastavená příliš**měsíc** a interval je **1**, což znamená, že hello vstupní řezy jsou k dispozici každý měsíc.</span><span class="sxs-lookup"><span data-stu-id="27b50-239">frequency set too**Month** and interval is **1**, which means that hello input slices are available monthly.</span></span> |
   | <span data-ttu-id="27b50-240">external</span><span class="sxs-lookup"><span data-stu-id="27b50-240">external</span></span> | <span data-ttu-id="27b50-241">Tato vlastnost nastavena příliš**true** Pokud hello vstupní data nevygenerovala pomocí tohoto kanálu.</span><span class="sxs-lookup"><span data-stu-id="27b50-241">This property is set too**true** if hello input data is not generated by this pipeline.</span></span> <span data-ttu-id="27b50-242">V tomto kurzu není soubor input.log hello generována tohoto kanálu, abyste nám nastavit vlastnost tootrue hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-242">In this tutorial, hello input.log file is not generated by this pipeline, so we set hello property tootrue.</span></span> |

    <span data-ttu-id="27b50-243">Další informace o těchto vlastnostech JSON najdete v článku [konektor Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="27b50-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="27b50-244">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello nově vytvořený datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="27b50-244">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span> <span data-ttu-id="27b50-245">Měli byste vidět hello datovou sadu v hello stromového zobrazení na levé straně hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-245">You should see hello dataset in hello tree view on hello left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="27b50-246">Vytvoření výstupní datové sady</span><span class="sxs-lookup"><span data-stu-id="27b50-246">Create output dataset</span></span>
<span data-ttu-id="27b50-247">Teď vytvořte hello výstupní datovou sadu toorepresent hello výstupní data ve hello úložiště objektů Blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-247">Now, you create hello output dataset toorepresent hello output data stored in hello Azure Blob storage.</span></span>

1. <span data-ttu-id="27b50-248">V hello **editoru služby Data Factory**, klikněte na tlačítko **... Další** na panelu příkazů hello, klikněte na tlačítko **nová datová sada**a vyberte **úložiště objektů Azure Blob**.</span><span class="sxs-lookup"><span data-stu-id="27b50-248">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="27b50-249">Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-249">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="27b50-250">V hello fragmentu kódu JSON vytváříte datovou sadu s názvem **AzureBlobOutput**a určení hello struktura hello dat, která je vytvořena pomocí skriptu Hive hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-250">In hello JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying hello structure of hello data that is produced by hello Hive script.</span></span> <span data-ttu-id="27b50-251">Kromě toho určíte, že hello výsledky ukládat do kontejneru objektů blob hello s názvem **adfgetstarted** a hello složku s názvem **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="27b50-251">In addition, you specify that hello results are stored in hello blob container called **adfgetstarted** and hello folder called **partitioneddata**.</span></span> <span data-ttu-id="27b50-252">Hello **dostupnosti** část určuje, že hello výstupní sada vytváří jednou měsíčně.</span><span class="sxs-lookup"><span data-stu-id="27b50-252">hello **availability** section specifies that hello output dataset is produced on a monthly basis.</span></span>

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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
    <span data-ttu-id="27b50-253">V tématu **vytvoření vstupní datové sady hello** části popisy těchto vlastností.</span><span class="sxs-lookup"><span data-stu-id="27b50-253">See **Create hello input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="27b50-254">Sady nenajdete vlastnost external hello u výstupní datové jako hello datovou sadu vytváří služba Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-254">You do not set hello external property on an output dataset as hello dataset is produced by hello Data Factory service.</span></span>
3. <span data-ttu-id="27b50-255">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello nově vytvořený datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="27b50-255">Click **Deploy** on hello command bar toodeploy hello newly created dataset.</span></span>
4. <span data-ttu-id="27b50-256">Ověřte, že tuto datovou sadu hello byla úspěšně vytvořena.</span><span class="sxs-lookup"><span data-stu-id="27b50-256">Verify that hello dataset is created successfully.</span></span>

    ![Zobrazení stromu s propojenými službami](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="27b50-258">Vytvoření kanálu</span><span class="sxs-lookup"><span data-stu-id="27b50-258">Create pipeline</span></span>
<span data-ttu-id="27b50-259">V tomto kroku vytvoříte svůj první kanál s aktivitou **HDInsightHive**.</span><span class="sxs-lookup"><span data-stu-id="27b50-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="27b50-260">Vstupní řez je dostupný jednou měsíčně (frekvence: Month, interval: 1), výstupní řez se vytváří jednou měsíčně a vlastnost scheduler hello aktivity hello nastavena také toomonthly.</span><span class="sxs-lookup"><span data-stu-id="27b50-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and hello scheduler property for hello activity is also set toomonthly.</span></span> <span data-ttu-id="27b50-261">nastavení Hello hello výstupní datovou sadu a hello aktivitu Plánovač se musí shodovat.</span><span class="sxs-lookup"><span data-stu-id="27b50-261">hello settings for hello output dataset and hello activity scheduler must match.</span></span> <span data-ttu-id="27b50-262">Výstupní datové sady v současné době je, jaké jednotky hello plánu, takže je nutné vytvořit datovou sadu výstupů i v případě, že hello aktivita nevytváří žádný výstup.</span><span class="sxs-lookup"><span data-stu-id="27b50-262">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="27b50-263">Pokud aktivita hello neberou žádný vstup, můžete přeskočit vytvoření vstupní datové sady hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-263">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="27b50-264">na konci hello v této části jsou vysvětleny Hello vlastnosti používané ve hello následující JSON.</span><span class="sxs-lookup"><span data-stu-id="27b50-264">hello properties used in hello following JSON are explained at hello end of this section.</span></span>

1. <span data-ttu-id="27b50-265">V hello **editoru služby Data Factory**, klikněte na tlačítko **třemi tečkami (...) Další příkazy** a pak klikněte na **nový kanál**.</span><span class="sxs-lookup"><span data-stu-id="27b50-265">In hello **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![Tlačítko Nový kanál](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="27b50-267">Zkopírujte a vložte následující fragment kódu toohello koncept-1 okno hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-267">Copy and paste hello following snippet toohello Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="27b50-268">Nahraďte **storageaccountname** hello název účtu úložiště v hello JSON.</span><span class="sxs-lookup"><span data-stu-id="27b50-268">Replace **storageaccountname** with hello name of your storage account in hello JSON.</span></span>
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
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
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    <span data-ttu-id="27b50-269">V hello fragmentu kódu JSON vytváříte kanál sestávající z jediné aktivity, který používá Hive tooprocess Data v clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="27b50-269">In hello JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive tooprocess Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="27b50-270">soubor skriptu Hive Hello **partitionweblogs.hql**, je uložený v účtu úložiště Azure hello (určeného hello scriptLinkedService, nazývá **AzureStorageLinkedService**) a v  **skript** složky v kontejneru hello **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="27b50-270">hello Hive script file, **partitionweblogs.hql**, is stored in hello Azure storage account (specified by hello scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in hello container **adfgetstarted**.</span></span>

    <span data-ttu-id="27b50-271">Hello **definuje** část se nastavení používané toospecify hello běhového prostředí, které se předávají toohello skriptu hive jako konfigurační hodnoty Hive (např. ${hiveconf: inputtable}, ${určuje}).</span><span class="sxs-lookup"><span data-stu-id="27b50-271">hello **defines** section is used toospecify hello runtime settings that are passed toohello hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="27b50-272">Hello **spustit** a **end** vlastnosti kanálu hello určuje hello aktivní období kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-272">hello **start** and **end** properties of hello pipeline specifies hello active period of hello pipeline.</span></span>

    <span data-ttu-id="27b50-273">V kódu JSON aktivity hello určujete, že hello skript Hive běžet ve výpočetní hello určeného hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="27b50-273">In hello activity JSON, you specify that hello Hive script runs on hello compute specified by hello **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="27b50-274">V části "JSON kanálu" v [kanály a aktivity v Azure Data Factory](data-factory-create-pipelines.md) podrobnosti o vlastnostech JSON použitých v příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in hello example.</span></span>
   >
   >
3. <span data-ttu-id="27b50-275">Potvrďte hello následující:</span><span class="sxs-lookup"><span data-stu-id="27b50-275">Confirm hello following:</span></span>

   1. <span data-ttu-id="27b50-276">**Input.log** soubor existuje v hello **inputdata** složky hello **adfgetstarted** kontejneru v hello úložiště objektů blob v Azure</span><span class="sxs-lookup"><span data-stu-id="27b50-276">**input.log** file exists in hello **inputdata** folder of hello **adfgetstarted** container in hello Azure blob storage</span></span>
   2. <span data-ttu-id="27b50-277">**partitionweblogs.hql** soubor existuje v hello **skriptu** složky hello **adfgetstarted** kontejneru v hello úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-277">**partitionweblogs.hql** file exists in hello **script** folder of hello **adfgetstarted** container in hello Azure blob storage.</span></span> <span data-ttu-id="27b50-278">Předpokladem pro dokončení hello kroky hello [přehled kurzu](data-factory-build-your-first-pipeline.md) Pokud nevidíte tyto soubory.</span><span class="sxs-lookup"><span data-stu-id="27b50-278">Complete hello prerequisite steps in hello [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="27b50-279">Potvrďte, že nahrazen **storageaccountname** hello název účtu úložiště v hello kanálu JSON.</span><span class="sxs-lookup"><span data-stu-id="27b50-279">Confirm that you replaced **storageaccountname** with hello name of your storage account in hello pipeline JSON.</span></span>
4. <span data-ttu-id="27b50-280">Klikněte na tlačítko **nasadit** na hello příkazovém řádku toodeploy hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="27b50-280">Click **Deploy** on hello command bar toodeploy hello pipeline.</span></span> <span data-ttu-id="27b50-281">Od hello **spustit** a **end** jsou nastavené v posledních hello a **isPaused** je sada toofalse, hello kanál (aktivita v kanálu hello) spustí hned po nasazení.</span><span class="sxs-lookup"><span data-stu-id="27b50-281">Since hello **start** and **end** times are set in hello past and **isPaused** is set toofalse, hello pipeline (activity in hello pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="27b50-282">Zkontrolujte, jestli kanál hello v zobrazení stromu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-282">Confirm that you see hello pipeline in hello tree view.</span></span>

    ![Zobrazení stromu s kanálem](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="27b50-284">Úspěšně jste vytvořili první kanál, blahopřejeme!</span><span class="sxs-lookup"><span data-stu-id="27b50-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="27b50-285">Monitorování kanálu</span><span class="sxs-lookup"><span data-stu-id="27b50-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="27b50-286">Monitorování kanálu pomocí Zobrazení diagramu</span><span class="sxs-lookup"><span data-stu-id="27b50-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="27b50-287">Klikněte na tlačítko **X** okna editoru služby Data Factory tooclose toonavigate zpět toohello okno objekt pro vytváření dat a klikněte na tlačítko **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="27b50-287">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Dlaždice Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="27b50-289">V zobrazení diagramu hello zobrazí se přehled hello kanálů a datové sady použité v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="27b50-289">In hello Diagram View, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Zobrazení diagramu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="27b50-291">tooview všechny aktivity v kanálu hello, klikněte pravým tlačítkem na kanál v hello diagram a klikněte na Otevřít kanál.</span><span class="sxs-lookup"><span data-stu-id="27b50-291">tooview all activities in hello pipeline, right-click pipeline in hello diagram and click Open Pipeline.</span></span>

    ![Nabídka Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="27b50-293">Zkontrolujte, jestli aktivita HDInsightHive hello v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-293">Confirm that you see hello HDInsightHive activity in hello pipeline.</span></span>

    ![Zobrazení Otevřít kanál](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="27b50-295">toonavigate zpět toohello předchozího zobrazení, klikněte na tlačítko **objekt pro vytváření dat** v nabídce hello s popisem cesty v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-295">toonavigate back toohello previous view, click **Data factory** in hello breadcrumb menu at hello top.</span></span>
5. <span data-ttu-id="27b50-296">V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="27b50-296">In hello **Diagram View**, double-click hello dataset **AzureBlobInput**.</span></span> <span data-ttu-id="27b50-297">Potvrďte, že hello řez je v **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="27b50-297">Confirm that hello slice is in **Ready** state.</span></span> <span data-ttu-id="27b50-298">Může trvat několik minut, než se řez tooshow hello ve stavu Připraveno.</span><span class="sxs-lookup"><span data-stu-id="27b50-298">It may take a couple of minutes for hello slice tooshow up in Ready state.</span></span> <span data-ttu-id="27b50-299">Pokud neodehrává se po určité době čekat na, zobrazit, pokud máte hello vstupní soubor (input.log) umístit do hello správném kontejneru (adfgetstarted) a složce (inputdata).</span><span class="sxs-lookup"><span data-stu-id="27b50-299">If it does not happen after you wait for sometime, see if you have hello input file (input.log) placed in hello right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Vstupní řez ve stavu Připraveno](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="27b50-301">Klikněte na tlačítko **X** tooclose **AzureBlobInput** okno.</span><span class="sxs-lookup"><span data-stu-id="27b50-301">Click **X** tooclose **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="27b50-302">V hello **zobrazení diagramu**, dvakrát klikněte na datovou sadu hello **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="27b50-302">In hello **Diagram View**, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="27b50-303">Uvidíte, že hello řez, který se právě zpracovává.</span><span class="sxs-lookup"><span data-stu-id="27b50-303">You see that hello slice that is currently being processed.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="27b50-305">Po dokončení zpracování uvidíte hello řez ve **připraven** stavu.</span><span class="sxs-lookup"><span data-stu-id="27b50-305">When processing is done, you see hello slice in **Ready** state.</span></span>

   ![Datová sada](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="27b50-307">Vytváření clusteru HDInsight na vyžádání většinou nějakou dobu trvá (přibližně 20 minut).</span><span class="sxs-lookup"><span data-stu-id="27b50-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="27b50-308">Proto očekávat hello kanálu trvat příliš **přibližně 30 minut** tooprocess hello řez.</span><span class="sxs-lookup"><span data-stu-id="27b50-308">Therefore, expect hello pipeline too      take **approximately 30 minutes** tooprocess hello slice.</span></span>
   >
   >

9. <span data-ttu-id="27b50-309">Pokud je řez hello v **připraven** stavu, zkontrolujte hello **partitioneddata** složky v hello **adfgetstarted** kontejneru ve službě blob storage hello výstupní data.</span><span class="sxs-lookup"><span data-stu-id="27b50-309">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

   ![Výstupní data](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="27b50-311">Klikněte na tlačítko hello řez toosee podrobnosti o jeho **datový řez** okno.</span><span class="sxs-lookup"><span data-stu-id="27b50-311">Click hello slice toosee details about it in a **Data slice** blade.</span></span>

   ![Podrobnosti o datovém řezu](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="27b50-313">Klikněte na aktivitu spustit v hello **aktivita spuštěna seznamu** toosee podrobnosti o aktivitě spustit (Hive aktivitu v tomto scénáři) **podrobnosti o spuštění aktivit** okno.</span><span class="sxs-lookup"><span data-stu-id="27b50-313">Click an activity run in hello **Activity runs list** toosee details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Podrobnosti o spuštění aktivit](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="27b50-315">Ze souborů protokolů hello se zobrazí informace o stavu a hello dotaz Hive, která byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="27b50-315">From hello log files, you can see hello Hive query that was executed and status information.</span></span> <span data-ttu-id="27b50-316">Tyto protokoly jsou užitečné při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="27b50-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="27b50-317">Další podrobnosti najdete v článku [Monitorování a správa kanálů pomocí oken Azure Portal](data-factory-monitor-manage-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="27b50-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="27b50-318">vstupní soubor Hello se po úspěšném zpracování řezu hello se odstraní.</span><span class="sxs-lookup"><span data-stu-id="27b50-318">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="27b50-319">Chcete-li řez hello toorerun nebo znovu hello kurzu, tedy nahrajte hello vstupní soubor (input.log) toohello složky inputdata hello kontejneru adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="27b50-319">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="27b50-320">Monitorování kanálu pomocí aplikace pro monitorování a správu</span><span class="sxs-lookup"><span data-stu-id="27b50-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="27b50-321">Také můžete použít monitorování a Správa aplikací toomonitor vašeho kanálů.</span><span class="sxs-lookup"><span data-stu-id="27b50-321">You can also use Monitor & Manage application toomonitor your pipelines.</span></span> <span data-ttu-id="27b50-322">Podrobnosti o použití této aplikace najdete v tématu [Monitorování a správa kanálů služby Azure Data Factory pomocí aplikace pro monitorování a správu](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="27b50-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="27b50-323">Klikněte na tlačítko **monitorování a správa** na domovskou stránku hello objektu pro vytváření dat dlaždici.</span><span class="sxs-lookup"><span data-stu-id="27b50-323">Click **Monitor & Manage** tile on hello home page for your data factory.</span></span>

    ![Dlaždice Monitorování a správa](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="27b50-325">Měla by se zobrazit **aplikace pro monitorování a správu**.</span><span class="sxs-lookup"><span data-stu-id="27b50-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="27b50-326">Změna hello **počáteční čas** a **čas ukončení** toomatch spuštění a ukončení vašeho kanálu a klikněte na tlačítko **použít**.</span><span class="sxs-lookup"><span data-stu-id="27b50-326">Change hello **Start time** and **End time** toomatch start and end times of your pipeline, and click **Apply**.</span></span>

    ![Aplikace pro monitorování a správu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="27b50-328">Vyberte okno s aktivity v hello **aktivity Windows** seznamu toosee podrobnosti o něm.</span><span class="sxs-lookup"><span data-stu-id="27b50-328">Select an activity window in hello **Activity Windows** list toosee details about it.</span></span>

    ![Podrobnosti o okně aktivity](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="27b50-330">Souhrn</span><span class="sxs-lookup"><span data-stu-id="27b50-330">Summary</span></span>
<span data-ttu-id="27b50-331">V tomto kurzu jste vytvořili Azure data factory tooprocess data pomocí skriptu Hive v clusteru HDInsight hadoop.</span><span class="sxs-lookup"><span data-stu-id="27b50-331">In this tutorial, you created an Azure data factory tooprocess data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="27b50-332">Jste použili hello editoru služby Data Factory v hello Azure portálu toodo hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="27b50-332">You used hello Data Factory Editor in hello Azure portal toodo hello following steps:</span></span>  

1. <span data-ttu-id="27b50-333">Vytvořili jste **objekt pro vytváření dat** Azure.</span><span class="sxs-lookup"><span data-stu-id="27b50-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="27b50-334">Vytvořili jste dvě **propojené služby**:</span><span class="sxs-lookup"><span data-stu-id="27b50-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="27b50-335">**Úložiště Azure** propojené služby toolink služby Azure blob storage, který obsahuje vstupní/výstupní soubory toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-335">**Azure Storage** linked service toolink your Azure blob storage that holds input/output files toohello data factory.</span></span>
   2. <span data-ttu-id="27b50-336">**Azure HDInsight** na vyžádání propojené služby toolink služby na vyžádání HDInsight Hadoop clusteru toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-336">**Azure HDInsight** on-demand linked service toolink an on-demand HDInsight Hadoop cluster toohello data factory.</span></span> <span data-ttu-id="27b50-337">Azure Data Factory vytvoří HDInsight Hadoop clusteru za běhu tooprocess vstupní data a výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="27b50-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time tooprocess input data and produce output data.</span></span>
3. <span data-ttu-id="27b50-338">Vytvořili jste dvě **datové sady**, které popisují vstupní a výstupní data aktivity HDInsight Hive v kanálu hello.</span><span class="sxs-lookup"><span data-stu-id="27b50-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in hello pipeline.</span></span>
4. <span data-ttu-id="27b50-339">Vytvořili jste **kanál** s aktivitou **HDInsight Hive**.</span><span class="sxs-lookup"><span data-stu-id="27b50-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="27b50-340">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27b50-340">Next Steps</span></span>
<span data-ttu-id="27b50-341">V tomto článku jste vytvořili kanál s aktivitou transformace (aktivita HDInsight), která v clusteru HDInsight na vyžádání spouští skript Hive.</span><span class="sxs-lookup"><span data-stu-id="27b50-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="27b50-342">jak zjistit, toouse toocopy aktivity kopírování dat z tooAzure objektů Blob v Azure SQL, toosee [kurz: kopírování dat ze objektů blob v Azure tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="27b50-342">toosee how toouse a Copy Activity toocopy data from an Azure Blob tooAzure SQL, see [Tutorial: Copy data from an Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="27b50-343">Viz také</span><span class="sxs-lookup"><span data-stu-id="27b50-343">See Also</span></span>
| <span data-ttu-id="27b50-344">Téma</span><span class="sxs-lookup"><span data-stu-id="27b50-344">Topic</span></span> | <span data-ttu-id="27b50-345">Popis</span><span class="sxs-lookup"><span data-stu-id="27b50-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="27b50-346">Kanály</span><span class="sxs-lookup"><span data-stu-id="27b50-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="27b50-347">Tento článek vám pomůže pochopit kanály a aktivity v Azure Data Factory a jak toouse je tooconstruct začátku do konce řízené daty pracovní postupy pro vaší situaci nebo podniku.</span><span class="sxs-lookup"><span data-stu-id="27b50-347">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="27b50-348">Datové sady</span><span class="sxs-lookup"><span data-stu-id="27b50-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="27b50-349">Tento článek vám pomůže pochopit datové sady ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="27b50-350">Plánování a provádění</span><span class="sxs-lookup"><span data-stu-id="27b50-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="27b50-351">Tento článek vysvětluje aspekty plánování a provádění hello aplikačního modelu služby Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="27b50-351">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="27b50-352">Monitorování a správa kanálů pomocí monitorovací aplikace</span><span class="sxs-lookup"><span data-stu-id="27b50-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="27b50-353">Tento článek popisuje, jak toomonitor, spravovat a ladit kanály pomocí hello monitorování a správu aplikací.</span><span class="sxs-lookup"><span data-stu-id="27b50-353">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |
